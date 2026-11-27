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

# Chat Authentication

> Authenticate users for embedded chat with account-scoped tokens or OAuth

> [card] [Whop's credential types](https://docs.whop.com/developer/guides/auth-scoping.md)
>
> See Auth & API keys for all four Whop credential types, when to use each, and how to scope a token down to the actions it needs.

Before using the chat SDK, you need to authenticate users. Pick one of two approaches:

| Approach | Best for |
| - | - |
| **[Account-scoped user tokens](#account-scoped-user-tokens)** | Apps that already have their own users. Your server mints Whop tokens for them without a sign-in step. |
| **[OAuth](https://docs.whop.com/developer/guides/chat/oauth.md)** | Apps that want users to sign in to Whop themselves via a sign-in page or webview. |

## Account-scoped user tokens

Use this approach if **your app already has its own users** (via your own auth / session system). You keep your existing login flow and mint a Whop token for whichever user is currently signed in — no extra sign-in step.

Your server exchanges your Whop API key for a short-lived user token on demand and returns it to the client. You decide which user the token is for.

### 1. Gather your credentials

You'll need three things:

1. **API key** — create one at the [Whop Developer Dashboard](https://whop.com/dashboard/developer). Treat it like a password and keep it on your server only.
2. **Account ID**. Find it in your dashboard URL: `whop.com/dashboard/biz_XXXXXXXXX/`.
3. **User ID** — the user you want to authenticate. In production, derive this from your own auth / session system.

> [!NOTE]
>
> For embedded chat, both `account_id` and `user_id` are required together when minting a token.

### 2. Create a token endpoint on your server

Add an endpoint that calls `POST https://api.whop.com/api/v1/access_tokens` with your API key and returns the resulting token to the client.

> [tab] Next.js
>
> ```typescript app/api/chat/token/route.ts theme={null}
> import { NextResponse } from "next/server";
> import { WhopClient } from "@whop/sdk";
>
> const client = new WhopClient({ token: process.env.WHOP_API_KEY });
>
> export async function POST() {
>   // Derive these from your own auth / session in production.
>   const user_id = "user_XXXXXXXXXXXX";
>   const account_id = "biz_XXXXXXXXXXXXX";
>
>   const { token } = await client.accessTokens.create({
>     account_id,
>     user_id,
>     scoped_actions: [
>       "chat:message:create",
>       "chat:read",
>       "dms:read",
>       "dms:message:manage",
>       "dms:channel:manage",
>       "support_chat:read",
>       "support_chat:message:create",
>     ],
>   });
>
>   return NextResponse.json({ token });
> }
> ```

> [tab] Express
>
> ```typescript server.ts theme={null}
> import express from "express";
> import { WhopClient } from "@whop/sdk";
>
> const app = express();
> app.use(express.json());
>
> const client = new WhopClient({ token: process.env.WHOP_API_KEY });
>
> app.post("/api/chat/token", async (_req, res) => {
>   // Derive these from your own auth / session in production.
>   const user_id = "user_XXXXXXXXXXXX";
>   const account_id = "biz_XXXXXXXXXXXXX";
>
>   const { token } = await client.accessTokens.create({
>     account_id,
>     user_id,
>     scoped_actions: [
>       "chat:message:create",
>       "chat:read",
>       "dms:read",
>       "dms:message:manage",
>       "dms:channel:manage",
>       "support_chat:read",
>       "support_chat:message:create",
>     ],
>   });
>
>   res.json({ token });
> });
>
> app.listen(3000);
> ```

> [tab] cURL
>
> ```bash theme={null}
> curl -X POST https://api.whop.com/api/v1/access_tokens \
>   -H "Authorization: Bearer $WHOP_API_KEY" \
>   -H "Content-Type: application/json" \
>   -d '{
>     "account_id": "biz_XXXXXXXXXXXXX",
>     "user_id": "user_XXXXXXXXXXXX",
>     "scoped_actions": [
>       "chat:message:create",
>       "chat:read",
>       "dms:read",
>       "dms:message:manage",
>       "dms:channel:manage",
>       "support_chat:read",
>       "support_chat:message:create"
>     ]
>   }'
> ```

### 3. Fetch the token from your app

On web, fetch a token from your endpoint and pass it as `accessToken`. On iOS, give the SDK a token provider and it fetches a new token whenever it needs one.

> [tab] React
>
> ```tsx theme={null}
> "use client";
>
> import { useEffect, useState } from "react";
> import { WhopElements, Chat, ChatElement } from "@whop/elements-react";
> import { loadWhop } from "@whop/elements";
>
> const REFRESH_INTERVAL = 45 * 60 * 1000;
>
> async function fetchToken() {
>   const res = await fetch("https://your-server.com/api/chat/token", { method: "POST" });
>   const data = await res.json();
>   return data.token;
> }
>
> export function ChatView() {
>   const [accessToken, setAccessToken] = useState<string | null>(null);
>
>   useEffect(() => {
>     let ignore = false;
>
>     async function refreshToken() {
>       const token = await fetchToken();
>       if (!ignore) {
>         setAccessToken(token);
>       }
>     }
>
>     refreshToken();
>     const interval = setInterval(refreshToken, REFRESH_INTERVAL);
>
>     return () => {
>       ignore = true;
>       clearInterval(interval);
>     };
>   }, []);
>
>   if (!accessToken) return null;
>
>   return (
>     <WhopElements elements={loadWhop()}>
>       <Chat accessToken={accessToken} channelId="chat_feed_XXXXXXXXXXXXXX">
>         <ChatElement style={{ height: "100dvh", width: "100%" }} />
>       </Chat>
>     </WhopElements>
>   );
> }
> ```

> [tab] Vanilla JS
>
> ```html theme={null}
> <div id="chat-chat" style="height: 100vh"></div>
>
> <script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>
> <script type="module">
>   const REFRESH_INTERVAL = 45 * 60 * 1000;
>
>   async function fetchToken() {
>     const res = await fetch("https://your-server.com/api/chat/token", { method: "POST" });
>     const data = await res.json();
>     return data.token;
>   }
>
>   const chat = window.WhopElements().chat.create({
>     accessToken: await fetchToken(),
>     channelId: "chat_feed_XXXXXXXXXXXXXX",
>   });
>
>   chat.create("chat").mount("#chat-chat");
>
>   setInterval(async () => {
>     chat.update({ accessToken: await fetchToken() });
>   }, REFRESH_INTERVAL);
> </script>
> ```

> [tab] Swift
>
> ```swift theme={null}
> import WhopElements
>
> class WhopAPITokenProvider: WhopTokenProvider {
>     private let serverURL = URL(string: "https://your-server.com")!
>
>     func getToken() async -> WhopTokenResponse {
>         do {
>             var req = URLRequest(url: serverURL.appendingPathComponent("api/chat/token"))
>             req.httpMethod = "POST"
>
>             let (data, _) = try await URLSession.shared.data(for: req)
>             let json = try JSONSerialization.jsonObject(with: data) as! [String: Any]
>             return WhopTokenResponse(accessToken: json["token"] as? String ?? "")
>         } catch {
>             return WhopTokenResponse(accessToken: "")
>         }
>     }
> }
> ```
>
> Pass the token provider to the SDK on app launch:
>
> ```swift theme={null}
> .task {
>     await WhopSDK.configure(tokenProvider: WhopAPITokenProvider())
> }
> ```

### Required scopes

Request these scopes when creating the token:

| Scope | Purpose |
| - | - |
| `chat:message:create`, `chat:read` | Channels |
| `dms:read`, `dms:message:manage`, `dms:channel:manage` | Direct messages |
| `support_chat:read`, `support_chat:message:create` | Support chats |

> [!NOTE]
>
> If your app owns the user accounts and mints account-scoped chat tokens, sync those users before passing their `user_id` here. User syncing isn't part of OAuth.

> [card] [Sync your users](https://docs.whop.com/developer/guides/chat/sync-users.md)
>
> Create or map Whop users before minting account-scoped chat tokens.

## OAuth

OAuth lives on its own page so this guide can stay focused on account-scoped user tokens.

Use OAuth when your app wants users to sign in to Whop themselves through a sign-in page or iOS webview. This is the right choice when your app needs access to a user-owned Whop account, or when you don't want your backend to mint Whop user tokens.

> [card] [OAuth](https://docs.whop.com/developer/guides/chat/oauth.md)
>
> Set up OAuth scopes, redirect URIs, web token endpoints, and SDK-managed OAuth on iOS.


## Related topics

- [Support Chats](https://docs.whop.com/developer/guides/chat/support-chats.md)
- [Chat Quickstart](https://docs.whop.com/developer/guides/chat/quickstart.md)
- [Embedded Chat Channels](https://docs.whop.com/developer/guides/chat/channels.md)
