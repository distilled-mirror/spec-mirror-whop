> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Build a Website with the CLI

> Scaffold a Vite app, run it locally against real Whop data, and deploy it to your own whop.site address.

Four commands take you from an empty directory to a live site at `<route>.whop.site`.

<Note>
  Install the [Whop CLI](/developer/cli) and run `whop` once to sign in and select a business.
</Note>

<Steps>
  <Step title="Create the app">
    ```bash theme={null}
    whop apps init --app_type website --name "Shine Time" --route shine-time
    ```

    Registers the app, scaffolds the project into `./shine-time`, and writes a `whop.app.json` linking the directory to the app. Omit the flags to be prompted instead. Your address is the route: `shine-time.whop.site`.

    <Warning>
      `--app_type` is permanent. `website` means visitors browse it at your route. `b2c_app` means creators install it into their whop.
    </Warning>
  </Step>

  <Step title="Run it locally">
    ```bash theme={null}
    cd shine-time
    whop apps dev
    ```

    Starts your `dev` script with the environment the hosted runtime would give it: `WHOP_APP_ID`, a short-lived token as `WHOP_API_KEY`, and every stored [app secret](/developer/websites/hosting#secrets). Server-side SDK calls work with no setup. Anything you export yourself wins, and restarting refreshes the token.
  </Step>

  <Step title="Deploy">
    ```bash theme={null}
    whop apps deploy
    ```

    Builds, type-checks, uploads the build and a source archive, and promotes it. Your site is live when it finishes.

    ```bash theme={null}
    whop apps deploy --preview                  # upload without going live
    whop apps builds promote abld_xxxxxxxx      # ship it later, or roll back to it
    ```
  </Step>

  <Step title="Watch it run">
    ```bash theme={null}
    whop apps logs app_xxxxxxxx --level error
    ```

    Server-runtime logs, kept for 7 days. See [Hosting](/developer/websites/hosting#logs).
  </Step>
</Steps>

## What the scaffold gives you

`whop apps init` scaffolds a React project with TanStack's own create command, then applies the Whop wiring:

| Change                                | Why                                                       |
| ------------------------------------- | --------------------------------------------------------- |
| `whop()` plugin from `@whop/cli/vite` | Packs `dist/` into the archive `whop apps deploy` uploads |
| `@whop/cli` as a dev dependency       | The Vite config imports the plugin from it                |
| `deploy` and `typecheck` scripts      | What `whop apps deploy` runs                              |
| Build config named after your route   | Keeps the project aligned with the app                    |
| `whop.app.json`                       | Links the directory to the app                            |

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

On a fresh machine this creates `./<route>` from the production build's source archive. Inside an existing project it three-way merges the deployed source with your local code through git. Pass `--build abld_xxxxxxxx` for a specific build, or `--overwrite` to skip the merge.

## Next steps

<CardGroup cols={2}>
  <Card title="How hosting works" href="/developer/websites/hosting">
    Secrets, rollbacks, logs, and the injected API key.
  </Card>

  <Card title="Track visitors" href="/developer/websites/tracking">
    The pixel is already installed — add your own events.
  </Card>

  <Card title="Blueprints" href="/developer/websites/blueprints">
    Start from a working business instead of an empty project.
  </Card>

  <Card title="CLI reference" href="/developer/cli">
    Every command, plus non-interactive use.
  </Card>
</CardGroup>
