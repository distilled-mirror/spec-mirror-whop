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

# ChatElement

> A real-time message view.

> [!NOTE]
>
> This page documents `@whop/elements@1.10.0` and `@whop/elements-react@1.10.0`.

*Since `v1.10.0`.*

Mounts inside [`Chat`](https://docs.whop.com/elements/latest/chat/overview). It fills the space you mount it into instead of growing with its content, so give the mount target a height. `accessToken` and `channelId` come from there. Pass props and callbacks through the create options or React props. Keep the created handle, or React `ref`, to call `prefetchChannel()` and `focusMessage()`.

```tsx React theme={null}
import { WhopElements, Chat, ChatElement } from "@whop/elements-react";
import { loadWhop } from "@whop/elements";

function Example() {
  return (
    <WhopElements elements={loadWhop()}>
      <Chat /* options */>
        <ChatElement style={{ height: 600 }} onMessageSent={(e) => console.log(e)} onMessageDeleted={(e) => console.log(e)} onProfileClick={(e) => console.log(e)} onLinkClick={(e) => console.log(e)} onExperienceMentionClick={(e) => console.log(e)} onAttachmentClick={(e) => console.log(e)} />
      </Chat>
    </WhopElements>
  );
}
```

```html JavaScript theme={null}
<div id="chat-chat" style="height: 600px"></div>
<script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>
<script type="module">
  const chat = window.WhopElements().chat.create({ /* options */ });
  chat.create('chat', {
    onMessageSent: (e) => console.log(e),
    onMessageDeleted: (e) => console.log(e),
    onProfileClick: (e) => console.log(e),
    onLinkClick: (e) => console.log(e),
    onExperienceMentionClick: (e) => console.log(e),
    onAttachmentClick: (e) => console.log(e)
  }).mount('#chat-chat');
</script>
```

## Props

**focusMessageId** `string | null`

The message ID to scroll to and highlight, prefixed `post_`.

**defaultFocusMessageId** `string | null`

The initial message ID to scroll to and highlight, prefixed `post_`.

**disableUserBans** `boolean`

Prevent admins from banning users. Hides the "Ban user" option from the message menu. Defaults to `false`.

**disableUserMutes** `boolean`

Prevent admins from muting users. Hides the "Mute user" option from the message menu. Defaults to `false`.

**disableAttachmentPreview** `boolean`

Prevent users from previewing attachments. Use the `onAttachmentClick` callback to handle the preview yourself. Defaults to `false`.

**disableLinkNavigation** `boolean`

Disable link navigation. Use the `onLinkClick` callback to handle navigation yourself. Defaults to `false`.

**disableExperienceMentions** `boolean`

Prevent users from mentioning experiences. Defaults to `false`.

**emptyState** `EmptyState`

Custom empty state displayed when there are no messages.

**properties**

**image** `object`

**properties**

**url** `string` *required*

**width** `number`

**height** `number`

**title** `string`

**description** `string`

## Events

Pass callbacks in the create options or React props.

### `onMessageSent`

Emitted when the user sends a message.

**payload** `object`

**properties**

**messageId** `string` *required*

**content** `string` *required*

**channelId** `string` *required*

### `onMessageDeleted`

Emitted when the user deletes a message.

**payload** `object`

**properties**

**messageId** `string` *required*

### `onProfileClick`

Emitted when the user clicks a profile.

**payload** `object`

**properties**

**userId** `string` *required*

### `onLinkClick`

Emitted when the user clicks a link.

**payload** `object`

**properties**

**url** `string` *required*

### `onExperienceMentionClick`

Emitted when the user clicks an experience mention.

**payload** `object`

**properties**

**experienceId** `string` *required*

### `onAttachmentClick`

Emitted when the user clicks an attachment.

**payload** `object`

**properties**

**messageId** `string` *required*

**attachments** `Attachment[]` *required*

**properties**

**id** `string` *required*

**url** `string` *required*

**contentType** `string`

**filename** `string`

**width** `number`

**height** `number`

### `onLoaderStart`

Runs after the loading skeleton first paints and before `onReady`.

### `onReady`

Runs after the element's first complete paint.

### `onError`

Runs when the element fails to load or crashes. The fallback remains visible. Use `code` for programmatic handling. `sourceKey` identifies a failed host-state source.

**e** `object`

**properties**

**message** `string` *required*

**code** `string`

**sourceKey** `string`

## Methods

Call these on the handle returned by `create`, or through a React `ref`.

### `prefetchChannel`

Prefetch a channel.

**Parameters**

**input** `object` *required*

**properties**

**channelId** `string` *required*

**Returns** `Promise<void>`

### `focusMessage`

Scroll to and highlight a message in the current channel. To open the chat at a message, use `defaultFocusMessageId` instead.

**Parameters**

**input** `object` *required*

**properties**

**messageId** `string` *required*

**Returns** `Promise<void>`

### `mount`

Mounts the element in `target` and starts loading. React components mount themselves.

**Parameters**

**target** `string | HTMLElement` *required*

### `destroy`

Removes the element and releases its frame and subscriptions. You can call it more than once. React removes the element automatically.

### `update`

Merges new props into the mounted element. In React, change the component props instead.

**Parameters**

**options** `Partial\<ChatElementProps\>` *required*

## Styling

This element doesn't expose class names for styling. Use `appearance` (theme, accent color, variables) to restyle it.


## Related topics

- [Chat Element](https://docs.whop.com/developer/guides/chat/chat-element.md)
- [Chat](https://docs.whop.com/elements/latest/chat/overview.md)
- [Chat Authentication](https://docs.whop.com/developer/guides/chat/authentication.md)
