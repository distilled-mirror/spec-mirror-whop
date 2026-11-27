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

# Get Started with Whop

> Start selling on Whop with setup guides for popular business models, including courses, agencies, coaching, newsletters, paid groups, SaaS, and events.

## For merchants

If you're new to Whop, [launch your business](https://docs.whop.com/launch-your-business.md) to create your account and first whop, then set up one of these popular business models:

> [card] [Educational programs](https://docs.whop.com/supported-business-models/educational-programs.md)
>
> Sell courses, workshops, and other educational programs.

> [card] [Agency services](https://docs.whop.com/supported-business-models/agency-services.md)
>
> Send invoices, chat with clients, and get paid.

> [card] [Coaching](https://docs.whop.com/supported-business-models/coaches.md)
>
> Charge for your services and organize your clients in one spot.

> [card] [Newsletters](https://docs.whop.com/supported-business-models/newsletters.md)
>
> Publish long form writing and charge subscribers for exclusive content.

> [card] [Paid groups](https://docs.whop.com/supported-business-models/paid-groups.md)
>
> Build a paid community where members connect and share alpha.

> [card] [SaaS](https://docs.whop.com/supported-business-models/saas.md)
>
> Launch your app with built-in payments and user management.

> [card] [Events](https://docs.whop.com/supported-business-models/events.md)
>
> Run paid masterminds, events, and give attendees a unified place to chat.

> [card] [DTC Ecommerce](https://docs.whop.com/supported-business-models/dtc-ecommerce.md)
>
> Sell physical products online with an out of the box social layer.

> [card] [Brick and Mortar](https://docs.whop.com/supported-business-models/brick-and-mortar.md)
>
> Accept in-store payments and manage orders for your physical location.


## Related topics

- [Content Rewards](https://docs.whop.com/memberships-and-access/third-party-apps/content-rewards.md)
- [Whop Ads](https://docs.whop.com/manage-your-business/growth-marketing/ads.md)
- [Coaches](https://docs.whop.com/supported-business-models/coaches.md)
