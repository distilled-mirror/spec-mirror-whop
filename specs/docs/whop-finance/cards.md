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

# Whop Cards

> Spend your pending Whop balance instantly with a Visa card, earn 5% cashback at select merchants, add it to a mobile wallet, and issue cards to employees.

Spend your balance instantly and earn 5% cashback on select merchants. No extra fees, no waiting.

## How it works

With Whop Cards, you can spend your balance instantly without waiting for payments to settle. Spend anywhere that accepts Visa and add your card to Apple Pay or Google Wallet in seconds. Earn 5% cashback on Uber and more.

## Use your pending balance instantly

No more waiting days for funds to clear. Your pending balance is available to spend on your card the moment a payment comes in.

This applies to card spending, instant payouts, and ad spend. A standard withdrawal to your bank pays out settled funds only, and transfers to another account don't draw on pending — for those, the funds have to clear first.

## 5% cashback with select merchants

Maximize your returns with 5% cashback on select merchants. Earn automatically when you use your Whop Card at participating merchants:

* Uber
* More coming soon

## Add your card to a mobile wallet

Add your Whop Card to Apple Wallet or Google Wallet in seconds. Tap to pay anywhere that accepts Visa and start spending immediately.

## Why businesses love it

* **Speed.** Use it anywhere that accepts Visa, including online, and in stores. Add it to Apple Pay and start spending.
* **Cashback.** Earn 5% back from selected merchants.
* **Management.** Keep all your business funds in one place.

## Create cards for employees and set spending limits

Issue cards to your team in seconds. Set custom spending limits per card to keep budgets in check.

## Next steps

> [card] [Supported regions](https://docs.whop.com/whop-finance/supported-regions.md)
>
> Check supported U.S. states and unsupported countries for consumer and business Whop Cards

> [card] [Set up payouts](https://docs.whop.com/manage-your-business/manage-payouts/set-up-payouts.md)
>
> Connect a payout method and start moving money out of Whop

> [card] [Card declines & risk rules](https://docs.whop.com/whop-finance/card-declines-and-risk-rules.md)
>
> Understand why a card transaction was declined and which risk rules applied during authorization

> [card] [Blocked categories & merchants](https://docs.whop.com/whop-finance/blocked-categories-and-merchants.md)
>
> See the merchant categories and merchants that Whop Cards block by default


## Related topics

- [Whop Card Supported Regions](https://docs.whop.com/whop-finance/supported-regions.md)
- [Whop Ads](https://docs.whop.com/manage-your-business/growth-marketing/ads.md)
- [Card Declines & Risk Rules](https://docs.whop.com/whop-finance/card-declines-and-risk-rules.md)
