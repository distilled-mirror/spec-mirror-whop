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

# Upload Files Directly

> Upload files to Whop storage with presigned single-part and multipart uploads

Whop uploads are **direct-to-storage**. When you call [Create File](https://docs.whop.com/api-reference/files/create-file.md), Whop doesn't receive your bytes through the API. Instead it creates a file record and hands back a **presigned upload URL** that you `PUT` the bytes to yourself. The file becomes usable once those bytes land in Whop's storage.

This is the raw two-step flow behind the SDK's `files.create` and `files.retrieve` methods. Reach for it when you upload from the browser (so the bytes never touch your server) or work in a language without an SDK. It's also how you send files large enough to need multipart.

> [!NOTE]
>
> The `/files` endpoints require API version `2026-08-21-1` or later. Every request below pins it with the `Api-Version-Date` header. See [API versions](https://docs.whop.com/developer/api/versioning.md).

## The upload lifecycle

Every upload is three steps, whether it's one part or many:

1. **Create** the file record → Whop returns a presigned destination and the file starts as `pending`.
2. **Upload** the bytes straight to the presigned URLs.
3. **Poll** [Retrieve File](https://docs.whop.com/api-reference/files/retrieve-file.md) until `upload_status` is `ready` — then `url` and `size` are populated and the file's ID can be attached anywhere Whop accepts a document.

A file's `url` and `size` stay `null` until processing finishes. `upload_status` walks `pending` → `processing` → `ready` (or `failed`).

## Single-part upload

Use a single `PUT` for anything under a few hundred megabytes.

**1. Create the file**

Send the `filename` and, optionally, a `visibility` (`private` by default). The response's `upload_url` is where the bytes go, and `upload_headers` are the headers you must send with them.

```bash theme={null}
curl https://api.whop.com/api/v1/files \
  -H "Authorization: Bearer $WHOP_API_KEY" \
  -H "Api-Version-Date: 2026-08-21-1" \
  -H "Content-Type: application/json" \
  -d '{ "filename": "terms.pdf", "visibility": "private" }'
```

```json theme={null}
{
  "object": "file",
  "id": "file_XXXXXXXX",
  "filename": "terms.pdf",
  "content_type": "application/pdf",
  "size": null,
  "url": null,
  "removed_by_moderation": false,
  "upload_status": "pending",
  "visibility": "private",
  "created_at": "2026-08-21T18:24:00.000Z",
  "width": null,
  "height": null,
  "blurhash": null,
  "duration_seconds": null,
  "waveform_url": null,
  "upload_url": "https://upload.whop.com/uploads/...&signature=...",
  "upload_headers": { "Content-Type": "application/pdf" },
  "multipart_upload_id": null,
  "multipart_upload_urls": null,
  "multipart_chunk_size": null
}
```

**2. Upload the bytes**

`PUT` the file's contents to `upload_url`, sending every header from `upload_headers`. This request goes straight to Whop's file storage, **not** to `api.whop.com`, and carries no Whop `Authorization` header — the credentials are baked into the presigned URL.

```bash theme={null}
curl -X PUT "$UPLOAD_URL" \
  -H "Content-Type: application/pdf" \
  --data-binary @terms.pdf
```

> [!WARNING]
>
> The presigned URL expires one hour after it's issued. Create the file right before you upload, and request a fresh one if a `PUT` fails with `403` after a long delay. Multipart part URLs expire sooner, after 15 minutes.

**3. Poll until ready**

Retrieve the file by its ID until `upload_status` becomes `ready`. The `url` field then holds a download link: a permanent content delivery network URL when the file is `public`, or a signed, expiring one when it's `private`.

```bash theme={null}
curl https://api.whop.com/api/v1/files/file_XXXXXXXX \
  -H "Authorization: Bearer $WHOP_API_KEY" \
  -H "Api-Version-Date: 2026-08-21-1"
```

```json theme={null}
{
  "object": "file",
  "id": "file_XXXXXXXX",
  "filename": "terms.pdf",
  "content_type": "application/pdf",
  "size": 82345,
  "url": "https://files.whop.com/...&signature=...",
  "removed_by_moderation": false,
  "upload_status": "ready",
  "visibility": "private",
  "created_at": "2026-08-21T18:24:00.000Z",
  "width": null,
  "height": null,
  "blurhash": null,
  "duration_seconds": null,
  "waveform_url": null
}
```

## Multipart upload

For large files, upload in parts. Whop returns one presigned URL per part. You upload them (optionally in parallel), then tell Whop to assemble them.

Multipart is **required above 5 GB**, since a single upload request can't exceed that. It's also a good idea above \~100 MB, where one long upload is fragile. The file must be larger than 5 MB, since every part except the last must be at least 5 MB.

**1. Create the file with multipart enabled**

Pass `multipart: true` and the total `byte_size` so Whop knows how many presigned part URLs to return. The response carries a `multipart_upload_id`, one presigned `url` per `part_number`, and the `multipart_chunk_size` (bytes) each part except the last must be.

```bash theme={null}
curl https://api.whop.com/api/v1/files \
  -H "Authorization: Bearer $WHOP_API_KEY" \
  -H "Api-Version-Date: 2026-08-21-1" \
  -H "Content-Type: application/json" \
  -d '{ "filename": "release.mp4", "visibility": "private", "multipart": true, "byte_size": 12582912 }'
```

```json theme={null}
{
  "object": "file",
  "id": "file_XXXXXXXX",
  "filename": "release.mp4",
  "content_type": "video/mp4",
  "size": null,
  "url": null,
  "removed_by_moderation": false,
  "upload_status": "pending",
  "visibility": "private",
  "created_at": "2026-08-21T18:24:00.000Z",
  "width": null,
  "height": null,
  "blurhash": null,
  "duration_seconds": null,
  "waveform_url": null,
  "upload_url": null,
  "upload_headers": {},
  "multipart_upload_id": "2~aBcD1234...",
  "multipart_upload_urls": [
    { "part_number": 1, "url": "https://upload.whop.com/...&part_number=1&signature=..." },
    { "part_number": 2, "url": "https://upload.whop.com/...&part_number=2&signature=..." },
    { "part_number": 3, "url": "https://upload.whop.com/...&part_number=3&signature=..." }
  ],
  "multipart_chunk_size": 5242880
}
```

> [!NOTE]
>
> Multipart uploads support at most **10,000 parts** of 5 MB each — about 50 GB. A `byte_size` that would need more parts is rejected with a `400`.

**2. Upload each part and capture its ETag**

Slice the file into `multipart_chunk_size`-byte chunks (the final part may be smaller) and `PUT` each chunk to the matching part's `url`. Each upload response returns an **`ETag` header** — keep it alongside the `part_number`, you'll need both to finish.

```javascript theme={null}
import { readFile } from "node:fs/promises";

const bytes = await readFile("./release.mp4");
const chunkSize = created.multipart_chunk_size;

const parts = await Promise.all(
  created.multipart_upload_urls.map(async ({ part_number, url }) => {
    const start = (part_number - 1) * chunkSize;
    const chunk = bytes.subarray(start, start + chunkSize);

    const res = await fetch(url, { method: "PUT", body: chunk });
    if (!res.ok) throw new Error(`Part ${part_number} failed: ${res.status}`);

    // The ETag header comes back quoted, e.g. "\"abc123\"" — send it as-is.
    return { part_number, etag: res.headers.get("etag") };
  }),
);
```

**3. Complete the upload**

Hand back the `multipart_upload_id` and every part's `part_number` + `etag` to [Complete File Multipart Upload](https://docs.whop.com/api-reference/files/complete-file-multipart-upload.md). Whop assembles the parts into the final file.

```bash theme={null}
curl https://api.whop.com/api/v1/files/file_XXXXXXXX/complete \
  -H "Authorization: Bearer $WHOP_API_KEY" \
  -H "Api-Version-Date: 2026-08-21-1" \
  -H "Content-Type: application/json" \
  -d '{
    "multipart_upload_id": "2~aBcD1234...",
    "multipart_parts": [
      { "part_number": 1, "etag": "\"5eb63bbbe01eeed093cb22bb8f5acdc3\"" },
      { "part_number": 2, "etag": "\"9e107d9d372bb6826bd81d3542a419d6\"" },
      { "part_number": 3, "etag": "\"e4d909c290d0fb1ca068ffaddf22cbd0\"" }
    ]
  }'
```

> [!WARNING]
>
> Every part must be listed, in order, with the exact `ETag` returned when you uploaded it. An unknown `multipart_upload_id` or a mismatched `ETag` comes back as a `400` — re-upload the offending part and complete again.

**4. Poll until ready**

As with single-part uploads, retrieve the file until `upload_status` is `ready`. The ID is then ready to attach.

## Choosing a visibility

You set `visibility` at create time and can't change it afterward:

| Visibility | `url` you get | Use for |
| - | - | - |
| `private` (default) | Signed URL that expires | Legal documents, dispute evidence, user uploads |
| `public` | Permanent, unsigned content delivery network URL | Product images, thumbnails, branding, marketing assets |

Public files are cached on Whop's CDN and can't be revoked, so keep the default `private` for anything sensitive. See [Upload files](https://docs.whop.com/developer/guides/upload-files.md#accessing-private-files) for more.

## Using an uploaded file

Once a file is `ready`, pass its `id` anywhere Whop accepts a document — for example a [dispute's](https://docs.whop.com/api-reference/disputes/dispute) evidence or an account's legal documents.

## Next steps

> [card] [Upload files (SDK)](https://docs.whop.com/developer/guides/upload-files.md)
>
> How the SDK's `files.create` and `files.retrieve` methods wrap this flow.

> [card] [Create File](https://docs.whop.com/api-reference/files/create-file.md)
>
> Full request and response reference for the create endpoint.

> [card] [Complete File Multipart Upload](https://docs.whop.com/api-reference/files/complete-file-multipart-upload.md)
>
> Reference for assembling multipart parts.

> [card] [API versions](https://docs.whop.com/developer/api/versioning.md)
>
> Why the `Api-Version-Date` header matters and how to pin it.


## Related topics

- [Upload Files](https://docs.whop.com/developer/guides/upload-files.md)
- [Complete File Multipart Upload](https://docs.whop.com/api-reference/files/complete-file-multipart-upload.md)
- [Files API](https://docs.whop.com/api-reference/files/file.md)
