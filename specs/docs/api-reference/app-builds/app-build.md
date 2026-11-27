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

# App Builds API

> An App Build is a versioned artifact uploaded for an app — a hosted web archive, or an iOS/Android bundle.

Builds start as drafts, go through review, and one approved build per platform is served to users as the production build.

Use the App Builds API to upload a build for an app, list an app's builds with platform and status filters, retrieve a build, and promote a draft or approved build to production.

> [!NOTE]
>
> Replaces the Legacy [App builds](https://docs.whop.com/api-reference/app-builds/app-build)
> resource. Existing Legacy integrations keep working; see [API
> versions](https://docs.whop.com/developer/api/versioning.md) for the stability contract.

## Endpoints

| Endpoint | Request |
| - | - |
| [List App Builds](https://docs.whop.com/api-reference/app-builds/list-app-builds.md) | GET `/app_builds` |
| [Retrieve App Build](https://docs.whop.com/api-reference/app-builds/retrieve-app-build.md) | GET `/app_builds/{id}` |
| [Create App Build](https://docs.whop.com/api-reference/app-builds/create-app-build.md) | POST `/app_builds` |
| [Promote App Build](https://docs.whop.com/api-reference/app-builds/promote-app-build.md) | POST `/app_builds/{id}/promote` |


## Related topics

- [App Build](https://docs.whop.com/api-reference/legacy/app-builds/app-build.md)
- [Apps API](https://docs.whop.com/api-reference/apps/app.md)
- [Create app build](https://docs.whop.com/api-reference/legacy/app-builds/create-app-build.md)
