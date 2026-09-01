> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Upload Files Directly

> Upload files to Whop storage with presigned single-part and multipart uploads

Whop uploads are **direct-to-storage**. When you call [Create File](/api-reference/beta/files/create-file), Whop doesn't receive your bytes through the API. Instead it creates a file record and hands back a **presigned upload URL** that you `PUT` the bytes to yourself. The file becomes usable once those bytes land in Whop's storage.

This is the raw two-step flow behind the SDK's `files.create` and `files.retrieve` methods. Reach for it when you upload from the browser (so the bytes never touch your server) or work in a language without an SDK. It's also how you send files large enough to need multipart.

<Note>
  The `/files` endpoints require API version `2026-08-21-1` or later. Every request below pins it with the `Api-Version-Date` header. See [API versions](/developer/api/versioning).
</Note>

## The upload lifecycle

Every upload is three steps, whether it's one part or many:

1. **Create** the file record → Whop returns a presigned destination and the file starts as `pending`.
2. **Upload** the bytes straight to the presigned URLs.
3. **Poll** [Retrieve File](/api-reference/beta/files/retrieve-file) until `upload_status` is `ready` — then `url` and `size` are populated and the file's ID can be attached anywhere Whop accepts a document.

A file's `url` and `size` stay `null` until the bytes arrive. `upload_status` walks `pending` → `processing` → `ready` (or `failed`).

## Single-part upload

Use a single `PUT` for anything under a few hundred megabytes.

<Steps>
  <Step title="Create the file">
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
      "upload_status": "pending",
      "visibility": "private",
      "created_at": "2026-08-21T18:24:00.000Z",
      "upload_url": "https://upload.whop.com/uploads/...&signature=...",
      "upload_headers": { "Content-Type": "application/pdf" },
      "multipart_upload_id": null,
      "multipart_upload_urls": null,
      "multipart_chunk_size": null
    }
    ```
  </Step>

  <Step title="Upload the bytes">
    `PUT` the file's contents to `upload_url`, sending every header from `upload_headers`. This request goes straight to Whop's file storage, **not** to `api.whop.com`, and carries no Whop `Authorization` header — the credentials are baked into the presigned URL.

    ```bash theme={null}
    curl -X PUT "$UPLOAD_URL" \
      -H "Content-Type: application/pdf" \
      --data-binary @terms.pdf
    ```

    <Warning>
      The presigned URL expires one hour after it's issued. Create the file right before you upload, and request a fresh one if a `PUT` fails with `403` after a long delay. Multipart part URLs expire sooner, after 15 minutes.
    </Warning>
  </Step>

  <Step title="Poll until ready">
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
      "upload_status": "ready",
      "visibility": "private",
      "created_at": "2026-08-21T18:24:00.000Z"
    }
    ```
  </Step>
</Steps>

## Multipart upload

For large files, upload in parts. Whop returns one presigned URL per part. You upload them (optionally in parallel), then tell Whop to assemble them.

Multipart is **required above 5 GB**, since a single upload request can't exceed that. It's also a good idea above \~100 MB, where one long upload is fragile. The file must be larger than 5 MB, since every part except the last must be at least 5 MB.

<Steps>
  <Step title="Create the file with multipart enabled">
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
      "upload_status": "pending",
      "visibility": "private",
      "created_at": "2026-08-21T18:24:00.000Z",
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

    <Note>
      Multipart uploads support at most **10,000 parts** of 5 MB each — about 50 GB. A `byte_size` that would need more parts is rejected with a `400`.
    </Note>
  </Step>

  <Step title="Upload each part and capture its ETag">
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
  </Step>

  <Step title="Complete the upload">
    Hand back the `multipart_upload_id` and every part's `part_number` + `etag` to [Complete File Multipart Upload](/api-reference/beta/files/complete-file-multipart-upload). Whop assembles the parts into the final file.

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

    <Warning>
      Every part must be listed, in order, with the exact `ETag` returned when you uploaded it. An unknown `multipart_upload_id` or a mismatched `ETag` comes back as a `400` — re-upload the offending part and complete again.
    </Warning>
  </Step>

  <Step title="Poll until ready">
    As with single-part uploads, retrieve the file until `upload_status` is `ready`. The ID is then ready to attach.
  </Step>
</Steps>

## Choosing a visibility

You set `visibility` at create time and can't change it afterward:

| Visibility          | `url` you get                                    | Use for                                                |
| ------------------- | ------------------------------------------------ | ------------------------------------------------------ |
| `private` (default) | Signed URL that expires                          | Legal documents, dispute evidence, user uploads        |
| `public`            | Permanent, unsigned content delivery network URL | Product images, thumbnails, branding, marketing assets |

Public files are cached on Whop's CDN and can't be revoked, so keep the default `private` for anything sensitive. See [Upload files](/developer/guides/upload-files#accessing-private-files) for more.

## Using an uploaded file

Once a file is `ready`, pass its `id` anywhere Whop accepts a document — for example a [dispute's](/api-reference/beta/disputes/dispute) evidence or an account's legal documents.

## Next steps

<CardGroup cols={2}>
  <Card title="Upload files (SDK)" href="/developer/guides/upload-files">
    How the SDK's `files.create` and `files.retrieve` methods wrap this flow.
  </Card>

  <Card title="Create File" href="/api-reference/beta/files/create-file">
    Full request and response reference for the create endpoint.
  </Card>

  <Card title="Complete File Multipart Upload" href="/api-reference/beta/files/complete-file-multipart-upload">
    Reference for assembling multipart parts.
  </Card>

  <Card title="API versions" href="/developer/api/versioning">
    Why the `Api-Version-Date` header matters and how to pin it.
  </Card>
</CardGroup>
