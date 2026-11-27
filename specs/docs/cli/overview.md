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

# Whop CLI

> Install the Whop CLI, sign in, and manage your whole business from the terminal: 80 command groups across commerce, money, ads, and apps.

The Whop CLI (`whop`) puts the Current API in your terminal: deploy apps, create products, set pricing, send payouts, run ads, and pull stats. Every command also works non-interactively for scripts and AI agents.

> [!WARNING]
>
> The CLI has no sandbox or test mode. Every command runs in production, and many can create real resources or move real money. Commands that mutate state ask for confirmation, and `whop send --dry-run` previews a payment without sending it.

## Install

```bash macOS / Linux theme={null}
curl -fsSL https://whop.com/install.sh | sh
```

```powershell Windows theme={null}
irm https://github.com/whopio/whop-public-cli/releases/latest/download/install.ps1 | iex
```

```bash Homebrew theme={null}
brew install whopio/tap/whop
```

```bash npm theme={null}
npm install -g @whop/cli
```

> [!NOTE]
>
> The install scripts put `whop` in `~/.local/bin` (`%USERPROFILE%\.local\bin` on Windows) and don't edit your shell profile. If `whop` isn't found afterwards, add that directory to your `PATH`, for example `export PATH="$HOME/.local/bin:$PATH"`. Set `WHOP_INSTALL_DIR` to install somewhere else. The npm package requires Node.js 22 or later.

Check the install. It prints the version number:

```bash theme={null}
whop --version
```

`whop --help` also shows the API version the CLI speaks.

## Sign in

```bash theme={null}
whop login
```

`whop login --method oauth` opens a browser. `whop login --method api-key --apiKey whop_xxx` is non-interactive. Profiles let you keep several identities: `whop auth list`, `whop auth switch`, and `whop auth status` manage them. Then pick the business the CLI acts on:

```bash theme={null}
whop quickstart
```

## What's here

80 command groups, organized the way `whop --help` groups them:

| Category | Groups |
| - | - |
| Get started | `quickstart` · `apps` |
| Commerce | `products` · `variants` · `promo-codes` · `checkout-configurations` · `waitlist-entries` · `memberships` · `shipments` · `reviews` · `review-reports` · `product-affiliates` |
| Core resources | `accounts` · `users` · `members` · `team-members` · `search` · `external-accounts` · `social-accounts` |
| Payments | `payments` · `invoices` · `confirmation-tokens` · `payment-quotes` · `setup-intents` · `refunds` · `resolution-center-cases` · `dispute-alerts` · `disputes` · `payment-rules` · `financing-applications` |
| Money | `deposits` · `transfers` · `payouts` · `ledgers` · `cards` · `cashback-rules` · `swaps` · `trades` · `verifications` · `send` |
| Websites | `domains` · `app-installations` · `experiments` |
| Ads | `ad-campaigns` · `ad-groups` · `ads` · `audiences` · `ad-pixels` |
| Analytics | `stats` · `people` · `events` · `exports` · `economic-intelligence` |
| Communication | `livestreams` · `notifications` · `email-addresses` |
| Partners | `partners` · `partner-referral-requests` |
| Workforce | `bounties` · `bounty-submissions` |
| Media | `files` · `media` |
| Developer | `api-keys` · `webhooks` · `permissions` · `api-logs` · `report-feedback` · `ask-question` |
| Auth | `auth` · `login` · `logout` |
| Integrations | `completions` · `mcp` · `skills` |

See [Commands](https://docs.whop.com/cli/commands.md) for every command, or run `whop <group> --help`.

## Everyday commands

```bash theme={null}
whop --help                  # all commands
whop products list           # what you're selling
whop variants create --help  # options for any command
whop checkout-configurations create --variant_id plan_xxx   # shareable checkout link
whop stats time-series list  # the metric catalog
whop stats time-series get net_revenue --from 2026-01-01 --to 2026-01-31
whop accounts get jordan     # retrieve a business by route
```

Most groups follow the same shape: `whop <resource> list|get|create|update`. Every command takes `--format json` for structured output. Account-scoped commands use the selected business by default, and `--account_id` chooses a different one.

## Deploy an app

```bash theme={null}
whop apps deploy
```

Builds, type-checks, uploads, and promotes the linked project to production. Pass `--preview` to upload without promoting, then ship it later with `whop apps builds promote <build_id>`. Promoting an older build is also how you roll back.

After a deploy, read your app's server logs:

```bash theme={null}
whop apps logs app_xxx               # recent logs for your app
whop apps logs app_xxx --level error --query "checkout"
```

## Non-interactive use

For CI, scripts, or headless agents, set `WHOP_API_KEY` instead of logging in. Create a key under **Developer** in your [dashboard](https://whop.com/dashboard), and see [Auth & API keys](https://docs.whop.com/developer/guides/auth-scoping.md) before giving one to an agent.

```bash theme={null}
WHOP_API_KEY=whop_xxx whop products list --format json
```

Standalone installs on macOS and Linux update themselves in the background. Set `WHOP_CLI_AUTO_UPDATE=0` to only get a notice. Otherwise, an interactive terminal tells you when a newer release is out, and `whop --update` installs it. Homebrew installs update with `brew upgrade whop`.

## Next steps

> [card] [Commands](https://docs.whop.com/cli/commands.md)
>
> Every command group, with descriptions from the CLI itself.

> [card] [Agent mode](https://docs.whop.com/cli/agent-mode.md)
>
> Skills sync and the machine-readable flags.

> [card] [Build with the CLI and Claude](https://docs.whop.com/developer/guides/cli-with-claude.md)
>
> Scaffold and ship a Whop app with an agent in the loop.

> [card] [Auth & API keys](https://docs.whop.com/developer/guides/auth-scoping.md)
>
> Scope the credential you sign the CLI in with.


## Related topics

- [Whop CLI Commands](https://docs.whop.com/cli/commands.md)
- [Whop CLI Agent Mode](https://docs.whop.com/cli/agent-mode.md)
- [Build a Website with the CLI](https://docs.whop.com/developer/websites/quickstart.md)
