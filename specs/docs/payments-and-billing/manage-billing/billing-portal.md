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

# Billing Portal

> The billing portal lets customers update payment methods, view billing history, and cancel or transfer memberships. Developers can link to it from the API.

The billing portal is where customers can manage active memberships and view billing history.

> [!TIP]
>
> The billing portal is the easiest way to let customers manage their own
> subscriptions. It saves you time on support requests and gives customers
> instant control.

## What customers can do in the portal

The billing portal gives customers full control over their memberships:

* **View membership details:** See active plans, renewal dates, and billing cycles
* **Update payment methods:** Add, remove, or change credit cards and other payment methods
* **View billing history:** Access past invoices and receipts
* **Cancel subscriptions:** Cancel memberships without contacting support
* **Transfer memberships:** Move memberships to another account

## How to access the billing portal

Customers can access the billing portal by going to [Profile → Orders](https://whop.com/@me/settings/orders/) and selecting an active subscription.

## For developers

If you're building an integration, you can access the billing portal URL programmatically through the Current API. Each membership has a `manage_url` field that provides a direct link to that customer's billing portal.

When you retrieve a membership through the API, the `manage_url` field will look like:

```
https://whop.com/billing/manage/mber_*************/
```

This URL is unique to each membership. Users must log in to access the portal. Whop prompts users to log in when needed. You can include this link in your app, emails, or customer dashboards to give users quick access to their billing portal.

> [card] [Retrieve Membership API](https://docs.whop.com/api-reference/memberships/retrieve-membership.md)
>
> See the API reference for retrieving memberships and accessing the
> `manage_url` field.


## Related topics

- [Splitit Guide](https://docs.whop.com/payments-and-billing/financing/splitit-guide.md)
- [Financing Options](https://docs.whop.com/payments-and-billing/financing/all-bnpl-options.md)
- [Checkout Branding](https://docs.whop.com/manage-your-business/payment-processing/checkout-branding.md)
