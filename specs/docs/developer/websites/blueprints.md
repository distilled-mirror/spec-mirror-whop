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

# Deploy a Blueprint Business

> Deploy a working Whop business — products, pricing, and a live website — in one step, or clone its code with the CLI.

A blueprint is a whole Whop business: products with their pricing, and a website that sells them. The one in the gallery is a real business that's running, and deploying it gives you your own copy. Browse them at [whop.com/blueprints](https://whop.com/blueprints).

Deploying a blueprint sets up:

* **Your products**, copied from the blueprint with their pricing variants, images, and store page styling
* **Your website**, live at your own `whop.site` address
* **The wiring between them**, so the site can take payments from the moment it's up

| | Curated | Community blueprints |
| - | - | - |
| **What's in it** | Blueprints Whop has verified | Every other published blueprint |
| **How it gets there** | Whop verifies it | Published by its owner |
| **Sorting** | Most businesses created from it | Trending or newest |

The rows partition the set, so a verified blueprint appears in the curated row only. Each card shows how many businesses have been created from it.

## Deploy a blueprint

> [tab] From the gallery
>
> 1. Open [whop.com/blueprints](https://whop.com/blueprints) and select a blueprint
> 2. Select the deploy button on its detail page
>
> Whop creates your business if you don't have one, copies the products and the site over, and serves it at your route. Nothing builds on your machine.

> [tab] From your dashboard
>
> 1. Open **Websites** in your [dashboard](https://whop.com/dashboard)
> 2. Select **New website**, then **Use a blueprint**
> 3. Pick one from the gallery
>
> The new site appears in the **Websites** list, where the **Edit**, **Settings**, and **Versions** tabs let you change it, rename its address, and roll builds back.

## The 10% affiliate cut

Blueprints are a revenue stream for the people who publish them. When a business deploys your blueprint, you earn **10% of Whop's profit** on the sales its site makes — attributed automatically, the same model as [referring a business to Whop](https://docs.whop.com/refer-businesses-to-whop.md).

It comes out of Whop's share, not the seller's: if you deploy another creator's blueprint, their 10% doesn't change your pricing or your payouts. The cut is on sales only, and it's tied to the deployed `whop.site` site — the sales that run through the copy you shipped are the ones that count.

## Clone the source

Deploying gives you a running business. To change how the site works, get the code:

```bash theme={null}
whop apps init --template app_xxxxxxxx --name "Shine Time" --domain shine-time
```

This registers a new app and downloads the source archive from the blueprint's production build. The directory links to *your* app, so the first `whop apps deploy` ships to your domain. Cloning a website blueprint carries the `website` type over, and `--app_type` overrides it.

From there it's an ordinary project — see the [quickstart](https://docs.whop.com/developer/websites/quickstart.md).

## Next steps

> [card] [Build a website with the CLI](https://docs.whop.com/developer/websites/quickstart.md)
>
> Start from scratch instead of from an existing site.

> [card] [How hosting works](https://docs.whop.com/developer/websites/hosting.md)
>
> Secrets, rollbacks, logs, and the injected API key.

> [card] [Accept payments](https://docs.whop.com/developer/guides/accept-payments.md)
>
> Turn the site you deployed into something that sells.

> [card] [Track visitors](https://docs.whop.com/developer/websites/tracking.md)
>
> The pixel is already installed — add your own events.


## Related topics

- [Start a Project from a Blueprint](https://docs.whop.com/developer/guides/blueprints.md)
- [Host a Website on Whop](https://docs.whop.com/developer/websites/overview.md)
- [Apps API](https://docs.whop.com/api-reference/apps/app.md)
