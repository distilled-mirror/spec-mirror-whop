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

# Embedded Chat Direct Messages

> Set up private one-on-one or group direct messages for your app's users, kept separate from their other Whop conversations.

Direct messages are private conversations between two or more users. Use them when your app already has users and you want private messaging inside your product.

For embedded chat, DMs belong to your account context by default. That means users only see conversations created for your app, not unrelated Whop messages.

## When to use direct messages

Use direct messages when only specific members should join the conversation.

Examples:

* Buyer-to-seller messaging inside your app
* A private coaching thread between a coach and client
* A small group conversation for a project, cohort, or team
* A marketplace conversation between two matched users
* User-to-user messaging inside your app

### What makes direct messages different

DMs are the chat type for private conversations between specific users.

* They can be one-on-one or group conversations with up to 50 members.
* Only the users added as members can see and participate in the conversation.
* They appear in the [channel list](https://docs.whop.com/developer/guides/chat/dms-list-element.md), so users can browse and open their recent conversations.
* You can scope them to your account, giving your embedded app its own inbox separate from unrelated Whop messages.
* They support custom conversation names and member management.

### Constraints

* DMs don't grant access based on product membership. Add or remove members directly when access changes.
* DMs don't have admin-only, read-only, or room-level moderation controls. Use [channels](https://docs.whop.com/developer/guides/chat/channels.md) for moderated rooms.
* DMs don't include a support inbox or open/resolved workflow. Use [support chats](https://docs.whop.com/developer/guides/chat/support-chats.md) for customer support.
* DMs aren't designed for large communities, announcements, or member-wide rooms. Use [channels](https://docs.whop.com/developer/guides/chat/channels.md) for that.

## Access model

Each DM has members. Those members are the only users who can see and participate in the conversation.

When you create DMs for embedded chat, keep them tied to your account. This gives your app its own inbox and keeps conversations separate from the rest of Whop.

> [!NOTE]
>
> DM channel IDs start with `feed_`.

## Set up a direct message

**1. Sync or create users**

Make sure every participant has a Whop user ID. If your app has its own users, map them to Whop users first. See [Sync your users](https://docs.whop.com/developer/guides/chat/sync-users.md).

**2. Create the DM channel**

Use [Create Channel](https://docs.whop.com/api-reference/channels/create-channel.md) with `kind` set to `private` to create the conversation. List the other members in `participant_user_ids`. With an account API key, also pass `sender_user_id`.

**3. Add more members when needed**

Use [Add Channel Member](https://docs.whop.com/api-reference/channel-members/add-channel-member.md) to add a user to an existing DM. Use [Remove Channel Member](https://docs.whop.com/api-reference/channel-members/remove-channel-member.md) to remove one.

**4. Render the conversation or list**

Use the [Chat element](https://docs.whop.com/developer/guides/chat/chat-element.md) for a single DM, or the [channel list](https://docs.whop.com/developer/guides/chat/dms-list-element.md) to let users pick from their conversations.

Then render the DM with the returned channel ID:

> [tab] React
>
> ```tsx theme={null}
> <Chat accessToken={accessToken} channelId="feed_XXXXXXXXXXXXXX">
>   <ChatElement style={{ height: "100dvh", width: "100%" }} />
> </Chat>
> ```

> [tab] Vanilla JS
>
> ```html theme={null}
> <div id="chat-chat" style="height: 100dvh"></div>
> <script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>
> <script type="module">
>   const chat = window.WhopElements().chat.create({
>     accessToken,
>     channelId: "feed_XXXXXXXXXXXXXX",
>   });
>
>   chat.create("chat").mount("#chat-chat");
> </script>
> ```

> [tab] Swift
>
> ```swift theme={null}
> WhopChatView(
>     channelId: "feed_XXXXXXXXXXXXXX",
>     style: .imessage
> )
> ```

## Manage direct messages

Use [List Channels](https://docs.whop.com/api-reference/channels/list-channels.md) with `kind` set to `private`, [Update Channel](https://docs.whop.com/api-reference/channels/update-channel.md), and [Delete Channel](https://docs.whop.com/api-reference/channels/delete-channel.md) to manage conversations.

Use [List Channel Members](https://docs.whop.com/api-reference/channel-members/list-channel-members.md), [Add Channel Member](https://docs.whop.com/api-reference/channel-members/add-channel-member.md), and [Remove Channel Member](https://docs.whop.com/api-reference/channel-members/remove-channel-member.md) to manage participants.

> [!TIP]
>
> If you use the channel list, keep it tied to your account so users only see conversations for your embedded chat experience.

## Next steps

> [card] [Channel list](https://docs.whop.com/developer/guides/chat/dms-list-element.md)
>
> Show DM conversations and handle channel selection.

> [card] [Sync your users](https://docs.whop.com/developer/guides/chat/sync-users.md)
>
> Create or map Whop users before creating DM channels.


## Related topics

- [Embedded Chat Channels](https://docs.whop.com/developer/guides/chat/channels.md)
- [Message](https://docs.whop.com/api-reference/legacy/messages/message.md)
- [Chat Quickstart](https://docs.whop.com/developer/guides/chat/quickstart.md)
