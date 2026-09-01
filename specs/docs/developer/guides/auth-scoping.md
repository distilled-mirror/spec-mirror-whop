> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Auth & API Keys

> The four Whop credential types, when to use each, and how to scope a credential down to the actions it actually needs.

Whop has four credential types. Which one you need depends on whose data you are reaching and where the code runs. Pick from this table, then scope the credential down before it goes near production.

| Credential                    | Acts as                      | Lives                                      | Use when                                                                                      |
| ----------------------------- | ---------------------------- | ------------------------------------------ | --------------------------------------------------------------------------------------------- |
| **API key**                   | Your account, or your app    | Your server                                | Your backend reaches your own account, or every account that installed your app               |
| **Account-scoped user token** | One user, inside one account | Minted on your server, used in your client | Your product already has its own users and you need them to reach Whop without a Whop sign-in |
| **iframe user token**         | The user viewing your app    | Sent by Whop, verified by you              | Your app renders inside Whop and you need to know who is looking at it                        |
| **OAuth token**               | A user who signed in to Whop | Your server, after the OAuth flow          | You want users to sign in with Whop and grant your app access to their account                |

## API keys

An Account API key authenticates your server as one account. An App API key authenticates your app across every account that installed it. Both are secrets: keep them server-side.

See [Choose your key type](/developer/quickstart#choose-your-key-type) to create either.

## Account-scoped user tokens

An account-scoped user token acts as one specific user inside one specific account. Your server mints it with your API key and hands it to your client, so your users reach Whop without ever signing in to Whop.

Create one with `POST /api/v1/access_tokens`, passing `company_id`, `user_id`, and the actions you want to allow:

```bash theme={null}
curl https://api.whop.com/api/v1/access_tokens \
  -H "Authorization: Bearer $WHOP_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "company_id": "biz_XXXXXXXX",
    "user_id": "user_XXXXXXXX",
    "scoped_actions": ["chat:read", "chat:message:create"]
  }'
```

`company_id` starts with `biz_` and `user_id` starts with `user_`. Your API key must have permission to reach both.

Tokens expire in **one hour** by default. Set `expires_at` to change that, up to a **three-hour** maximum. Mint them on demand rather than caching them.

<Note>
  See [Create access token](/api-reference/access-tokens/create-access-token).
</Note>

### Scoping a token

`scoped_actions` is the list of things the token may do. Each entry is a colon-separated action string.

<Warning>
  Leave `scoped_actions` empty or omit it, and the token **inherits every permission the minting credential has**. A token handed to a browser then carries your API key's full reach. Always pass an explicit list.
</Warning>

An action must be a subset of the minting credential's own permissions. You can't grant what your API key doesn't have.

These actions appear across Whop's guides today:

| Action                        | Grants                          |
| ----------------------------- | ------------------------------- |
| `chat:read`                   | Read chat channels and messages |
| `chat:message:create`         | Send chat messages              |
| `dms:read`                    | Read direct messages            |
| `dms:message:manage`          | Send and manage direct messages |
| `dms:channel:manage`          | Create and manage DM channels   |
| `support_chat:read`           | Read support chats              |
| `support_chat:message:create` | Send support chat messages      |
| `company:balance:read`        | Read an account's balance       |

The API doesn't publish a fixed list, so treat this as the documented set rather than the complete one. The authoritative bound is always the minting credential's own permissions.

## `iframe` user tokens

When your app renders inside Whop, every same-origin request carries a short-lived JWT in the `x-whop-user-token` header. You verify it with the SDK to learn who is asking, then check what they may see.

You never mint this one. Whop sends it, and you validate it. See [Authentication](/developer/guides/authentication).

## OAuth tokens

An OAuth token represents a user who signed in to Whop and approved your app for a set of scopes. Unlike an API key, its reach is bounded by what that individual user can do.

Use it for Sign in with Whop, and for acting on a user's behalf against their own account. See [OAuth](/developer/guides/oauth).

## Scoping checklist

Before a credential reaches production:

* Pass explicit `scoped_actions` on every minted token. Never rely on inheritance.
* Give API keys a custom permission set once you know which endpoints you call, rather than leaving them on Admin.
* Keep API keys server-side. Only user tokens belong in a client.
* Mint short-lived tokens on demand instead of caching long-lived ones.
* Use separate credentials per environment, and check which one you are holding before running anything that moves money.

## Next steps

<Columns cols={2}>
  <Card title="Choose your key type" icon="key" href="/developer/quickstart#choose-your-key-type">
    Create an Account or App API key.
  </Card>

  <Card title="Core Concepts" icon="book" href="/developer/concepts">
    How accounts, users, and permissions relate.
  </Card>

  <Card title="OAuth" icon="user-check" href="/developer/guides/oauth">
    The full OAuth 2.1 and PKCE flow.
  </Card>

  <Card title="Test in the sandbox" icon="flask" href="/developer/guides/sandbox">
    Try credentials against test data first.
  </Card>
</Columns>
