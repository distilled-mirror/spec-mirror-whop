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

# TaxIdElement

> Collects a business tax registration accepted by the API. Labels use buyer-facing names. The placeholder matches the selected format. `country` preselects a type. `onChange` emits committed pairs. The host supplies API validation errors.

> [!NOTE]
>
> This page documents `@whop/elements@1.10.0` and `@whop/elements-react@1.10.0`.

*Since `v1.0.0`.*

Mounts inside [`Payments`](https://docs.whop.com/elements/latest/payments/overview). Pass props and callbacks through the create options or React props.

Mounts inside a `WhopPayments` scope. A registration-type picker beside its value field, preselected from the buyer's country.

Mount inside `<Payments>`, which owns the charge and the confirmation token. `<Payments>` itself mounts inside `<WhopElements>`. A registration-type picker beside a value input, so a buyer entering a VAT or CUIT number picks the right kind first.

```tsx React theme={null}
import { WhopElements, Payments, TaxIdElement } from "@whop/elements-react";
import { loadWhop } from "@whop/elements";

function Example() {
  return (
    <WhopElements elements={loadWhop()}>
      <Payments /* options */>
        <TaxIdElement onChange={(e) => console.log(e)} />
      </Payments>
    </WhopElements>
  );
}
```

```tsx React Native theme={null}
import { Payments, TaxIdElement } from '@whop/elements-react-native';

export function TaxStep({ country }: { country: string }) {
  return (
    <Payments accountId="biz_xxxxxxxx" plan="plan_xxxxxxxx">
      <TaxIdElement country={country} onChange={({ taxId }) => console.log(taxId.type, taxId.value)} />
    </Payments>
  );
}
```

```html JavaScript theme={null}
<script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>
<script type="module">
  const payments = window.WhopElements().payments.create({ /* options */ });
  payments.create('taxId', { onChange: (e) => console.log(e) }).mount('#payments-taxId');
</script>
```

```swift Swift theme={null}
import Elements
import SwiftUI

// .whopElements(environment:) runs once at the app root. See Getting started.
struct CheckoutScreen: View {
    @State private var address = PostalAddress()
    @State private var taxID: WhopTaxID?

    var body: some View {
        WhopPayments(accountID: "biz_xxxx", charge: .plan(id: "plan_xxxx")) { payments in
            ScrollView {
                VStack(alignment: .leading, spacing: 20) {
                WhopAddressElement(address: $address)
                WhopTaxIDElement(taxID: $taxID, country: address.country)
                    WhopBrandingElement()
                }
                .padding()
            }
        }
    }
}
```

Example data. [Open the Playground](https://docs.whop.com/elements/latest/payments/overview#playground).

[TaxIdElement running on Android, in the React Native example app](https://app.revyl.ai/embed/713e08f7-91e6-4449-905b-ceb4c18b83c2?controls=0)

## Props

**disabled** `boolean`

Disables both fields while the host saves the last committed registration. Defaults to `false`.

**country** `string`

ISO 3166-1 alpha-2 buyer country used to preselect the type. `AR` selects `ar_cuit`, `LI` selects `li_uid`. EU and unmapped or empty values select `eu_vat`. Other mapped countries select their own first type. A manual selection persists across country changes. Defaults to `""`.

**defaultValue** `object`

Initial `type` and `value` from a resumed session. An unsupported `type` falls back to the country default.

**properties**

**type** `string` *required*

**value** `string` *required*

**error** `string`

API validation error shown below the value input. Empty (default) shows nothing. Defaults to `""`.

## Events

Pass callbacks in the create options or React props.

### `onChange`

Fires on blur or Enter when the value is nonempty, or when the type changes with a value present. Identical consecutive values fire once. Clearing the value doesn't fire. The host must clear stored registrations.

**payload** `object`

**properties**

**taxId** `object` *required*

**properties**

**type** `string` *required*

**values**

One of `"ad_nrt"`, `"ao_tin"`, `"ar_cuit"`, `"al_tin"`, `"am_tin"`, `"aw_tin"`, `"au_abn"`, `"au_arn"`, `"eu_vat"`, `"az_tin"`, `"bs_tin"`, `"bh_vat"`, `"bd_bin"`, `"bb_tin"`, `"by_tin"`, `"bj_ifu"`, `"bo_tin"`, `"ba_tin"`, `"br_cnpj"`, `"br_cpf"`, `"bg_uic"`, `"bf_ifu"`, `"kh_tin"`, `"cm_niu"`, `"ca_bn"`, `"ca_gst_hst"`, `"ca_pst_bc"`, `"ca_pst_mb"`, `"ca_pst_sk"`, `"ca_qst"`, `"cv_nif"`, `"cl_tin"`, `"cn_tin"`, `"co_nit"`, `"cd_nif"`, `"cr_tin"`, `"hr_oib"`, `"do_rcn"`, `"ec_ruc"`, `"eg_tin"`, `"sv_nit"`, `"et_tin"`, `"eu_oss_vat"`, `"ge_vat"`, `"gh_tin"`, `"de_stn"`, `"gb_vat"`, `"gn_nif"`, `"hk_br"`, `"hu_tin"`, `"is_vat"`, `"in_gst"`, `"id_npwp"`, `"il_vat"`, `"jp_cn"`, `"jp_rn"`, `"jp_trn"`, `"kz_bin"`, `"ke_pin"`, `"kg_tin"`, `"la_tin"`, `"li_uid"`, `"li_vat"`, `"my_frp"`, `"my_itn"`, `"my_sst"`, `"mr_nif"`, `"mx_rfc"`, `"md_vat"`, `"me_pib"`, `"ma_vat"`, `"np_pan"`, `"nz_gst"`, `"ng_tin"`, `"mk_vat"`, `"no_vat"`, `"no_voec"`, `"om_vat"`, `"pe_ruc"`, `"ph_tin"`, `"pl_nip"`, `"ro_tin"`, `"ru_inn"`, `"ru_kpp"`, `"sa_vat"`, `"sn_ninea"`, `"rs_pib"`, `"sg_gst"`, `"sg_uen"`, `"si_tin"`, `"za_vat"`, `"kr_brn"`, `"es_cif"`, `"ch_uid"`, `"ch_vat"`, `"tw_vat"`, `"tj_tin"`, `"tz_vat"`, `"th_vat"`, `"tr_tin"`, `"ug_tin"`, `"ua_vat"`, `"ae_trn"`, `"us_ein"`, `"uy_ruc"`, `"uz_tin"`, `"uz_vat"`, `"ve_rif"`, `"vn_tin"`, `"zm_tin"`, `"zw_tin"`, `"sr_fin"`, or `"xi_vat"`.

**value** `string` *required*

### `onLoaderStart`

Runs after the loading skeleton first paints and before `onReady`.

### `onReady`

Runs after the element's first complete paint.

### `onError`

Runs when the element fails to load or crashes. The fallback remains visible. Use `code` for programmatic handling. `sourceKey` identifies a failed host-state source.

**e** `object`

**properties**

**message** `string` *required*

**code** `string`

**sourceKey** `string`

## Methods

Call these on the handle returned by `create`, or through a React `ref`.

### `mount`

Mounts the element in `target` and starts loading. React components mount themselves.

**Parameters**

**target** `string | HTMLElement` *required*

### `destroy`

Removes the element and releases its frame and subscriptions. You can call it more than once. React removes the element automatically.

### `update`

Merges new props into the mounted element. In React, change the component props instead.

**Parameters**

**options** `Partial\<TaxIdElementProps\>` *required*

## Styling

Style these parts through `appearance.classes`. Use camel case or kebab case for property names and include units. Page stylesheets can't reach the element's frame. The framework validates each declaration before injecting it.

| Class | Targets |
| - | - |
| `.whop-TaxId` | Tax registration root |
| `.whop-TaxIdError` | API validation error |
| `.whop-TaxIdInput` | Registration value input |
| `.whop-TaxIdLabel` | Registration value label |
| `.whop-TaxIdTypeLabel` | Registration type label |
| `.whop-TaxIdTypeSelect` | Registration type selector |

```ts theme={null}
const payments = whop.payments.create({
  appearance: {
    classes: {
      'whop-TaxId': { borderRadius: '8px', fontWeight: '600' },
      'whop-TaxIdError': { borderRadius: '8px', fontWeight: '600' },
      'whop-TaxIdInput': { borderRadius: '8px', fontWeight: '600' }
    }
  }
});

// 6 classes use this shape
payments.update({
  appearance: { classes: { 'whop-TaxId': { fontWeight: '700' } } }
});
```

In React, pass `appearance` to `<Payments>`. Set it globally with `WhopElements({ appearance })`.

## Parameters

**taxID** `Binding\<WhopTaxID?\>?`

Receives each committed registration.

**country** `String?`

ISO 3166-1 alpha-2 buyer country, used to preselect the type.

**defaultValue** `WhopTaxID?`

An initial pair from a resumed session. An unsupported type falls back.

**isDisabled** `Bool`

Disables both fields while you save the last committed registration. Defaults to `false`.

**error** `String?`

An API validation error, shown below the value input.

## `WhopTaxID`

What a selection hands back:

* `type: WhopTaxIDType`: the registration type, `eu_vat`, `ar_cuit`, …
* `value: String`: the registration itself

## States

Renders immediately. A value that does not match the selected type's format shows its error. `error` renders an API validation failure under the value field.

## Good to know

* `AR` preselects `ar_cuit`; EU members and anything unmapped preselect `eu_vat`. A manual pick persists across country changes.
* Pass the country from the address element so the two stay in step.
* The binding fires on each committed registration, not on every keystroke.

## Install

```swift theme={null}
dependencies: [
    .package(url: "https://github.com/whopio/elements-swift.git", from: "0.1.0")
]
```

> [!NOTE]
>
> Mount it inside a `WhopPayments(accountID:charge:)` scope, which creates the controller and hands it to its content. `payments.buyer` is the signed-in buyer once an email sign-in has proven one. `WhopBrandingElement` has to be on screen too, because Whop is merchant of record on these sales and `createConfirmationToken` refuses without it. Style with `.whopElementsAppearance(_:)`. The module is `Elements`, not the wallet SDK's `WhopElements`. See [Getting started](https://docs.whop.com/elements/latest/getting-started.md) and [Appearance](https://docs.whop.com/elements/latest/appearance.md).

## Props

**country** `string`

ISO 3166-1 alpha-2 buyer country, used to preselect the type. `AR` selects `ar_cuit`; EU and unmapped values select `eu_vat`.

**defaultValue** `\{ type: string; value: string \}`

Initial pair from a resumed session. An unsupported type falls back.

**disabled** `boolean`

Disables both fields, for while you save the last committed registration.

**error** `string`

A validation message from your API, shown below the value input.

**onChange** `(payload: TaxIdChangePayload) =\> void`

Fires on every type or value change.

**style** `StyleProp\<ViewStyle\>`

Applied to the element's outer `View`. For theming, prefer `appearance.parts` on the provider, which covers every element on this surface. Note the React Native part names are their own set today, not the web's `whop-*` class names, so a web appearance object does not port across unchanged.

**fallback** `ReactNode`

Rendered instead of the built-in skeleton while the element loads.

**onReady** `() =\> void`

Fires once the element is interactive. `<Payments>` groups these, so its own `onLoadingChange` is usually the one you want.

**onError** `(error: \{ message: string; code?: string \}) =\> void`

A load or configuration failure for this element. The element renders its own error face either way.

## `TaxIdChangePayload`

What `onChange` hands back:

* `taxId: { type: TaxIdType; value: string }`: the selected registration type and what the buyer typed

## States

Renders immediately. The type preselects from `country` and the value input validates against that type's format. `onChange` fires with the current pair.

## Good to know

* `country` only preselects. A buyer who picks a type manually keeps it across country changes.
* Pass `error` to show a message from your own API validation beneath the value input.
* The element reports the pair. Sending it is yours: attach it to your own order or customer record.

> [!NOTE]
>
> Wrap your app in `<WhopElements getToken={…}>` once, then mount `<Payments>` around the elements. See [Getting started](https://docs.whop.com/elements/latest/getting-started.md) and [Appearance](https://docs.whop.com/elements/latest/appearance.md).


## Related topics

- [Getting Started with Elements](https://docs.whop.com/elements/latest/getting-started.md)
- [BrandingElement](https://docs.whop.com/elements/latest/payments/branding.md)
- [Payments](https://docs.whop.com/elements/latest/payments/overview.md)
