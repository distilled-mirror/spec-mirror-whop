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

# Upload Legal Documents

> Upload your terms of service, privacy policy, and other legal documents to your whop, and require customers to agree to them before checkout.

Adding legal documents to your Whop helps cut down on fake disputes and chargebacks, and your customers will know exactly what to expect when they buy from you.

When someone buys access to your Whop, they automatically agree to Whop's terms. But you can add your own legal documents for more protection.

## Which legal documents you can upload

You can upload 4 different legal documents to your Whop:

* **Terms of Service**: Your business terms and conditions
* **Privacy Policy**: How you handle customer data
* **Return Policy**: Your refund and return procedures
* **EULA** (End User License Agreement): Software or digital product usage terms

> [!TIP]
>
> Set up your legal documents before you sell anything. This way, you're
> protected from day one, and your customers know what they're signing up for.

## How to upload your legal documents

To add your legal documents:

1. Go to **Dashboard** > **Settings** > **Legal** (3rd option under Settings)
2. Select **Upload file** to the right of the document type you want to add
3. Either drag your file or select **Browse files** to find it on your computer
4. Once you pick your file, select **Upload file** at the bottom left
5. Repeat steps 2 to 4 for any other documents you want to upload

> [!TIP]
>
> You can check your uploaded files by selecting View file next to each document
> or remove one by selecting Delete.

## How to require customers to agree to your terms before checkout

To have customers agree to your terms before purchasing:

1. Go to **Dashboard** > **Settings** > **Legal** (3rd option under Settings)
2. Under **Options**, toggle **Require terms and conditions acceptance**

> [!TIP]
>
> When you turn this on, customers have to check a box saying they agree to your
> terms before they can buy anything.


## Related topics

- [Review Refund Claims in the Resolution Center](https://docs.whop.com/payments/resolution-center.md)
- [Respond to Refund Requests in the Resolution Center](https://docs.whop.com/manage-your-business/manage-payments/resolution-center.md)
- [Managing Dispute Rates](https://docs.whop.com/trust-and-safety/account-health/managing-dispute-rates.md)
