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

# Instant Withdrawals

> See how much of your Whop balance you can withdraw instantly and how the daily instant limit works.

An instant withdrawal sends money from your Whop balance to your payout method in minutes, instead of on the standard payout timeline. When you choose instant speed, the withdraw screen shows how much you can send instantly. This page explains where that amount comes from.

## How much you can withdraw instantly

The amount shown as available instantly is the smaller of two numbers:

* **Your eligible balance:** the part of your balance that can go out instantly right now
* **What's left of your daily instant limit:** see [Daily instant limit](#daily-instant-limit)

If the daily limit sets the amount, the withdraw screen also shows how much of the limit you have left today.

### Why it can be lower than your balance

Instant withdrawals can include sales that haven't finished settling yet. Some money in your balance still can't go out instantly:

* **Reserves:** money Whop holds back to cover future refunds and disputes. Learn more about [reserves](https://docs.whop.com/trust-and-safety/account-health/reserves.md).
* **Charges:** money that covers a negative balance in another currency
* **Withdrawals in progress:** money already on its way to a payout method
* **Money on your Whop Card:** a single withdrawal comes from either your Whop Card or the rest of your balance, not both

## Daily instant limit

Instant withdrawals have a daily limit:

| Instant withdrawal | Daily limit |
| - | - |
| To a bank account or other payout method | \$9,999 |
| Whop Card top-up | \$49,999 |

Both limits count the same daily total. Everything you send instantly in a day, including instant Whop Card top-ups, counts toward each one. For example, after a $5,000 instant Whop Card top-up, you have $4,999 left for other instant withdrawals that day.

If your balance is in another currency, the limit is the equivalent amount in that currency.

The limit resets every day at midnight US Central Time. When the withdraw screen shows the reset time, it uses your local time.

> [!NOTE]
>
> Some accounts don't have a daily instant limit. For those accounts, only the
> eligible balance sets the instant amount.

## If you go over the limit

If you enter more than you have left of your daily instant limit, the withdraw screen tells you how much you have left. You can:

* Lower the amount to what's left of your limit
* Switch to standard speed. Standard withdrawals don't use the daily instant limit.
* Wait for the limit to reset and withdraw the rest instantly then

## Next steps

> [card] [Payout methods](https://docs.whop.com/manage-your-business/manage-payouts/payout-methods.md)
>
> Add a payout method and compare payout options.

> [card] [Troubleshoot payouts](https://docs.whop.com/manage-your-business/manage-payouts/troubleshoot-payouts.md)
>
> Check payout statuses and fix failed payouts.


## Related topics

- [Payout Methods](https://docs.whop.com/manage-your-business/manage-payouts/payout-methods.md)
- [Account Suspensions](https://docs.whop.com/trust-and-safety/suspensions/account-suspensions.md)
- [Whop Cards](https://docs.whop.com/whop-finance/cards.md)
