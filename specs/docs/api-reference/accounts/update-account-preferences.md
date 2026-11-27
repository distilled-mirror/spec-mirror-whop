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

# Update Account Preferences

> Updates the account's preferences. Keys left out keep their current values. In `affiliate_program` and `ads_triple_whale_integration`, only the fields you send change. `customer_tracking`, `card_surcharge` and `ads_payment_methods` replace their stored value as a whole.

The required scope depends on which preferences you update.

- `ad_campaign:create`: `ads_payment_methods`, `ads_reporting_currency`, `ads_scheduling_timezone`, `ads_triple_whale_integration`.
- `payout:account:update`: `cards_auto_top_up`, `cards_notifications`, `covers_subaccount_negative_balances`, `merchant_of_record`.
- `payment:manage`: `off_session_payment_rules`.
- `payment:dispute`: `dispute_fighter_enabled`.
- `company:update`: `economic_intelligence_duration_key`, `economic_intelligence_auto_renew`, `review_request_emails`, `cancellation_emails`, `renewal_reminder_emails`, `failed_payment_emails`, `subaccount_notifications`, `subaccount_team_view_only`, `card_surcharge`, `apple_iap_fee_payer`.
- `company:manage_checkout`: `feedback_options`, `customer_tracking`.
- `affiliate:update`: `affiliate_program`.

To update keys under more than one scope, the API key needs all of those scopes.




## OpenAPI

<!-- OpenAPI source: `patch /accounts/{account_id}/preferences`, docs.whop.com/openapi/api-v1-native.json (inlined by docs.whop.com; stripped on download; see specs/openapi.json) -->

## Related topics

- [Accounts API](https://docs.whop.com/api-reference/accounts/account.md)
- [Preferred Settlement Currency](https://docs.whop.com/developer/payouts/preferred-settlement-currency.md)
- [API Versions](https://docs.whop.com/developer/api/versioning.md)
