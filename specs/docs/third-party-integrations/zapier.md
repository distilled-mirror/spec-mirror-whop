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

# Zapier

> Automate Whop with Zapier: start Zaps from Whop events, and manage memberships, payments, and invoices from other apps.

> [card] [Whop on Zapier](https://zapier.com/apps/whop/integrations)
>
> See the Whop app on Zapier

Zapier connects Whop to thousands of other apps without code. A Zap starts with a **trigger**, an event in one app, and runs one or more **actions** in other apps. For example, a new Whop payment can post to Slack and add a row to a Google Sheet.

The Whop app on Zapier has 16 triggers, 20 actions, and 12 searches.

## Triggers

Triggers start a Zap when something happens in your Whop business.

| Area | Triggers |
| - | - |
| **Memberships** | New Member Joined, Membership Canceled, Membership Scheduled to Cancel |
| **Payments** | New Payment, Payment Pending, Failed Payment, Payment Refunded, Payment Method Saved |
| **Disputes** | New Dispute, New Dispute Alert, Dispute Updated |
| **Invoices** | New Invoice, Invoice Paid, Invoice Past Due |
| **Waitlists** | New Waitlist, Waitlist Approved |

## Actions

Actions change something in Whop.

| Area | Actions |
| - | - |
| **Memberships** | Cancel Membership, Uncancel Membership, Pause Membership, Resume Membership, Update Membership, Add Free Days to Membership |
| **Payments** | Create Payment, Refund Payment |
| **Invoices** | Create Invoice, Void Invoice |
| **Waitlists** | Approve Waitlist, Deny Waitlist |
| **Products and pricing** | Create Plan, Create Promo Code, Create Checkout Session |
| **Affiliates** | Find or Create Affiliate, Archive Affiliate, Unarchive Affiliate |

**Create Payment** charges a member's saved payment method. The result arrives asynchronously, so use the New Payment or Failed Payment trigger to confirm it.

## Searches

Searches look up Whop records to use in later steps of a Zap.

| Area | Searches |
| - | - |
| **Members and memberships** | Find Member, Find Membership |
| **Payments** | Find Payment, Find Refund, Get Payment Fees |
| **Products** | Find Product, Find Products, Find Plan |
| **Invoices** | Find Invoice |
| **Waitlists** | Find Entry, Find Entries |

## Get started

**1. Pick a Whop trigger**

In Zapier, create a Zap and choose **Whop** as the trigger app. Pick a trigger event, such as **New Payment**.

**2. Connect your Whop account**

Sign in when Zapier asks you to connect Whop, then choose the Whop business to use.

**3. Test the trigger**

Pull in a test event to check the data the trigger sends.

**4. Add actions**

Add actions in Whop or any other app. Use Zapier **Paths** to run different actions based on the payment amount, the product, or other data.

**5. Publish**

Test the Zap and turn it on.

Zaps built on an older version of the Whop app can use trigger names that no longer appear here. If Zapier offers an update to the latest Whop app version, update the Zap so it keeps working.

## Next steps

> [card] [Webhooks](https://docs.whop.com/developer/guides/webhooks.md)
>
> Receive Whop events in your own code instead.

> [card] [HubSpot](https://docs.whop.com/third-party-integrations/hubspot.md)
>
> Sync Whop payments into HubSpot.


## Related topics

- [HubSpot](https://docs.whop.com/third-party-integrations/hubspot.md)
