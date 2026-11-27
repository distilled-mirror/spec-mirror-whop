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

# App Installations

> Know when an account installs or removes your app, and link each installation to the record it belongs to in your own system.

An app installation is one install of your app on one Whop account. Your app gets `app_installation.created` when an account installs it and `app_installation.deleted` when an account removes it. Each installation also stores two values that only your app can change. `external_id` links the installation to a record in your system, and `metadata` holds small values such as a sync cursor.

## A storefront that takes payments through Whop

This guide follows a made-up app. Market Lane is an online store builder, and its Whop app lets a business take payments on its Market Lane store through Whop. Every store has a handle, such as `shine-time-austin`, and every checkout on Market Lane starts with that handle.

Market Lane has to know three things:

1. When a business installs the app, so it can connect the business's store.
2. Which Whop account takes the payment when a checkout starts on a store.
3. When a business removes the app, so its store stops offering Whop at checkout.

**1. Request the permissions your app needs**

In your app's [permissions](https://docs.whop.com/developer/guides/permissions.md), request `checkout_configuration:create` to create checkouts on the account, and `webhook_receive:payments` to hear when they're paid. The business approves both when it installs the app.

**2. Subscribe your app's webhook**

Create one webhook that belongs to your app, not to your account: set `resource_id` to your app's ID. It receives the installation events for every account that installs your app, plus the payment events those accounts granted.

```bash theme={null}
curl -X POST "https://api.whop.com/api/v1/webhooks" \
  -H "Authorization: Bearer $WHOP_DEVELOPER_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "url": "https://marketlane.example/webhooks/whop",
    "resource_id": "app_XXXXXXXX",
    "events": ["app_installation.created", "app_installation.deleted", "payment.succeeded"]
  }'
```

The key needs `developer:manage_webhook` on the account that owns the app. Store the `webhook_secret` from the response, and verify every delivery as described in [Webhooks](https://docs.whop.com/developer/guides/webhooks.md).

Only your app's own webhooks receive installation events. An account's webhooks never do, even for the apps installed on that account.

**3. Connect the store when a business installs the app**

`app_installation.created` carries the installation in `data`. The envelope's `account_id` is the business that installed the app:

```json theme={null}
{
  "type": "app_installation.created",
  "account_id": "biz_J8n2R5p9T1w4Kx",
  "data": {
    "id": "aag_T4k8M2p6W9r3Qx",
    "app": { "id": "app_XXXXXXXX", "name": "Market Lane" },
    "account": { "id": "biz_J8n2R5p9T1w4Kx", "title": "Shine Time Auto Detailing" },
    "external_id": null,
    "metadata": {},
    "requesting_reauthorization": false
  }
}
```

Market Lane asks the business which store to connect. When it picks `shine-time-austin`, Market Lane stores the handle on the installation as `external_id`:

```bash theme={null}
curl -X PATCH "https://api.whop.com/api/v1/app_installations/aag_T4k8M2p6W9r3Qx" \
  -H "Authorization: Bearer $WHOP_APP_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "external_id": "shine-time-austin",
    "metadata": { "connected_at": "2026-10-26T00:00:00Z" }
  }'
```

**4. Find the account when a checkout starts**

A shopper checks out on `shine-time-austin`. The request carries only the store handle, so Market Lane looks the installation up by `external_id`:

```bash theme={null}
curl "https://api.whop.com/api/v1/app_installations?external_id=shine-time-austin" \
  -H "Authorization: Bearer $WHOP_APP_API_KEY"
```

The result has at most one installation, and its `account.id` is the account that takes the payment. Market Lane creates a checkout on that account with `POST /checkout_configurations`, puts its own order ID in `metadata`, and sends the shopper to the `purchase_url`. Market Lane keeps no table of store handles and Whop accounts, because the installation is the mapping.

**5. Mark the order paid**

`payment.succeeded` arrives on the same webhook. Its `account_id` is the store's business, and the payment carries the `metadata` from the checkout, so Market Lane finds the order by its ID and marks it paid.

**6. Turn off Whop payments when the business removes the app**

`app_installation.deleted` arrives as soon as a business removes your app. Market Lane stops offering Whop at checkout on `shine-time-austin` right away, so no shopper reaches a checkout that would fail. The installation no longer appears in `GET /app_installations`, and your app can no longer act on that account.

**7. Reconnect on a reinstall**

If the business installs the app again, you get another `app_installation.created`. It's the same installation: the `id`, `created_at`, `external_id`, and `metadata` all come back. Market Lane sees `external_id: "shine-time-austin"` in the payload and turns Whop payments back on for that store, without asking the business to connect it again.

## Store your own ID on an installation

* **`external_id` is unique across your app's installations.** Setting a value that another installation already has returns a `409`, so two businesses can never claim the same store. Values are case-sensitive: `Shine-Time-Austin` and `shine-time-austin` are different.
* **`metadata` merges.** The keys you send are set, a key set to `null` is removed, keys you leave out don't change, and `{}` clears every key. Values are strings.
* **A removed installation keeps its `external_id`.** While a business has your app removed, no other installation of your app can take that value. If the business installs the app again, the value comes back with it.
* **Don't store secrets.** Your developer account can read `metadata` and `external_id`.

## Handle repeated events

* **Expect repeats.** A business can install, remove, and install your app again. Treat `app_installation.created` for an installation you already know as a reinstall.
* **Ignore duplicates by `webhook-id`.** Whop retries deliveries that don't get a `2xx` response, and a retry keeps the same `webhook-id`.

## Who can read installations

| Caller | Access |
| - | - |
| Your app's API key | Lists, retrieves, and updates your app's installations, including `metadata` and `external_id`. The key needs `developer:app_installation:read` to read and `developer:app_installation:update` to update. |
| Your developer account | Lists every installation of your app with `GET /app_installations?app_id=app_XXXXXXXX`. Needs `developer:basic:read`. |
| The account the app is installed on | Lists the apps installed on it. It always sees `metadata` and `external_id` as `null`. |

## Next steps

> [card] [Webhooks](https://docs.whop.com/developer/guides/webhooks.md)
>
> Verify signatures, handle retries, and see every event.

> [card] [App Installations API reference](https://docs.whop.com/api-reference/app-installations/app-installation)
>
> Every field and endpoint for app installations.

> [card] [Accept payments](https://docs.whop.com/developer/guides/accept-payments.md)
>
> Create checkouts and fulfill orders from `payment.succeeded`.

> [card] [Permissions](https://docs.whop.com/developer/guides/permissions.md)
>
> Declare the permissions your app requests at install.


## Related topics

- [App Installations API](https://docs.whop.com/api-reference/app-installations/app-installation.md)
- [List App Installations](https://docs.whop.com/api-reference/app-installations/list-app-installations.md)
- [Retrieve App Installation](https://docs.whop.com/api-reference/app-installations/retrieve-app-installation.md)
