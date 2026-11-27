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

# Financing

> Let customers pay in monthly installments with providers like Klarna and Afterpay while you get paid upfront.

Give your customers the flexibility to split their purchases into monthly installments while you get paid upfront.

After Whop approves your business, it automatically adds financing options to your checkout page.

## Available financing options

Whop partners with five financing providers. Depending on your business, you may qualify for one or more of these options:

* **Splitit**
* **Sezzle**
* **Afterpay**
* **Klarna**
* **Zip Pay**

## How to apply for financing

Apply for financing via this [financing application form](https://hunterd.app.n8n.cloud/form/e9e21a0c-91d5-4cda-9b14-a9506be5af44).

You can also apply for financing directly from the [Checkout links page](https://whop.com/dashboard/links/checkout/) of your dashboard.

1. Select **+ Create checkout link**
2. Select **One-time** as your pricing type
3. Under **Payment methods**, select **Apply for financing**
4. Complete the application form

Whop will email you if it can't accept your application or needs more information. After acceptance, Whop will send a direct message confirming your financing access.

Once accepted, all active financing options automatically appear at customer checkout. On the [Checkout links page](https://whop.com/dashboard/links/checkout/), you can choose the financing options for each link.

## Eligibility requirements

To qualify for financing options, your business must meet these requirements:

### 1. Basic setup requirements

* Your **Whop Payments account** must be fully set up and verified
  * Go to **[Balance](https://whop.com/dashboard/balance/)** in your dashboard to complete setup
* **Complete your store page**
  * Whop needs to understand what you're selling and how you fulfill orders

### 2. Business requirements

* **Eligible categories**: Financing companies **can't** support gambling, sports betting, trading signals (pure education may be acceptable), credit offers, or adult content
* **Low dispute rate**: Keep your dispute rate below 2% over the last 90 days
  * New Whop businesses must submit 90 days of statements from their previous **payment processor**. Submit processing statements, not bank statements. They must show total transactions and disputes.
* **Processing volume**: At least \$30,000 processed from 10+ different customers in the last 90 days

> [!TIP]
>
> You'll receive a Whop DM after Whop approves your business and enables
> financing. If you haven't heard back about your application, check your email
> for requests for additional documentation or application updates.

## How it works for customers

When you enable financing:

1. Financing options appear automatically on your checkout page
2. Customers choose their preferred provider (e.g. Klarna, Afterpay)
3. Whop sends customers to the financing provider for instant approval
4. They complete their purchase through the financing platform

You can control which financing options appear on each of your checkout links through your pricing settings.

> [!TIP]
>
> You still receive the full payment amount (minus standard fees) while the
> provider handles customer repayment. Sezzle is the exception: funds from
> Sezzle orders settle on a 15-day basis before they're available in your
> balance.

## Keep your financing access

Whop’s priority is to make sure you keep your financing access for the long term. Whop has the following features to help you keep your dispute rates low and keep financing active on your account:

**Dispute rate monitoring**

* View dispute rates for each financing partner on your **Health report** (in the **Payouts** section)
* Alerts sent via email and Whop DM when dispute rates hit warning levels:
  * Klarna: warning at 0.5% dispute rate on Klarna transactions
  * All others: warning 1% dispute rate on any other BNPL option

> [!TIP]
>
> When you receive an alert, take extra precautions to prevent further disputes.
> Set clear return policies, refund angry customers, and make sure your
> customers know where to go to resolve issues so they don't dispute directly
> with the bank.

**Splitit minimum balance (Splitit only)**

* Whop holds 15% of every Splitit transaction in your balance for 180 days
* This shows Splitit that you can cover any potential disputes
* Track your Splitit balance and release dates on your [**Balance**](https://whop.com/dashboard/balance/) page

### **Dispute rate limits**

Stay under these thresholds to keep your financing access:

* **Klarna:** Under 1% dispute rate
* **All other providers:** Under 2% dispute rate

Whop automatically revokes financing access if your financed transactions hit these dispute rate thresholds. Whop will email you about the lost access.

## Troubleshooting

If financing isn't showing on your checkout links after approval, check these common issues:

**Transaction too large**

* Sezzle: \$2,500 limit
* Afterpay: \$4,000 limit
* Splitit: \$20,000 limit
* Klarna: \$10,000 limit
* Zip: \$1,500 limit

**Wrong pricing type**

* Financing only works with one-time payments, not subscriptions

**Location restrictions**

* Sezzle, Klarna, and Afterpay only show for customers in the United States

**Restricted business type**

> [!TIP]
>
> Financing providers (Splitit, Klarna, Sezzle, Afterpay, and Zip Pay) don't
> support certain categories of products or services. If your business falls
> under one of these categories, you won't be eligible for financing on Whop.

* **Adult, Gambling & Dating:** Adult entertainment or pornography, escort
  services, gambling, gaming, raffles, lotteries, dating, or matchmaking services.

* **Financial Services & High-Risk Lending:** This category includes banks, credit unions, mortgage lenders, non-bank finance companies, factoring, and purchase order financing. It also includes money services businesses, payroll companies, securities or commodities dealers or brokers, and investment opportunities or advice. Collection agencies, debt buyers, credit repair, debt relief, identity theft protection, and bail bonds also belong to this category.

* **Regulated & Prohibited Products:** Firearms, guns, ammunition sales or manufacturing, drug paraphernalia, dispensaries, medical marijuana, nutraceuticals, unverified health or weight loss products, precious metals, and counterfeit goods.

* **Marketing & Telemarketing:** Telemarketing companies (fax blasting, email spamming, and prize-with-purchase schemes), negative response marketing, search engine optimization services, call centers, and answering services.

* **Subscription & Service-Based Restrictions:** Discount travel clubs or subscriptions, magazine subscriptions, video rental services, timeshares, and fractional ownership. This category also includes real estate brokers, agencies, consultants, lessors, property managers, and contracted management services.

* **Miscellaneous High-Risk Businesses:** Government, nonprofit, charitable, or religious organizations. This category includes social networking sites, sports forecasting, flea markets, fortune telling, astrology, horoscopes, mining, farming, energy generation or distribution, and logistics and freight forwarding. It also includes liquidation companies, manufactured or mobile home dealers, massage services, future service liability beyond one year, and products or services with unreasonable guarantees or claims.

*If you’re unsure whether your product or service is eligible for financing, please contact Whop support before applying for financing*

**Dispute rate too high**

* Check your Health report - you may have exceeded the 2% dispute threshold (1% for Klarna)

**Incomplete setup**

* Check that your Whop Payments account is fully configured
* Verify your store page is complete with clear product descriptions


## Related topics

- [Apply for Financing](https://docs.whop.com/payments-and-billing/financing/apply.md)
- [Financing Options](https://docs.whop.com/payments-and-billing/financing/all-bnpl-options.md)
- [Whop Partner Program Guide](https://docs.whop.com/partners.md)
