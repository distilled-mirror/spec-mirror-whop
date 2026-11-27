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

# Chat API

> Send and read chat messages in a Whop community from your server, and manage its channels.

Use the Chat API to send messages, manage channels, open support conversations, and read reactions. Identify channels by `experience_id` or `channel_id`.

> [!TIP]
>
> This page covers the **server-side Chat API**, for calling Whop from your backend to post, read, or moderate messages. If you want to render a live Whop chat UI inside your own frontend, see the [embedded chat quickstart](https://docs.whop.com/developer/guides/chat/quickstart.md) instead.

## Initialize the SDK

```typescript TypeScript theme={null}
import { WhopClient } from "@whop/sdk";

const client = new WhopClient({
  token: process.env.WHOP_API_KEY,
});
```

```python Python theme={null}
import os
from whop_sdk import Whop

client = Whop(
    token=os.environ["WHOP_API_KEY"],
)
```

```rust Rust theme={null}
use whop_sdk::prelude::*;

let config = ClientConfig {
    token: Some(std::env::var("WHOP_API_KEY").unwrap()),
    ..Default::default()
};
let client = Whop::new(config).expect("Failed to build client");
```

```go Go theme={null}
import (
    "os"

    "github.com/whopio/whopsdk-go/v2/client"
    "github.com/whopio/whopsdk-go/v2/option"
)

client := client.NewWhop(option.WithToken(os.Getenv("WHOP_API_KEY")))
```

## Send a message

Messages support Markdown and optional attachments uploaded via the [Files API](https://docs.whop.com/developer/guides/upload-files.md).

```typescript TypeScript theme={null}
const message = await client.messages.create({
  channel_id: "chat_feed_xxxxxxxxxxxxx", // or an experience_id
  content: "Hello! **Markdown** is supported.",
  attachments: [
    { id: "file_xxxxxxxxxxxxx" }, // upload via the Files API first
  ],
});
```

```python Python theme={null}
message = client.messages.create(
    channel_id="chat_feed_xxxxxxxxxxxxx",
    content="Hello! **Markdown** is supported.",
    attachments=[
        {"id": "file_xxxxxxxxxxxxx"},
    ],
)
```

```rust Rust theme={null}
let message = client
    .messages
    .create(
        &CreateMessagesRequest {
            channel_id: "chat_feed_xxxxxxxxxxxxx".to_string(), // or an experience_id
            content: "Hello! **Markdown** is supported.".to_string(),
            attachments: Some(vec![CreateMessagesRequestAttachmentsItem {
                id: "file_xxxxxxxxxxxxx".to_string(), // upload via the Files API first
            }]),
            ..Default::default()
        },
        None,
    )
    .await?;

println!("{}", message.id);
```

```go Go theme={null}
message, err := client.Messages.Create(context.TODO(), &whopsdk.CreateMessagesRequest{
    ChannelID: "chat_feed_xxxxxxxxxxxxx", // or an experience_id
    Content:   "Hello! **Markdown** is supported.",
})
if err != nil {
    log.Fatal(err)
}

fmt.Println(message.ID)
```

## Read messages

List operations paginate automatically. Iterate the response and the SDK fetches additional pages for you.

```typescript TypeScript theme={null}
const messages = await client.messages.list({
  channel_id: "chat_feed_xxxxxxxxxxxxx",
  direction: "desc",
  first: 20,
});

for await (const message of messages) {
  console.log(message);
}

const single = await client.messages.retrieve({ id: "msg_xxxxxxxxxxxxx" });
```

```python Python theme={null}
for message in client.messages.list(
    channel_id="chat_feed_xxxxxxxxxxxxx",
    direction="desc",
    first=20,
):
    print(message)

single = client.messages.retrieve("msg_xxxxxxxxxxxxx")
```

```rust Rust theme={null}
let messages = client
    .messages
    .list(
        &MessagesListQueryRequest {
            channel_id: "chat_feed_xxxxxxxxxxxxx".to_string(),
            direction: Some(Direction::Desc),
            first: Some(20),
            ..Default::default()
        },
        None,
    )
    .await?;

for message in messages.data {
    println!("{message:?}");
}

let single = client
    .messages
    .retrieve(&"msg_xxxxxxxxxxxxx".to_string(), None)
    .await?;
```

```go Go theme={null}
page, err := client.Messages.List(context.TODO(), &whopsdk.ListMessagesRequest{
    ChannelID: "chat_feed_xxxxxxxxxxxxx",
    Direction: whopsdk.DirectionDesc.Ptr(),
    First:     whopsdk.Int(20),
})
if err != nil {
    log.Fatal(err)
}

iter := page.Iterator()
for iter.Next(context.TODO()) {
    fmt.Println(iter.Current())
}
if err := iter.Err(); err != nil {
    log.Fatal(err)
}

single, err := client.Messages.Retrieve(context.TODO(), &whopsdk.RetrieveMessagesRequest{
    ID: "msg_xxxxxxxxxxxxx",
})
if err != nil {
    log.Fatal(err)
}
_ = single
```

## Manage channels

Use [Update Channel](https://docs.whop.com/api-reference/channels/update-channel.md) to change a community channel's moderation settings: banned words, posting cooldowns, and who can post or react. Use [List Channels](https://docs.whop.com/api-reference/channels/list-channels.md) with `kind` set to `community` to list the chat channels on an account.

```typescript TypeScript theme={null}
const channel = await client.channels.update({
  id: "chat_feed_xxxxxxxxxxxxx",
  moderation: {
    ban_media: false,
    ban_urls: false,
    banned_words: ["spam", "scam"],
    user_posts_cooldown_seconds: 10,
    who_can_post: "admins",
    who_can_react: "everyone",
  },
});

const channels = await client.channels.list({
  kind: "community",
  account_id: "biz_xxxxxxxxxxxxx",
  first: 10,
});

for await (const item of channels) {
  console.log(item);
}
```

```python Python theme={null}
channel = client.channels.update(
    "chat_feed_xxxxxxxxxxxxx",
    moderation={
        "ban_media": False,
        "ban_urls": False,
        "banned_words": ["spam", "scam"],
        "user_posts_cooldown_seconds": 10,
        "who_can_post": "admins",
        "who_can_react": "everyone",
    },
)

for item in client.channels.list(
    kind="community",
    account_id="biz_xxxxxxxxxxxxx",
    first=10,
):
    print(item)
```

```go Go theme={null}
channel, err := client.Channels.Update(context.TODO(), &whopsdk.UpdateChannelsRequest{
    ID: "chat_feed_xxxxxxxxxxxxx",
    Moderation: &whopsdk.UpdateChannelsRequestModeration{
        BanMedia:                 whopsdk.Bool(false),
        BanURLs:                  whopsdk.Bool(false),
        BannedWords:              []string{"spam", "scam"},
        UserPostsCooldownSeconds: whopsdk.Int(10),
        WhoCanPost:               whopsdk.UpdateChannelsRequestModerationWhoCanPostAdmins.Ptr(),
        WhoCanReact:              whopsdk.UpdateChannelsRequestModerationWhoCanReactEveryone.Ptr(),
    },
})
if err != nil {
    log.Fatal(err)
}
_ = channel

page, err := client.Channels.List(context.TODO(), &whopsdk.ListChannelsRequest{
    Kind:      []*whopsdk.ListChannelsRequestKindItem{whopsdk.ListChannelsRequestKindItemCommunity.Ptr()},
    AccountID: whopsdk.String("biz_xxxxxxxxxxxxx"),
    First:     whopsdk.Int(10),
})
if err != nil {
    log.Fatal(err)
}

iter := page.Iterator()
for iter.Next(context.TODO()) {
    fmt.Println(iter.Current())
}
if err := iter.Err(); err != nil {
    log.Fatal(err)
}
```

```bash cURL theme={null}
curl -X PATCH https://api.whop.com/api/v1/channels/chat_feed_xxxxxxxxxxxxx \
  -H "Authorization: Bearer $WHOP_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "moderation": {
      "ban_media": false,
      "ban_urls": false,
      "banned_words": ["spam", "scam"],
      "user_posts_cooldown_seconds": 10,
      "who_can_post": "admins",
      "who_can_react": "everyone"
    }
  }'

curl -G https://api.whop.com/api/v1/channels \
  -H "Authorization: Bearer $WHOP_API_KEY" \
  --data-urlencode "kind=community" \
  --data-urlencode "account_id=biz_xxxxxxxxxxxxx" \
  --data-urlencode "first=10"
```

`who_can_post` accepts `everyone` or `admins`. `who_can_react` accepts `everyone` or `no_one`. Account API keys can update community channels only.

## React to a message

```typescript TypeScript theme={null}
await client.reactions.create({
  resource_id: "msg_xxxxxxxxxxxxx",
  emoji: "😀", // Unicode or ':heart:' shortcode
});
```

```python Python theme={null}
client.reactions.create(
    resource_id="msg_xxxxxxxxxxxxx",
    emoji="😀",
)
```

```rust Rust theme={null}
client
    .reactions
    .create(
        &CreateReactionsRequest {
            resource_id: "msg_xxxxxxxxxxxxx".to_string(),
            emoji: Some("😀".to_string()), // Unicode or ':heart:' shortcode
            ..Default::default()
        },
        None,
    )
    .await?;
```

```go Go theme={null}
_, err := client.Reactions.Create(context.TODO(), &whopsdk.CreateReactionsRequest{
    ResourceID: "msg_xxxxxxxxxxxxx",
    Emoji:      whopsdk.String("😀"), // Unicode or ':heart:' shortcode
})
if err != nil {
    log.Fatal(err)
}
```

## Support channels

Support channels are 1:1 threads between a customer and an account. Use them for help desks or concierge flows. [Create Channel](https://docs.whop.com/api-reference/channels/create-channel.md) with `kind` set to `support` returns the existing channel if one already exists for the customer. To list your account's support inbox, call [List Channels](https://docs.whop.com/api-reference/channels/list-channels.md) with `kind` set to `support` and `support_role` set to `staff`.

```typescript TypeScript theme={null}
const support = await client.channels.create({
  kind: "support",
  account_id: "biz_xxxxxxxxxxxxx",
  customer_user_id: "user_xxxxxxxxxxxxx",
});

const supportChannels = await client.channels.list({
  kind: "support",
  support_role: "staff",
  account_id: "biz_xxxxxxxxxxxxx",
  status: "unresolved",
  order: "last_message_at",
  direction: "desc",
  first: 10,
});

for await (const item of supportChannels) {
  console.log(item);
}
```

```python Python theme={null}
support = client.channels.create(
    kind="support",
    account_id="biz_xxxxxxxxxxxxx",
    customer_user_id="user_xxxxxxxxxxxxx",
)

for item in client.channels.list(
    kind="support",
    support_role="staff",
    account_id="biz_xxxxxxxxxxxxx",
    status="unresolved",
    order="last_message_at",
    direction="desc",
    first=10,
):
    print(item)
```

```go Go theme={null}
support, err := client.Channels.Create(context.TODO(), &whopsdk.CreateChannelsRequest{
    Kind:           whopsdk.CreateChannelsRequestKindSupport,
    AccountID:      whopsdk.String("biz_xxxxxxxxxxxxx"),
    CustomerUserID: whopsdk.String("user_xxxxxxxxxxxxx"),
})
if err != nil {
    log.Fatal(err)
}
_ = support

page, err := client.Channels.List(context.TODO(), &whopsdk.ListChannelsRequest{
    Kind:        []*whopsdk.ListChannelsRequestKindItem{whopsdk.ListChannelsRequestKindItemSupport.Ptr()},
    SupportRole: whopsdk.ListChannelsRequestSupportRoleStaff.Ptr(),
    AccountID:   whopsdk.String("biz_xxxxxxxxxxxxx"),
    Status:      whopsdk.ListChannelsRequestStatusUnresolved.Ptr(),
    Order:       whopsdk.ListChannelsRequestOrderLastMessageAt.Ptr(),
    Direction:   whopsdk.ListChannelsRequestDirectionDesc.Ptr(),
    First:       whopsdk.Int(10),
})
if err != nil {
    log.Fatal(err)
}

iter := page.Iterator()
for iter.Next(context.TODO()) {
    fmt.Println(iter.Current())
}
if err := iter.Err(); err != nil {
    log.Fatal(err)
}
```

```bash cURL theme={null}
curl -X POST https://api.whop.com/api/v1/channels \
  -H "Authorization: Bearer $WHOP_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "kind": "support",
    "account_id": "biz_xxxxxxxxxxxxx",
    "customer_user_id": "user_xxxxxxxxxxxxx"
  }'

curl -G https://api.whop.com/api/v1/channels \
  -H "Authorization: Bearer $WHOP_API_KEY" \
  --data-urlencode "kind=support" \
  --data-urlencode "support_role=staff" \
  --data-urlencode "account_id=biz_xxxxxxxxxxxxx" \
  --data-urlencode "status=unresolved" \
  --data-urlencode "order=last_message_at" \
  --data-urlencode "direction=desc" \
  --data-urlencode "first=10"
```

## Required permissions

Add these from the [Permissions guide](https://docs.whop.com/developer/guides/permissions.md) before publishing your app.

| Permission | Needed for |
| - | - |
| `chat:read` | Reading messages, channels, reactions |
| `chat:message:create` | Posting messages |
| `chat:moderate` | Updating channel settings |
| `support_chat:create` | Opening support channels |
| `support_chat:read` | Reading support channels |

> [accordion] Message object shape
>
> ```typescript theme={null}
> {
>   id: string;
>   content: string | null;
>   created_at: string;
>   is_edited: boolean;
>   is_pinned: boolean;
>   message_type: "text" | "image" | "video" | "poll";
>   poll: { options: Array<{ id: string; text: string }> | null } | null;
>   poll_votes: Array<{ count: number; option_id: string | null }>;
>   reaction_counts: Array<{ count: number; emoji: string | null }>;
>   replying_to_message_id: string | null;
>   updated_at: string;
>   user: { id: string; name: string | null; username: string };
> }
> ```
>
> Full schema: see the [Messages API reference](https://docs.whop.com/api-reference/legacy/messages/message.md).

## Next steps

> [card] [Listen to events with webhooks](https://docs.whop.com/developer/guides/webhooks.md)
>
> Receive chat events on your server instead of polling.

> [card] [Send notifications](https://docs.whop.com/developer/guides/notifications.md)
>
> Push notifications to users who aren't live in chat.

> [card] [Upload files](https://docs.whop.com/developer/guides/upload-files.md)
>
> Attach images and videos to messages.

> [card] [Embed chat in your app](https://docs.whop.com/developer/guides/chat/quickstart.md)
>
> Drop-in Whop chat UI for your frontend (separate product).


## Related topics

- [API Versions](https://docs.whop.com/developer/api/versioning.md)
- [Chat Authentication](https://docs.whop.com/developer/guides/chat/authentication.md)
- [Channels API](https://docs.whop.com/api-reference/channels/channel.md)
