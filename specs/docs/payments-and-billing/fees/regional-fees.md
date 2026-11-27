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

# Regional Fees

> See the card and local payment method rates Whop charges in each region, including Latin America, and what decides which rate applies.

Whop's processing rate depends on the region where a payment is processed. Use this page to estimate what you'll pay on sales to customers outside the United States. The standard rate of 2.7% + \$0.30 applies in the US, Canada, the EU, the UK, and Australia, while rates in Latin America vary by country.

> [!NOTE]
>
> The region comes from where the payment is processed, not where your business
> is based. A business in Mexico selling to a customer in the US pays the US
> rate.

## Card rates by region

Whop routes each payment to a processor. When that processor is local to the customer's region, the regional rate applies and there's no international card fee.

| Region | Rate per successful card payment |
| - | - |
| United States, Canada, EU, UK, Australia | 2.7% + \$0.30 |
| Argentina, Chile, Colombia, Costa Rica, Guatemala, Mexico, Uruguay | 2.9% + \$0.30 |
| Dominican Republic, Peru | 4.9% + \$0.30 |
| Brazil | 8% + \$0.30 |

## What can apply on top

| Fee | Rate | When it applies |
| - | - | - |
| International card | 1.5% | The payment is processed in the US and the card or customer is from outside the US. |
| Currency conversion | 1% (2% in Argentina) | The processor settles in a different currency than the one you receive. |

Processors in Latin America settle in US dollars, so a product priced in local currency there usually adds the currency conversion fee. If you price in US dollars and use [adaptive pricing](https://docs.whop.com/payments-and-billing/fees/adaptive-pricing.md), this fee doesn't apply, because the buyer's price already covers the conversion.

The other fees on the [Fees](https://docs.whop.com/payments-and-billing/fees/fees.md) page, such as those for billing and optimization, apply the same way in every region.

## Local payment method rates

Some local payment methods have their own rate. These rates replace the card rate when the customer pays with that method.

| Country | Payment method | Rate |
| - | - | - |
| Argentina | MODO, Rapipago | 2.9% + \$0.30 |
| Brazil | Boleto | 3.9% + \$0.30 |
| Chile | Sencillito | 3.9% + \$0.30 |
| Chile | Servipag | 3.9% + \$3.00 |
| Chile | Mercado Pago, Webpay | 4.9% + \$0.30 |
| Colombia | Bancolombia, Bre-B, Efecty, Nequi, PSE | 2.9% + \$0.30 |
| Colombia | Addi | 15% + \$0.30 |
| India | UPI | 2.9% + \$0.30 |
| Kenya | M-PESA | 3.9% + \$2.00 |
| Malaysia | DuitNow QR, GrabPay, ShopeePay | 3.9% + \$0.30 |
| Mexico | SPEI | 2.9% + \$0.30 |
| Mexico | Mercado Pago, OXXO | 4.9% + \$0.30 |
| Mexico | Kueski | 15% + \$0.30 |
| Nigeria | Bank transfer, OPay | 3.9% + \$0.30 |
| Peru | PagoEfectivo, Yape | 3.9% + \$0.30 |
| Philippines | GCash | 3.9% + \$0.30 |

See [Local payment methods](https://docs.whop.com/payments-and-billing/local-payment-methods.md) for methods in other regions and where each one is available.

## How fees appear in your currency

Whop sets fixed fees in US dollars and converts them to your settlement currency when a buyer pays. If you settle in British pounds, the \$0.30 fixed fee appears as its pound equivalent on that sale. To settle every sale in one currency, whatever the buyer paid in, choose a [preferred settlement currency](https://docs.whop.com/developer/payouts/preferred-settlement-currency.md).

## Check your own rates

The rates on this page are Whop's standard rates. If your business has negotiated pricing, your rates may differ.

To see the exact rate for your account in each region, call [Retrieve Account Fees](https://docs.whop.com/api-reference/accounts/retrieve-account-fees.md). The `card_processing.regions` and `payment_methods` fields list your rate by region.

## Next steps

> [card] [Fees](https://docs.whop.com/payments-and-billing/fees/fees.md)
>
> See every fee for payments, payouts, fraud, and revenue optimization.

> [card] [Adaptive pricing](https://docs.whop.com/payments-and-billing/fees/adaptive-pricing.md)
>
> Show buyers local prices and route payments through domestic rails.

> [card] [Local payment methods](https://docs.whop.com/payments-and-billing/local-payment-methods.md)
>
> See every regional payment option you can offer at checkout.


## Related topics

- [Whop Pricing and Fees](https://docs.whop.com/payments-and-billing/fees/fees.md)
- [Local Payment Methods](https://docs.whop.com/payments-and-billing/local-payment-methods.md)
- [Payment Processing and Payout Fees](https://docs.whop.com/fees.md)
