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

# App Store Listing Requirements

> What an app needs to be listed in the Whop App Store, and how to relist one that Whop removed.

Every app in the Whop App Store must meet the requirements below. Whop checks each app when it's submitted and removes any listed app that fails a requirement.

## Requirements

| Requirement | Details |
| - | - |
| **App availability** | The app's experience or dashboard view loads inside Whop. Apps fail this requirement when the URL doesn't resolve, the server returns errors, or the deployment is turned off or deleted. A placeholder screen or a `localhost` production URL also fails it. |
| **Listing details** | The listing includes an icon, name, description, base URL, experience or dashboard path, at least two gallery images, and an app store description. |
| **Permission reasons** | Each [permission](https://docs.whop.com/developer/guides/permissions.md) the app requests has its own justification. Blank justifications, or one justification copied across several permissions, aren't accepted. |
| **Whop brand** | The app's name, icon, and gallery images don't use the Whop name, logo, or other Whop intellectual property. The description can state that the app works with Whop, and gallery images can show the app running inside Whop. |

## Removal from the store

When a listed app no longer meets a requirement, Whop removes it from the App Store and emails the business's owners and admins with the reason.

> [!NOTE]
>
> Removal doesn't uninstall the app. Businesses that already installed it keep using it.

To relist the app after fixing the issue, open it in the [Developer dashboard](https://whop.com/dashboard/developer), select **Publish app**, then select **Submit for review**.

## Next steps

> [card] [Build Whop apps](https://docs.whop.com/developer/apps/overview.md)
>
> What apps can do and where they appear.

> [card] [Permissions](https://docs.whop.com/developer/guides/permissions.md)
>
> Request access and justify each permission.

> [card] [App views](https://docs.whop.com/developer/guides/app-views.md)
>
> Configure where an app appears.


## Related topics

- [Request App Permissions](https://docs.whop.com/developer/guides/permissions.md)
- [Build Whop Apps](https://docs.whop.com/developer/apps/overview.md)
- [List App Categories](https://docs.whop.com/api-reference/apps/list-app-categories.md)
