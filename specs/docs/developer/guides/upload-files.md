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

# Upload Files

> Upload files through the API with a presigned URL, then attach them to Whop resources.

Upload files to Whop for use in courses, forums, profiles, disputes, and more.

## How uploads work

Uploading is a two-step flow, and the SDK helps with the first step:

1. **Create a file record.** `files.create` returns the new file's `id` plus a presigned `upload_url` and the `upload_headers` to send with it.
2. **Send the bytes yourself.** `PUT` the file's contents to `upload_url` with those headers. The SDK doesn't transfer the bytes for you.

Once the bytes land, the file processes and its `upload_status` becomes `ready`. Use `files.retrieve` to poll until it's `ready`, then attach the `id` to a resource and read the final `url`.

To upload straight from the browser so the bytes never touch your server, or to send a large file in parts, see [Upload files directly](https://docs.whop.com/developer/guides/direct-file-uploads.md).

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

  whopsdk "github.com/whopio/whopsdk-go/v2"
  whopclient "github.com/whopio/whopsdk-go/v2/client"
  "github.com/whopio/whopsdk-go/v2/option"
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

## Upload a file

Create the record, then `PUT` the bytes to the presigned URL that comes back.

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

console.log(file.id); // file_xxxxxxxxxxxxx — attach it once it's ready
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

print(file.id)  # file_xxxxxxxxxxxxx — attach it once it's ready
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

puts file.id # file_xxxxxxxxxxxxx — attach it once it's ready
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

fmt.Println(file.ID) // file_xxxxxxxxxxxxx — attach it once it's ready
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

println!("{}", file.id); // file_xxxxxxxxxxxxx — attach it once it's ready
```

## Public and private files

Every file you upload has a **visibility** that controls access. You choose visibility at upload time and can't change it later.

| Visibility | URL type | Who can access | Use for |
| - | - | - | - |
| `private` (default) | Signed URL that expires | Only your app, via the API | Message attachments, user-uploaded documents, sensitive content, AI-generated files |
| `public` | Permanent, unsigned content delivery network URL | Anyone with the link | Product images, thumbnails, branding, marketing assets |

> [!WARNING]
>
> **Set the visibility before uploading.** Public files are cached on Whop's CDN and accessible to anyone with the URL — there is no way to revoke access. If the content is user-specific or sensitive, keep the default `private`.

### Uploading a public file

Pass a `public` visibility when creating the file, then upload the bytes the same way:

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

Attaching a file to a resource moves it to the storage that matches that resource's own visibility. An explicit `visibility` mainly matters for files you link to directly without attaching.

### Confirm the upload finished

After the bytes land, the file processes asynchronously. Retrieve it until `upload_status` leaves `pending` and `processing`. When it's `ready`, read the final `url`. When it's `failed`, stop and surface the error.

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

### Accessing private files

Private file URLs expire. To get a fresh URL, retrieve the file by ID again:

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

## Using uploaded files

Once the file is `ready`, pass its `id` in any API call that accepts file attachments:

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

## File properties

| Property | Description |
| - | - |
| `id` | Unique identifier (e.g., `file_xxxxxxxxxxxxx`) |
| `filename` | Original filename |
| `content_type` | Media type (e.g., `image/jpeg`), inferred from the file name's extension |
| `size` | File size in bytes. `null` until the upload finishes |
| `url` | Where to download the file: a permanent content delivery network URL when public, a signed expiring URL when private. `null` until the upload finishes |
| `upload_status` | Status: `pending`, `processing`, `ready`, or `failed` |
| `visibility` | `public` or `private` |
| `upload_url` | A presigned URL to `PUT` the bytes to. Only present in the `create` response |
| `upload_headers` | Headers to send with the upload `PUT`. Only present in the `create` response |


## Related topics

- [Upload Files Directly](https://docs.whop.com/developer/guides/direct-file-uploads.md)
- [Upload Dispute Evidence](https://docs.whop.com/api-reference/disputes/upload-dispute-evidence.md)
- [Update Dispute](https://docs.whop.com/api-reference/disputes/update-dispute.md)
