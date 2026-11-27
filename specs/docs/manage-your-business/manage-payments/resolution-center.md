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

# Respond to Refund Requests in the Resolution Center

> Accept, deny, or ask for more information on customer refund cases within 7 days, set up automatic refunds for small claims, and handle escalations.

The Resolution Center lets you respond to unhappy customers and handle refund requests. You can manage all Resolution Center cases from the **[Payments](https://whop.com/dashboard/payments/)** tab of your dashboard.

If you have open Resolution Center cases, you will see an **Open resolutions** button at the top of your screen. Select that button to access and handle the refund cases that require your response.

You have 7 days from when a customer opens a Resolution Center case to respond. You can accept the case, deny the refund, or request more information from the customer. If you don't respond within 7 days, Whop's team will step in and make a decision.

## How cases work

Here's what happens when a customer opens a claim:

**1. Customer opens a claim**

They explain the issue and choose a reason. They can upload up to 5 files
(screenshots, videos, receipts).

**2. You receive a notification**

Whop sends you an email and the case appears in your **Resolution Center**.

**3. You respond**

Chat directly with the customer and select **Accept**, **Deny**, or **Request more info**.

**4. A decision is made**

If you and the customer resolve the issue, Whop closes the case. If the case is
not resolved or you don't respond within 7 days, Whop will step in and make
a decision.

## Respond to a case

**1. Go to Resolution Center**

Go to **Dashboard** > **Resolution Center**

**2. Select the case**

Select the case you want to manage

**3. Take action**

Select **Accept**, **Deny**, or **Request more info**

You can upload any proof in your response, so make sure to have solid Terms of Service and Return policy.

> [card] [Upload legal documents](https://docs.whop.com/manage-your-business/manage-business/legal-documents.md)
>
> Learn how to upload legal documents and policies to your whop.

> [!TIP]
>
> Whop's team automatically receives cases without a response after 7 days.
> Responding fast shows you care and helps keep customers happy.

## Set up auto respond for small refunds

You can set up automatic refunds for smaller claims:

**1. Go to Resolution Center**

Go to **Dashboard** > **Resolution Center**

**2. Select Auto respond**

Select **Auto respond**

**3. Set auto-refund thresholds**

Choose the automatic refund thresholds under **Auto-refund card payments below** and **Auto-refund PayPal payments below**

**4. Add a custom message (optional)**

Add a custom message for customers who receive an automatic refund

> [!NOTE]
>
> Resolution Center cases for purchases made with financing options are always
> automatically refunded. This applies to Splitit, Sezzle, Klarna, Afterpay, Zip
> Pay, Scalapay, SeQura, and ClarityPay. Automatic refunds help protect your
> payments, keep your dispute rates low, and preserve your access to BNPL
> financing options. Whop handles these refunds automatically, so you don't
> need to act.
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
* Whop notifies both you and the customer of the outcome

**Appeal process:**

* If either you or your customer disagrees with a decision, either party can appeal
* Whop will review the case again
* That decision is final, and neither party can appeal it further

## Notifications

Whop will email you when:

* A customer opens a new dispute
* A customer replies to your request for more info
* Whop resolves, escalates, or receives an appeal for a case
* Whop makes a final decision

If you're not getting emails from the Resolution Center, go to **Dashboard** > **Settings** > **Notifications** and turn on the **Resolution Center** toggle.

## Frequently asked questions

> [accordion] How many cases can a customer open?
>
> Customers can open a case within 120 days of each payment they've made. For
> example, if a customer has made three payments, they can open three cases.
> However, a customer can open only one case per payment. After a case closes and
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
> Only fiat payments are eligible for the Resolution Center. Payments made
> with cryptocurrency via Coinbase Commerce aren't eligible since they're
> irreversible and non-refundable.

> [accordion] What happens when Whop decides in favor of the customer?
>
> Whop either refunds the customer on your behalf or credits the customer's
> Whop Balance. A balance credit doesn't cost you. The Resolution Center
> timeline shows the result to you and the customer.


## Related topics

- [Review Refund Claims in the Resolution Center](https://docs.whop.com/payments/resolution-center.md)
- [Status of a Resolution Center Case](https://docs.whop.com/memberships-and-access/cancellations-and-refunds/status-of-resolution-center-case.md)
- [Request a Refund](https://docs.whop.com/memberships-and-access/cancellations-and-refunds/request-a-refund.md)
