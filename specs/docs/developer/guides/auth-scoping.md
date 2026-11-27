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

# Auth & API Keys

> The five Whop credential types, when to use each, and how to scope a credential down to the actions it actually needs.

Whop has five credential types. Which one you need depends on whose data you are reaching and where the code runs. Pick from this table, then scope the credential down before it goes near production.

| Credential | Acts as | Lives | Use when |
| - | - | - | - |
| **API key** | Your account, or your app | Your server | Your backend reaches your own account, or every account that installed your app |
| **Account-scoped user token** | One user, inside one account | Minted on your server, used in your client | Your product already has its own users and you need them to reach Whop without a Whop sign-in |
| **Account access token** | Your account, with narrowed permissions | Minted on your server, used in your client | A client needs short-lived access to your account without holding your API key |
| **iframe user token** | The user viewing your app | Sent by Whop, verified by you | Your app renders inside Whop and you need to know who is looking at it |
| **OAuth token** | A user who signed in to Whop | Your server, after the OAuth flow | You want users to sign in with Whop and grant your app access to their account |

## API keys

An Account API key authenticates your server as one account. An App API key authenticates your app across every account that installed it. Both are secrets: keep them server-side.

### Create an Account API key

Use an Account API key when your server acts for your own account and its [subaccounts](https://docs.whop.com/developer/platforms/enroll-subaccounts.md). In the dashboard's Developer page, open **Account API Keys** and select **Create**. The [Quickstart](https://docs.whop.com/developer/quickstart.md) walks through it in the sandbox.

### Create an App API key

Use an App API key when you build an app and need to reach data on the accounts that installed it.

1. In the dashboard, open **Apps**.
2. Select **Create app** and give your app a name. You can change the name later.
3. Your key is the hidden value after `WHOP_API_KEY` in the **Environment variables** section. Use the reveal button to show it, then store it somewhere safe.

## Account-scoped user tokens

An account-scoped user token acts as one specific user inside one specific account. Your server mints it with your API key and hands it to your client, so your users reach Whop without ever signing in to Whop.

Create one with `POST /api/v1/access_tokens`, passing `account_id`, `user_id`, and the actions you want to allow:

```bash theme={null}
curl https://api.whop.com/api/v1/access_tokens \
  -H "Authorization: Bearer $WHOP_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "account_id": "biz_XXXXXXXX",
    "user_id": "user_XXXXXXXX",
    "scoped_actions": ["chat:read", "chat:message:create"]
  }'
```

`account_id` starts with `biz_` and `user_id` starts with `user_`. Your API key must have permission to reach both.

Tokens expire in **one hour** by default. Set `expires_at` to change that, up to a **three-hour** maximum. Mint them on demand rather than caching them.

> [!NOTE]
>
> See [Create access token](https://docs.whop.com/api-reference/legacy/access-tokens/create-access-token.md).

### Scoping a token

`scoped_actions` is the list of things the token may do. Each entry is a colon-separated action string.

> [!WARNING]
>
> Leave `scoped_actions` empty or omit it, and the token **inherits every permission the minting credential has**. A token handed to a browser then carries your API key's full reach. Always pass an explicit list.

An action must be a subset of the minting credential's own permissions. You can't grant what your API key doesn't have.

These actions appear across Whop's guides today:

| Action | Grants |
| - | - |
| `chat:read` | Read chat channels and messages |
| `chat:message:create` | Send chat messages |
| `dms:read` | Read direct messages |
| `dms:message:manage` | Send and manage direct messages |
| `dms:channel:manage` | Create and manage DM channels |
| `support_chat:read` | Read support chats |
| `support_chat:message:create` | Send support chat messages |
| `company:balance:read` | Read an account's balance |

The API doesn't publish a fixed list, so treat this as the documented set rather than the complete one. The authoritative bound is always the minting credential's own permissions.

## Account access tokens

An account access token acts as the account itself, with no user attached. Create one with `POST /api/v1/access_tokens`, passing only `account_id` and the `scoped_actions` to allow. It expires on the same schedule as an account-scoped user token.

Use it when a client needs your account's access for a short time, such as an embedded component, and no specific user is involved. Endpoints that act as a user, like notification or preference endpoints, reject it.

## `iframe` user tokens

When your app renders inside Whop, every same-origin request carries a short-lived JWT in the `x-whop-user-token` header. You verify its signature to learn who is asking, then check what they may see. The Python and Ruby SDKs ship a helper for this. The TypeScript SDK doesn't, so verify the JWT yourself.

You never mint this one. Whop sends it, and you validate it. See [Authentication](https://docs.whop.com/developer/guides/authentication.md).

## OAuth tokens

An OAuth token represents a user who signed in to Whop and approved your app for a set of scopes. Unlike an API key, its reach is bounded by what that individual user can do.

Use it for Sign in with Whop, and for acting on a user's behalf against their own account. See [OAuth](https://docs.whop.com/developer/guides/oauth.md).

## Scoping checklist

Before a credential reaches production:

* Pass explicit `scoped_actions` on every minted token. Never rely on inheritance.
* Give API keys a custom permission set once you know which endpoints you call, rather than leaving them on Admin.
* Keep API keys server-side. Only user tokens belong in a client.
* Mint short-lived tokens on demand instead of caching long-lived ones.
* Use separate credentials per environment, and check which one you are holding before running anything that moves money.

## Next steps

> [card] [Quickstart](https://docs.whop.com/developer/quickstart.md)
>
> Create a sandbox key and make your first call.

> [card] [Core Concepts](https://docs.whop.com/developer/concepts.md)
>
> How accounts, users, and permissions relate.

> [card] [OAuth](https://docs.whop.com/developer/guides/oauth.md)
>
> The full OAuth 2.1 and PKCE flow.

> [card] [Test in the sandbox](https://docs.whop.com/developer/guides/sandbox.md)
>
> Try credentials against test data first.


## Related topics

- [Whop API Quickstart](https://docs.whop.com/developer/quickstart.md)
- [Whop API Core Concepts](https://docs.whop.com/developer/concepts.md)
- [Chat Authentication](https://docs.whop.com/developer/guides/chat/authentication.md)
