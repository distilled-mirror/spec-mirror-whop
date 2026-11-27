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

# Run Events on Whop

> Sell event tickets on Whop: create a product, charge a one-time price or use a free waitlist, add a chat for attendees, and share your checkout link.

Whop continues to improve support for hosting events. In the meantime, many creators use Whop to accept payments and bring social features to their events.

## Run an event on Whop in 4 simple steps

### 1. Create a product for your event

Go to your whop > select **Products** > **Add product**. Name your event and
write a catchy headline so guests know what to expect.

### 2. Pick your price

Choose a one-time price for a ticket or make it free with a waitlist. You
can always adjust later.

### 3. Add a Chat for attendees

Add the [**Chat**](https://whop.com/apps/app_xml5hbizmZPgUT/install/) app so
patrons can coordinate, share updates, and connect before and after the
event.

### 4. Share your link with guests

Copy your product checkout link and share it anywhere—your website, socials,
email, or DMs.


## Related topics

- [Brick and Mortar](https://docs.whop.com/supported-business-models/brick-and-mortar.md)
- [Newsletters](https://docs.whop.com/supported-business-models/newsletters.md)
- [Whop Ads](https://docs.whop.com/manage-your-business/growth-marketing/ads.md)
