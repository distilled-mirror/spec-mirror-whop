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

# Local Payment Methods

> Let customers pay with local options like bank transfers, iDEAL, and crypto, which appear at checkout with no setup.

Beyond card payments, Whop supports a wide range of local payment methods, so customers can pay using the options most common in their region. You don't need to do anything to enable these — eligible options appear automatically at checkout.

> [card] No setup required
>
> All eligible payment methods appear automatically at checkout.

> [card] Customer location
>
> Customers see the most popular payment methods in their region.

> [card] Configure per link
>
> Choose which local payment methods to show or hide when you set up each checkout link.

## Which local payment methods display at checkout?

Local payment methods will automatically appear at checkout if:

* Customer is in **eligible geography** (based on IP address)
* The checkout link is in an **eligible currency**
* The price is within the **minimum and maximum amount** the local payment method supports
* The local payment method supports the **pricing type** (i.e. one-time payment vs subscription)
* The merchant hasn't disabled the payment method when configuring the link
* The payment method is **enabled for your account**. Methods marked ¹ in the tables below need Whop to enable them before they appear.

The customer's geography, not the merchant's, determines the available local payment methods.
For example, a customer in the Netherlands will see iDEAL – even if the
merchant operates from the US. A customer in the US may see ACH or Cash App.

> [!TIP]
>
> If a local payment method doesn't appear, make sure the link uses an eligible
> currency and the customer is in an eligible region. Ask the customer to turn
> off any VPN. iDEAL also appears only in the checkout opened from your store
> or product page. It doesn't appear on direct checkout links or in embedded checkouts.

## Local payment method overview

Fees apply to each successful payment. Fixed fees are set in USD and charged in the payment's currency at the current exchange rate. Where a row lists an FX fee, it applies when the payment settles in a different currency from your payout currency.

ACH, SEPA Debit, iDEAL, Cash App, and PayPal support one-time payments and recurring subscriptions. All other local payment methods support one-time payments only.

¹ Needs Whop to enable it for your account.

### Bank transfers and debits

| Method | Countries | Currency | Minimum–maximum | Fee |
| - | - | - | - | - |
| **US Bank Transfer (ACH)** | US | USD | $0.50–$1M | 1.5% (capped at \$5) |
| **US bank transfer** ¹ | US | USD | $5–$1M | 1% (capped at \$5) |
| **Bank wire** | US | USD | $1–$1M | 0.8% (capped at \$5) |
| **International bank wire** | All countries except US | USD | $1–$1M | 0.8% (capped at \$5) |
| **EU bank transfers** | All countries except Germany | EUR | €1–€1M | 1% (capped at €5) |
| **UK bank transfer** | UK | GBP | £5–£1M | 1% (capped at £5) |
| **Australian bank transfer** | AU | AUD | A$5–A$1M | 1% (capped at A\$5) |
| **Canadian bank transfer** | CA | CAD | C$5–C$1M | 1% (capped at C\$5) |
| **SEPA Debit** | EU | EUR | €0.50–€20k | 1% + €0.30 |
| **iDEAL** | EU | EUR | €0–€1M | 1.5% + €0.80 |
| **Bancontact** | EU | EUR | €0.50–€1M | 2.7% + \$0.30 |
| **Blik** ¹ | PL | PLN | zł0.01–zł50k | 15% + \$0.30 |
| **PayTo** | AU | AUD | A$5–A$10M | 2.9% + \$0.30 |
| **Pix** | BR | BRL | R$0.01–R$1M | 2.9% + \$0.30 |
| **SPEI** | MX | MXN | MX$1–MX$100M | 2.9% + \$0.30 |
| **PSE** | CO | COP | COP 1,600–100M | 2.9% + \$0.30 |
| **Bancolombia** | CO | COP | COP 100–100M | 2.9% + \$0.30 |
| **Bre-B** | CO | COP | COP 100–100M | 2.9% + \$0.30 |
| **Webpay** ¹ | CL | CLP | CLP 1–100M | 4.9% + \$0.30 |
| **Nigerian bank transfer** | NG | NGN | ₦51–₦100M | 3.9% + \$0.30 |

### Wallets and apps

| Method | Countries | Currency | Minimum–maximum | Fee |
| - | - | - | - | - |
| **PayPal** | Global | USD + 18 more | No set limits | 2.9% + \$0.30 |
| **Cash App** | US | USD | $0.50–$1M | 2.7% + \$0.30 |
| **Klarna Pay Now** ¹ | AT, BE, CH, DE, FI, NL, SE | EUR, SEK, CHF | €1–€10k / kr 10–150k / CHF 1–10k | 2.7% + \$0.30 |
| **MobilePay** | DK, FI | DKK, EUR | kr 0.50–40k / €0.50–€5k | 2.7% + \$0.30 |
| **Swish** ¹ | SE | SEK | kr 0.50–kr 50k | 2.7% + \$0.30 |
| **Vipps** ¹ | NO | NOK | kr 0.50–kr 50k | 2.7% + \$0.30 |
| **Satispay** | BE, DE, FR, IT, LU | EUR | From €0.01 | 4.5% + \$0.30 |
| **MB WAY** | PT | EUR | No set limits | 2.9% + \$0.30 + 2.5% FX |
| **UPI** | IN | INR | ₹1–₹100k | 2.9% + \$0.30 |
| **AlipayHK** | HK | HKD, USD, EUR, AUD | From 0.10 | 3.9% + \$0.30 |
| **WeChat Pay** | CN, HK | CNY, GBP, AUD | From 0.01 (CNY up to ¥300k) | 3.9% + \$0.30 |
| **Kakao Pay** | KR | KRW, HKD, SGD | ₩1–₩1M / HK$0.01–HK$1k / S$0.01–S$500 | 2.9% + \$0.30 + 2.5% FX |
| **PayPay** | JP | JPY | ¥1–¥999,999 | 2.7% + \$0.30 |
| **GCash** | PH | PHP | ₱1–₱100M | 3.9% + \$0.30 |
| **GoPay** | ID | IDR | No set limits | 2.7% + \$0.30 |
| **GrabPay** | SG, MY | SGD, MYR | S$0.01–S$5k / From RM5 | 3.9% + \$0.30 |
| **ShopeePay** | MY | MYR | From RM5 | 3.9% + \$0.30 |
| **Touch 'n Go** | MY | MYR | RM0.01–RM1k | 2.9% + \$0.30 + 2.5% FX |
| **DuitNow QR** | MY | MYR | From RM5 | 3.9% + \$0.30 |
| **Nequi** | CO | COP | COP 1–999M | 2.9% + \$0.30 |
| **Mercado Pago** | CL | CLP | CLP 1–100M | 4.9% + \$0.30 |
| **MODO** | AR | ARS | ARS 1–100M | 2.9% + \$0.30 + 2% FX |
| **Yape** | PE | PEN | S/1–S/100M | 3.9% + \$0.30 |
| **OPay** | NG | NGN | From ₦51 | 3.9% + \$0.30 |
| **M-Pesa** | KE | KES | KSh 51–100M | 3.9% + \$2.00 |

### Cash and vouchers

| Method | Countries | Currency | Minimum–maximum | Fee |
| - | - | - | - | - |
| **OXXO** | MX | MXN | MX$1–MX$10k | 4.9% + \$0.30 |
| **Boleto** | BR | BRL | R$1–R$100M | 3.9% + \$0.30 |
| **Multibanco** | PT | EUR | €0.01–€100k | 2.9% + \$0.30 + 2.5% FX |
| **Efecty** | CO | COP | COP 1,000–100M | 2.9% + \$0.30 |
| **PagoEfectivo** ¹ | PE | PEN | S/1–S/100M | 3.9% + \$0.30 |
| **Rapipago** | AR | ARS | ARS 10–100M | 2.9% + \$0.30 + 2% FX |
| **Sencillito** | CL | CLP | CLP 1–100M | 3.9% + \$0.30 |
| **Servipag** | CL | CLP | CLP 1–100M | 3.9% + \$3.00 |

### Crypto

| Method | Countries | Currency | Minimum–maximum | Fee |
| - | - | - | - | - |
| **Crypto** | Global | USD | $0.50–$1M | 1.5% |

### Installments and buy now, pay later

Buy now, pay later methods have no fixed fee unless one is listed. See [Financing](https://docs.whop.com/payments-and-billing/financing/all-bnpl-options.md) for how each provider works.

| Method | Countries | Currency | Minimum–maximum | Fee |
| - | - | - | - | - |
| **Klarna** ¹ | US, CA, AU, UK, AT, CZ, DE, ES, FI, FR, GR, IE, IT, NL, NO, PT, SE | USD, EUR, GBP, CAD, AUD, SEK, NOK, CZK | Varies by country | 15% |
| **Afterpay / Clearpay** ¹ | US, UK, AU | USD, GBP, AUD | $1–$4k / £1–£1k / A$1–A$2k | 15% |
| **Zip** ¹ | US | USD | $35–$1.5k | 15% |
| **Sezzle** ¹ | Global | USD | Up to \$2.5k | 15% |
| **Sunbit** ¹ | US | USD | $60–$20k | 15% |
| **Flex Pay** ¹ | US | USD | $50–$25k | 15% |
| **ClarityPay** ¹ | US, PR | USD | $500–$30k | 15% (20% for no-interest plans) |
| **Splitit** ¹ | US, UK, AU, CA, CH, HK, NO, EU | USD, GBP | Varies by country | 15% + \$0.30 |
| **Scalapay** ¹ | ES, FR, IT, PT | EUR | €50–€5k | 15% |
| **SeQura** ¹ | ES | EUR | €50–€5k | 3% |
| **Oney (Pay in 3, Pay in 4)** ¹ | FR | EUR | €100–€3k | 15% + \$0.30 |
| **Tabby** ¹ | AE | AED | AED 2–20k | 15% |
| **Tamara** ¹ | SA, AE | SAR, AED | 1–5k | 15% |
| **Addi** ¹ | CO | COP | COP 100–100M | 15% + \$0.30 |
| **Kueski Pay** ¹ | MX | MXN | MX$60–MX$100M | 15% + \$0.30 |
| **Card installments (3, 6, or 12 months)** | MX, PE, CO, CL | MXN, PEN, COP, CLP | Minimum rises with the number of months | 2.9% + $0.30 (4.9% + $0.30 in Peru) |

## Method details

Learn more about the most common local payment methods below:

## Bank transfers

#### 1. US Bank Transfer (ACH)

ACH (Automated Clearing House) is the main US bank-to-bank transfer network. Customers pay directly from their bank account with low fees and high reliability, and it's one of the most trusted options for larger US transactions.

* **Country:** US
* **Currency:** USD
* **Minimum–maximum amount:** \$0.50-\$1M
* **Fees:** 1.5% (capped at \$5)
* **Payment support:** One-time and recurring subscriptions
* **Settlement:** 4 business days from payment creation (Stripe). Eligible US merchants can get 2 business days. Then normal payout to your bank.
* **Refunds:** Yes. Full and partial refunds supported. Whop processes refunds from your balance. Timing to the customer's account depends on their bank (often 5–10 business days).
* **Disputes:** Yes, but decisions are final. NACHA permits only 3 reasons. The customer can claim they never authorized the payment or revoked authorization. They can also claim the payment occurred earlier than authorized or the amount differed from the authorized amount. You can't appeal. A successful dispute invalidates the mandate. A second dispute on the same account can trigger an account block.

#### 2. EU bank transfers

Whop accepts one-time payments via EU bank transfers.

* **Country:** All countries except Germany
* **Currency:** EUR
* **Minimum–maximum amount:** €1 – €1M
* **Fees:** 1% (capped at €5)
* **Payment support:** One-time payments only
* **Settlement:** The transfer usually arrives in 1 business day, followed by the standard payout time for your country (e.g. 3 business days in many EU countries).
* **Refunds:** Yes. Refunds require customer bank details. You can request them within a limited window (e.g. up to 180 days). They usually reach the customer in 5–10 business days.
* **Disputes:** No card-style dispute flow. Customers need to raise issues with their own bank. There is no standard chargeback or dispute process like with cards or ACH.

#### 3. iDEAL

iDEAL is the most widely used online banking payment method in the Netherlands and in parts of the EU.

* **Country:** EU
* **Currency:** EUR
* **Minimum–maximum amount:** €0 – €1M
* **Fees:** 1.5% + €0.80
* **Payment support:** One-time and recurring subscriptions
* **Where it shows:** Only in the checkout that opens from your Whop store page or product page. iDEAL doesn't appear on direct checkout links (`whop.com/checkout/...`) or in embedded checkouts, even when you've turned it on and priced in EUR.
* **Settlement:** Instant bank-to-bank transfer. The standard country payout follows (e.g. Netherlands 3 business days after settlement).
* **Refunds:** Yes. Full and partial. Often 5–10 business days to customer.
* **Disputes:** Yes. Customers can dispute through their bank. The PSP supports iDEAL disputes. The customer's bank decides the outcome.

#### 4. Bancontact

Bancontact is Belgium's leading payment method, linked to customers' bank cards and mobile banking apps.

* **Country:** EU
* **Currency:** EUR
* **Minimum–maximum amount:** €0.50 – €1M
* **Fees:** 2.7% + \$0.30 (the \$0.30 is charged in EUR at the current exchange rate)
* **Payment support:** One-time payments only
* **Settlement:** Instant. The standard country payout follows (e.g. Belgium 3 business days).
* **Refunds:** Yes. Full and partial refunds typically reach the customer in 5–10 business days.
* **Disputes:** Yes. Card-linked disputes and chargebacks work similarly to card payments. The customer disputes with their bank, you can submit evidence, and the bank or network decides.

#### 5. SEPA Debit

SEPA Direct Debit lets EU customers pay from their bank account over European banking rails.

* **Country:** EU
* **Currency:** EUR
* **Minimum–maximum amount:** €0.50 – €20k
* **Fees:** 1% + €0.30
* **Payment support:** One-time and recurring subscriptions
* **Settlement:** 6 business days (Stripe: T+6 from payment creation). Longer than cards because of SEPA scheme and return risk.
* **Refunds:** Yes. Standard full and partial SEPA refunds often reach the customer in 3–4 business days.
* **Disputes:** Yes. Strong consumer protection lets the customer get a refund "no questions asked" within 8 weeks. From 8 weeks to 13 months, they can get a refund only if they didn't authorize the transaction. You can reverse a debit within 5 business days.

#### 6. Swish

Swish is Sweden's main bank-linked mobile payment method. Customers pay in real time using their mobile number.

* **Country:** Sweden (SE)
* **Availability:** Needs Whop to enable it for your account
* **Currency:** SEK
* **Minimum–maximum amount:** kr 0.50 – kr 50k
* **Fees:** 2.7% + \$0.30 (the \$0.30 is charged in SEK at the current exchange rate)
* **Payment support:** One-time payments only
* **Settlement:** Standard payout for Sweden (e.g. 3 business days).
* **Refunds:** Yes. Full and partial refunds supported.
* **Disputes:** Yes. Disputes and complaints follow the bank or PSP process.

#### 7. MobilePay

MobilePay is a leading mobile wallet in Denmark and Finland. Payments go through app-based banking links.

* **Countries:** Denmark (DK), Finland (FI)
* **Currencies:** DKK, EUR
* **Minimum–maximum amount:** DK: kr 0.50 – 40k | FI: €0.50 – €5k
* **Fees:** 2.7% + \$0.30 (the \$0.30 is charged in DKK or EUR at the current exchange rate)
* **Payment support:** One-time payments only
* **Settlement:** Standard payout for Denmark and Finland (e.g. 3 business days).
* **Refunds:** Yes. Full and partial refunds supported.
* **Disputes:** Yes. Consumers can raise complaints about unauthorized or problematic payments. Consumer protection and complaint handling apply.

#### 8. Vipps

Vipps is Norway's most popular mobile payment app. Customers pay from their bank account or mobile number.

* **Country:** Norway (NO)
* **Availability:** Needs Whop to enable it for your account
* **Currency:** NOK
* **Minimum–maximum amount:** kr 0.50 – kr 50k
* **Fees:** 2.7% + \$0.30 (the \$0.30 is charged in NOK at the current exchange rate)
* **Payment support:** One-time payments only
* **Settlement:** Standard payout for Norway (e.g. 3 business days).
* **Refunds:** Yes. Full and partial refunds typically reach the customer in 2–3 business days (up to \~10 depending on bank). Merchants can often issue refunds for a long window (e.g. 365 days).
* **Disputes:** Yes. Complaint and payment-regret mechanisms apply. Consumer protection and complaint handling also apply.

## Crypto and wallets

#### 1. Crypto

Customers can pay with digital assets, which Whop converts to USD at checkout. Crypto supports global customers and offers fast settlement without traditional banking friction.

* **Country:** Global
* **Currency:** USD
* **Minimum–maximum amount:** \$0.50-\$1M
* **Fees:** 1.5%
* **Payment support:** One-time payments only
* **Settlement:** Fast (minutes for on-chain confirmation). Funds then settle to your balance in USD.
* **Refunds:** Yes. You can refund from your balance (e.g. send back to the customer).
* **Disputes:** No. On-chain transactions are irreversible. There is no chargeback or dispute mechanism.

#### 2. Cash App

Cash App Pay is a US mobile wallet that lets customers pay instantly from their Cash App balance or a linked card.

* **Country:** US
* **Currency:** USD
* **Minimum–maximum amount:** \$0.50 – \$1M
* **Fees:** 2.7% + \$0.30
* **Payment support:** One-time and recurring subscriptions
* **Settlement:** Daily batch (ACH) to the PSP, with a configurable cutoff (e.g. 23:00 UTC). Net of refunds and disputes. Then normal payout to you.
* **Refunds:** Yes. You can issue full and partial refunds for a long time (e.g. up to 7 years per Cash App). The refund returns to the customer's Cash App balance, linked card, or bank account.
* **Disputes:** Yes. Customers can dispute (fraud, duplicate charge, refund not received, etc.). Settlement and reconciliation records reflect disputes as adjustments.

#### 3. PayPal

PayPal is a globally recognized digital wallet that lets customers pay using their PayPal balance, linked bank account, or card. PayPal supports 19 different currencies.

* **Country:** Global
* **Currency:** USD, EUR, GBP, SEK, NOK, CZK, + 13 more
* **Minimum–maximum amount:** No set limits
* **Fees:** 2.9% + \$0.30
* **Payment support:** One-time and recurring subscriptions

## Related resources

> [card] [Fees](https://docs.whop.com/payments-and-billing/fees/fees.md)
>
> See full fee details for cards and local payment methods.

> [card] [Regional fees](https://docs.whop.com/payments-and-billing/fees/regional-fees.md)
>
> See card and local payment method rates by country.


## Related topics

- [Regional Fees](https://docs.whop.com/payments-and-billing/fees/regional-fees.md)
- [Adaptive Pricing](https://docs.whop.com/payments-and-billing/fees/adaptive-pricing.md)
- [Whop Pricing and Fees](https://docs.whop.com/payments-and-billing/fees/fees.md)
