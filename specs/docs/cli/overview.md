> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Whop CLI

> Install the Whop CLI, sign in, and manage your whole business from the terminal: 51 command groups across commerce, money, ads, and apps.

The Whop CLI (`whop`) puts the Current API in your terminal: deploy apps, create products, set pricing, send payouts, run ads, and pull stats. Every command also works non-interactively for scripts and AI agents.

<Warning>
  The CLI has no sandbox, test, or dry-run mode. Every command runs in production, and many can create real resources or move real money. Commands that mutate state ask for confirmation.
</Warning>

## Install

<CodeGroup>
  ```bash macOS / Linux theme={null}
  curl -fsSL https://whop.com/install.sh | sh
  ```

  ```bash Homebrew theme={null}
  brew install whopio/tap/whop
  ```

  ```bash npm theme={null}
  npm install -g @whop/cli
  ```
</CodeGroup>

<Note>
  Prebuilt binaries cover macOS and Linux. On other platforms, install via npm (requires Node.js 22 or later).
</Note>

Check the install and see the API version it speaks:

```bash theme={null}
whop --version
```

## Sign in

```bash theme={null}
whop login
```

`whop login --method oauth` opens a browser. `whop login --method api-key --apiKey whop_xxx` is non-interactive. Profiles let you keep several identities: `whop auth list`, `whop auth switch`, and `whop auth status` manage them. Then pick the business the CLI acts on:

```bash theme={null}
whop quickstart
```

## What's here

51 command groups, organized the way `whop --help` groups them:

| Category       | Groups                                                                                                                                                          |
| -------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Get started    | `quickstart` · `apps` · `upgrade`                                                                                                                               |
| Commerce       | `products` · `plans` · `promo-codes` · `memberships` · `checkout-configurations` · `payment-method-domains` · `shipments`                                       |
| Core resources | `accounts` · `users` · `team-members` · `members` · `webhooks` · `stats` · `verifications` · `exports`                                                          |
| Money          | `payments` · `setup-intents` · `ledgers` · `payouts` · `cards` · `transfers` · `disputes` · `dispute-alerts` · `deposits` · `swaps` · `resolution-center-cases` |
| Ads            | `ads` · `ad-campaigns` · `ad-groups` · `audiences`                                                                                                              |
| Tracking       | `people` · `events` · `recommended-actions`                                                                                                                     |
| Workforce      | `bounties` · `bounty-submissions`                                                                                                                               |
| Partners       | `partners`                                                                                                                                                      |
| Media          | `media`                                                                                                                                                         |
| Identity       | `social-accounts`                                                                                                                                               |
| Developer      | `app-builds` · `api-keys` · `permissions` · `files`                                                                                                             |
| Notifications  | `notifications`                                                                                                                                                 |
| Auth           | `auth` · `login` · `logout`                                                                                                                                     |
| Integrations   | `skills` · `completions`                                                                                                                                        |

See [Commands](/cli/commands) for every command, or run `whop <group> --help`.

## Everyday commands

```bash theme={null}
whop --help                  # all commands
whop products list           # what you're selling
whop plans create --help     # options for any command
whop checkout-configurations create --plan_id plan_xxx   # shareable checkout link
whop stats list              # the metric catalog
whop stats get net_revenue --from 2026-01-01 --to 2026-01-31
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
whop apps logs               # recent logs for the linked app
whop apps logs --level error --query "checkout"
```

## Non-interactive use

For CI, scripts, or headless agents, set `WHOP_API_KEY` instead of logging in. Create a key under **Developer** in your [dashboard](https://whop.com/dashboard), and see [Auth & API keys](/developer/guides/auth-scoping) before giving one to an agent.

```bash theme={null}
WHOP_API_KEY=whop_xxx whop products list --format json
```

The CLI keeps itself up to date in the background. `whop upgrade` updates immediately.

## Next steps

<Columns cols={2}>
  <Card title="Commands" icon="terminal" href="/cli/commands">
    Every command group, with descriptions from the CLI itself.
  </Card>

  <Card title="Agent mode" icon="robot" href="/cli/agent-mode">
    Skills sync and the machine-readable flags.
  </Card>

  <Card title="Build with the CLI and Claude" icon="wand-magic-sparkles" href="/developer/guides/cli-with-claude">
    Scaffold and ship a Whop app with an agent in the loop.
  </Card>

  <Card title="Auth & API keys" icon="key" href="/developer/guides/auth-scoping">
    Scope the credential you sign the CLI in with.
  </Card>
</Columns>
