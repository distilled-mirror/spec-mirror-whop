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

# Splitit Guide

> Pay in monthly installments with no interest or credit check using your existing credit card.

Splitit is a unique buy now, pay later option that lets customers break a purchase into smaller monthly payments with no interest, no credit check, and no extra fees. Splitit supports customers in [34 countries](#supported-countries) and, unlike traditional BNPL options, doesn't require customers to open a new line of credit. Instead, Splitit uses your existing, approved line of credit to split purchases up to \$20,000 into monthly installments.

If the creator has Splitit enabled, the payment option appears automatically at checkout.

> [!WARNING]
>
> To stop leftover monthly charges or request a refund, file a case in the [Whop Resolution Center](https://whop.com/@me/settings/orders/) or contact [Whop support](https://whop.com/support). An approved case cancels remaining installments. The Shopper Portal doesn't stop leftover charges.

## How Splitit works

At checkout, Splitit asks your credit card provider to approve the full purchase amount. If approved, Splitit places a **temporary hold** on your card for the total amount. This isn't a charge — it reserves your available credit and guarantees the payments to the merchant.

* The hold temporarily reduces your available credit but **won't appear as a charge** on your credit card bill. You can continue using your full line of credit after the authorization goes through.
* Splitit requires the hold because your installments aren't a loan. Splitit needs to guarantee the purchase to the merchant.
* Splitit releases the authorization within 30 days (but often much sooner — typically within a week). For some plans, Splitit reauthorizes lower amounts as you pay each installment until the plan is complete.
* Your bank or card issuer determines the hold length. Contact them with questions.

**Transaction limit:** <abbr title="United States dollars">USD</abbr> 20,000 per Splitit purchase.

You’ll get email updates throughout your plan and can log in to the [Splitit Shopper Portal](https://consumer.splitit.com/) anytime to view your plan or make changes.

> [!TIP]
>
> If you see the full amount at checkout instead of installment options, you’re
> likely using a **debit card** or **business credit card**. Splitit only works
> with **personal credit cards**. Switch to a personal credit card to use
> installments.

## Supported cards

Splitit works with most major **credit cards** on Whop.

**Supported:**

* Visa
* Mastercard
* American Express
* Discover
* Debit cards (for pay-in-full Splitit purchases only)

Splitit can work with some, but not all, business credit cards.

**Not supported:**

* Prepaid cards
* Flexible spending account and health savings account cards

Your card must have enough **available credit to cover the full purchase amount** (even though you only pay in monthly installments), because Splitit places a hold for the total.

If your card doesn’t work with Splitit, try another method that the creator has enabled. Options may include a standard card, Klarna, Cash App Pay, Google Pay, Apple Pay, Zip Pay, Sezzle, Link Pay, PayPal, crypto, or an automated clearing house payment.

## Supported countries

Splitit supports customers in the following countries:

Australia, Canada, every European Union member state, Hong Kong, Norway, Switzerland, the United Kingdom, and the United States.

Checkout only offers Splitit when the billing address is in one of these countries.

> [!WARNING]
>
> Splitit only supports USD currency checkout links. If Splitit isn't appearing
> on checkout, make sure the checkout link you created is in USD.

## Why your payment might not be going through

Common reasons and what to do:

### 1. You’re using a debit card

Splitit works with credit cards, but not debit or prepaid cards. Use a valid credit card.

### 2. Not enough available credit

Splitit holds the full purchase amount on your card. If your available credit is too low, the transaction will fail.

> [!TIP]
>
> Try a different credit card with a higher available balance.

### 3. Billing info doesn’t match

If your billing address or card details don’t exactly match what your card issuer has on file, the payment may fail. Double-check everything and try again.

### 4. Your bank declined the authorization

Banks sometimes block unfamiliar or large transactions. You can:

* Try the purchase again
* Contact your bank to approve the authorization
* Use a different credit card

### 5. Splitit fraud decline

Splitit uses a fraud-prevention system and may decline some transactions even when funds are available.

**What to do:**

* Retry with another payment method, or
* Try again with a smaller amount

> [!WARNING]
>
> Don't try to update your card more than 2 times in 24 hours. Too many attempts
> may trigger fraud alerts, and your bank could block or cancel the card.

## What to expect after paying with Splitit

### 1. Confirmation email

Splitit sends a confirmation email with your installment plan details:

* Total amount authorized
* Monthly payment schedule
* Number of payments

### 2. First installment and automatic payments

Splitit charges your card for the first installment right away. Splitit automatically charges the remaining installments on the same date each month until the plan is complete. You don't need to take further action.

### 3. Hold on your card

A temporary hold for the full purchase amount stays on your card. It’s not a charge. The hold shrinks as you pay each installment until Splitit fully releases it.

> [!TIP]
>
> **Example:** Mike buys a 1,200 USD product with Splitit. At checkout, Mike sees a
> 1,100 USD hold and a 100 USD charge. After one month, Splitit charges another 100
> USD, and the hold drops to 1,000 USD. This continues each month until Mike
> completes all payments.

### 4. Managing your plan

View your schedule, update your card, or check your balance in the [Splitit Shopper Portal](https://consumer.splitit.com/) (log in with the email you used at checkout).

## Pay off a Splitit plan early

You can always pay your plan in full early. Log in to the [Splitit Shopper Portal](https://consumer.splitit.com/) and select **Pay Outstanding Balance (Credit/Debit)**. This charges the full outstanding amount from your chosen credit card.

## Change the card for a Splitit plan

Log in to the [Splitit Shopper Portal](https://consumer.splitit.com/) and select **Update Card**. Fill in every requested field. Blank fields prevent the update. You can use only a credit card for monthly installment payments or to secure a reauthorization.

After a successful update, Splitit reauthorizes your card. Check that it has enough available credit to cover your outstanding plan balance. Otherwise, Splitit declines the update.

## How reserves work for sellers

When a customer pays with Splitit, you're paid for the full sale up front while the customer pays their card over the following months. Because the payment stays disputable for the life of the installment plan, every Splitit sale carries its own reserve. Whop holds **15%** of the sale for **180 days** to cover the installment window. After 180 days, Whop releases the reserve to your available balance automatically.

A few things to know:

* These holds are per-sale, so they release continuously — each sale's hold comes back 180 days after that sale, not in one batch.
* They appear on your [Balances](https://whop.com/dashboard/balance/) page as a **Splitit reserve**, with your current percentage and each hold's release date, separate from any **Whop reserve** on your account.
* The Splitit reserve applies to financed sales regardless of your account's health. Whop may also place a separate reserve based on your dispute risk.

For how reserves work across your account, see [Reserves](https://docs.whop.com/trust-and-safety/account-health/reserves.md).

## How to request a refund or stop leftover installments

Refunds and leftover installment cancellations for Splitit purchases go through the Whop Resolution Center, the same as any other Whop purchase. An approved case cancels remaining installments. Amounts already paid stay.

Request a refund from your [Orders page](https://whop.com/@me/settings/orders/) under your Account Settings.

For help, contact [Whop support](https://whop.com/support).


## Related topics

- [Whop Partner Program Guide](https://docs.whop.com/partners.md)
- [Reserves](https://docs.whop.com/trust-and-safety/account-health/reserves.md)
- [SeQura Guide](https://docs.whop.com/payments-and-billing/financing/sequra-guide.md)
