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

# Events API

> An Event records conversion or engagement activity for an account, such as page views, purchases, or leads.

Each event ties the action to the [person](https://docs.whop.com/api-reference/people/person) who took it, so activity can be attributed to the ads and links that drove it.

Use the Events API to send new tracking events, list recent identity-linked events for an account, and inspect the events recorded for a person. The resource also exposes an anonymized read mode — the pulse feed — a platform-wide snapshot of recent purchases that carries nothing identifying. The pulse feed is public; other Events endpoints require authentication and are scoped to an account.

Events are only as good as the pixel sending them, so [Validate Pixel](https://docs.whop.com/api-reference/events/validate-pixel.md) answers whether an account's pixel is working: it reads the events the pixel has sent, and when you pass a `url` whose page hasn't sent any lately, it fetches that page and looks for the pixel in its source. Use it before launching an ad to confirm its destination is tracked, or in a setup flow to tell a merchant whether their install is live.

## Endpoints

| Endpoint | Request |
| - | - |
| [List Events](https://docs.whop.com/api-reference/events/list-events.md) | GET `/events` |
| [Retrieve the pulse feed](https://docs.whop.com/api-reference/events/retrieve-the-pulse-feed.md) | GET `/events/pulse` |
| [Create Event](https://docs.whop.com/api-reference/events/create-event.md) | POST `/events` |
| [Validate Pixel](https://docs.whop.com/api-reference/events/validate-pixel.md) | POST `/events/validate_pixel` |


## Related topics

- [Send Server-Side Events](https://docs.whop.com/developer/ads/events-api.md)
- [Events created](https://docs.whop.com/api-reference/events/events-created.md)
- [Create Event](https://docs.whop.com/api-reference/events/create-event.md)
