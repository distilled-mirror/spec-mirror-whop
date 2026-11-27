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

# Payment Processing and Payout Fees

> Per-transaction rates for cards, wallets, financing, and ACH on Whop, plus payout, 3DS, Radar, dispute, and revenue optimization and automation fees.

![Fees](https://mintcdn.com/whop/O2UveoaTiTL7ZPwK/how-to-videos/fees.gif?s=886d19958b6417a8d123cc54856b56c2)

> [card] Global access
>
> 195 countries, 135+ currencies, 100+ payment methods.

> [card] Fraud prevention
>
> Powerful machine learning that adapts to new fraud patterns.

> [card] Optimized checkout
>
> Flexible UI components and accelerated checkout.

> [card] Payment optimizations
>
> Real-time retries & automatically updated cards to boost revenue.

> [card] Financing options
>
> Let your customers pay over time with Klarna, Afterpay, and more.

> [card] Embedded payments
>
> Instant onboarding and platform management tools.

> [card] Fast, predictable payouts
>
> Flexible payout options with like-for-like settlement.

> [card] 24×7 support
>
> Phone, chat, and email assistance whenever you need.

> [card] Security & compliance
>
> 99.999% uptime and PCI-compliant infrastructure.

**Global payments**

100+ payment methods

![Fees](https://mintcdn.com/whop/vkZJmcXML9qFeth4/images/payments.png?fit=max\&auto=format\&n=vkZJmcXML9qFeth4\&q=85\&s=6955c07c9b75d157afd4ae12a6c3407f)

## Payments

An online payments platform engineered for growth.

[Start today →](https://whop.com/start)

&#x20;Increase customer acquisition with 100+ payment methods.

&#x20;Improve conversion with Whop's accelerated checkout.

&#x20;Save engineering time with prebuilt UIs.

### Cards and wallets

Accept widely used credit and debit cards, prepaid cards, and digital
wallets for online payments.

### 2.7% + \$0.30

per successful transaction for domestic cards\
\\

\+ 1.5% for international cards\\

\+ 1% if currency conversion applies\
\\

Rates differ in Latin America. See [regional fees](https://docs.whop.com/payments-and-billing/fees/regional-fees.md)

### Financing

Give your customers the flexibility to split their purchases into monthly installments while you get paid upfront.

### 15%

per successful transaction through a financing partner\\

20% for ClarityPay No Interest\
\\

**Klarna supported countries & transaction limits**

| Country | Minimum Amount | Maximum Amount |
| - | - | - |
| Austria | €25,00 | €10.000,00 |
| Czechia | 25,00 Kč | 25.000,00 Kč |
| Finland | €25,00 | €5.000,00 |
| France | €1,00 | €1.500,00 |
| Germany | €25,00 | €10.000,00 |
| Greece | €1,00 | €1.000,00 |
| Ireland | €1.00 | €1,500.00 |
| Italy | €1,00 | €4.500,00 |
| Netherlands | €35,00 | €4.000,00 |
| Norway | kr 0,00 | kr 150.000,00 |
| Portugal | €1,00 | €1.000,00 |
| Spain | €1,00 | €1.500,00 |
| Sweden | 250,00 kr | 150.000,00 kr |
| United Kingdom | £1.00 | £2,000.00 |
| United States | \$0.50 | \$10,000.00 |

### Collect ACH payments

Accept ACH direct debit payments from US bank accounts.

### 1.5% (Maximum fee of \$5)

per successful ACH debit transaction

**Global payouts**

170+ countries and territories

![Fees](https://mintcdn.com/whop/vkZJmcXML9qFeth4/images/payouts.png?fit=max\&auto=format\&n=vkZJmcXML9qFeth4\&q=85\&s=8f0f8fd4a00941062a09cee28a318f39)

## Payouts

Accept money and get paid out as fast as you want.

[Start today →](https://whop.com/start)

&#x20;Get paid anywhere in the world with global payout options.

&#x20;Flexible payout methods including crypto, Venmo, and wire transfers.

&#x20;Same-day payouts available for immediate cash flow.

### Withdraw via next day ACH

Accelerated ACH transfer to U.S. bank accounts, delivered by the next business day (faster settlement speed).

### \$2.50

per successful payout

### Instant Bank Deposit (RTP)

Real-time bank transfer to supported accounts with funds available instantly (where supported).

### 4% + \$2.50

per successful payout

### Crypto

Cryptocurrency payout (e.g. Bitcoin, Ethereum) with global reach.

### 5% + \$1.00

per successful payout

### Venmo

Instant payout to a recipient's Venmo wallet (available for U.S. recipients only).

### 5% + \$2.50

per successful payout

### Bank wire

Traditional bank wire transfer (will typically take 1–2 business days to process in the US).

### \$23.00

per successful payout

### International local banks

Local bank transfer in your country and currency (timing varies by country).

### Varies by country

per successful payout

**Fraud and security**

![Fees](https://mintcdn.com/whop/vkZJmcXML9qFeth4/images/fraud.png?fit=max\&auto=format\&n=vkZJmcXML9qFeth4\&q=85\&s=6ba6d3ace3fa3f59ae62842ec034eb05)

## Fraud and security

Protect your business from fraud and disputes.

[Start today →](https://whop.com/start)

&#x20;0% of transaction amount.

&#x20;Transparent flat-rate pricing per event.

&#x20;No hidden fees. No guessing.

### 3DS

Fee for authenticating cards using 3D Secure to reduce fraud.

### \$0.03

per transaction

### Radar

ML-based fraud detection fee.

### \$0.07

per transaction

### Dispute

Flat fee applied when a card chargeback or dispute occurs.

### \$15.00

per dispute

### Bank debit returns and disputes

Flat fees for SEPA Direct Debit and ACH payments. A return is when the customer's bank sends the payment back unpaid, for example for insufficient funds. A dispute is when the account holder tells their bank they didn't authorize the payment. A payment is charged one fee or the other, never both, and the dispute fee replaces the \$15.00 card dispute fee.

SEPA Direct Debit fees are charged in euros. ACH fees are charged in US dollars.

### SEPA Direct Debit: €7.50

per returned payment

### SEPA Direct Debit: €20.00

per dispute

### ACH: \$10.00

per returned payment

### ACH: \$25.00

per dispute

### Early dispute alert (RDR & Ethoca)

Proactive chargeback notification fee.

### \$29.00

per alert

**Revenue optimization**

![Fees](https://mintcdn.com/whop/vkZJmcXML9qFeth4/images/revenue.png?fit=max\&auto=format\&n=vkZJmcXML9qFeth4\&q=85\&s=928aeafa6fb3ec779a7f2732cff16632)

## Revenue optimization and automation

Tools to reduce payment failures and increase revenue.

[Start today →](https://whop.com/start)

&#x20;Automated systems that boost revenue and reduce churn.

&#x20;Pricing optimized to reflect real cost of global scale infra.

&#x20;Transparent and minimal fees per event.

### Orchestration

Route payments through multiple payment service providers to boost revenue by \~6%.

### 0.8%

per transaction (when enabled)

### Billing

Automated invoice generation, retries, and lifecycle handling.

### 0.5%

per transaction (when enabled)

### Tax and remittance

Handle global tax calculation, collection, and compliance.

### You remit the tax: 0.5%

per transaction (when enabled and tax is collected)

### Whop remits the tax: 2%

per transaction (when enabled and tax is collected)

Revenue optimization fees stack on top of the base processing fee. Below is what a domestic card
transaction costs as you turn each one on. International cards and currency conversion add to the base
rate.

Card processing
2.7% + \$0.30

\+ Orchestration (when enabled)
0.8%

\+ Billing (when enabled)
0.5%

\+ Tax and remittance (when Whop collects and remits the tax)
2%

Everything enabled, tax collected
6% + \$0.30

Compare the fees that other platforms charge below. This pricing applies to new merchants that sign up today. Enterprise customers can [book a call with the Whop sales team](https://calendly.com/d/cv3h-5mq-vc2/whop-payments-enterprise-call).

| Platform | Pricing |
| - | - |
| Stripe | [stripe.com/pricing](https://stripe.com/pricing) |
| Square | [squareup.com/pricing](https://squareup.com/us/en/pricing) |
| Shopify | [Shopify pricing](https://www.shopify.com/pricing) |
| Braintree | [braintreepayments.com/pricing](https://www.braintreepayments.com/pricing) |
| Adyen | [Adyen pricing](https://www.adyen.com/pricing) |


## Related topics

- [In-app iOS Purchases](https://docs.whop.com/payments-and-billing/fees/in-app-ios-purchases.md)
- [Regional Fees](https://docs.whop.com/payments-and-billing/fees/regional-fees.md)
- [Troubleshoot Payouts](https://docs.whop.com/manage-your-business/manage-payouts/troubleshoot-payouts.md)
