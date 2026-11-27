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

# Chat

> Real-time chat with a channel list and message search.

> [!NOTE]
>
> This page documents `@whop/elements@1.10.0` and `@whop/elements-react@1.10.0`.

*Since `v1.10.0`.*

```tsx React theme={null}
import { WhopElements, Chat } from "@whop/elements-react";
import { loadWhop } from "@whop/elements";

function Example() {
  return (
    <WhopElements elements={loadWhop()}>
      <Chat /* options */>
        {/* mount elements here */}
      </Chat>
    </WhopElements>
  );
}
```

```html JavaScript theme={null}
<script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>
<script type="module">
  const chat = window.WhopElements().chat.create({ /* options */ });
</script>
```

## Options

Pass these to `whop.chat.create({ … })`, or as props on `<Chat>` in React.

**accessToken** `string`

A scoped user token. Create one on your server with `POST /api/v1/access_tokens` and set a fresh one with `update({ accessToken })` before it expires. Channels need the `chat:read` and `chat:message:create` scopes, direct messages `dms:read`, `dms:message:manage` and `dms:channel:manage`, and support chats `support_chat:read` and `support_chat:message:create`.

**accountId** `string`

The account ID to scope chat by, prefixed `biz_`.

**channelId** `string | null`

The active channel ID, prefixed `feed_` or `chat_feed_`.

**defaultChannelId** `string | null`

The initial active channel ID, prefixed `feed_` or `chat_feed_`.

**style** `"imessage" | "discord"`

The style of the chat. Defaults to `"imessage"`.

**appearance** `Appearance`

Visual customization for this group's elements. Overrides the global `WhopElements({ appearance })`. Change it live with `update({ appearance })`.

**locale** `WhopElementsLocale`

Locale for this group's element UI text. Set it to one of the app's built locales to override the global configuration. Any other value falls back to the default locale.

## Events

Pass callbacks in the create options or React props.

### `onChannelChange`

Emitted when the active channel changes. Includes `focusMessageId` when the channel opens with a focused message.

**payload** `object`

**properties**

**channelId** `string` *required*

**focusMessageId** `string`

### `onLoadingChange`

Runs when the grouped loading state changes. The value is `true` while any mounted element is still loading.

**loading** `boolean`

## Methods

Call these on the Chat handle from `whop.chat.create({ … })` or `useChat()`.

### `update`

Merges new handle options into every mounted element. In React, change the namespace props instead.

**Parameters**

**options** `Partial\<ChatOptions\>` *required*

### `destroy`

Destroys every element and sub-controller this handle created, removes the controller frame, and releases its subscriptions. You can call it more than once, but a destroyed handle refuses any other call — create a new handle to start over. React removes the group automatically when the provider unmounts.

## Elements

The elements this group mounts. Each has its own page:

> [card] [ChatElement](https://docs.whop.com/elements/latest/chat/chat.md)
>
> A real-time message view.

> [card] [ChannelListElement](https://docs.whop.com/elements/latest/chat/channelList.md)
>
> A list of the user's channels.

> [card] [SearchElement](https://docs.whop.com/elements/latest/chat/search.md)
>
> A modal for searching messages across the user's channels.


## Related topics

- [Support Chats](https://docs.whop.com/developer/guides/chat/support-chats.md)
- [Chat Authentication](https://docs.whop.com/developer/guides/chat/authentication.md)
- [Chat Element](https://docs.whop.com/developer/guides/chat/chat-element.md)
