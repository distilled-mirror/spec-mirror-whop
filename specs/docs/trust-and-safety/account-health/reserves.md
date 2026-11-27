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

# Reserves

> Why Whop holds part of your balance in reserve, how each reserve type works, and when Whop releases held funds

A reserve is a portion of your balance that Whop holds back from payout for a period of time. Reserved funds are still your money. They count toward your total balance, are never a fee, and release back to your available balance automatically.

## Why reserves are necessary

When a customer disputes a payment, Whop deducts the payment amount and any card-network dispute fee from your balance. As the merchant of record, Whop is responsible for those amounts even when a business has already withdrawn its funds or is unable to fulfill its orders.

It's possible that an account's balance isn't enough to cover future disputes and refunds. To avoid that situation, Whop places reserves where it anticipates losses. The reserve exists to cover those losses. If the losses never materialize, Whop releases the funds in full and lifts the reserve. This process can be difficult, but it protects both your business and your customers.

## When a reserve is placed

Whop continuously reviews every account. It sizes reserves for the specific risk on each account. That risk includes how much of your recent volume remains exposed to refunds or disputes and how likely those outcomes are, based on your track record. Whop may place or increase a reserve when:

* Your account is new, or doesn't have much sales history yet
* Refunds and disputes are high relative to your sales, or your dispute rate is trending toward card network thresholds
* You have a high rate of unresolved [Resolution Center](https://docs.whop.com/payments/resolution-center.md) cases
* You deliver long after the customer pays, such as pre-orders, bookings, or multi-month services, so payments stay disputable long after the sale
* A sudden change in your sales patterns increases refund or dispute exposure

The same review works in both directions. Your reserve shrinks and eventually comes off as these conditions improve. For example, your dispute rate may come down, or older sales may age out of their dispute windows.

> [!NOTE]
>
> You can see every reserve on your account, broken down by type with amounts
> and release dates, on the [Balances](https://whop.com/dashboard/balance/)
> page of your dashboard.

## Rolling reserve

Whop holds a percentage of each sale for a set number of days, then releases it. Whop ties every hold to the sale that created it, so funds return continuously as each sale ages past its hold period. You receive older funds back at the same rate Whop holds funds from new sales.

For example, with a 10% reserve and a 90-day hold period:

| Sale | Amount | Held | Releases |
| - | - | - | - |
| March 1 | \$1,000 | \$100 | May 30 |
| March 15 | \$500 | \$50 | June 13 |
| April 1 | \$2,000 | \$200 | June 30 |

Mar 1 sale

\$100 held → releases May 30

Mar 15 sale

\$50 held → releases Jun 13

Apr 1 sale

\$200 held → releases Jun 30

Mar 1
Apr 1
May 1
Jun 1
Jun 30

Your reserve breakdown always shows your current percentage and hold period. If Whop lowers your reserve percentage, the lower rate applies to new sales. Money already in reserve keeps its original release date. If Whop removes your reserve entirely, it immediately releases the held funds into your available balance.

### How the hold period is determined

A sale doesn't stay at risk forever. Once you deliver the product and its typical refund or dispute window passes, little remains for a reserve to cover. Whop sizes the hold period to that lifecycle. It lasts long enough to cover your fulfillment cycle and the period after delivery when refunds and disputes typically arrive. If you deliver instantly, that window is short. If customers pay months before you deliver, it's longer.

## Fixed-amount reserve

Sometimes Whop holds a specific dollar amount from your existing balance, rather than taking a percentage of new sales. This is a fixed-amount reserve. It can apply on its own or alongside a rolling reserve, and it never holds more than what's actually in your balance.

A fixed-amount reserve is one lump sum with a single release date, shown as its own line in your reserve breakdown. On that date the whole amount releases together. It can also release early. If Whop lowers the amount, the difference returns to your available balance right away. If Whop removes the reserve, the entire amount returns right away.

For example, a \$5,000 fixed-amount reserve placed on June 1 with a 90-day hold releases in full on August 30:

Jun 1 hold

\$5,000 held → releases in full Aug 30

Jun 1
Jul 1
Aug 1
Aug 30

## How reserves are released

Every hold carries a release date, visible in your reserve breakdown. When the date arrives, the funds move to your available balance automatically.

If you refund a payment in full, Whop releases its corresponding reserve back to your available balance.

## Financing (BNPL) reserves

Sales financed through a buy now, pay later provider carry a reserve set by that provider, not by Whop. You're paid up front while the customer pays the provider in installments, so the provider requires a hold on part of each financed sale until the plan settles. These reserves apply to financed sales regardless of your account's health, and appear in your reserve breakdown as their own lines with their own release dates.

Each provider's guide covers its reserve in detail:

* [Splitit](https://docs.whop.com/payments-and-billing/financing/splitit-guide.md): 15% of each financed sale, held for 180 days
* [SeQura](https://docs.whop.com/payments-and-billing/financing/sequra-guide.md): 12% of each financed sale, with a hold period of about 12 months

## Seeing your reserves

The [Balances](https://whop.com/dashboard/balance/) page shows your full reserve breakdown: the total held, each reserve type with its current percentage and hold period, and the exact dates funds unlock.

You can also retrieve the same breakdown programmatically via API, using `GET /accounts/{account_id}/reserves`.

## Next steps

> [card] [Managing dispute rates](https://docs.whop.com/trust-and-safety/account-health/managing-dispute-rates.md)
>
> Keeping disputes low is the fastest way to shrink a reserve, and to never
> get one.

> [card] [Payment health](https://docs.whop.com/trust-and-safety/account-health/payment-health.md)
>
> Read your Payment health dashboard and the controls on your account.


## Related topics

- [Splitit Guide](https://docs.whop.com/payments-and-billing/financing/splitit-guide.md)
- [SeQura Guide](https://docs.whop.com/payments-and-billing/financing/sequra-guide.md)
- [Manage Disputes](https://docs.whop.com/manage-your-business/manage-payments/manage-disputes.md)
