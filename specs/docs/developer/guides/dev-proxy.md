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

# Development Proxy

> Run the Whop development proxy to replicate production authentication and iframe behavior during development.

In production, your app runs inside the Whop iframe behind a reverse proxy that injects the `x-whop-user-token` header on every request. Every Whop app verifies that token through [`whopsdk.verifyUserToken`](https://docs.whop.com/developer/guides/authentication), so you need the same header on `localhost` to test with real users. The development proxy injects it for you.

> [!WARNING]
>
> `verifyUserToken` is not available in the current `@whop/sdk`. The header described here is still injected as documented.

## Pick your setup

There are two installation modes. Both ship the same proxy binary. The difference is whether the proxy spawns your development server or runs alongside it.

| | Wrapped (Node/JS) | Standalone |
| - | - | - |
| **Best for** | Next.js, Express, anything with a node-based development script | Python, Ruby, Go, anything non-JS |
| **How it runs** | `whop-proxy --command 'next dev'` spawns your server | Your server runs on a port, and the proxy forwards to it |
| **Port** | One port (proxy + server) | Two ports (proxy on 3000, server on e.g. 5000) |
| **Install** | `pnpm add -D @whop-apps/dev-proxy` | `pnpm dlx @whop-apps/dev-proxy --standalone` (no install needed) |

## `Next.js` and `JavaScript` app

**1. Add the proxy as a development dependency**

```bash pnpm theme={null}
pnpm add -D @whop-apps/dev-proxy
```

```bash bun theme={null}
bun install -D @whop-apps/dev-proxy
```

```bash npm theme={null}
npm install -D @whop-apps/dev-proxy
```

```bash yarn theme={null}
yarn add -D @whop-apps/dev-proxy
```

**2. Update the dev script in package.json**

Update the `dev` script in `package.json` to include the proxy.

```json theme={null}
"scripts": {
    "dev": "whop-proxy --command 'next dev --turbopack'",
}
```

> [!NOTE]
>
> You can update the `dev` command to match your framework requirements.
> You can also wrap other commands with the proxy in a similar way.

**3. Run the proxy**

```bash pnpm theme={null}
pnpm dev
```

```bash npm theme={null}
npm run dev
```

```bash yarn theme={null}
yarn dev
```

## Standalone mode (other frameworks)

**1. Run your app locally**

Run your app on your local machine on some port, for example 5000.

```bash Python + FastAPI theme={null}
uvicorn main:app --port 5000
```

```bash Ruby on Rails theme={null}
rails server -p 5000
```

```bash Go theme={null}
go run main.go --port 5000
```

**2. Run the proxy in standalone mode**

```bash pnpm theme={null}
pnpm dlx @whop-apps/dev-proxy --standalone --upstreamPort=5000 --proxyPort=3000
```

```bash bun theme={null}
bunx @whop-apps/dev-proxy --standalone --upstreamPort=5000 --proxyPort=3000
```

```bash npm theme={null}
npx @whop-apps/dev-proxy --standalone --upstreamPort=5000 --proxyPort=3000
```

```bash yarn theme={null}
yarn dlx @whop-apps/dev-proxy --standalone --upstreamPort=5000 --proxyPort=3000
```

> [!NOTE]
>
> This will run the proxy as an independent process. It will start a server on
> port 3000 and forward requests to port 5000 and append the user token in the
> headers.

## Proxy command options

Configure the proxy with the following command-line options:

```bash theme={null}
Usage: pnpm dlx @whop-apps/dev-proxy [options]

Options:

--proxyPort <port>      The port the proxy should listen on (3000 by default)
--upstreamPort <port>   The port the upstream server is listening on (set automatically by default)
--npmCommand <command>  The npm command to run to start the upstream server (dev by default)
--command <command>     The command to run to start the upstream server (npm run dev by default)
--standalone            Run the proxy as an independent process proxying requests from one port to another port. Ignores the command / npmCommand options.
```

## Next steps

> [card] [Authentication](https://docs.whop.com/developer/guides/authentication.md)
>
> Verify the iframe user token in the same code locally and in production.

> [card] [Listen to webhooks](https://docs.whop.com/developer/guides/webhooks.md)
>
> Forward Whop webhooks to localhost while developing (use ngrok or Cloudflare Tunnel).

> [card] [Build app views](https://docs.whop.com/developer/guides/app-views.md)
>
> Set up the iframe entry points your app will render inside.

> [card] [Request permissions](https://docs.whop.com/developer/guides/permissions.md)
>
> Configure the scopes your app needs before publishing.


## Related topics

- [Authenticate Users in Whop Apps](https://docs.whop.com/developer/guides/authentication.md)
- [Request App Permissions](https://docs.whop.com/developer/guides/permissions.md)
- [Build Whop Apps with React Native](https://docs.whop.com/developer/guides/react-native.md)
