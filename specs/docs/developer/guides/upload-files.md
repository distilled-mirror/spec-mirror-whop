> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Upload Files

> Upload images and files for use across the Whop platform

Upload files to Whop for use in courses, forums, profiles, disputes, and more.

## How uploads work

Uploading is a two-step flow, and the SDK helps with the first step:

1. **Create a file record.** `files.create` returns the new file's `id` plus a presigned `upload_url` and the `upload_headers` to send with it.
2. **Send the bytes yourself.** `PUT` the file's contents to `upload_url` with those headers. The SDK doesn't transfer the bytes for you.

Once the bytes land, the file processes and its `upload_status` becomes `ready`. You can attach the `id` to a resource right away, and use `files.retrieve` to poll for the final `url`.

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

  ```ruby Ruby theme={null}
  require "whop_sdk"

  client = Whop_sdk::Client.new(
    token: ENV.fetch("WHOP_API_KEY"),
  )
  ```

  ```go Go theme={null}
  import (
    "context"
    "os"

    whopsdk "github.com/whopio/whopsdk-go"
    whopclient "github.com/whopio/whopsdk-go/client"
    "github.com/whopio/whopsdk-go/option"
  )

  client := whopclient.NewWhop(
    option.WithToken(os.Getenv("WHOP_API_KEY")),
  )
  ```

  ```rust Rust theme={null}
  use whop_sdk::prelude::*;

  let config = ClientConfig {
      token: Some(std::env::var("WHOP_API_KEY").expect("WHOP_API_KEY is not set")),
      ..Default::default()
  };
  let client = Whop::new(config).expect("Failed to build client");
  ```
</CodeGroup>

## Upload a file

Create the record, then `PUT` the bytes to the presigned URL that comes back.

<CodeGroup>
  ```typescript TypeScript theme={null}
  import { readFile } from "node:fs/promises";

  // 1. Create the file record and get a presigned upload URL
  const file = await client.files.create({ filename: "photo.jpg" });

  // 2. Send the bytes to storage yourself
  const bytes = await readFile("./photo.jpg");
  await fetch(file.upload_url!, {
    method: "PUT",
    headers: file.upload_headers as Record<string, string>,
    body: bytes,
  });

  console.log(file.id); // file_xxxxxxxxxxxxx — ready to attach
  ```

  ```python Python theme={null}
  import requests

  # 1. Create the file record and get a presigned upload URL
  file = client.files.create(filename="photo.jpg")

  # 2. Send the bytes to storage yourself
  with open("./photo.jpg", "rb") as contents:
      requests.put(
          file.upload_url,
          headers=file.upload_headers or {},
          data=contents,
      )

  print(file.id)  # file_xxxxxxxxxxxxx — ready to attach
  ```

  ```ruby Ruby theme={null}
  require "net/http"

  # 1. Create the file record and get a presigned upload URL
  file = client.files.create(filename: "photo.jpg")

  # 2. Send the bytes to storage yourself
  uri = URI(file.upload_url)
  request = Net::HTTP::Put.new(uri)
  (file.upload_headers || {}).each { |key, value| request[key] = value }
  request.body = File.binread("./photo.jpg")
  Net::HTTP.start(uri.hostname, uri.port, use_ssl: true) { |http| http.request(request) }

  puts file.id # file_xxxxxxxxxxxxx — ready to attach
  ```

  ```go Go theme={null}
  import (
    "bytes"
    "fmt"
    "net/http"
    "os"
  )

  // 1. Create the file record and get a presigned upload URL
  file, err := client.Files.Create(context.TODO(), &whopsdk.CreateFilesRequest{
    Filename: "photo.jpg",
  })
  if err != nil {
    return err
  }

  // 2. Send the bytes to storage yourself
  data, err := os.ReadFile("./photo.jpg")
  if err != nil {
    return err
  }
  request, err := http.NewRequest(http.MethodPut, *file.UploadURL, bytes.NewReader(data))
  if err != nil {
    return err
  }
  for key, value := range file.UploadHeaders {
    request.Header.Set(key, fmt.Sprint(value))
  }
  if _, err := http.DefaultClient.Do(request); err != nil {
    return err
  }

  fmt.Println(file.ID) // file_xxxxxxxxxxxxx — ready to attach
  ```

  ```rust Rust theme={null}
  // 1. Create the file record and get a presigned upload URL
  let file = client
      .files
      .create(
          &CreateFilesRequest {
              filename: "photo.jpg".to_string(),
              ..Default::default()
          },
          None,
      )
      .await?;

  // 2. Send the bytes to storage yourself
  let contents = std::fs::read("./photo.jpg")?;
  let mut request = reqwest::Client::new()
      .put(file.upload_url.as_deref().unwrap())
      .body(contents);
  if let Some(headers) = &file.upload_headers {
      for (key, value) in headers {
          if let Some(value) = value.as_str() {
              request = request.header(key, value);
          }
      }
  }
  request.send().await?;

  println!("{}", file.id); // file_xxxxxxxxxxxxx — ready to attach
  ```
</CodeGroup>

## Public and private files

Every file you upload has a **visibility** that controls access. You choose visibility at upload time and can't change it later.

| Visibility          | URL type                                         | Who can access             | Use for                                                                             |
| ------------------- | ------------------------------------------------ | -------------------------- | ----------------------------------------------------------------------------------- |
| `private` (default) | Signed URL that expires                          | Only your app, via the API | Message attachments, user-uploaded documents, sensitive content, AI-generated files |
| `public`            | Permanent, unsigned content delivery network URL | Anyone with the link       | Product images, thumbnails, branding, marketing assets                              |

<Warning>
  **Set the visibility before uploading.** Public files are cached on Whop's CDN and accessible to anyone with the URL — there is no way to revoke access. If the content is user-specific or sensitive, keep the default `private`.
</Warning>

### Uploading a public file

Pass a `public` visibility when creating the file, then upload the bytes the same way:

<CodeGroup>
  ```typescript TypeScript theme={null}
  const file = await client.files.create({
    filename: "product-cover.jpg",
    visibility: "public",
  });

  // PUT the bytes to file.upload_url, then file.url is a permanent CDN URL
  ```

  ```python Python theme={null}
  file = client.files.create(
      filename="product-cover.jpg",
      visibility="public",
  )

  # PUT the bytes to file.upload_url, then file.url is a permanent CDN URL
  ```

  ```ruby Ruby theme={null}
  file = client.files.create(
    filename: "product-cover.jpg",
    visibility: "public",
  )

  # PUT the bytes to file.upload_url, then file.url is a permanent CDN URL
  ```

  ```go Go theme={null}
  file, err := client.Files.Create(context.TODO(), &whopsdk.CreateFilesRequest{
    Filename:   "product-cover.jpg",
    Visibility: whopsdk.CreateFilesRequestVisibilityPublic.Ptr(),
  })

  // PUT the bytes to file.UploadURL, then file.URL is a permanent CDN URL
  ```

  ```rust Rust theme={null}
  let file = client
      .files
      .create(
          &CreateFilesRequest {
              filename: "product-cover.jpg".to_string(),
              visibility: Some(CreateFilesRequestVisibility::Public),
              ..Default::default()
          },
          None,
      )
      .await?;

  // PUT the bytes to file.upload_url, then file.url is a permanent CDN URL
  ```
</CodeGroup>

Attaching a file to a resource moves it to the storage that matches that resource's own visibility. An explicit `visibility` mainly matters for files you link to directly without attaching.

### Confirm the upload finished

After the bytes land, the file processes asynchronously. Retrieve it until `upload_status` leaves `pending` and `processing`. When it's `ready`, read the final `url`. When it's `failed`, stop and surface the error.

<CodeGroup>
  ```typescript TypeScript theme={null}
  let uploaded = await client.files.retrieve({ id: file.id });

  while (uploaded.upload_status === "pending" || uploaded.upload_status === "processing") {
    await new Promise((resolve) => setTimeout(resolve, 1000));
    uploaded = await client.files.retrieve({ id: file.id });
  }

  if (uploaded.upload_status !== "ready") {
    throw new Error(`Upload failed: ${uploaded.upload_status}`);
  }

  console.log(uploaded.url);
  ```

  ```python Python theme={null}
  import time

  uploaded = client.files.retrieve(file.id)

  while uploaded.upload_status in ("pending", "processing"):
      time.sleep(1)
      uploaded = client.files.retrieve(file.id)

  if uploaded.upload_status != "ready":
      raise RuntimeError(f"Upload failed: {uploaded.upload_status}")

  print(uploaded.url)
  ```

  ```ruby Ruby theme={null}
  uploaded = client.files.retrieve(id: file.id)

  while %w[pending processing].include?(uploaded.upload_status)
    sleep 1
    uploaded = client.files.retrieve(id: file.id)
  end

  raise "Upload failed: #{uploaded.upload_status}" unless uploaded.upload_status == "ready"

  puts uploaded.url
  ```

  ```go Go theme={null}
  import "time"

  uploaded, err := client.Files.Retrieve(context.TODO(), &whopsdk.RetrieveFilesRequest{ID: file.ID})
  if err != nil {
    return err
  }

  for uploaded.UploadStatus == whopsdk.FileUploadStatusPending ||
    uploaded.UploadStatus == whopsdk.FileUploadStatusProcessing {
    time.Sleep(time.Second)
    uploaded, err = client.Files.Retrieve(context.TODO(), &whopsdk.RetrieveFilesRequest{ID: file.ID})
    if err != nil {
      return err
    }
  }

  if uploaded.UploadStatus != whopsdk.FileUploadStatusReady {
    return fmt.Errorf("upload failed: %s", uploaded.UploadStatus)
  }

  fmt.Println(*uploaded.URL)
  ```

  ```rust Rust theme={null}
  let mut uploaded = client.files.retrieve(&file.id, None).await?;

  while matches!(
      uploaded.upload_status,
      FileUploadStatus::Pending | FileUploadStatus::Processing
  ) {
      tokio::time::sleep(std::time::Duration::from_secs(1)).await;
      uploaded = client.files.retrieve(&file.id, None).await?;
  }

  if !matches!(uploaded.upload_status, FileUploadStatus::Ready) {
      panic!("upload failed: {}", uploaded.upload_status);
  }

  println!("{}", uploaded.url.unwrap());
  ```
</CodeGroup>

### Accessing private files

Private file URLs expire. To get a fresh URL, retrieve the file by ID again:

<CodeGroup>
  ```typescript TypeScript theme={null}
  const file = await client.files.retrieve({ id: "file_xxxxxxxxxxxxx" });
  console.log(file.url); // fresh signed URL
  ```

  ```python Python theme={null}
  file = client.files.retrieve("file_xxxxxxxxxxxxx")
  print(file.url)  # fresh signed URL
  ```

  ```ruby Ruby theme={null}
  file = client.files.retrieve(id: "file_xxxxxxxxxxxxx")
  puts file.url # fresh signed URL
  ```

  ```go Go theme={null}
  file, err := client.Files.Retrieve(context.TODO(), &whopsdk.RetrieveFilesRequest{
    ID: "file_xxxxxxxxxxxxx",
  })
  fmt.Println(*file.URL) // fresh signed URL
  ```

  ```rust Rust theme={null}
  let file = client
      .files
      .retrieve(&"file_xxxxxxxxxxxxx".to_string(), None)
      .await?;
  println!("{}", file.url.unwrap()); // fresh signed URL
  ```
</CodeGroup>

## Using uploaded files

Once uploaded, pass the file `id` in any API call that accepts file attachments:

<CodeGroup>
  ```typescript TypeScript theme={null}
  await client.courses.update({
    id: "course_xxxxxxxxxxxxx",
    thumbnail: { id: file.id },
  });
  ```

  ```python Python theme={null}
  client.courses.update(
      "course_xxxxxxxxxxxxx",
      thumbnail={"id": file.id},
  )
  ```

  ```ruby Ruby theme={null}
  client.courses.update(
    id: "course_xxxxxxxxxxxxx",
    thumbnail: { id: file.id },
  )
  ```

  ```go Go theme={null}
  client.Courses.Update(context.TODO(), &whopsdk.UpdateCoursesRequest{
    ID:        "course_xxxxxxxxxxxxx",
    Thumbnail: &whopsdk.UpdateCoursesRequestThumbnail{ID: file.ID},
  })
  ```

  ```rust Rust theme={null}
  client
      .courses
      .update(
          &"course_xxxxxxxxxxxxx".to_string(),
          &UpdateCoursesRequest {
              thumbnail: Some(UpdateCoursesRequestThumbnail { id: file.id }),
              ..Default::default()
          },
          None,
      )
      .await?;
  ```
</CodeGroup>

## File properties

| Property         | Description                                                                                                                                            |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `id`             | Unique identifier (e.g., `file_xxxxxxxxxxxxx`)                                                                                                         |
| `filename`       | Original filename                                                                                                                                      |
| `content_type`   | Media type (e.g., `image/jpeg`). `null` until processing finishes                                                                                      |
| `size`           | File size in bytes. `null` until the upload finishes                                                                                                   |
| `url`            | Where to download the file: a permanent content delivery network URL when public, a signed expiring URL when private. `null` until the upload finishes |
| `upload_status`  | Status: `pending`, `processing`, `ready`, or `failed`                                                                                                  |
| `visibility`     | `public` or `private`                                                                                                                                  |
| `upload_url`     | A presigned URL to `PUT` the bytes to. Only present in the `create` response                                                                           |
| `upload_headers` | Headers to send with the upload `PUT`. Only present in the `create` response                                                                           |
