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

# Consumer Apps

> The Whop apps you can add to your whop for members, such as Chat, Courses, and Events.

Building a thriving community requires more than great content—it takes engaging spaces where people can learn, share, and connect. Whop apps make it effortless by giving you plug-and-play experiences that feel tailor-made for your members.

## Foundational community apps

These core apps form the backbone of most online communities. Add one or all to craft the right mix for your audience.

### Community & education

* **Chat** – Real-time group discussions. [Install](https://whop.com/apps/app_xml5hbizmZPgUT/install)
* **Courses** – Multi-lesson learning with videos, quizzes, and progress tracking. [Install](https://whop.com/apps/app_0vPZThfBpAwLo/install)
* **Forums** – Threaded conversations for deeper topics. [Install](https://whop.com/apps/app_dYfm2IdXhDMquv/install)
* **Events** – Host live or in-person events and optionally charge per ticket. [Install](https://whop.com/apps/app_RnVNENx5vtP8Zp/install)

### Content & resources

* **Content** – Notion-style pages with rich media. [Install](https://whop.com/apps/app_mmjPLOxIgL1qxe/install)
* **Files** – Share templates, e-books, and digital assets. [Install](https://whop.com/apps/app_5UWRrs7haZZeiQ/install)
* **Calendar Bookings** – Let members book 1-on-1 time with you (paid or free). [Install](https://whop.com/apps/app_dC2dkRARmXE4qf/install)

![Add app](https://mintcdn.com/whop/XkQkLHsY-Hk8KVyG/how-to-videos/addwhopapp720p30fps.gif?s=caa5f823baafef727a330f5b35378bd9)

## Social apps & games

Elevate engagement with interactive experiences that keep members coming back.

* **HQ Trivia-style games** – Run live game shows for your community.
* **Video networking** – Omegle-style random video chats between members.
* **Anonymous forums** – YikYak-style posts for candid conversation.
* **Community giveaways** – Platform-wide contests and rewards.

> **Tip:** Higher engagement means higher retention, which translates into more stable recurring revenue for your community.

## Next steps

1. Open your whop dashboard → Apps → *Add app*.
2. Browse the community app category.
3. Activate the apps that best fit your culture and goals.

Experiment with different combinations—every community is unique, and you can always add or remove apps based on member feedback.


## Related topics

- [Direct-to-consumer E-commerce](https://docs.whop.com/supported-business-models/dtc-ecommerce.md)
- [Add Apps](https://docs.whop.com/add-apps.md)
- [Local Payment Methods](https://docs.whop.com/payments-and-billing/local-payment-methods.md)
