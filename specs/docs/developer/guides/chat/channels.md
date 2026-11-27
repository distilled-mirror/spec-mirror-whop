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

# Embedded Chat Channels

> Use channels for shared chat rooms connected to products and memberships

Channels provide shared chat rooms for many users. Use them for communities, cohorts, event rooms, member-only announcements, or any conversation where access should follow a product membership.

A Chat experience powers each channel. Customers can access the channel when they have access to a product that contains that experience.

## When to use channels

Use a channel when you want a room that belongs to a product or community, not a private conversation between a few users.

Examples:

* A paid community's main chat room
* A course cohort discussion room
* An announcements channel where only admins can post
* A premium room attached to a higher-tier product
* A live event chat for members who bought access

### What makes channels different

Channels are the chat type for product-backed rooms and communities.

* Chat experiences power them, and you can attach them to products.
* Access follows product membership, so everyone with access to the product can participate.
* They support large rooms and communities without the 50-member cap that applies to DMs.
* They include moderation controls for media, links, reactions, blocked words, posting permissions, and posting cooldowns.
* You can make them read-only or announcement-style by limiting posting to admins.

### Constraints

* Users need access to the product that contains the Chat experience before they can participate.
* Channels don't appear in the channel list. Render them directly with the [Chat element](https://docs.whop.com/developer/guides/chat/chat-element.md).
* Channels aren't for private user-to-user or small group conversations. Use [direct messages](https://docs.whop.com/developer/guides/chat/direct-messages.md) for that.
* Channels don't include a support inbox or open/resolved workflow. Use [support chats](https://docs.whop.com/developer/guides/chat/support-chats.md) for customer support.

## Access model

Channels are usually membership-gated:

1. Create a Chat experience.
2. Attach the experience to a product.
3. Give users access to the product through checkout or a free membership.
4. Render the channel with the `ChatElement` using the channel ID.

> [!NOTE]
>
> Channel IDs start with `chat_feed_`. You can also retrieve a channel by the Chat experience ID, which starts with `exp_`.

## Set up a channel

**1. Create or choose a product**

Create the product that should grant access to the channel. Users get access to channel experiences through products and memberships.

**2. Create a Chat experience**

Create an experience for the Chat app. In the dashboard, this is the Chat app you add to a product. Through the API, use [Create Experience](https://docs.whop.com/api-reference/legacy/experiences/create-experience.md).

**3. Attach the experience to the product**

Attach the experience with [Attach Experience](https://docs.whop.com/api-reference/legacy/experiences/attach-experience.md). This makes the channel available to customers with access to that product.

**4. Grant users access**

Let users buy the product through checkout, or [invite them to a membership](https://docs.whop.com/api-reference/memberships/invite-to-a-membership.md) if you are giving access through a free variant.

**5. Render the channel**

Pass the channel ID to the [Chat element](https://docs.whop.com/developer/guides/chat/chat-element.md). If you only have the experience ID, pass it to [Retrieve Channel](https://docs.whop.com/api-reference/channels/retrieve-channel.md) to get the channel.

> [tab] React
>
> ```tsx theme={null}
> <Chat accessToken={accessToken} channelId="chat_feed_XXXXXXXXXXXXXX">
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
>     channelId: "chat_feed_XXXXXXXXXXXXXX",
>   });
>
>   chat.create("chat").mount("#chat-chat");
> </script>
> ```

> [tab] Swift
>
> ```swift theme={null}
> WhopChatView(
>     channelId: "chat_feed_XXXXXXXXXXXXXX",
>     style: .imessage
> )
> ```

## Manage channels

Channels support moderation tools for controlling who can participate and what members can post. For example, you can make an announcements channel where only admins can send messages, limit media or links, block words, or slow down noisy conversations with posting cooldowns.

See the `moderation` field of [Update Channel](https://docs.whop.com/api-reference/channels/update-channel.md) for the full list of moderation options.

## Next steps

> [card] [Chat element](https://docs.whop.com/developer/guides/chat/chat-element.md)
>
> Render a channel, DM, or support chat in your app.

> [card] [Authentication](https://docs.whop.com/developer/guides/chat/authentication.md)
>
> Mint tokens with the scopes needed to read and send channel messages.


## Related topics

- [Embedded Chat Direct Messages](https://docs.whop.com/developer/guides/chat/direct-messages.md)
- [Theme and Style Embedded Chat](https://docs.whop.com/developer/guides/chat/theming-and-styles.md)
- [Embedded Chat Playground](https://docs.whop.com/developer/guides/chat/playground.md)
