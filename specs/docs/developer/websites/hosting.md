> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Hosting

> What Whop runs when someone visits your site: builds and rollbacks, app secrets, automatic API authentication, and server logs.

Whop serves your site straight from your uploaded build. There is no server to configure and no deploy target to point at.

## Builds and versions

Every deploy produces a build, and exactly one is production at a time.

```bash theme={null}
whop apps deploy                            # build, upload, promote
whop apps deploy --preview                  # upload only
whop apps builds promote abld_xxxxxxxx      # make a build live
```

Promoting an older build is the rollback — there's no separate revert command. In the dashboard, use the **Versions** tab.

The Vite plugin packs your build into the uploaded archive: assets from `dist/client` are served directly, and `dist/server` runs your server code.

## Secrets

App secrets are encrypted bindings that become environment variables at runtime.

```bash theme={null}
whop apps secrets list
whop apps secrets set --secret MAIL_API_KEY=mail-key-123
whop apps secrets unset --key MAIL_API_KEY
```

Run these inside a linked project, or pass `--app app_xxxxxxxx`. `whop apps dev` injects the same secrets locally, except names that control the local runtime. An environment variable you export yourself always wins.

The runtime also sets:

| Binding           | Purpose                                                                                   |
| ----------------- | ----------------------------------------------------------------------------------------- |
| `APP_ID`          | The app being served                                                                      |
| `BUILD_ID`        | The build currently promoted                                                              |
| `WHOP_API_ORIGIN` | The Whop API origin to build requests against                                             |
| `WHOP_ACCOUNT_ID` | The `biz_` id of the account that owns the app, for scoping API reads to your own catalog |
| `ASSETS`          | Serves your static assets                                                                 |
| `REALTIME`        | Backs realtime connections                                                                |

<Note>
  These names are reserved — a secret of the same name is ignored.
</Note>

## Call the Whop API

Server-side `fetch` calls pass through an outbound proxy that attaches the app's API key to Whop API requests:

```typescript src/routes/api.account.ts theme={null}
const response = await fetch(`${process.env.WHOP_API_ORIGIN}/api/v1/accounts/me`);
const account = await response.json();
```

The key never reaches your code, so it can't be read, logged, or bundled. Requests elsewhere pass through untouched.

* **Only server-side calls are signed.** A browser `fetch` leaves the visitor's machine, not your server — route those through your own handler.
* **Send `x-whop-inject-key: none`** to leave a server-side request unauthenticated.

<Warning>
  The injected key belongs to the app's own business and can't move money. Payout and transfer **reads** are granted. Withdrawing funds, sending transfers, and managing payout destinations aren't, so those return a scope error until you supply your own key. It also authenticates as your business, never as the visitor — for visitor identity, use [OAuth](/developer/guides/oauth).
</Warning>

## Logs

The runtime captures every `console.log`, uncaught exception, and failed request for 7 days.

```bash theme={null}
whop apps logs app_xxxxxxxx
whop apps logs app_xxxxxxxx --level error
whop apps logs app_xxxxxxxx --query "checkout"
```

Use `--created_after` and `--created_before` with ISO 8601 timestamps to select a window. Client-side JavaScript errors aren't here — they're in the visitor's browser console.

## Next steps

<CardGroup cols={2}>
  <Card title="Track visitors" href="/developer/websites/tracking">
    The pixel is already installed — add your own events.
  </Card>

  <Card title="Blueprints" href="/developer/websites/blueprints">
    Start from a working business instead of an empty project.
  </Card>

  <Card title="Accept payments" href="/developer/guides/accept-payments">
    Sell directly from the site you're hosting.
  </Card>

  <Card title="CLI reference" href="/developer/cli">
    Every `whop apps` command.
  </Card>
</CardGroup>
