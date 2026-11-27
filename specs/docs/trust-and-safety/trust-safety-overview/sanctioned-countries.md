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

# Sanctioned Countries

> List of countries Whop doesn't support due to international sanctions and regulatory restrictions.

Whop doesn't support payouts, payments, or accounts in certain countries due to international sanctions and regulatory restrictions. This list applies to both creators (sellers) and customers. Whop reviews it as sanction programs change.

> [!WARNING]
>
> Whop doesn't support the following sanctioned or restricted countries:

* Afghanistan
* Belarus
* Burundi
* Central African Republic
* Comoros
* Cuba
* Democratic Republic of the Congo
* Eritrea
* Haiti
* Iran
* Iraq
* Lesotho
* Libya
* Myanmar
* North Korea
* Palestine
* Papua New Guinea
* Russia
* Somalia
* South Sudan
* Sudan
* Syria
* Ukraine (the following jurisdictions only)
  * Crimea
  * Sevastopol
  * Donetsk
  * Luhansk
  * Zaporizhzhia
  * Kherson
* Venezuela
* Yemen

## What this means for you

* **Creators** in sanctioned countries can't register a Whop account, sell access passes, or receive payouts.
* **Customers** in sanctioned countries can't purchase access passes or memberships on Whop.
* **Existing accounts** — Whop may suspend accounts that become subject to sanctions, in line with the [Terms of Service](https://whop.com/terms-of-service) and applicable law.

> [!NOTE]
>
> This list reflects current sanctions enforcement and may change as
> regulations change. Always check this page for the most up-to-date
> list before assuming Whop supports a country.

## Next steps

> [card] [Set up payouts](https://docs.whop.com/manage-your-business/manage-payouts/set-up-payouts.md)
>
> See the full list of countries Whop supports for payouts and learn how to withdraw your balance.

> [card] [Prohibited businesses](https://docs.whop.com/trust-and-safety/trust-safety-overview/what-is-not-allowed-on-whop.md)
>
> Review the categories of products and services prohibited on Whop.


## Related topics

- [Set Up Payouts](https://docs.whop.com/manage-your-business/manage-payouts/set-up-payouts.md)
- [Card Declines & Risk Rules](https://docs.whop.com/whop-finance/card-declines-and-risk-rules.md)
- [Tap to Pay on iPhone](https://docs.whop.com/payments-and-billing/tap-to-pay.md)
