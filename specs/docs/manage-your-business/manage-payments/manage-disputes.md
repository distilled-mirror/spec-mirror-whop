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

# Manage Disputes

> Respond to credit card disputes with evidence in the Dispute fighter, understand dispute fees and timelines, and set up early alerts to win more cases.

When a customer disputes a charge with their bank, you can respond with evidence and manage the case from your dashboard. Whop's **Dispute fighter** helps you track, get ahead of, respond to, and resolve disputes.

Credit card disputes happen when customers contact their bank or card issuer to challenge a payment they made to your whop.

## What you need to know

* Whop **immediately withdraws** the disputed amount from your balance to cover the cost if you lose the dispute
* You have **7–21 days** to respond with proof via the **Dispute fighter**
* The bank's decision typically takes **2–3 months** — you can check status anytime in the Dispute fighter
* To respond: go to **Dashboard** > **Dispute fighter**

## What happens after a customer files a dispute

When a customer files a dispute:

* Whop notifies you instantly through the **Dispute fighter**
* Whop debits the disputed amount and a \$15 dispute fee from your balance
* Whop automatically uploads key evidence (access logs, policy disclosures, customer info) into the Dispute fighter
* You can upload additional proof and submit your response from your dashboard
* The **Dispute fighter** tracks the dispute status, outcome, and deadline

### Common reasons customers file disputes

* They don't recognize the charge on their statement
* They think the transaction was fraudulent
* They're unhappy with their purchase and aren't aware of the [Resolution Center](https://docs.whop.com/manage-your-business/manage-payments/resolution-center.md)
* They tried to cancel but got charged anyway

## Managing a dispute

**1. Go to Disputes**

Go to your **Dashboard** > **Disputes** (or **Resolution Center**)

**2. Review the dispute**

Open the dispute and review the reason and deadline

**3. Upload evidence**

Upload **evidence** (receipts, communications, terms, proof of delivery)

**4. Submit your response**

Submit your response before the deadline

Winning a dispute depends on strong evidence. Respond in time to avoid an automatic loss.

## How to view and manage disputes

You can monitor and manage all your disputes in the **Dispute fighter**:

**1. Go to Dispute fighter**

Go to **Dashboard** > **Dispute fighter**

**2. Filter by status**

Select **Status** and choose from: Needs response, Under review, Won, or Lost

Track response deadlines and outcomes from this main view.

## How to respond to a dispute

When you get a dispute notification, act fast and submit your evidence before the deadline.

**To fight a dispute:**

**1. Go to Dispute fighter**

Go to **Dashboard** > **[Dispute fighter](https://whop.com/dashboard/disputes/)**

**2. Select the dispute**

Select the dispute you want to respond to

**3. Upload evidence**

Upload your evidence and documentation, such as chat logs with the customer, product details and descriptions, cancellation policy, and refund policy

**4. Add supporting evidence**

Add any supporting evidence like cancellation request records, customer communications, refund records, and proof of subscription agreement

**5. Submit**

Select **Submit** to send your response

> [!WARNING]
>
> Each dispute shows a "Respond by" date under Status. Submit your evidence
> before this deadline or you'll automatically lose the dispute.

## What Whop automatically submits

Whop automatically uploads relevant files and user logs to strengthen your case:

**Customer details**

* Email address (proves they have an account)
* Customer name (confirms identity)
* Billing address (matches payment method to prove legitimate purchase)

**Product details**

* Purchase date (shows exactly when they bought)
* Description (proves what they purchased and agreed to)
* Access activity log showing when they bought access, if they agreed to Terms of Service and Refund policy, if they claimed access, and if they engaged with your content

**Cancellation policy**

* Policy disclosure (shows customer saw and agreed to cancellation rules)
* Your actual cancellation policy document

**Refund policy**

* Refund refusal explanation (explains why refund wasn't given)
* Policy disclosure (proves customer knew refund rules before buying)
* Your actual refund policy document

> [!TIP]
>
> Add your own evidence: beyond what Whop automatically includes, you can upload
> supporting evidence and customer communications to strengthen your case.

## Dispute fees

* Whop immediately takes the disputed amount from your account
* You're charged a \$15 dispute fee
* Whop holds the disputed funds during the investigation and returns them if you win

**If you win:** Whop returns the disputed amount to your Whop balance.

**If you lose:** The customer receives a refund from their card issuer. Whop already deducted the money, so it takes no additional funds from your account.

## How to win disputes

### Set up early dispute alerts

Early dispute alerts warn you when a customer is about to file a dispute. You can resolve the issue before the official dispute hurts your account.

**How early dispute alerts work**

* **Early warning:** When a customer starts a dispute, you're notified immediately so you can resolve it before it becomes official on your record.
* **Automatic refunds:** Any transaction under your set amount can be automatically refunded, saving you from chargeback fees that can exceed the refund.
* **Clearer transaction info:** Each purchase can show on the customer's bank statement with a clear description and website link, reducing confusion-based disputes.

**To set it up:**

**1. Go to Risk settings**

Go to **Dashboard** > **Settings** > **Risk**

**2. Find Early dispute alerts**

Scroll to **Early dispute alerts**

**3. Set the refund threshold**

Enter the amount under **Refund alerted payments under**

**4. Save**

Select **Save**

When you're new to Whop and don't have healthy payment history yet, disputes can lead to reserves or account suspension. Whop automatically refunds transactions under \$500 when they trigger an early dispute alert. This protects your payments account during this period. After you build healthy payment history, you can turn off dispute alert refunds or change the maximum.

> [!WARNING]
>
> Each early dispute alert costs \$29.

### The decision process

**1. Initial review**

The card issuer looks into the customer's complaint

**2. Submitting evidence**

You have 7–21 days to send your evidence (depending on the card company)

**3. Evaluation**

The card issuer reviews all evidence and makes a decision (usually 60–75
days)

**4. Final answer**

You either win or lose the dispute

### What card issuers look for in your evidence

* **Proof of transaction:** Clear records showing the customer sent the payment
* **Delivery proof:** Evidence you gave them what they paid for
* **Customer chats:** Any messages between you and the customer
* **Terms of Service:** Proof the customer agreed to your terms
* **Return policy:** Your clearly written return policy

> [!WARNING]
>
> Even with Whop's help, the card issuer has the final call on whether you win
> or lose the dispute.

## Reserves and account health

High dispute rates or unresolved [Resolution Center](https://docs.whop.com/manage-your-business/manage-payments/resolution-center.md) cases can lead to [reserves](https://docs.whop.com/trust-and-safety/account-health/reserves.md) on your balance. To keep your account healthy and dispute rates low, see [Managing dispute rates](https://docs.whop.com/trust-and-safety/account-health/managing-dispute-rates.md).

## Frequently asked questions

> [accordion] How long do I have to respond to a dispute?
>
> Each dispute has a deadline shown under Status in the Dispute fighter.
> Response times vary, so check them individually.

> [accordion] How do I increase my chances of winning?
>
> Provide evidence that directly answers the customer's complaint. Upload
> clear policies, show proof they received and used your product, and include
> screenshots of them using your service.

> [accordion] Why did I lose a dispute?
>
> The customer's bank makes the final decision, not Whop. Banks review all
> evidence you submit through the Dispute fighter, but they decide the
> outcome. Whop gives you every tool to win, but the bank controls the final
> decision.

> [accordion] Can a customer dispute the same payment multiple times?
>
> Yes, but it's rare. Card companies and payment processors permit this. Fight
> each dispute fully using the Dispute fighter, even if it's the same
> purchase.

> [accordion] Can I refund a disputed payment?
>
> If it's become a full chargeback, you can't refund it. If the customer
> contacts their bank and cancels the dispute, then you can give them a
> refund.

> [accordion] Why is the dispute amount different from what the customer originally paid?
>
> The dispute amount can differ for several reasons:
>
> * **Currency changes** — Exchange rates shift between the payment and dispute.
> * **Partial disputes** — The customer disputes only part of the purchase.
> * **Multiple charges** — The card issuer combines several disputed payments into one dispute.
> * **Payments with partial refunds** — The customer disputes the full amount after receiving some money back.

> [accordion] Why can't I respond to a dispute?
>
> You automatically lose some disputes and can't challenge them:
>
> * Certain fraud cases, depending on the card company
> * Disputes from some countries due to local laws
> * Cases where the card issuer won't accept evidence

> [accordion] Do I need to submit evidence if the customer cancels their dispute?
>
> Yes, you still need to submit evidence.


## Related topics

- [Team Management](https://docs.whop.com/manage-your-business/team-management/manage-team-roles.md)
- [Manage Users](https://docs.whop.com/manage-your-business/manage-payments/manage-users.md)
- [Business Analytics Dashboard](https://docs.whop.com/manage-your-business/manage-business/analytics.md)
