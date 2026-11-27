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

# Apps API

> An App is software you build on Whop.

It can be a hosted web app served at its domains, such as a `whop.site` address or a custom domain, or an API integration installed as an experience, and it belongs to the account that owns its credentials, settings, builds, and runtime logs.

Use the Apps API to manage app configuration, deploy an app's working copy and follow the run on the app's `deployment` field, and, for hosted apps, read server runtime logs for console output, uncaught exceptions, and failed requests.

Business apps have one App Store category. List the categories with `GET /apps/categories`, then set the app's `category_id` to the chosen category's ID. Use a category's `route` as `marketplace_category_route` when listing apps.

Apps are also reusable blueprints. List official blueprints with `app_type=website&verified=true&order=template_usage`, or community blueprints with `app_type=website&verified=false&recommended=true&order=template_usage`. Pass the returned App `id` as `blueprint_id` when creating an Account.

> [!NOTE]
>
> Replaces the Legacy [Apps](https://docs.whop.com/api-reference/apps/app) resource. Existing Legacy
> integrations keep working; see [API versions](https://docs.whop.com/developer/api/versioning.md) for
> the stability contract.

## Endpoints

| Endpoint | Request |
| - | - |
| [List Apps](https://docs.whop.com/api-reference/apps/list-apps.md) | GET `/apps` |
| [Retrieve App](https://docs.whop.com/api-reference/apps/retrieve-app.md) | GET `/apps/{id}` |
| [List App Logs](https://docs.whop.com/api-reference/apps/list-app-logs.md) | GET `/apps/{id}/logs` |
| [List App Categories](https://docs.whop.com/api-reference/apps/list-app-categories.md) | GET `/apps/categories` |
| [Create App](https://docs.whop.com/api-reference/apps/create-app.md) | POST `/apps` |
| [Deploy App](https://docs.whop.com/api-reference/apps/deploy-app.md) | POST `/apps/{id}/deploy` |
| [Update App](https://docs.whop.com/api-reference/apps/update-app.md) | PATCH `/apps/{id}` |
| [Update App Permissions](https://docs.whop.com/api-reference/apps/update-app-permissions.md) | PATCH `/apps/{id}/permissions` |
| [Delete App](https://docs.whop.com/api-reference/apps/delete-app.md) | DELETE `/apps/{id}` |


## Related topics

- [API Versions](https://docs.whop.com/developer/api/versioning.md)
- [Retrieve App](https://docs.whop.com/api-reference/apps/retrieve-app.md)
- [Update App](https://docs.whop.com/api-reference/apps/update-app.md)
