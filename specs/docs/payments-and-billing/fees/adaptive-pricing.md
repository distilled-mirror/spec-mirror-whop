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

# Adaptive Pricing

> Show buyers prices in their local currency at checkout to lower fees, unlock local payment methods, and reduce declines.

Adaptive pricing automatically displays your checkout in the buyer's local currency and routes the payment through domestic rails. It's free for sellers, configured per checkout link, and turned off by default.

> [card] Lower fees
>
> Domestic processing replaces international card and FX fees.

> [card] Local payment methods
>
> iDEAL, SEPA, Klarna, and other regional options appear automatically.

> [card] Higher acceptance
>
> The buyer's bank is less likely to decline domestic transactions.

## How it works

When you enable adaptive pricing on a checkout link, Whop detects the buyer's region from their IP address and displays the price in their local currency. Whop then processes the payment domestically in that region.

Buyers can switch back to the original currency at checkout if they prefer.

> [!NOTE]
>
> Buyers in unsupported regions automatically see the original price and
> currency — no fallback configuration needed.

## Benefits

* **Lower processing fees.** Non-domestic transactions get billed at domestic rates instead of incurring international card fees and currency conversion fees. See [Fees](https://docs.whop.com/payments-and-billing/fees/fees.md).
* **Local payment methods and financing.** iDEAL, bank transfers in the Single Euro Payments Area, Klarna, and other regional options depend on currency. They appear only when the checkout uses the matching local currency. See [Local payment methods](https://docs.whop.com/payments-and-billing/local-payment-methods.md) and [Financing](https://docs.whop.com/payments-and-billing/financing/all-bnpl-options.md).
* **Higher bank acceptance.** The buyer's bank is less likely to decline domestic transactions than cross-border ones.
* **Familiar amounts for buyers.** Local currency reduces post-purchase confusion and "I don't recognize this charge" disputes.

## Example

A seller in the United States has a link priced at <abbr title="United States dollars">USD</abbr> 100. An Australian buyer opens it.

| | Without adaptive pricing | With adaptive pricing |
| - | - | - |
| **Buyer sees** | <abbr title="United States dollars">USD</abbr> 100 | Equivalent in <abbr title="Australian dollars">AUD</abbr> |
| **Processed as** | International | Domestic in Australia |
| **Seller pays** | Domestic rate + international card fee + FX fee | Domestic rate |
| **Local payment methods shown** | No | Yes |

## Fees

**Sellers:** Free. There's no additional Whop fee for enabling adaptive pricing — you simply pay the domestic rate instead of international + FX.

**Buyers:** The displayed local price includes a small currency conversion fee. The amount shown at checkout is the total they pay.

## Enable adaptive pricing

Configure adaptive pricing for each checkout link.

**1. Open your checkout link's settings**

Go to your [Dashboard](https://whop.com/dashboard/links/checkout) and select the checkout link you want to update.

**2. Toggle Adaptive pricing on**

Switch **Accept local currency payments** to on in the link's settings.

**3. Save**

Buyers in supported regions will now see prices in their local currency the next time they open the link.

## Subscriptions

Adaptive pricing works for one-time payments and subscriptions. A subscriber's first payment uses the local price shown at checkout. Each renewal bills in the same local currency at the exchange rate at the time of billing. The local amount can change slightly from one billing period to the next. You still receive your variant's price in your currency.

## Next steps

> [card] [Local payment methods](https://docs.whop.com/payments-and-billing/local-payment-methods.md)
>
> See every regional payment option that becomes available with local currency checkouts.

> [card] [Financing options](https://docs.whop.com/payments-and-billing/financing/all-bnpl-options.md)
>
> Offer Klarna, Afterpay, and other BNPL methods that depend on local currency.

> [card] [Fees](https://docs.whop.com/payments-and-billing/fees/fees.md)
>
> Compare domestic and international rates so you know exactly what adaptive pricing saves.

> [card] [Create a checkout link](https://docs.whop.com/manage-your-business/payment-processing/create-checkout-link.md)
>
> Set up the checkout link you'll enable adaptive pricing on.


## Related topics

- [Whop Pricing and Fees](https://docs.whop.com/payments-and-billing/fees/fees.md)
- [Regional Fees](https://docs.whop.com/payments-and-billing/fees/regional-fees.md)
- [Set Up Pricing](https://docs.whop.com/manage-your-business/payment-processing/set-up-pricing.md)
