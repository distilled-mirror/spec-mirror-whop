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

# Idempotent Requests

> Retry POST requests with an idempotency key, so a request is never executed twice.

Every authenticated `POST` on the Current API accepts an `Idempotency-Key` header. When a request with a key succeeds, its response is stored for 24 hours. Retrying with the same key replays the stored response instead of executing the request again. This makes it safe to retry a request that timed out or errored on your side without risking a duplicate charge, transfer, or resource.

Any unique string up to 255 bytes works as a key — a UUID is a good default. Generate a fresh key for every distinct operation, and reuse that exact key only to retry that operation.

```bash theme={null}
curl -X POST https://api.whop.com/api/v1/transfers \
  -H "Authorization: Bearer $WHOP_API_KEY" \
  -H "Idempotency-Key: d9105228-4a08-46b1-8b91-42fed586d383" \
  -H "Content-Type: application/json" \
  -d '{"type": "balance", "amount": "10.00", "currency": "usd", "destination_id": "user_XXXXXXXX"}'
```

| Your retry | You get |
| - | - |
| Same key, same request | The original response, replayed — not a second execution |
| Same key, different request | A `400` error |
| Same key, while the first request is still running | A `409` error — retry after the first request finishes |
| Same key, more than 24 hours later | A fresh execution, as if the key were new |

Replayed responses are byte-for-byte identical to the original — including the status code — and carry an `Idempotent-Replayed: true` header so you can tell a replay apart from a fresh execution.

> [!NOTE]
>
> **Error responses are replayed too.** If the original request failed with a `4xx`, retrying with the same key returns that same error. That failure was the real outcome of the request — send a new key with the corrected request instead.

A few details worth knowing:

* "Same request" means the method, path, query string, body, and `Api-Version-Date` all match the original exactly. Any difference is treated as a key reuse and rejected with a `400` whose `error.code` is `idempotency_key_mismatch`.
* Keys are scoped to the authenticated caller. Two different API keys or users can use the same key value without colliding.
* The header is ignored on unauthenticated requests and on non-`POST` methods — `GET`, `PATCH`, and `DELETE` requests are safe to retry.
* While the first request with a key is still running, a retry returns a `409` whose `error.code` is `idempotency_request_in_progress`. If the first request failed with an unknown outcome (for example, a server crash mid-request), retries keep returning that `409` until the key expires. The API never risks executing a request twice.


## Related topics

- [Troubleshoot Whop Integrations](https://docs.whop.com/developer/troubleshooting.md)
- [Decline DM Request](https://docs.whop.com/api-reference/channels/decline-dm-request.md)
- [Accept DM Request](https://docs.whop.com/api-reference/channels/accept-dm-request.md)
