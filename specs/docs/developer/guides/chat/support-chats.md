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

# Support Chats

> Use support chats for one-on-one customer support conversations with your account

Support chats are one-on-one conversations between a customer and your account. Use them when the conversation is official support, needs to be visible to team members, or should have an open/resolved workflow.

Unlike direct messages, support chats organize conversations around a customer support relationship with your account. They're best for customer service, not peer-to-peer messaging.

## When to use support chats

Use support chats when your team needs to respond to customers as the account.

Examples:

* A help inbox for paid members
* A billing or account support conversation
* A buyer support thread after checkout
* A creator-to-customer support line
* An escalation path from your app into Whop-hosted chat UI

### What makes support chats different

Support chats are the chat type for official customer-to-account conversations.

* Each support chat connects one customer with your account.
* For the customer, the conversation behaves like a normal DM-style chat and can appear in their channel list.
* Team members manage support chats through an inbox with open and resolved states.
* Team members can list, sort, and filter support chats by customer, status, and admin/team view.
* Moderators can help respond to support chats without getting full account admin access.

### Constraints

* Support chats aren't for group user-to-user conversations. Use [direct messages](https://docs.whop.com/developer/guides/chat/direct-messages.md) for that.
* Support chats don't appear in the channel list for admins. Build a support inbox with the support channels API instead.
* Support chats don't grant product or membership room access. Use [channels](https://docs.whop.com/developer/guides/chat/channels.md) for member-only rooms.
* Support chats aren't peer-to-peer DMs. Use [direct messages](https://docs.whop.com/developer/guides/chat/direct-messages.md) when users should message each other directly.

## Access model

A support chat belongs to an account and a customer. Team members with support access can list and respond to support chats for the account. The customer can render and participate in their own support chat when authenticated with the required support scopes.

If someone on your team should help manage support chats without getting full admin access, add them as a team member with the moderator role. Moderators can help handle customer conversations while restricting access to broader account settings.

> [!NOTE]
>
> Support chat IDs start with `feed_`.

## Set up a support chat

**1. Sync or identify the customer**

You need the customer's Whop user ID or username. If your app has its own user system, map users first with [Sync your users](https://docs.whop.com/developer/guides/chat/sync-users.md).

**2. Create the support channel**

Use [Create Channel](https://docs.whop.com/api-reference/channels/create-channel.md) with `kind` set to `support`, your `account_id`, and the customer's `customer_user_id`. Whop returns the existing support chat if one already exists for that customer.

**3. Render the conversation**

Pass the returned support channel ID to the [Chat element](https://docs.whop.com/developer/guides/chat/chat-element.md).

**4. List support chats for your team**

Use [List Channels](https://docs.whop.com/api-reference/channels/list-channels.md) with `kind` set to `support` and `support_role` set to `staff` to build an inbox for your team.

Then render the support chat with the returned channel ID:

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

## Manage support chats

Use [Create Channel](https://docs.whop.com/api-reference/channels/create-channel.md), [List Channels](https://docs.whop.com/api-reference/channels/list-channels.md), and [Retrieve Channel](https://docs.whop.com/api-reference/channels/retrieve-channel.md) to manage support chats. Use [Resolve Support Channel](https://docs.whop.com/api-reference/channels/resolve-support-channel.md) and [Reopen Support Channel](https://docs.whop.com/api-reference/channels/reopen-support-channel.md) to track whether a conversation is done.

Support chats also support inbox-style workflows. You can build views for open conversations, resolved conversations, customer-visible conversations, and team/admin views. See [List Channels](https://docs.whop.com/api-reference/channels/list-channels.md) for the available filters, such as `status` and `awaiting_reply`.

## Next steps

> [card] [Chat element](https://docs.whop.com/developer/guides/chat/chat-element.md)
>
> Render a support chat after you create or retrieve it.

> [card] [Authentication](https://docs.whop.com/developer/guides/chat/authentication.md)
>
> Add support chat scopes to your token endpoint.


## Related topics

- [Chat API](https://docs.whop.com/developer/guides/chat.md)
- [Chat Authentication](https://docs.whop.com/developer/guides/chat/authentication.md)
- [Chat Quickstart](https://docs.whop.com/developer/guides/chat/quickstart.md)
