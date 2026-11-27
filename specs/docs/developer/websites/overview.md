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

# Host a Website on Whop

> Ship a fully hosted website on Whop — served at a whop.site address or your own domain, versioned on every deploy, and wired to payments and analytics.

A Whop website is an app of type `website`: a site visitors browse at its domains — a free `whop.site` address, your own domain, or both — hosted by Whop. No server to provision, no deploy pipeline to wire. Write a Vite app and run one command, or start from a [blueprint](https://docs.whop.com/developer/websites/blueprints.md): a working business with products already set up.

> [!WARNING]
>
> Choose the app type up front. A `website` can't be converted into a [Whop app](https://docs.whop.com/developer/guides/app-views.md) (`b2c_app`) later.

## Four ways to start

| | Deploy a blueprint | Describe it to AI | Build it with the CLI | Migrate an existing site |
| - | - | - | - | - |
| **Effort** | One step | A few minutes of chat | A normal dev loop | One step |
| **You get** | A working business with products and a site | A site built from your prompt | Your own code and framework | A copy of a site you run |
| **Code access** | Via `whop apps init --template` | Edit in the dashboard, or pull the source | It's your repo | Yes |
| **Best for** | Launching today | Non-developers, first drafts | Anything custom | A second brand, a staging copy |

> [!NOTE]
>
> Blueprints share revenue both ways. Deploy one and its creator earns 10% of Whop's profit on your site's sales. Publish one and you earn 10% from every business that deploys it. See [The 10% affiliate cut](https://docs.whop.com/developer/websites/blueprints.md#the-10%-affiliate-cut).

This section covers everything except the AI path. For that one, open **Websites** in your [dashboard](https://whop.com/dashboard), select **New website**, then **Create your own**.

## What you get

* **Payments.** Use checkout links or the Checkout element — see [Accept payments](https://docs.whop.com/developer/guides/accept-payments.md).
* **A live address** as soon as the first build is promoted: a free `whop.site` address, or a custom domain you connect or buy through Whop.
* **Versioned deploys.** Promoting an older build is how you roll back, from the CLI or the **Versions** tab.
* **API calls with no key handling.** The runtime signs server-side requests to the Whop API. Your code never sees the key — see [Hosting](https://docs.whop.com/developer/websites/hosting.md#call-the-whop-api).
* **The Whop pixel, pre-installed.** Page views and checkout conversions are tracked with no snippet to paste — see [Track visitors](https://docs.whop.com/developer/websites/tracking.md).

## Next steps

> [card] [Blueprints](https://docs.whop.com/developer/websites/blueprints.md)
>
> Deploy a working business — products, pricing, and a site — in one step.

> [card] [Build a website with the CLI](https://docs.whop.com/developer/websites/quickstart.md)
>
> Scaffold, run locally, and deploy your first site in four commands.

> [card] [How hosting works](https://docs.whop.com/developer/websites/hosting.md)
>
> Builds, secrets, logs, rollbacks, and the injected API key.

> [card] [Accept payments](https://docs.whop.com/developer/guides/accept-payments.md)
>
> Add checkout to the site you just shipped.


## Related topics

- [Website Hosting on Whop](https://docs.whop.com/developer/websites/hosting.md)
- [Websites](https://docs.whop.com/elements/latest/websites/overview.md)
- [WebsitesElement](https://docs.whop.com/elements/latest/websites/websites.md)
