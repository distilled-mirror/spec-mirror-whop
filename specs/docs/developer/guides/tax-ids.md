> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

> ## Agent Instructions
> Treat these docs and the Whop MCP server as the source of truth for Whop API behavior. Don't infer endpoint behavior from marketing pages.
> MCP server: connect https://mcp.whop.com/mcp (streamable HTTP, browser sign-in, no API key in the client config) to search these docs and read or change live Whop data. Setup: https://docs.whop.com/developer/guides/ai_and_mcp.md
> Whop CLI: install with `curl -fsSL https://whop.com/install.sh | sh` to scaffold, run, and deploy Whop apps and websites. It acts on production, not the sandbox. Overview: https://docs.whop.com/cli/overview.md Agent mode: https://docs.whop.com/cli/agent-mode.md
> Hosting: Whop serves a site or app from the build `whop apps deploy` uploads, with rollbacks, secrets, and server logs. Guide: https://docs.whop.com/developer/websites/hosting.md
> Plugins: in Claude Code, run `/plugin marketplace add whopio/plugins`, then `/plugin install whop@whop`. Guide: https://docs.whop.com/developer/guides/plugins/claude-code.md Every agent tool: https://docs.whop.com/developer/ai/overview.md
> Environments: build and test in the sandbox (API `https://sandbox-api.whop.com/api/v1`, keys at https://sandbox.whop.com/dashboard/developer, SDK environment `Sandbox`; no real money). Production (API `https://api.whop.com/api/v1`, keys at https://whop.com/dashboard/developer, SDK environment `Production`, the SDK default) moves real money, so use it only at launch. The two share no accounts, keys, products, or webhooks, and their keys look identical: never mix them. Sandbox guide: https://docs.whop.com/developer/guides/sandbox.md Comparison: https://docs.whop.com/developer/quickstart.md
> API: send `Authorization: Bearer <API_KEY>` from your server only, and pin a dated version with the `Api-Version-Date` header. OpenAPI spec: https://api.whop.com/api/v1/openapi.json
> Which API: build on the Current API, whose pages live under https://docs.whop.com/api-reference/ (start at https://docs.whop.com/api-reference/overview.md). Pages under /api-reference/legacy/ are the Legacy API. Use one only when https://docs.whop.com/api-reference/stability.md lists the resource as Legacy-only.

# Tax IDs

> Format VAT, GST, and other tax registration numbers so Whop accepts them and applies them to tax.

A tax ID is the registration number a tax authority issues to a business. You send it as a `tax_id_type`, such as `eu_vat` or `gb_vat`, and the number itself. You send tax IDs in two places. On an account, a tax ID also records that the business is registered for tax in that country. On a payment quote, the buyer's tax ID prices a business purchase. This page covers the format each type expects, what Whop changes in a number you send, and what it rejects. For who collects and remits tax on Whop sales, see [Taxes](https://docs.whop.com/payments-and-billing/fees/taxes.md).

> [!NOTE]
>
> All EU member states share the `eu_vat` type. Whop works out the country from the first two letters of the number, so a VAT number from an EU country must start with its country code.

| | Account tax IDs | Buyer tax ID |
| - | - | - |
| **Field** | `tax_identifiers` on [Update Account](https://docs.whop.com/api-reference/accounts/update-account.md) | `tax_ids` on [Create Payment Quote](https://docs.whop.com/api-reference/payment-quotes/create-payment-quote.md) |
| **What it does** | Records where the business is registered for tax. When the business remits its own tax, Whop collects tax outside the US only in countries where the account has a tax ID. US collection follows `tax_collection_enabled_states`. | Prices the purchase as business-to-business where that applies, such as EU reverse charge. |
| **How many** | One per `tax_id_type`. | One per quote. |
| **Formatting** | `eu_vat`: spaces, dots, and dashes removed, letters uppercased, and a leading `GR` changed to `EL`. Other types: stored as you send them. | `eu_vat`: spaces, dots, and dashes removed. Whop keeps the letter case and prefix you send. Other types: spaces removed. |

## Add a tax ID to an account

Send each tax ID as a `tax_id_type` and `tax_id_value`. Whop stores `DE 123.456.789` below as `DE123456789`.

```typescript TypeScript theme={null}
import { WhopClient } from "@whop/sdk";

const client = new WhopClient({ token: process.env.WHOP_API_KEY });

await client.accounts.update({
  id: "biz_xxxxxxxxxxxxx",
  tax_identifiers: [{ tax_id_type: "eu_vat", tax_id_value: "DE 123.456.789" }],
});
```

```python Python theme={null}
import os
from whop_sdk import Whop

client = Whop(token=os.environ["WHOP_API_KEY"])

client.accounts.update(
    "biz_xxxxxxxxxxxxx",
    tax_identifiers=[{"tax_id_type": "eu_vat", "tax_id_value": "DE 123.456.789"}],
)
```

An account holds one tax ID per `tax_id_type`. Sending a type the account already has replaces its value instead of adding a second tax ID. All EU countries share `eu_vat`, so an account holds one VAT number for the whole EU.

## The `eu_vat` type

An `eu_vat` number starts with the two-letter code of the member state that issued it, followed by the national number.

* **Greece uses `EL`**, not `GR`: `EL123456789`. On an account, a number sent with `GR` is stored with `EL`. On a payment quote, send `EL` yourself.
* **Northern Ireland uses `XI`** with the `eu_vat` type: `XI123456789`. On an account, an `XI` number counts as the business's registration in the UK, but checkout doesn't use it for UK tax. Add a `gb_vat` number for that.
* **Spaces, dots, and dashes are removed.** On an account, letters are also uppercased, so `de 123.456.789` and `DE-123-456-789` both become `DE123456789`.

A business outside the EU that's registered under the non-Union One Stop Shop (OSS) scheme sends its OSS number as `eu_oss_vat`, for example `EU123456789`. On an account, one `eu_oss_vat` number counts as registered in every EU country.

On an account, Whop checks an `eu_vat` number against its country's format. The number has to start with an EU member state's VAT prefix or `XI`, and the rest has to be the length and characters that country issues. For example, a Belgian number is `BE` followed by 10 digits starting with `0` or `1`, so `BE123456789` is rejected. Whop doesn't add a missing prefix, because an `eu_vat` number doesn't say which country it's from.

On a payment quote, Whop only rejects a number that doesn't start with two letters followed by at least two letters or digits.

Both endpoints respond with a `400`. On an account, the error has a `code` you can match on:

```json theme={null}
{
  "error": {
    "type": "bad_request",
    "message": "Enter your EU VAT number with its two-letter country code, for example DE123456789.",
    "code": "eu_vat_number_invalid"
  }
}
```

On a payment quote, the message is `tax_ids.value is not a valid eu_vat registration`.

## Other tax ID types

Every other type keeps the punctuation and letter case from its published format. A Swiss UID stays `CHE-123.456.789`, and a Brazilian CNPJ stays `12.345.678/0001-00`. Send the number the way the tax authority prints it. On a payment quote, spaces are still removed, so `CHE-123.456.789 MWST` becomes `CHE-123.456.789MWST`.

In Canada, a `ca_gst_hst` number counts as registered in every province. A provincial number, such as `ca_qst` or `ca_pst_bc`, counts only in its province, and a `ca_bn` business number on its own doesn't count anywhere.

## Examples by country

These match the examples shown in the Whop dashboard, which uses `eu_vat` for Poland and Northern Ireland. The [Update Account](https://docs.whop.com/api-reference/accounts/update-account.md) reference lists the other accepted `tax_id_type` values. A payment quote also accepts `pl_nip` and `xi_vat`.

| Country | `tax_id_type` | Example |
| - | - | - |
| Albania | `al_tin` | `J12345678N` |
| Andorra | `ad_nrt` | `A-123456-B` |
| Angola | `ao_tin` | `1234567890` |
| Argentina | `ar_cuit` | `20-12345678-9` |
| Armenia | `am_tin` | `12345678` |
| Aruba | `aw_tin` | `1234567` |
| Australia | `au_abn` | `12345678901` |
| Australia | `au_arn` | `12345678901` |
| Austria | `eu_vat` | `ATU12345678` |
| Azerbaijan | `az_tin` | `1234567890` |
| Bahamas | `bs_tin` | `123456789` |
| Bahrain | `bh_vat` | `123456789012345` |
| Bangladesh | `bd_bin` | `123456789` |
| Barbados | `bb_tin` | `1234567890` |
| Belarus | `by_tin` | `123456789` |
| Belgium | `eu_vat` | `BE0123456789` |
| Benin | `bj_ifu` | `1234567890123` |
| Bolivia | `bo_tin` | `1234567` |
| Bosnia and Herzegovina | `ba_tin` | `1234567890123` |
| Brazil | `br_cnpj` | `12.345.678/0001-00` |
| Brazil | `br_cpf` | `123.456.789-00` |
| Bulgaria | `bg_uic` | `123456789` |
| Bulgaria | `eu_vat` | `BG123456789` |
| Burkina Faso | `bf_ifu` | `12345678901234` |
| Cambodia | `kh_tin` | `K001-234567890` |
| Cameroon | `cm_niu` | `M012345678901A` |
| Canada | `ca_bn` | `123456789` |
| Canada | `ca_gst_hst` | `123456789RT0001` |
| Canada | `ca_pst_bc` | `PST-1234-5678` |
| Canada | `ca_pst_mb` | `123456-7` |
| Canada | `ca_pst_sk` | `1234567` |
| Canada | `ca_qst` | `1234567890TQ1234` |
| Cape Verde | `cv_nif` | `123456789` |
| Chile | `cl_tin` | `12.345.678-9` |
| China | `cn_tin` | `123456789012345678` |
| Colombia | `co_nit` | `123.456.789-0` |
| Costa Rica | `cr_tin` | `1-234-567890` |
| Croatia | `eu_vat` | `HR12345678901` |
| Croatia | `hr_oib` | `12345678901` |
| Cyprus | `eu_vat` | `CY12345678A` |
| Czechia | `eu_vat` | `CZ12345678` |
| Democratic Republic of the Congo | `cd_nif` | `A1234567B` |
| Denmark | `eu_vat` | `DK12345678` |
| Dominican Republic | `do_rcn` | `123-4567890-1` |
| Ecuador | `ec_ruc` | `1234567890001` |
| Egypt | `eg_tin` | `123456789` |
| El Salvador | `sv_nit` | `0614-123456-123-4` |
| Estonia | `eu_vat` | `EE123456789` |
| Ethiopia | `et_tin` | `1234567890` |
| European Union (non-Union OSS) | `eu_oss_vat` | `EU123456789` |
| Finland | `eu_vat` | `FI12345678` |
| France | `eu_vat` | `FR12345678901` |
| Georgia | `ge_vat` | `123456789` |
| Germany | `de_stn` | `12/345/67890` |
| Germany | `eu_vat` | `DE123456789` |
| Ghana | `gh_tin` | `C0001234567` |
| Greece | `eu_vat` | `EL123456789` |
| Guinea | `gn_nif` | `123456789` |
| Hong Kong | `hk_br` | `12345678` |
| Hungary | `eu_vat` | `HU12345678` |
| Hungary | `hu_tin` | `12345678` |
| Iceland | `is_vat` | `123456` |
| India | `in_gst` | `22AAAAA0000A1Z5` |
| Indonesia | `id_npwp` | `12.345.678.9-012.345` |
| Ireland | `eu_vat` | `IE1234567A` |
| Israel | `il_vat` | `123456789` |
| Italy | `eu_vat` | `IT12345678901` |
| Japan | `jp_cn` | `1234-56-7890` |
| Japan | `jp_rn` | `12345` |
| Japan | `jp_trn` | `T1234567890123` |
| Kazakhstan | `kz_bin` | `123456789012` |
| Kenya | `ke_pin` | `P000123456A` |
| Kyrgyzstan | `kg_tin` | `12345678` |
| Laos | `la_tin` | `1234567890` |
| Latvia | `eu_vat` | `LV12345678901` |
| Liechtenstein | `li_uid` | `CHE-123.456.789` |
| Liechtenstein | `li_vat` | `12345` |
| Lithuania | `eu_vat` | `LT123456789012` |
| Luxembourg | `eu_vat` | `LU12345678` |
| Malaysia | `my_frp` | `12345678` |
| Malaysia | `my_itn` | `C1234567890` |
| Malaysia | `my_sst` | `A12-3456-78901234` |
| Malta | `eu_vat` | `MT12345678` |
| Mauritania | `mr_nif` | `12345678` |
| Mexico | `mx_rfc` | `XAXX010101000` |
| Moldova | `md_vat` | `1234567` |
| Montenegro | `me_pib` | `12345678` |
| Morocco | `ma_vat` | `12345678` |
| Nepal | `np_pan` | `123456789` |
| Netherlands | `eu_vat` | `NL123456789B01` |
| New Zealand | `nz_gst` | `123456789` |
| Nigeria | `ng_tin` | `12345678-0001` |
| North Macedonia | `mk_vat` | `MK1234567890123` |
| Northern Ireland | `eu_vat` | `XI123456789` |
| Norway | `no_vat` | `123456789MVA` |
| Norway | `no_voec` | `1234567` |
| Oman | `om_vat` | `OM1234567890` |
| Peru | `pe_ruc` | `12345678901` |
| Philippines | `ph_tin` | `123-456-789-000` |
| Poland | `eu_vat` | `PL1234567890` |
| Portugal | `eu_vat` | `PT123456789` |
| Romania | `eu_vat` | `RO1234567890` |
| Romania | `ro_tin` | `1234567890` |
| Russia | `ru_inn` | `1234567890` |
| Russia | `ru_kpp` | `123456789` |
| Saudi Arabia | `sa_vat` | `123456789012345` |
| Senegal | `sn_ninea` | `12345678901` |
| Serbia | `rs_pib` | `123456789` |
| Singapore | `sg_gst` | `M12345678X` |
| Singapore | `sg_uen` | `123456789A` |
| Slovakia | `eu_vat` | `SK1234567890` |
| Slovenia | `eu_vat` | `SI12345678` |
| Slovenia | `si_tin` | `12345678` |
| South Africa | `za_vat` | `4123456789` |
| South Korea | `kr_brn` | `123-45-67890` |
| Spain | `es_cif` | `A12345678` |
| Spain | `eu_vat` | `ESA12345678` |
| Suriname | `sr_fin` | `12345678` |
| Sweden | `eu_vat` | `SE123456789012` |
| Switzerland | `ch_uid` | `CHE-123.456.789` |
| Switzerland | `ch_vat` | `CHE-123.456.789 MWST` |
| Taiwan | `tw_vat` | `12345678` |
| Tajikistan | `tj_tin` | `123456789` |
| Tanzania | `tz_vat` | `12-345-6789` |
| Thailand | `th_vat` | `1234567890123` |
| Türkiye | `tr_tin` | `1234567890` |
| Uganda | `ug_tin` | `1234567890` |
| Ukraine | `ua_vat` | `1234567890` |
| United Arab Emirates | `ae_trn` | `123456789012345` |
| United Kingdom | `gb_vat` | `GB123456789` |
| United States | `us_ein` | `12-3456789` |
| Uruguay | `uy_ruc` | `123456789012` |
| Uzbekistan | `uz_tin` | `123456789` |
| Uzbekistan | `uz_vat` | `123456789012` |
| Venezuela | `ve_rif` | `J-12345678-9` |
| Vietnam | `vn_tin` | `1234567890` |
| Zambia | `zm_tin` | `1234567890` |
| Zimbabwe | `zw_tin` | `1234567890` |

## Next steps

> [card] [Update an account](https://docs.whop.com/api-reference/accounts/update-account.md)
>
> Set an account's tax IDs and how it remits tax.

> [card] [Quote a business purchase](https://docs.whop.com/api-reference/payment-quotes/create-payment-quote.md)
>
> Price a purchase with the buyer's tax ID before charging it.


## Related topics

- [Update Verification](https://docs.whop.com/api-reference/verifications/update-verification.md)
- [Create Payment Quote](https://docs.whop.com/api-reference/payment-quotes/create-payment-quote.md)
- [Authenticate Users in Whop Apps](https://docs.whop.com/developer/guides/authentication.md)
