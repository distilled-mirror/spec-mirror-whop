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

# Free Trials

> Add a free trial to a checkout link so people get access for a set number of days before they're charged.

Free trials give people access to your whop for a limited time. Once the trial ends, they're automatically charged the standard price for access.

## Add a free trial from your Dashboard

1. Go to **Dashboard** > **Checkout links**
2. Pick an existing link and select the 3 dots (⋮) > **Edit**, or make a new one by selecting **+ Create checkout link**
3. Check **Free trial**
4. Pick how many free days you want to offer (like 1 day, 7 days, or 30 days)
5. Select **Update checkout link** to activate your free trial

> [!NOTE]
>
> You can only add a free trial to 'Recurring' checkout links, not one-time
> payments.

## What members receive before the trial ends

The [upcoming renewal email](https://docs.whop.com/manage-your-business/payment-processing/checkout-branding.md#customer-emails) reminds members before a trial converts to paid. Whop sends it 7 days before the first charge for trials longer than 7 days, and about 24 hours before for shorter trials. For your own reminder, use the `membership.trial_ending_soon` [webhook](https://docs.whop.com/developer/guides/webhooks.md), which fires up to 72 hours before the trial ends.

## Understanding trial abuse and prevention

* **Trial abuse:** This happens when someone keeps signing up for free trials but cancels before they have to pay. They might use different emails or make new accounts to avoid paying.
* **How Whop stops this:** Whop automatically catches when the same person tries to sign up for multiple free trials. If someone cancels and then tries to rejoin with another trial, the system will charge them full price instead of giving them another free period.

> [!TIP]
>
> Don't worry too much about trial abuse: 99% of people use trials as intended, and
> Whop handles the bad actors automatically.


## Related topics

- [Create a Checkout Link to Accept Payments](https://docs.whop.com/manage-your-business/payment-processing/create-checkout-link.md)
- [Create Checkout Links for Your Products](https://docs.whop.com/payments/create-checkout-link.md)
- [Checkout Branding](https://docs.whop.com/manage-your-business/payment-processing/checkout-branding.md)
