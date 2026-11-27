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

# Whop Partner Program Guide

> Introduce businesses to Whop and help them set up Payments and Ads as a Whop Partner.

Whop Partners introduce businesses to Whop and help them accept payments and run ads.

## Partner basics

You can introduce a business owner, another partner, or a platform. Start with the prospect's problem, such as failed checkouts, campaign funding, account setup, or reporting.

You earn commission in two ways:

* Payments commissions are a share of eligible Whop gross profit, not of the merchant's sales or total fees.
* Ads commissions are a share of eligible Whop Ads spend.

For example, a 30% share of \$1,000 in eligible gross profit is \$300, and a 1% share of \$20,000 in eligible Ads spend is \$200.

### Demo the customer journey

1. Open the one-time checkout and show the offer, price, and payment methods. On the membership checkout, also show the billing frequency, renewal terms, and how to cancel. Which methods appear depends on the buyer, currency, amount, billing structure, and how the business is configured.
2. Show the business's payment record and balance.
3. In Ads, go from campaign to ad group to ad, checking spend and the intended outcome at each level, then open an ad's creative and destination.

## Referrals

How you refer depends on who you're introducing:

* **A new owner:** copy your standard referral link, then check the person and business it creates.
* **A specific business:** enter its details and the owner's correct email, then check the invitation and the ownership record.
* **Another partner:** they complete enrollment and any required agreement. Before they refer anyone, they should be able to show you their own link and explain how they'll verify a referral.
* **A platform:** bring in an engineer and agree on the scope for account creation or connection, billing, onboarding, and support.

After signup, check the referred user, the business owner, and the referral relationship. You earn on the businesses you referred directly. A business brought in by a partner you referred earns for that partner, not for you.

If the business already has a Whop account, use post-onboarding attribution: add its business ID or link in your partners dashboard, and the owner accepts the request in their business settings. Creating another account or using a different link doesn't change who it's attributed to.

For step-by-step referral links and invitations, see [Refer a business](https://docs.whop.com/refer-businesses-to-whop.md).

## Payments and financing

Ask what the business sells, where its customers are, and how they pay, then walk through a recent purchase to find the checkout or renewal problem worth solving. Compare authorization rates only when both sides count attempts and retries the same way.

Financing providers work differently:

* Splitit runs on the buyer's existing credit card, so the buyer needs available credit for the full purchase even though they pay in installments.
* ClarityPay is a loan application. The buyer sees the offers, monthly payments, total repayment, and due dates they qualify for. Pre-approval is conditional, and signing the agreement is a separate step.

Buyers enter their own details. Explain the terms the provider displays, and send questions about an individual decline to that provider. Find out why a decline happened before trying again.

Read more: [payment fees](https://docs.whop.com/payments-and-billing/fees/fees.md), [financing options](https://docs.whop.com/payments-and-billing/financing/all-bnpl-options.md), and [Splitit](https://docs.whop.com/payments-and-billing/financing/splitit-guide.md).

### Qualify the offer

Whether Whop supports the business, whether its price needs a higher checkout limit, and whether it can offer financing are separate decisions. So is whether an individual buyer gets financing.

Collect the website, sales materials, customer locations, what the customer receives, the access period, the refund policy, and the subscription renewal and cancellation terms. Escalate restricted or unclear offers for review, and never promise approval.

For financing, check the [current requirements](https://docs.whop.com/payments-and-billing/financing/apply.md): a verified Payments account, a complete store page, processing volume, customer counts, and dispute history. New Whop businesses provide processing statements with transaction and dispute counts, not bank statements. Financing applies only to one-time purchases, so assess a business's one-time products and subscriptions as separate offers.

Read more: [business restrictions](https://docs.whop.com/trust-and-safety/trust-safety-overview/what-is-not-allowed-on-whop.md) and [higher checkout limits](https://docs.whop.com/manage-your-business/payment-processing/access-higher-checkout-links.md).

### Activate Payments

The owner enters their own country, identity, and account details. Guide them, but never use your own identity or credentials.

1. Add an eligible payout method and review its timing, fee, and reserve.
2. For a higher-priced offer, submit the information the limit review asks for. Financing needs its own application.
3. Answer any requests for information. Submitting an application isn't the same as approval.
4. After review, open both checkout links with the owner and check the offer, price, billing frequency, and the methods actually available to that buyer.

Read more: [Payments setup](https://docs.whop.com/manage-your-business/manage-payouts/set-up-payouts.md), [payout methods](https://docs.whop.com/manage-your-business/manage-payouts/payout-methods.md), and [financing applications](https://docs.whop.com/payments-and-billing/financing/apply.md).

## Ads and tracking

Bring the decision-maker, the media buyer, and the website owner into onboarding. The advertiser owns strategy, creative, and budget; any agency service agreement is separate.

In Ads settings:

* **General:** check the reporting currency and time zone. Reporting currency only changes how numbers display, so confirm the billing currency too.
* **Billing:** review the available primary and backup funding methods. The owner adds and authorizes funding.
* **Facebook and Instagram:** connect the Facebook page and any required Instagram profile through the owner's connection link. Connecting a page doesn't import its old ad account or give access to Whop's Business Manager.

For an external funnel, add the business's pixel across the whole customer journey, including redirects, and fire the lead, booked-call, or purchase event where that action happens. Purchases on Whop checkout are tracked automatically; purchases elsewhere need their own integration. If you combine browser and server events, have the technical owner check identifiers and duplicate handling.

Before spending, walk through the checkout and make sure your events are recorded. Then confirm:

* creative, landing page, policy requirements, and any special ad category
* events and funding
* budget, campaign owner, and launch date

Read more: [Whop Ads](https://docs.whop.com/manage-your-business/growth-marketing/ads.md), [Whop Pixel](https://docs.whop.com/developer/ads/pixel.md), and [tracking integrations](https://docs.whop.com/manage-your-business/growth-marketing/tracking-integrations.md).

## Migrations

| Job | What to verify |
| - | - |
| Future subscription billing | Which memberships move, their amounts and next renewals, customer communication, and the result report. |
| Processing evidence | The requested business, period, transaction counts, and disputes for a Payments or financing review. |
| Historical analytics or Ads data | The source, accepted records, destination, transfer channel, technical owner, and success check. |

For future billing, go to **Settings → Payments → Migrations → Stripe** and check the preview's customers, amounts, and renewal dates. Follow the payment-method transfer instructions shown there, including inviting the unique Whop email with the Data migration specialist role.

Review eligible subscriptions and exclusions before you confirm. A subscription is eligible if it's active or trialing, uses a card, has a customer email, and charges at least \$1. Link, ACH, and SEPA payments are excluded. Check for any business-specific exclusions and records that were already migrated.

Tell the owner what customers will receive: Whop sets migrated Stripe subscriptions to cancel at the end of the current period, and Whop handles future renewals and billing communications. When the migration finishes, reconcile the report with the plan and give each exception an action and an owner.

Moving subscriptions doesn't move processing statements or historical Ads data. Agree on the approved transfer instructions before sending any customer data, and don't resubmit purchases Whop already recorded as external events.

## Fees and earnings

### Set merchant fees

Once a business is attributed to you, you can lower its processing fees yourself, within the range the fee editor shows. Use this only as a last resort to close a deal or keep a business: a lower fee lowers the gross profit Whop earns from that business, and so your commission. The owner is notified of any change.

1. In the fee editor, confirm the payment method, region, country, and unit. A global card fallback is a different row from a domestic card, and a percentage is different from a fixed fee.
2. Check the default and the allowed range, then enter the new value with a clear reason.
3. Save, then reopen the schedule and check it.

### Track earnings

Compare business volume and commissions over the same reporting period. Sales volume isn't commission.

| Commission status | Meaning |
| - | - |
| Awaiting settlement | The underlying transaction hasn't settled. |
| Pending | The transaction settled, but Whop hasn't paid the commission yet. |
| Completed | Whop has paid the commission. |
| Canceled or reversed | Check the reason and reconcile the affected activity. |

If Ads reporting looks off, check events and the visitor path first. Match dates, time zones, conversion definitions, and attribution settings before deciding performance changed, then review the campaign and agreed test window with the media buyer.

For rejected ads or restrictions, check the reason, special ad category, creative, and landing-page claims, then take it to your partner channel, where a named Whop contact owns your escalations. Bring one complete issue at a time: the business ID, the link, and a screenshot.

## Next steps

These product guides help when they fit a prospect:

* [Tap to Pay](https://docs.whop.com/payments-and-billing/tap-to-pay.md): in-person contactless payments on a supported iPhone.
* [Whop Cards](https://docs.whop.com/whop-finance/cards.md): spending from the business balance. Check current eligibility and card terms.
* [Payouts](https://docs.whop.com/manage-your-business/manage-payouts/set-up-payouts.md): moving funds through the business's available payout methods.
* [Whop CLI and agents](https://docs.whop.com/developer/guides/cli-with-claude.md): technical workflows to review with an engineer.

### Before your next introduction

* You know the prospect's problem, offer, and customer journey.
* The person, business, referral path, and terms are checked.
* Pricing is agreed and the review evidence is collected.
* Someone owns activation, funding, tracking, and support.
* The checkout works, and any launch or migration result is verified.
* You leave with a written decision, next step, owner, and date.

> [card] [Open Partners](https://whop.com/partners)
>
> Manage your referrals, links, and earnings.

> [card] [Get support](https://docs.whop.com/get-help/whop-support.md)
>
> Bring the business, transaction, and specific question to the right team.


## Related topics

- [Refer Businesses to Whop](https://docs.whop.com/refer-businesses-to-whop.md)
- [Affiliate Program](https://docs.whop.com/manage-your-business/growth-marketing/affiliate-program.md)
- [Educational Programs](https://docs.whop.com/supported-business-models/educational-programs.md)
