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

# Build a Website with the CLI

> Scaffold a Vite app, run it locally against real Whop data, and deploy it to a whop.site address or your own domain.

Four commands take you from an empty directory to a live site at its domain.

> [!NOTE]
>
> Install the [Whop CLI](https://docs.whop.com/developer/cli.md) and run `whop` once to sign in and select a business.

**1. Create the app**

```bash theme={null}
whop apps init --app_type website --name "Shine Time" --domain shine-time
```

Claims the domain, registers the app on it, scaffolds the project into `./shine-time`, and writes a `whop.app.json` linking the directory to the app. Omit the flags to be prompted instead.

`--domain` takes a whop.site name (`shine-time` → `shine-time.whop.site`) or your own domain, such as `shinetime.com`:

* A domain already on your business is used as it is.
* A domain nobody has registered is bought through Whop. The CLI returns a purchase link, and nothing is charged until you pay there.
* A domain you own elsewhere is connected: publish the DNS records the CLI lists.

Until you pay or publish those records, the site waits for that domain. `whop domains get shinetime.com` shows its status.

> [!WARNING]
>
> `--app_type` is permanent. `website` means visitors browse it at its domain. `b2c_app` means creators install it into their whop.

**2. Run it locally**

```bash theme={null}
cd shine-time
whop apps dev
```

Starts your `dev` script with the environment the hosted runtime would give it: `WHOP_APP_ID`, a short-lived token as `WHOP_API_KEY`, and every stored [app secret](https://docs.whop.com/developer/websites/hosting.md#secrets). Server-side SDK calls work with no setup. Anything you export yourself wins, and restarting refreshes the token.

**3. Deploy**

```bash theme={null}
whop apps deploy
```

Builds, type-checks, uploads the build and a source archive, and promotes it. Your site is live when it finishes.

```bash theme={null}
whop apps deploy --preview                  # upload without going live
whop apps builds promote abld_xxxxxxxx      # ship it later, or roll back to it
```

**4. Watch it run**

```bash theme={null}
whop apps logs app_xxxxxxxx --level error
```

Server-runtime logs, kept for 7 days. See [Hosting](https://docs.whop.com/developer/websites/hosting.md#logs).

## What the scaffold gives you

`whop apps init` scaffolds a React project with TanStack's own create command, then applies the Whop wiring:

| Change | Why |
| - | - |
| `whop()` plugin from `@whop/cli/vite` | Packs `dist/` into the archive `whop apps deploy` uploads |
| `@whop/cli` as a dev dependency | The Vite config imports the plugin from it |
| `deploy` and `typecheck` scripts | What `whop apps deploy` runs |
| Build config named after your route | Keeps the project aligned with the app |
| `whop.app.json` | Links the directory to the app |

## Bring your own framework

The plugin is framework-agnostic — TanStack Start, Nitro, Hono, or plain SSR all work. The one requirement is the build layout: static assets in `dist/client`, the server bundle in `dist/server`.

```typescript vite.config.ts theme={null}
import { defineConfig } from "vite";
import { whop } from "@whop/cli/vite";

export default defineConfig({
  plugins: [whop()],
});
```

```bash theme={null}
whop apps deploy --app app_xxxxxxxx
```

`--app` links the current directory to an existing app before deploying, replacing any link in `whop.app.json`.

## Work on a site from another machine

```bash theme={null}
whop apps pull --app app_xxxxxxxx
```

On a fresh machine this creates `./<app name>` from the production build's source archive. Inside an existing project it three-way merges the deployed source with your local code through git. Pass `--build abld_xxxxxxxx` for a specific build, or `--overwrite` to skip the merge.

## Next steps

> [card] [How hosting works](https://docs.whop.com/developer/websites/hosting.md)
>
> Secrets, rollbacks, logs, and the injected API key.

> [card] [Track visitors](https://docs.whop.com/developer/websites/tracking.md)
>
> The pixel is already installed — add your own events.

> [card] [Blueprints](https://docs.whop.com/developer/websites/blueprints.md)
>
> Start from a working business instead of an empty project.

> [card] [CLI reference](https://docs.whop.com/developer/cli.md)
>
> Every command, plus non-interactive use.


## Related topics

- [Host a Website on Whop](https://docs.whop.com/developer/websites/overview.md)
- [Deploy a Blueprint Business](https://docs.whop.com/developer/websites/blueprints.md)
- [Build with the CLI and Claude](https://docs.whop.com/developer/guides/cli-with-claude.md)
