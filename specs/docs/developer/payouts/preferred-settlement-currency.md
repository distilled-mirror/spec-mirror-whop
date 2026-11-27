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

# Preferred Settlement Currency

> Convert each new sale into USD, EUR, GBP, CAD, or AUD. See which sales convert and how refunds, disputes, and platform fees settle.

Set a preferred settlement currency to put all new revenue into one balance. Whop converts each new sale into that currency when the buyer pays, so you don't convert small balances before a payout.

| | Original currency (default) | Preferred settlement currency |
| - | - | - |
| **Where a sale settles** | A balance in the plan's currency | A balance in the currency you choose |
| **Balances to manage** | One for each plan currency | One |
| **Extra fee** | None | 0.5% on each converted sale |

## Choose a currency

> [tab] Dashboard
>
> 1. In your dashboard settings, select **Payments**.
> 2. Under **Preferred settlement currency**, select a currency in **Settle every new sale in**.
> 3. Select **Save changes**.

> [tab] API
>
> Send a lowercase currency code to [Update Account Preferences](https://docs.whop.com/api-reference/accounts/update-account-preferences.md). Send `null` to turn the setting off.
>
> ```typescript TypeScript theme={null}
> import { WhopClient } from "@whop/sdk";
>
> const client = new WhopClient({ token: process.env.WHOP_API_KEY });
>
> const preferences = await client.accounts.preferences.update({
> 	account_id: "biz_xxxxxxxxxxxxx",
> 	preferred_settlement_currency: "usd",
> });
>
> console.log(preferences.preferred_settlement_currency_changeable_at);
> ```
>
> ```python Python theme={null}
> import os
>
> from whop_sdk import Whop
>
> client = Whop(token=os.environ["WHOP_API_KEY"])
>
> preferences = client.accounts.preferences.update(
>     account_id="biz_xxxxxxxxxxxxx",
>     preferred_settlement_currency="usd",
> )
>
> print(preferences.preferred_settlement_currency_changeable_at)
> ```
>
> ```ruby Ruby theme={null}
> require "whop_sdk"
>
> client = Whop_sdk::Client.new(token: ENV.fetch("WHOP_API_KEY"))
>
> preferences = client.accounts.preferences.update(
>   account_id: "biz_xxxxxxxxxxxxx",
>   preferred_settlement_currency: "usd"
> )
>
> puts preferences.preferred_settlement_currency_changeable_at
> ```
>
> ```bash cURL theme={null}
> curl -X PATCH "https://api.whop.com/api/v1/accounts/biz_xxxxxxxxxxxxx/preferences" \
>   -H "Authorization: Bearer $WHOP_API_KEY" \
>   -H "Content-Type: application/json" \
>   -d '{ "preferred_settlement_currency": "usd" }'
> ```
>
> The response returns the account's preferences:
>
> ```json theme={null}
> {
> 	"preferred_settlement_currency": "usd",
> 	"preferred_settlement_currency_changeable_at": "2026-10-22T14:03:11.000Z"
> }
> ```

A change applies only to new sales. Money already in your balances stays in its currency.

## Know which sales convert

A sale converts when its plan's currency isn't your preferred currency. The currency that the buyer pays in doesn't change this.

These sales never convert:

* Sales on plans already in your preferred currency.
* App Store purchases. Apple pays them in USD.
* Sales that settle outside your Whop balance, such as sales through a Stripe account that you connected.

Whop sets the exchange rate when the buyer pays and stores it on the payment. The setting has no effect on the buyer. The buyer pays the price shown at checkout, and the receipt shows that amount.

## Fee structure

Each converted sale has a 0.5% fee. Your usual fees also apply. The payment shows the 0.5% on the **Currency conversion** line.

Whop doesn't charge the 0.5% when the buyer pays in a currency other than the plan's. [Adaptive pricing](https://docs.whop.com/payments-and-billing/fees/adaptive-pricing.md) already charges for that conversion at checkout.

These examples use USD as the preferred currency:

| Plan currency | Buyer pays in | Settles in | 0.5% fee |
| - | - | - | - |
| USD | USD or EUR | USD | No. The plan is already in USD. |
| GBP | GBP | USD | Yes |
| GBP | EUR or USD | USD | No. The buyer paid in another currency. |
| MXN | MXN | USD | Yes |
| GBP, App Store | GBP | USD | No. Apple pays in USD. |

## Refunds and disputes

A refund or dispute comes out of the balance that the sale settled in.

* **Refunds** use the exchange rate stored on the payment. A rate change after the sale doesn't change the amount.
* **Disputes** use the exchange rate on the day that the dispute opens. The amount can differ from the converted sale.

## Platforms and subaccounts

The setting applies only to the account that sets it. A subaccount doesn't get its platform's setting, and a platform doesn't get its subaccounts' settings.

Your application fees follow your setting. Whop converts each fee into your currency at the exchange rate on the day of the sale. The subaccount's share follows the subaccount's setting.

Whop charges you 0.5% on a converted application fee. Whop doesn't charge the 0.5% when the sale's plan is already in your currency.

## Read amounts on a payment

A [payment](https://docs.whop.com/api-reference/payments/payment) gives amounts in two currencies. Each amount includes its own `currency`.

| Fields | Currency | Use for |
| - | - | - |
| `total`, `subtotal`, `tax_amount`, `refunded_amount`, `amount_after_fees` | The settlement currency | Balances, payouts, and ledger reconciliation |
| `presentment_total`, `presentment_tax_amount`, `presentment_refunded_amount` | The currency the buyer paid in | Order totals, receipts, and customer emails |

To match a payment to an order in your system, compare `presentment_total`. The setting doesn't change it. `presentment_total` doesn't include buyer fees. [Payment webhooks](https://docs.whop.com/developer/guides/webhooks.md) send the same fields.

## Next steps

> [card] [Account Preferences reference](https://docs.whop.com/api-reference/accounts/update-account-preferences.md)
>
> Every preference field and the scope each one needs.

> [card] [Adaptive pricing](https://docs.whop.com/payments-and-billing/fees/adaptive-pricing.md)
>
> Show buyers prices in their local currency at checkout.


## Related topics

- [SettlementElement](https://docs.whop.com/elements/latest/wallet/balances-settlement.md)
- [List Payment Fees](https://docs.whop.com/api-reference/payments/list-payment-fees.md)
- [API Versions](https://docs.whop.com/developer/api/versioning.md)
