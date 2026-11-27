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

# Add Apps

> Add apps like Chat and Course to your whop from the Whop App Store, then configure each app's settings.

Apps are the building blocks for your business on Whop. 

Create your own whop, then choose which apps to include. Add the **Chat** app for a community chat room. Add the **Course** app to sell a course. 

There are hundreds of apps to choose from, and you can add any of them from the Whop App Store.

## How to add apps to your whop

### 1. Go to the Whop App Store

You can find all the available apps in the Whop App Store.

* Inside your whop, select **Add app**
* Browse the different categories of apps to find the apps your members will find most valuable

### 2. Add apps to your whop

When you find an app you'd like to add, select **Add**.

The app appears in the left-hand column of your whop.

> [!TIP]
>
> You can add any number of apps. You can also add the same app multiple times. For example, add the Chat app more than once to create groups for different topics.

## Configure the app settings

Each app comes with its own settings that let you control the experience and functionality.

* **Chat app**: Choose who can post and react, ban links or images
* **Courses app**: Upload videos, add files, create quizzes
* **Events app**: Add upcoming events with details and links

To customize any app:

1. Select the app in your Whop
2. Select **Configure**

> [!TIP]
>
> Only you (and other team members) can see and edit the configuration settings
> of each app.

## Learn more about Whop apps

> [card] [What are Whop apps?](https://docs.whop.com/whop-apps/what-are-whop-apps.md)
>
> Learn more about what Whop apps are and how to use them.

> [card] [The Whop App Store](https://docs.whop.com/whop-apps/whop-app-store.md)
>
> Learn more about the Whop App Store.

## Next steps

To launch your internet business, make sure you’ve completed the rest of the setup steps:

> [card] [Set up pricing](https://docs.whop.com/manage-your-business/payment-processing/set-up-pricing.md)
>
> Choose your pricing model and what members get when they join.


## Related topics

- [Consumer Apps](https://docs.whop.com/whop-apps/consumer-apps.md)
- [The Whop App Store](https://docs.whop.com/whop-apps/whop-app-store.md)
- [Content Rewards](https://docs.whop.com/memberships-and-access/third-party-apps/content-rewards.md)
