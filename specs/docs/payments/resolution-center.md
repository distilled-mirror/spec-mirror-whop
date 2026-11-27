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

# Review Refund Claims in the Resolution Center

> How customer refund claims reach your Resolution Center, how to respond before Whop steps in, and how automatic responses, escalations, and appeals work.

## Review refund claims

The **Resolution Center** is where you can review and respond to customer refund claims.

## How claims work

Here’s what happens when a customer opens a claim:

1. **Customer opens a claim**
   * They explain the issue and choose a reason
   * They can upload up to 5 files (screenshots, videos, receipts)

2. **You receive a notification**
   * Whop sends you an email
   * The case appears in your **Resolution Center**

3. **You respond**
   * Chat directly with the customer
   * Select **Accept**, **Deny**, or **Request more info**

4. **Whop or the merchant makes a decision**
   * If you and the customer resolve the issue, the Resolution Center closes the case
   * If the case isn't resolved or you don’t respond within seven days, Whop will step in and make a decision

## Respond to a case

1. Go to **Dashboard** > **Resolution Center**
2. Select the case you want to manage
3. Select **Accept**, **Deny**, or **Request more info**

You can upload any proof in your response, so make sure to have solid Terms of Service and Return policy.

> [card] [Upload legal documents](https://docs.whop.com/manage-your-business/manage-business/legal-documents.md)
>
> Learn how to upload legal documents and policies to your whop.

> [!TIP]
>
> Whop automatically sends cases without a response for seven days to its team.
> Responding fast shows you care and helps keep customers happy.

## Set up auto respond for small refunds

You can set up automatic refunds for smaller claims:

1. Go to **Dashboard** > **Settings** > **Risk**
2. Scroll to **Auto respond to resolution center cases**
3. Pick the amount that triggers automatic refunds:
   * Enter the card amount next to **Credit and debit cards**
   * Enter the PayPal amount next to **PayPal**
4. (Optional) Edit the **Refund message** that customers see when they get an automatic refund
5. Select **Save**

> [!NOTE]
>
> Resolution Center cases for purchases made with financing options always
> receive automatic refunds. This applies to Splitit, Sezzle, Klarna, Afterpay,
> Zip Pay, Scalapay, SeQura, and ClarityPay. Automatic refunds protect your
> payments, keep your dispute rates low, and preserve your access to BNPL
> financing options. You don't need to act on these cases.
>
> ClarityPay payments are an exception to the standard refund timeline.
> Customers can submit refund requests for up to 365 days after the original
> payment. However, automatic refunds only apply to cases submitted within the
> first 120 days. Cases submitted after this period aren't automatically
> refunded. They follow the standard Resolution Center process, and you
> review and resolve each case as you would any other.

## Escalations and appeals

**If you don't respond within 7 days:**

* Whop reviews the case and decides based on all available info
* Whop notifies you and the customer of the outcome

**Appeal process:**

* If either you or your customer disagrees with a decision, either party can appeal
* Whop will review the case again
* That decision is final, and neither party can appeal it again

## Notifications

Whop will email you when:

* A customer opens a new dispute
* A customer replies to your request for more info
* Someone resolves, escalates, or appeals a case
* Whop makes a final decision

If you're not getting emails from the Resolution Center, go to **Dashboard** > **Settings** > **Notifications** and turn on the **Resolution Center** toggle.

## Frequently asked questions

> [accordion] How many cases can a customer open?
>
> Customers can open a case within 120 days of each payment they've made. For
> example, if a customer has made three payments, they can open three cases.
> However, customers can open only one case per payment. After a case closes and
> no appeals remain, nobody can reopen it.
>
> ClarityPay payments are an exception to the standard refund timeline.
> Customers can submit refund requests for up to 365 days after the original
> payment. However, automatic refunds only apply to cases submitted within the
> first 120 days. Cases submitted after this period aren't automatically
> refunded. They follow the standard Resolution Center process, and you
> review and resolve each case as you would any other.

> [accordion] Can a customer open a case for a payment that has already been refunded?
>
> No. A customer can't open a case for a refunded payment.

> [accordion] Are all payments eligible for the Resolution Center?
>
> The Resolution Center accepts only fiat payments. Cryptocurrency payments
> through Coinbase Commerce don't qualify because they're irreversible and
> non-refundable.

> [accordion] What happens when Whop decides in favor of the customer?
>
> Whop will either refund the customer on your behalf or credit the customer's
> Whop Balance at no cost to you. Whop processes refunds automatically and
> clearly communicates the result to you and the customer through the case's
> Resolution Center timeline.


## Related topics

- [Respond to Refund Requests in the Resolution Center](https://docs.whop.com/manage-your-business/manage-payments/resolution-center.md)
- [Status of a Resolution Center Case](https://docs.whop.com/memberships-and-access/cancellations-and-refunds/status-of-resolution-center-case.md)
- [Issue Refunds](https://docs.whop.com/manage-your-business/manage-payments/issuing-refunds.md)
