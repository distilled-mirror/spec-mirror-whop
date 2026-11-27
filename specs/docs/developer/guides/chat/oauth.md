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

# Chat OAuth

> Authenticate embedded chat users with Whop OAuth and SDK-managed sign-in.

With OAuth, users sign in to Whop themselves through a sign-in page or an iOS webview. Your app receives Whop user tokens that can authenticate embedded chat and act on behalf of the signed-in user.

Use OAuth when your app needs access to the user's Whop account, or when you don't want to mint account-scoped user tokens from your own backend.

> [card] [Full OAuth guide](https://docs.whop.com/developer/guides/oauth.md)
>
> See the general OAuth guide for the complete OAuth 2.1 + PKCE flow, token exchange, refresh, and revocation details.

## Setup

> [!WARNING]
>
> The `App` object returned by `apps.create` in the current `@whop/sdk` does not expose a `client_secret` field. Copy the client secret from the Dashboard tab instead.

> [tab] API
>
> Use the [Create App](https://docs.whop.com/api-reference/apps/create-app.md) endpoint to create an OAuth app programmatically:
>
> ```typescript theme={null}
> import { WhopClient } from "@whop/sdk";
>
> const client = new WhopClient({ token: process.env.WHOP_API_KEY });
>
> const app = await client.apps.create({
>   name: "My App",
>   redirect_uris: [
>     "http://localhost:3000/api/auth/callback/whop",
>     "https://myapp.com/api/auth/callback/whop"
>   ],
> });
>
> console.log(app.id);            // App ID
> console.log(app.client_secret); // Client Secret
> ```
>
> Update redirect URIs later with the [Update App](https://docs.whop.com/api-reference/apps/update-app.md) endpoint.

> [tab] Dashboard
>
> **1. Create or select your app**
>
> Register your app in the [Whop Dashboard](https://whop.com/dashboard/) > Developer to get an app ID.
>
> **2. Add a redirect URL**
>
> In the app you created, open OAuth, and add a redirect URL.
>
> * For React or vanilla JavaScript apps, use your app's callback URL, such as `https://yourapp.com/oauth/callback`.
> * For Swift apps, use your bundle ID, such as `com.yourapp.bundle://oauth/callback`, or configure a custom redirect URI.
>
> **3. Select scopes**
>
> On the same page, copy the required scopes from **View available scopes**.

## Required scopes

Your OAuth configuration should include these scopes:

| Scope | Purpose |
| - | - |
| `openid`, `profile`, `email` | Basic profile info |
| `chat:message:create`, `chat:read` | Channels |
| `dms:read`, `dms:message:manage`, `dms:channel:manage` | Direct messages |
| `support_chat:read`, `support_chat:message:create` | Support chats |

## Web token endpoint

Your server needs a token endpoint that returns a valid OAuth token with the required scopes. On web, fetch a token from your endpoint and pass it as `accessToken`.

```typescript theme={null}
async function getToken() {
  const response = await fetch("/api/token");
  const data = await response.json();
  return data.token;
}
```

See the [OAuth guide](https://docs.whop.com/developer/guides/oauth.md) for implementing the server-side token exchange.

## Software development kit-managed OAuth on iOS

Call `configureWithOAuth` on app launch. The SDK handles the entire flow: showing a sign-in webview, obtaining tokens, and refreshing them automatically.

```swift theme={null}
.task {
    await WhopSDK.configureWithOAuth(
        appId: "app_XXXXXXXXXXXXXX",
        scopes: [
            "openid", "profile", "email",
            "chat:message:create", "chat:read",
            "dms:read", "dms:message:manage", "dms:channel:manage",
            "support_chat:read", "support_chat:message:create",
        ]
    )
}
```

By default, the SDK uses your bundle identifier for the redirect URI (`com.yourapp.bundle://oauth/callback`). You can customize this:

```swift theme={null}
await WhopSDK.configureWithOAuth(
    appId: "app_XXXXXXXXXXXXXX",
    redirectUri: "myapp://auth/callback",
    scopes: [
        "openid", "profile", "email",
        "chat:message:create", "chat:read",
        "dms:read", "dms:message:manage", "dms:channel:manage",
        "support_chat:read", "support_chat:message:create",
    ]
)
```

Make sure the redirect URI matches what you configured in the [Whop Dashboard](https://whop.com/dashboard/) > Developer > App > OAuth.

When a user navigates to a chat view, the SDK automatically triggers the OAuth flow if they're not already authenticated. You can also trigger sign-in and sign-out manually:

```swift theme={null}
// Sign in
try await WhopSDK.signIn()

// Sign out
WhopSDK.signOut()
```

### Tracking authentication state

Use the `.whopAuthState` modifier to reactively track whether the user has signed in:

```swift theme={null}
struct ChatView: View {
    @State private var isAuthenticated = false

    var body: some View {
        VStack {
            if isAuthenticated {
                Text("Signed In")
            } else {
                Text("Not Signed In")
            }
        }
        .whopAuthState($isAuthenticated)
    }
}
```

### Pre-filling tokens

If you already have the user's Whop tokens from another source, you can pre-fill them to skip the sign-in webview on first launch. Sources include a web OAuth flow or tokens synced from your backend. This is optional and only relevant if your users have already authenticated with Whop elsewhere.

```swift theme={null}
let tokens = await myBackend.getWhopTokens(for: currentUser)

try await WhopSDK.preSignIn(
    accessToken: tokens.accessToken,
    refreshToken: tokens.refreshToken
)
```

The SDK extracts the token expiration from the JSON Web Token (JWT) and handles refresh automatically. If the SDK can't refresh expired pre-filled tokens, the normal OAuth flow starts.

## Next steps

> [card] [Authentication](https://docs.whop.com/developer/guides/chat/authentication.md)
>
> Compare OAuth with account-scoped tokens for embedded chat.

> [card] [General OAuth guide](https://docs.whop.com/developer/guides/oauth.md)
>
> Implement the full OAuth flow, token exchange, refresh, and revocation.

> [card] [Chat element](https://docs.whop.com/developer/guides/chat/chat-element.md)
>
> Learn the props, events, styles, and deeplinking options for the chat UI.

> [card] [Embedded chat quickstart](https://docs.whop.com/developer/guides/chat/quickstart.md)
>
> Render your first embedded chat experience.


## Related topics

- [Sync Users to Embedded Chat](https://docs.whop.com/developer/guides/chat/sync-users.md)
- [Chat Authentication](https://docs.whop.com/developer/guides/chat/authentication.md)
- [OAuth](https://docs.whop.com/developer/guides/oauth.md)
