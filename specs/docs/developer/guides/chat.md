> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Chat

> Send messages, manage channels, and react to events in any Whop community.

Use the Chat API to send messages, manage channels, open support conversations, and read reactions. Identify channels by `experience_id` or `channel_id`.

<Tip>
  This page covers the **server-side Chat API**, for calling Whop from your backend to post, read, or moderate messages. If you want to render a live Whop chat UI inside your own frontend, see the [embedded chat quickstart](/developer/guides/chat/quickstart) instead.
</Tip>

## Initialize the SDK

<CodeGroup>
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

      "github.com/whopio/whopsdk-go/client"
      "github.com/whopio/whopsdk-go/option"
  )

  client := client.NewWhop(option.WithToken(os.Getenv("WHOP_API_KEY")))
  ```
</CodeGroup>

## Send a message

Messages support Markdown and optional attachments uploaded via the [Files API](/developer/guides/upload-files).

<CodeGroup>
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
</CodeGroup>

## Read messages

List operations paginate automatically. Iterate the response and the SDK fetches additional pages for you.

<CodeGroup>
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
</CodeGroup>

## Manage channels

Update moderation settings (banned words, posting cooldowns, and who can post or react) or list every channel on an account.

<CodeGroup>
  ```typescript TypeScript theme={null}
  const channel = await client.chatChannels.update({
    id: "chat_feed_xxxxxxxxxxxxx",
    ban_media: false,
    ban_urls: false,
    banned_words: ["spam", "scam"],
    user_posts_cooldown_seconds: 10,
    who_can_post: "admins",
    who_can_react: "everyone",
  });

  const channels = await client.chatChannels.list({
    company_id: "biz_xxxxxxxxxxxxx",
    first: 10,
  });

  for await (const item of channels) {
    console.log(item);
  }
  ```

  ```python Python theme={null}
  channel = client.chat_channels.update(
      "chat_feed_xxxxxxxxxxxxx",
      ban_media=False,
      ban_urls=False,
      banned_words=["spam", "scam"],
      user_posts_cooldown_seconds=10,
      who_can_post="admins",
      who_can_react="everyone",
  )

  for item in client.chat_channels.list(
      company_id="biz_xxxxxxxxxxxxx",
      first=10,
  ):
      print(item)
  ```

  ```rust Rust theme={null}
  let channel = client
      .chat_channels
      .update(
          &"chat_feed_xxxxxxxxxxxxx".to_string(),
          &UpdateChatChannelsRequest {
              ban_media: Some(false),
              ban_urls: Some(false),
              banned_words: Some(vec!["spam".to_string(), "scam".to_string()]),
              user_posts_cooldown_seconds: Some(10),
              who_can_post: Some(WhoCanPostTypes::Admins),
              who_can_react: Some(WhoCanReactTypes::Everyone),
          },
          None,
      )
      .await?;

  let channels = client
      .chat_channels
      .list(
          &ChatChannelsListQueryRequest {
              company_id: "biz_xxxxxxxxxxxxx".to_string(),
              first: Some(10),
              ..Default::default()
          },
          None,
      )
      .await?;

  for item in channels.data {
      println!("{item:?}");
  }
  ```

  ```go Go theme={null}
  channel, err := client.ChatChannels.Update(context.TODO(), &whopsdk.UpdateChatChannelsRequest{
      ID:                       "chat_feed_xxxxxxxxxxxxx",
      BanMedia:                 whopsdk.Bool(false),
      BanURLs:                  whopsdk.Bool(false),
      BannedWords:              []string{"spam", "scam"},
      UserPostsCooldownSeconds: whopsdk.Int(10),
      WhoCanPost:               whopsdk.WhoCanPostTypesAdmins.Ptr(),
      WhoCanReact:              whopsdk.WhoCanReactTypesEveryone.Ptr(),
  })
  if err != nil {
      log.Fatal(err)
  }
  _ = channel

  page, err := client.ChatChannels.List(context.TODO(), &whopsdk.ListChatChannelsRequest{
      CompanyID: "biz_xxxxxxxxxxxxx",
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
</CodeGroup>

`who_can_post` accepts `everyone` or `admins`. `who_can_react` accepts `everyone` or `no_one`.

## React to a message

<CodeGroup>
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
</CodeGroup>

## Support channels

Support channels are 1:1 threads between a user and an account and are useful for help desks or concierge flows. `create` returns the existing channel if one already exists for the user.

<CodeGroup>
  ```typescript TypeScript theme={null}
  const support = await client.supportChannels.create({
    company_id: "biz_xxxxxxxxxxxxx",
    user_id: "user_xxxxxxxxxxxxx",
  });

  const supportChannels = await client.supportChannels.list({
    company_id: "biz_xxxxxxxxxxxxx",
    open: true,
    order: "last_post_sent_at",
    direction: "desc",
    first: 10,
  });

  for await (const item of supportChannels) {
    console.log(item);
  }
  ```

  ```python Python theme={null}
  support = client.support_channels.create(
      company_id="biz_xxxxxxxxxxxxx",
      user_id="user_xxxxxxxxxxxxx",
  )

  for item in client.support_channels.list(
      company_id="biz_xxxxxxxxxxxxx",
      open=True,
      order="last_post_sent_at",
      direction="desc",
      first=10,
  ):
      print(item)
  ```

  ```rust Rust theme={null}
  let support = client
      .support_channels
      .create(
          &CreateSupportChannelsRequest {
              company_id: "biz_xxxxxxxxxxxxx".to_string(),
              user_id: "user_xxxxxxxxxxxxx".to_string(),
              ..Default::default()
          },
          None,
      )
      .await?;

  let support_channels = client
      .support_channels
      .list(
          &SupportChannelsListQueryRequest {
              company_id: Some("biz_xxxxxxxxxxxxx".to_string()),
              open: Some(true),
              order: Some(MessageChannelOrder::LastPostSentAt),
              direction: Some(Direction::Desc),
              first: Some(10),
              ..Default::default()
          },
          None,
      )
      .await?;

  for item in support_channels.data {
      println!("{item:?}");
  }
  ```

  ```go Go theme={null}
  support, err := client.SupportChannels.Create(context.TODO(), &whopsdk.CreateSupportChannelsRequest{
      CompanyID: "biz_xxxxxxxxxxxxx",
      UserID:    "user_xxxxxxxxxxxxx",
  })
  if err != nil {
      log.Fatal(err)
  }
  _ = support

  page, err := client.SupportChannels.List(context.TODO(), &whopsdk.ListSupportChannelsRequest{
      CompanyID: whopsdk.String("biz_xxxxxxxxxxxxx"),
      Open:      whopsdk.Bool(true),
      Order:     whopsdk.MessageChannelOrderLastPostSentAt.Ptr(),
      Direction: whopsdk.DirectionDesc.Ptr(),
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
</CodeGroup>

## Required permissions

Add these from the [Permissions guide](/developer/guides/permissions) before publishing your app.

| Permission            | Needed for                            |
| --------------------- | ------------------------------------- |
| `chat:read`           | Reading messages, channels, reactions |
| `chat:message:create` | Posting messages                      |
| `chat:moderate`       | Updating channel settings             |
| `support_chat:create` | Opening support channels              |
| `support_chat:read`   | Reading support channels              |

<Accordion title="Message object shape">
  ```typescript theme={null}
  {
    id: string;
    content: string | null;
    created_at: string;
    is_edited: boolean;
    is_pinned: boolean;
    message_type: "text" | "image" | "video" | "poll";
    poll: { options: Array<{ id: string; text: string }> | null } | null;
    poll_votes: Array<{ count: number; option_id: string | null }>;
    reaction_counts: Array<{ count: number; emoji: string | null }>;
    replying_to_message_id: string | null;
    updated_at: string;
    user: { id: string; name: string | null; username: string };
  }
  ```

  Full schema: see the [Messages API reference](/api-reference/messages/message).
</Accordion>

## Next steps

<CardGroup cols={2}>
  <Card title="Listen to events with webhooks" href="/developer/guides/webhooks">
    Receive chat events on your server instead of polling.
  </Card>

  <Card title="Send notifications" href="/developer/guides/notifications">
    Push notifications to users who aren't live in chat.
  </Card>

  <Card title="Upload files" href="/developer/guides/upload-files">
    Attach images and videos to messages.
  </Card>

  <Card title="Embed chat in your app" href="/developer/guides/chat/quickstart">
    Drop-in Whop chat UI for your frontend (separate product).
  </Card>
</CardGroup>
