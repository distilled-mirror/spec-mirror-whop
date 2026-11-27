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

# Chat Quickstart

> Add Whop chat to your web or iOS app, from creating an API key to rendering your first conversation.

1
Create an API key

Create an API key in the [Whop Developer Dashboard](https://whop.com/dashboard/developer) and save it on your server as `WHOP_API_KEY`.

This key lets you perform actions within the context of your account. Keep it on the server. Never put it in browser, mobile, or client-side code.

2
Install packages

Install the SDK in your backend:

> [tab] Next.js
>
> ```bash theme={null}
> pnpm add @whop/sdk
> ```

> [tab] Express
>
> ```bash theme={null}
> pnpm add @whop/sdk
> ```

> [tab] cURL
>
> You can also check [the API reference](https://docs.whop.com/developer/quickstart.md) if you prefer to make the requests yourself.

Install the elements packages in your frontend:

> [tab] React
>
> ```bash theme={null}
> pnpm add @whop/elements-react @whop/elements
> ```

> [tab] HTML
>
> ```html theme={null}
> <script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>
> ```

> [tab] iOS
>
> Add this Swift package in Xcode:
>
> ```text theme={null}
> https://github.com/whopio/whopsdk-elements-swift
> ```
>
> Chat supports photos, videos, and voice messages, so add these permissions to your app's `Info.plist`:
>
> ```xml Info.plist theme={null}
> <key>NSCameraUsageDescription</key>
> <string>Allow camera access to take photos and videos in chat</string>
> <key>NSMicrophoneUsageDescription</key>
> <string>Allow microphone access to record voice messages in chat</string>
> <key>NSPhotoLibraryUsageDescription</key>
> <string>Allow photo library access to share photos and videos in chat</string>
> ```

3
Get a user ID for your users

Enroll your own users as subaccounts before minting chat tokens. This creates a Whop account you can map back to your platform user.

> [tab] Next.js
>
> ```typescript theme={null}
> import { WhopClient } from "@whop/sdk";
>
> const client = new WhopClient({ token: process.env.WHOP_API_KEY });
>
> const account = await client.accounts.create({
>   email: "user@example.com",
>   title: "Dana Whitfield",
>   external_id: "user_12345", // your platform's user ID
> });
> ```

> [tab] Express
>
> ```typescript theme={null}
> import { WhopClient } from "@whop/sdk";
>
> const client = new WhopClient({ token: process.env.WHOP_API_KEY });
>
> const account = await client.accounts.create({
>   email: "user@example.com",
>   title: "Dana Whitfield",
>   external_id: "user_12345", // your platform's user ID
> });
> ```

> [tab] cURL
>
> ```bash theme={null}
> curl -X POST https://api.whop.com/api/v1/accounts \
>   -H "Authorization: Bearer $WHOP_API_KEY" \
>   -H "Content-Type: application/json" \
>   -d '{
>     "email": "user@example.com",
>     "title": "Dana Whitfield",
>     "external_id": "user_12345"
>   }'
> ```

Save the resulting Whop user ID (`owner_user.id` in the response) on your side and pass it as `user_id` when you create chat access tokens.

See [account-specific profiles](https://docs.whop.com/developer/guides/chat/sync-users.md#2-set-an-account-specific-name-and-profile-picture) for more details on how to update users avatars and display names.

4
Create a token endpoint on your server

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

5
Create or fetch a channel, DM, or support chat

All chat types use real-time messaging and can render in the Chat element. Pick the type based on your needs:

> [card] [Channels](https://docs.whop.com/developer/guides/chat/channels.md)
>
> A shared chat for public or large group conversations. Rooms with moderation, read-only options, and no fixed member cap.

> [card] [Direct messages](https://docs.whop.com/developer/guides/chat/direct-messages.md)
>
> A private conversation between users. They show in the channel list and can include up to 50 members.

> [card] [Support chat](https://docs.whop.com/developer/guides/chat/support-chats.md)
>
> A one-on-one support conversation between a user and your account. Customers see them like DMs. Admins manage them through a support inbox instead of the channel list.

Before rendering chat, get the channel ID you want to open.

* [**Channels**](https://docs.whop.com/developer/guides/chat/channels.md) IDs start with `chat_feed_`.
* [**DMs**](https://docs.whop.com/developer/guides/chat/direct-messages.md) and [**support chats**](https://docs.whop.com/developer/guides/chat/support-chats.md) IDs start with `feed_`.

6
Render chat in your app

This uses the token endpoint you created to fetch a token and render the chat element.

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

> [tab] HTML
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

> [tab] iOS
>
> ```swift theme={null}
> import SwiftUI
> import WhopElements
>
> struct ContentView: View {
>   var body: some View {
>     WhopChatView(
>       channelId: "chat_feed_XXXXXXXXXXXXXX",
>       style: .imessage
>     )
>     // This should be done as soon as you know your user is authenticated so its
>     // prepared and ready to use without delays.
>     .task {
>       await WhopSDK.configure(tokenProvider: WhopAPITokenProvider())
>     }
>   }
> }
>
> class WhopAPITokenProvider: WhopTokenProvider {
>   func getToken() async -> WhopTokenResponse {
>     let url = URL(string: "https://your-server.com/api/chat/token")!
>     var request = URLRequest(url: url)
>     request.httpMethod = "POST"
>
>     do {
>       let (data, _) = try await URLSession.shared.data(for: request)
>       let json = try JSONSerialization.jsonObject(with: data) as! [String: Any]
>       return WhopTokenResponse(accessToken: json["token"] as? String ?? "")
>     } catch {
>       return WhopTokenResponse(accessToken: "")
>     }
>   }
> }
> ```

## Next steps

> [card] [Authentication](https://docs.whop.com/developer/guides/chat/authentication.md)
>
> Choose between account-scoped tokens and OAuth for embedded chat.

> [card] [Sync your users](https://docs.whop.com/developer/guides/chat/sync-users.md)
>
> Create or map Whop users before minting account-scoped chat tokens.

> [card] [Chat element](https://docs.whop.com/developer/guides/chat/chat-element.md)
>
> Learn the props, events, styles, and deeplinking options for the chat UI.

> [card] [Channel list](https://docs.whop.com/developer/guides/chat/dms-list-element.md)
>
> Show a list of a user's direct message conversations.


## Related topics

- [Communities & Chat](https://docs.whop.com/developer/communities/overview.md)
- [Chat API](https://docs.whop.com/developer/guides/chat.md)
- [Chat OAuth](https://docs.whop.com/developer/guides/chat/oauth.md)
