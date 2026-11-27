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

# Search Account Records

> Searches one account's records in one call. `account` rows include the account in `account_id`. Use `include_subaccounts` to include its subaccounts. Every other row belongs to one of those accounts.

Each `type` needs its own read permission on the record's account, and rows you can't read are left out without an error: `account` (`biz_`, `company:basic:read`), `member` (`mber_`, `member:basic:read`), `membership` (`mem_`, `member:basic:read`), `payment` (`pay_`, `payment:basic:read`), `affiliate` (`aff_`, `affiliate:basic:read`), `review` (`rev_`, `company:basic:read`), `resolution_center_case` (`reso_`, `payment:resolution_center_case:read`) and `promo_code` (`promo_`, `promo_code:basic:read`). A query containing `@` also needs `member:email:read`, and a phone match `member:phone:read`. An invoice's number finds the invoice as its `payment`.

`query` must be 2 to 250 characters after trimming. A full card number returns a `400`. `data` lists identifier matches first, such as an ID, email address, card digits or invoice number. Within each identifier match type, the newest records appear first. Queries of 3 or more characters also return ranked results after identifier matches, best match first. Each record appears once. `match_reason` explains why a record matched. [Search](/api-reference/search/search) lists the query shapes and match reasons.

`first` limits the response to 20 rows by default and up to 100. Search returns a single page. To filter one resource or page through every match, use its list endpoint, such as `GET /payments?query=`.



## OpenAPI

<!-- OpenAPI source: `get /search`, docs.whop.com/openapi/api-v1-native.json (inlined by docs.whop.com; stripped on download; see specs/openapi.json) -->

## Related topics

- [Search API](https://docs.whop.com/api-reference/search/search.md)
- [Whop API Overview](https://docs.whop.com/api-reference/overview.md)
- [QuickBooks](https://docs.whop.com/third-party-integrations/quickbooks.md)
