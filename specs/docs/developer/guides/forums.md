> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Forums

> Create forum posts, fetch threads, and add comments or reactions inside any Whop community.

Use the Forums API to publish posts, comment on existing threads, and react to posts inside a Whop community's forum experience. The `experience_id` scopes forum content, so each forum tile in a community sidebar is a separate experience.

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

## Create a post

Posts support Markdown. Optional fields let you pin the post, paywall it, or mark it as a mention.

<CodeGroup>
  ```typescript TypeScript theme={null}
  const post = await client.forumPosts.create({
    experience_id: "exp_xxxxxxxxxxxxx",
    content: "This is the post body in **Markdown**.",
    title: "Optional title for paywalled posts",
    pinned: false,
    is_mention: false,
    paywall_amount: 0, // cents; 500 = $5.00
  });
  ```

  ```python Python theme={null}
  post = client.forum_posts.create(
      experience_id="exp_xxxxxxxxxxxxx",
      content="This is the post body in **Markdown**.",
      title="Optional title for paywalled posts",
      pinned=False,
      is_mention=False,
      paywall_amount=0,
  )
  ```

  ```rust Rust theme={null}
  let post = client
      .forum_posts
      .create(
          &CreateForumPostsRequest {
              experience_id: "exp_xxxxxxxxxxxxx".to_string(),
              content: Some("This is the post body in **Markdown**.".to_string()),
              title: Some("Optional title for paywalled posts".to_string()),
              pinned: Some(false),
              is_mention: Some(false),
              paywall_amount: Some(0.0), // cents; 500 = $5.00
              ..Default::default()
          },
          None,
      )
      .await?;
  ```

  ```go Go theme={null}
  post, err := client.ForumPosts.Create(context.TODO(), &whopsdk.CreateForumPostsRequest{
      ExperienceID:  "exp_xxxxxxxxxxxxx",
      Content:       whopsdk.String("This is the post body in **Markdown**."),
      Title:         whopsdk.String("Optional title for paywalled posts"),
      Pinned:        whopsdk.Bool(false),
      IsMention:     whopsdk.Bool(false),
      PaywallAmount: whopsdk.Float64(0), // cents; 500 = $5.00
  })
  if err != nil {
      log.Fatal(err)
  }
  _ = post
  ```
</CodeGroup>

## Read posts

List operations paginate automatically.

<CodeGroup>
  ```typescript TypeScript theme={null}
  const posts = await client.forumPosts.list({
    experience_id: "exp_xxxxxxxxxxxxx",
    first: 10,
  });

  for await (const item of posts) {
    console.log(item);
  }

  const post = await client.forumPosts.retrieve({ id: "post_xxxxxxxxxxxxx" });
  ```

  ```python Python theme={null}
  for item in client.forum_posts.list(
      experience_id="exp_xxxxxxxxxxxxx",
      first=10,
  ):
      print(item)

  post = client.forum_posts.retrieve("post_xxxxxxxxxxxxx")
  ```

  ```rust Rust theme={null}
  let posts = client
      .forum_posts
      .list(
          &ForumPostsListQueryRequest {
              experience_id: "exp_xxxxxxxxxxxxx".to_string(),
              first: Some(10),
              ..Default::default()
          },
          None,
      )
      .await?;

  for item in posts.data {
      println!("{item:?}");
  }

  let post = client
      .forum_posts
      .retrieve(&"post_xxxxxxxxxxxxx".to_string(), None)
      .await?;
  ```

  ```go Go theme={null}
  page, err := client.ForumPosts.List(context.TODO(), &whopsdk.ListForumPostsRequest{
      ExperienceID: "exp_xxxxxxxxxxxxx",
      First:        whopsdk.Int(10),
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

  post, err := client.ForumPosts.Retrieve(context.TODO(), &whopsdk.RetrieveForumPostsRequest{
      ID: "post_xxxxxxxxxxxxx",
  })
  if err != nil {
      log.Fatal(err)
  }
  _ = post
  ```
</CodeGroup>

## Comment on a post

Comments are posts with a `parent_id`. Fetch them by passing the parent's ID to `list`.

<CodeGroup>
  ```typescript TypeScript theme={null}
  const comment = await client.forumPosts.create({
    experience_id: "exp_xxxxxxxxxxxxx",
    content: "Great post!",
    parent_id: "post_xxxxxxxxxxxxx",
  });

  const comments = await client.forumPosts.list({
    experience_id: "exp_xxxxxxxxxxxxx",
    parent_id: "post_xxxxxxxxxxxxx",
    first: 10,
  });

  for await (const item of comments) {
    console.log(item);
  }
  ```

  ```python Python theme={null}
  comment = client.forum_posts.create(
      experience_id="exp_xxxxxxxxxxxxx",
      content="Great post!",
      parent_id="post_xxxxxxxxxxxxx",
  )

  for item in client.forum_posts.list(
      experience_id="exp_xxxxxxxxxxxxx",
      parent_id="post_xxxxxxxxxxxxx",
      first=10,
  ):
      print(item)
  ```

  ```rust Rust theme={null}
  let comment = client
      .forum_posts
      .create(
          &CreateForumPostsRequest {
              experience_id: "exp_xxxxxxxxxxxxx".to_string(),
              content: Some("Great post!".to_string()),
              parent_id: Some("post_xxxxxxxxxxxxx".to_string()),
              ..Default::default()
          },
          None,
      )
      .await?;

  let comments = client
      .forum_posts
      .list(
          &ForumPostsListQueryRequest {
              experience_id: "exp_xxxxxxxxxxxxx".to_string(),
              parent_id: Some("post_xxxxxxxxxxxxx".to_string()),
              first: Some(10),
              ..Default::default()
          },
          None,
      )
      .await?;

  for item in comments.data {
      println!("{item:?}");
  }
  ```

  ```go Go theme={null}
  comment, err := client.ForumPosts.Create(context.TODO(), &whopsdk.CreateForumPostsRequest{
      ExperienceID: "exp_xxxxxxxxxxxxx",
      Content:      whopsdk.String("Great post!"),
      ParentID:     whopsdk.String("post_xxxxxxxxxxxxx"),
  })
  if err != nil {
      log.Fatal(err)
  }
  _ = comment

  page, err := client.ForumPosts.List(context.TODO(), &whopsdk.ListForumPostsRequest{
      ExperienceID: "exp_xxxxxxxxxxxxx",
      ParentID:     whopsdk.String("post_xxxxxxxxxxxxx"),
      First:        whopsdk.Int(10),
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

## Like a post

<Note>
  Forum reactions are always `:heart:`. Chat messages accept any emoji, but forum reactions ignore other values.
</Note>

<CodeGroup>
  ```typescript TypeScript theme={null}
  await client.reactions.create({
    resource_id: "post_xxxxxxxxxxxxx",
    emoji: ":heart:",
  });
  ```

  ```python Python theme={null}
  client.reactions.create(
      resource_id="post_xxxxxxxxxxxxx",
      emoji=":heart:",
  )
  ```

  ```rust Rust theme={null}
  client
      .reactions
      .create(
          &CreateReactionsRequest {
              resource_id: "post_xxxxxxxxxxxxx".to_string(),
              emoji: Some(":heart:".to_string()),
              ..Default::default()
          },
          None,
      )
      .await?;
  ```

  ```go Go theme={null}
  _, err := client.Reactions.Create(context.TODO(), &whopsdk.CreateReactionsRequest{
      ResourceID: "post_xxxxxxxxxxxxx",
      Emoji:      whopsdk.String(":heart:"),
  })
  if err != nil {
      log.Fatal(err)
  }
  ```
</CodeGroup>

## Advanced features

### Pinned posts

Pinned posts appear at the top of the feed. Use `pinned: true` when creating a post.

<CodeGroup>
  ```typescript TypeScript theme={null}
  await client.forumPosts.create({
    experience_id: "exp_xxxxxxxxxxxxx",
    content: "Important announcement!",
    pinned: true,
  });
  ```

  ```python Python theme={null}
  client.forum_posts.create(
      experience_id="exp_xxxxxxxxxxxxx",
      content="Important announcement!",
      pinned=True,
  )
  ```

  ```rust Rust theme={null}
  client
      .forum_posts
      .create(
          &CreateForumPostsRequest {
              experience_id: "exp_xxxxxxxxxxxxx".to_string(),
              content: Some("Important announcement!".to_string()),
              pinned: Some(true),
              ..Default::default()
          },
          None,
      )
      .await?;
  ```

  ```go Go theme={null}
  _, err := client.ForumPosts.Create(context.TODO(), &whopsdk.CreateForumPostsRequest{
      ExperienceID: "exp_xxxxxxxxxxxxx",
      Content:      whopsdk.String("Important announcement!"),
      Pinned:       whopsdk.Bool(true),
  })
  if err != nil {
      log.Fatal(err)
  }
  ```
</CodeGroup>

### Mention users

Use `<@username>` inline. Mentioned users get notified.

<CodeGroup>
  ```typescript TypeScript theme={null}
  await client.forumPosts.create({
    experience_id: "exp_xxxxxxxxxxxxx",
    content: "Hey <@username> check this out!",
  });
  ```

  ```python Python theme={null}
  client.forum_posts.create(
      experience_id="exp_xxxxxxxxxxxxx",
      content="Hey <@username> check this out!",
  )
  ```

  ```rust Rust theme={null}
  client
      .forum_posts
      .create(
          &CreateForumPostsRequest {
              experience_id: "exp_xxxxxxxxxxxxx".to_string(),
              content: Some("Hey <@username> check this out!".to_string()),
              ..Default::default()
          },
          None,
      )
      .await?;
  ```

  ```go Go theme={null}
  _, err := client.ForumPosts.Create(context.TODO(), &whopsdk.CreateForumPostsRequest{
      ExperienceID: "exp_xxxxxxxxxxxxx",
      Content:      whopsdk.String("Hey <@username> check this out!"),
  })
  if err != nil {
      log.Fatal(err)
  }
  ```
</CodeGroup>

### Paywalled posts

`paywall_amount` is in cents. Readers must pay to see the full content.

<CodeGroup>
  ```typescript TypeScript theme={null}
  await client.forumPosts.create({
    experience_id: "exp_xxxxxxxxxxxxx",
    title: "Premium post",
    content: "Exclusive content here",
    paywall_amount: 500, // $5.00
  });
  ```

  ```python Python theme={null}
  client.forum_posts.create(
      experience_id="exp_xxxxxxxxxxxxx",
      title="Premium post",
      content="Exclusive content here",
      paywall_amount=500,
  )
  ```

  ```rust Rust theme={null}
  client
      .forum_posts
      .create(
          &CreateForumPostsRequest {
              experience_id: "exp_xxxxxxxxxxxxx".to_string(),
              title: Some("Premium post".to_string()),
              content: Some("Exclusive content here".to_string()),
              paywall_amount: Some(500.0), // $5.00
              ..Default::default()
          },
          None,
      )
      .await?;
  ```

  ```go Go theme={null}
  _, err := client.ForumPosts.Create(context.TODO(), &whopsdk.CreateForumPostsRequest{
      ExperienceID:  "exp_xxxxxxxxxxxxx",
      Title:         whopsdk.String("Premium post"),
      Content:       whopsdk.String("Exclusive content here"),
      PaywallAmount: whopsdk.Float64(500), // $5.00
  })
  if err != nil {
      log.Fatal(err)
  }
  ```
</CodeGroup>

### Pagination with cursors

Beyond automatic pagination, you can step through pages manually using the `end_cursor` on the response's `page_info`.

<CodeGroup>
  ```typescript TypeScript theme={null}
  const firstPage = await client.forumPosts.list({
    experience_id: "exp_xxxxxxxxxxxxx",
    first: 10,
  });

  const next = await client.forumPosts.list({
    experience_id: "exp_xxxxxxxxxxxxx",
    first: 10,
    after: firstPage.response.page_info.end_cursor ?? undefined,
  });
  ```

  ```python Python theme={null}
  first = client.forum_posts.list(
      experience_id="exp_xxxxxxxxxxxxx",
      first=10,
  )

  next_page = client.forum_posts.list(
      experience_id="exp_xxxxxxxxxxxxx",
      first=10,
      after=first.response.page_info.end_cursor,
  )
  ```

  ```rust Rust theme={null}
  let first_page = client
      .forum_posts
      .list(
          &ForumPostsListQueryRequest {
              experience_id: "exp_xxxxxxxxxxxxx".to_string(),
              first: Some(10),
              ..Default::default()
          },
          None,
      )
      .await?;

  let next = client
      .forum_posts
      .list(
          &ForumPostsListQueryRequest {
              experience_id: "exp_xxxxxxxxxxxxx".to_string(),
              first: Some(10),
              after: first_page.page_info.end_cursor.clone(),
              ..Default::default()
          },
          None,
      )
      .await?;

  for item in next.data {
      println!("{item:?}");
  }
  ```

  ```go Go theme={null}
  firstPage, err := client.ForumPosts.List(context.TODO(), &whopsdk.ListForumPostsRequest{
      ExperienceID: "exp_xxxxxxxxxxxxx",
      First:        whopsdk.Int(10),
  })
  if err != nil {
      log.Fatal(err)
  }

  // Step to the next page manually
  nextPage, err := firstPage.GetNextPage(context.TODO())
  if err != nil {
      log.Fatal(err)
  }
  _ = nextPage
  ```
</CodeGroup>

## Required permissions

Add these to your app's permission list from the [Permissions guide](/developer/guides/permissions) before publishing.

| Permission          | Needed for                     |
| ------------------- | ------------------------------ |
| `forum:read`        | Reading posts and comments     |
| `forum:post:create` | Creating posts and comments    |
| `chat:read`         | Creating and reading reactions |

<Accordion title="Post object shape">
  ```typescript theme={null}
  {
    id: string;
    comment_count: number;
    content: string | null;
    is_edited: boolean;
    is_pinned: boolean;
    is_poster_admin: boolean;
    like_count: number | null;
    parent_id: string | null; // null = top-level, set = comment
    title: string | null;
    view_count: number | null;
    user: { id: string; name: string | null; username: string };
  }
  ```

  Full schema: see the [Forum posts API reference](/api-reference/forum-posts/forum-post).
</Accordion>

## Next steps

<CardGroup cols={2}>
  <Card title="Build a chatbot" href="/developer/guides/chat">
    Pair forum posts with live chat messages in the same community.
  </Card>

  <Card title="Listen to events with webhooks" href="/developer/guides/webhooks">
    React to forum posts and comments in realtime on your server.
  </Card>

  <Card title="Send notifications" href="/developer/guides/notifications">
    Push users back to your forum when there's something new.
  </Card>

  <Card title="Upload files" href="/developer/guides/upload-files">
    Attach images and videos to posts.
  </Card>
</CardGroup>
