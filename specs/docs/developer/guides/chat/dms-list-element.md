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

# Embed the Channel List Element

> Show a user's direct message conversations next to the chat element and open the one they select.

The channel list element renders a navigable list of the user's direct message conversations. It picks up the same [theme](https://docs.whop.com/developer/guides/chat/theming-and-styles.md) as the rest of your chat elements. For every option it takes on the web, see the [`ChannelListElement` reference](https://docs.whop.com/elements/latest/chat/channelList).

### Basic usage

> [tab] React
>
> ```tsx theme={null}
> "use client";
>
> import { useEffect, useState } from "react";
> import { WhopElements, Chat, ChannelListElement, ChatElement } from "@whop/elements-react";
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
> export function ChannelListView() {
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
>       <Chat accessToken={accessToken}>
>         <div style={{ display: "flex", height: "100dvh" }}>
>           <ChannelListElement style={{ height: "100%", width: "400px" }} />
>           <ChatElement style={{ height: "100%", flex: 1 }} />
>         </div>
>       </Chat>
>     </WhopElements>
>   );
> }
> ```

> [tab] Vanilla JS
>
> ```html theme={null}
> <div style="display: flex; height: 100vh">
>   <div id="chat-channelList" style="height: 100%; width: 400px"></div>
>   <div id="chat-chat" style="height: 100%; flex: 1"></div>
> </div>
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
>   });
>
>   chat.create("channelList").mount("#chat-channelList");
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
> struct MessagesView: View {
>     @State private var selectedChannel: DMChannel?
>
>     var body: some View {
>         WhopDMsListView(
>             onEvent: { event in
>                 switch event {
>                 case let .channelSelected(channel):
>                     selectedChannel = channel
>                 }
>             }
>         )
>         .navigationDestination(item: $selectedChannel) { channel in
>             WhopChatView(
>                 channelId: channel.id,
>                 style: .imessage
>             )
>             .navigationBarTitleDisplayMode(.inline)
>             .navigationTitle(channel.name)
>         }
>     }
> }
> ```

### Account scoping

Filter the channel list to only show conversations belonging to a specific account by passing an `accountId`. To create a DM from your server, call [Create Channel](https://docs.whop.com/api-reference/channels/create-channel.md) with `kind` set to `private`. Pass the same account ID as `account_id` so the DM appears in the scoped list.

> [tab] React
>
> ```tsx theme={null}
> <Chat accessToken={accessToken} accountId="biz_xxxx">
> 	{/* elements */}
> </Chat>
> ```

> [tab] Vanilla JS
>
> ```typescript theme={null}
> const chat = window.WhopElements().chat.create({
> 	accessToken,
> 	accountId: "biz_xxxx",
> });
> ```

> [tab] Swift
>
> ```swift theme={null}
> WhopDMsListView(
>     companyId: "biz_xxxx",
>     onEvent: { event in
>         switch event {
>         case let .channelSelected(channel):
>             selectedChannel = channel
>         }
>     }
> )
> ```

### Event handling

Pass an event callback to listen for channel selections.

> [tab] React
>
> | Event | Payload | Description |
> | - | - | - |
> | `onChannelChange` | `{ channelId: string, focusMessageId?: string }` | Emitted when the active channel changes |
>
> ```tsx theme={null}
> <Chat
> 	accessToken={accessToken}
> 	onChannelChange={({ channelId }) => {
> 		console.log("Channel selected:", channelId);
> 	}}
> >
> 	<ChannelListElement />
> </Chat>
> ```

> [tab] Vanilla JS
>
> | Event | Payload | Description |
> | - | - | - |
> | `onChannelChange` | `{ channelId: string, focusMessageId?: string }` | Emitted when the active channel changes |
>
> ```typescript theme={null}
> const chat = window.WhopElements().chat.create({
> 	accessToken,
> 	onChannelChange: ({ channelId }) => {
> 		console.log("Channel selected:", channelId);
> 	},
> });
> ```

> [tab] Swift
>
> | Event | Detail | Description |
> | - | - | - |
> | `.channelSelected` | `channel: DMChannel` | Emitted when the user taps a channel |
>
> ```swift theme={null}
> WhopDMsListView(
>     onEvent: { event in
>         switch event {
>         case let .channelSelected(channel):
>             selectedChannel = channel
>         }
>     }
> )
> .navigationDestination(item: $selectedChannel) { channel in
>     WhopChatView(
>         channelId: channel.id,
>         style: .imessage
>     )
>     .navigationBarTitleDisplayMode(.inline)
>     .navigationTitle(channel.name)
> }
> ```


## Related topics

- [ChannelListElement](https://docs.whop.com/elements/latest/chat/channelList.md)
- [Embedded Chat Channels](https://docs.whop.com/developer/guides/chat/channels.md)
- [Embedded Chat Direct Messages](https://docs.whop.com/developer/guides/chat/direct-messages.md)
