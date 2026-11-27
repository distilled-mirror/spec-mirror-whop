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

# Theme and Style Embedded Chat

> Match the chat elements to your app with a theme on web or in Swift, and pick a Discord or iMessage chat style.

## Theming

Customize the appearance of chat elements with a theme. On web, pass a `theme` through the `appearance` option. On Swift, create a `WhopTheme` and apply it with `.whopTheme()`.

> [tab] React
>
> ```tsx theme={null}
> <WhopElements
> 	elements={loadWhop()}
> 	appearance={{
> 		theme: {
> 			appearance: "dark",
> 			accentColor: "blue",
> 			grayColor: "gray",
> 			dangerColor: "red",
> 			warningColor: "amber",
> 			successColor: "green",
> 			infoColor: "sky",
> 		},
> 	}}
> >
> 	{/* elements */}
> </WhopElements>
> ```

> [tab] Vanilla JS
>
> ```typescript theme={null}
> const chat = window.WhopElements().chat.create({
> 	appearance: {
> 		theme: {
> 			appearance: "dark",
> 			accentColor: "blue",
> 			grayColor: "gray",
> 			dangerColor: "red",
> 			warningColor: "amber",
> 			successColor: "green",
> 			infoColor: "sky",
> 		},
> 	},
> });
> ```

> [tab] Swift
>
> ```swift theme={null}
> struct ThemedMessagesView: View {
>     @State private var selectedChannel: DMChannel?
>
>     let theme = WhopTheme(
>         accent: .plum,
>         neutral: .gray,
>         danger: .tomato,
>         warning: .violet
>     )
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
>         .whopTheme(theme)
>         .navigationDestination(item: $selectedChannel) { channel in
>             WhopChatView(
>                 channelId: channel.id,
>                 style: .imessage
>             )
>             .whopTheme(theme)
>             .navigationBarTitleDisplayMode(.inline)
>             .navigationTitle(channel.name)
>         }
>     }
> }
> ```

### Theme properties

| Purpose | Web option | Swift option | Description |
| - | - | - | - |
| Appearance | `appearance` | — | Light or dark mode (`"light"`, `"dark"`) on web. Swift follows the app's color scheme. |
| Accent | `accentColor` | `accent` | Primary interactive elements, links, and selection highlights |
| Neutral | `grayColor` | `neutral` | Text, borders, and backgrounds |
| Danger | `dangerColor` | `danger` | Destructive actions and error states |
| Warning | `warningColor` | `warning` | Warning states |
| Success | `successColor` | `success` | Success states |
| Info | `infoColor` | `info` | Informational highlights |

### Available colors

Color properties accept these tints. Use strings on web, such as `"blue"`, and enum values on Swift, such as `.blue`.

`amber` · `blue` · `bronze` · `brown` · `crimson` · `cyan` · `gold` · `grass` · `gray` · `green` · `indigo` · `iris` · `jade` · `lemon` · `lime` · `magenta` · `mint` · `orange` · `pink` · `plum` · `purple` · `red` · `ruby` · `sky` · `teal` · `tomato` · `violet` · `yellow`

## Chat styles

Choose between Discord-style or iMessage-style chat. iMessage style (bubble chat) is the default if not provided.

> [tab] React
>
> ```tsx theme={null}
> <Chat
> 	accessToken={accessToken}
> 	channelId="chat_feed_XXXXXXXXXXXXXX"
> 	style="discord" // or "imessage" (default)
> >
> 	<ChatElement style={{ height: "100dvh", width: "100%" }} />
> </Chat>
> ```

> [tab] Vanilla JS
>
> ```typescript theme={null}
> const chat = window.WhopElements().chat.create({
> 	accessToken,
> 	channelId: "chat_feed_XXXXXXXXXXXXXX",
> 	style: "discord", // or "imessage" (default)
> });
> ```

> [tab] Swift
>
> ```swift theme={null}
> // iMessage style (bubble chat)
> WhopChatView(
>     channelId: "chat_feed_XXXXXXXXXXXXXX",
>     style: .imessage
> )
>
> // Discord style (full-width messages)
> WhopChatView(
>     channelId: "chat_feed_XXXXXXXXXXXXXX",
>     style: .discord
> )
> ```


## Related topics

- [Embedded Chat Channels](https://docs.whop.com/developer/guides/chat/channels.md)
- [Embedded Chat Direct Messages](https://docs.whop.com/developer/guides/chat/direct-messages.md)
- [Chat Element](https://docs.whop.com/developer/guides/chat/chat-element.md)
