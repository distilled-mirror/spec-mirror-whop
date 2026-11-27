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

# Chat Element

> Render a real-time chat for a channel, DM, or support chat on your site with the chat element from @whop/elements.

The chat element renders a real-time chat UI connected to a specific channel. Its colors and message layout follow the [theme and chat style](https://docs.whop.com/developer/guides/chat/theming-and-styles.md) you set. For every option it takes on the web, see the [`ChatElement` reference](https://docs.whop.com/elements/latest/chat/chat).

### Basic usage

Pass a `channelId` to connect to a specific channel, DM, or support chat.

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
> import SwiftUI
> import WhopElements
>
> struct ChatView: View {
>     var body: some View {
>         WhopChatView(
>             channelId: "chat_feed_XXXXXXXXXXXXXX",
>             style: .imessage
>         )
>     }
> }
> ```

### Deep linking to messages

> [tab] React
>
> To scroll to and highlight a specific message, pass `focusMessageId`. The view will automatically navigate to that message.
>
> ```tsx theme={null}
> <Chat accessToken={accessToken} channelId="chat_feed_XXXXXXXXXXXXXX">
> 	<ChatElement
> 		focusMessageId="post_XXXXXXXXXXXXXX"
> 		style={{ height: "100dvh", width: "100%" }}
> 	/>
> </Chat>
> ```

> [tab] Vanilla JS
>
> To scroll to and highlight a specific message, pass `focusMessageId`. The view will automatically navigate to that message.
>
> ```typescript theme={null}
> const chatElement = chat.create("chat", {
> 	focusMessageId: "post_XXXXXXXXXXXXXX",
> });
>
> chatElement.mount("#chat-chat");
> ```

> [tab] Swift
>
> To scroll to and highlight a specific message, pass `deeplinkToPostId`. The view will automatically navigate to that message.
>
> ```swift theme={null}
> struct ChatView: View {
>     @State private var targetPostId: String? = nil
>
>     var body: some View {
>         WhopChatView(
>             channelId: "chat_feed_XXXXXXXXXXXXXX",
>             deeplinkToPostId: targetPostId,
>             style: .imessage
>         )
>     }
> }
> ```

### Event handling

Pass event callbacks to listen for profile selections, link selections, and sent messages.

> [tab] React
>
> | Event | Payload | Description |
> | - | - | - |
> | `onMessageSent` | `{ messageId: string, content: string, channelId: string }` | Emitted when the user sends a message |
> | `onMessageDeleted` | `{ messageId: string }` | Emitted when the user deletes a message |
> | `onProfileClick` | `{ userId: string }` | Emitted when the user selects a profile |
> | `onLinkClick` | `{ url: string }` | Emitted when the user selects a link |
> | `onExperienceMentionClick` | `{ experienceId: string }` | Emitted when the user selects an experience mention |
> | `onAttachmentClick` | `{ messageId: string, attachments: Attachment[] }` | Emitted when the user selects an attachment |
>
> ```tsx theme={null}
> <Chat accessToken={accessToken} channelId="chat_feed_XXXXXXXXXXXXXX">
> 	<ChatElement
> 		onMessageSent={({ messageId, content, channelId }) => {
> 			console.log("Message sent:", messageId, content, channelId);
> 		}}
> 		onMessageDeleted={({ messageId }) => {
> 			console.log("Message deleted:", messageId);
> 		}}
> 		onProfileClick={({ userId }) => {
> 			console.log("Profile clicked:", userId);
> 		}}
> 		onLinkClick={({ url }) => {
> 			console.log("Link clicked:", url);
> 		}}
> 		onExperienceMentionClick={({ experienceId }) => {
> 			console.log("Experience clicked:", experienceId);
> 		}}
> 		onAttachmentClick={({ messageId, attachments }) => {
> 			console.log("Attachment clicked:", messageId, attachments);
> 		}}
> 	/>
> </Chat>
> ```

> [tab] Vanilla JS
>
> | Event | Payload | Description |
> | - | - | - |
> | `onMessageSent` | `{ messageId: string, content: string, channelId: string }` | Emitted when the user sends a message |
> | `onMessageDeleted` | `{ messageId: string }` | Emitted when the user deletes a message |
> | `onProfileClick` | `{ userId: string }` | Emitted when the user selects a profile |
> | `onLinkClick` | `{ url: string }` | Emitted when the user selects a link |
> | `onExperienceMentionClick` | `{ experienceId: string }` | Emitted when the user selects an experience mention |
> | `onAttachmentClick` | `{ messageId: string, attachments: Attachment[] }` | Emitted when the user selects an attachment |
>
> ```typescript theme={null}
> const chatElement = chat.create("chat", {
> 	onMessageSent: ({ messageId, content, channelId }) => {
> 		console.log("Message sent:", messageId, content, channelId);
> 	},
> 	onMessageDeleted: ({ messageId }) => {
> 		console.log("Message deleted:", messageId);
> 	},
> 	onProfileClick: ({ userId }) => {
> 		console.log("Profile clicked:", userId);
> 	},
> 	onLinkClick: ({ url }) => {
> 		console.log("Link clicked:", url);
> 	},
> 	onExperienceMentionClick: ({ experienceId }) => {
> 		console.log("Experience clicked:", experienceId);
> 	},
> 	onAttachmentClick: ({ messageId, attachments }) => {
> 		console.log("Attachment clicked:", messageId, attachments);
> 	},
> });
>
> chatElement.mount("#chat-chat");
> ```

> [tab] Swift
>
> | Event | Detail | Description |
> | - | - | - |
> | `.profileTap` | `username: String` | Emitted when the user taps on a profile |
> | `.urlTap` | `url: String` | Emitted when the user taps on a link |
> | `.messageSent` | `content: String, messageId: String` | Emitted when the user sends a message |
>
> ```swift theme={null}
> WhopChatView(
>     channelId: "chat_feed_XXXXXXXXXXXXXX",
>     style: .imessage,
>     onEvent: { event in
>         switch event {
>         case let .profileTap(username):
>             print("Profile tapped: \(username)")
>         case let .urlTap(url):
>             print("URL tapped: \(url)")
>         case let .messageSent(content, messageId):
>             print("Message sent: \(messageId) - \(content)")
>         }
>     }
> )
> ```


## Related topics

- [Chat](https://docs.whop.com/elements/latest/chat/overview.md)
- [ChatElement](https://docs.whop.com/elements/latest/chat/chat.md)
- [Support Chats](https://docs.whop.com/developer/guides/chat/support-chats.md)
