> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# What You Can Build with Whop

> Payments, payouts, identity, community, and distribution through one API. Pick your mode and make your first call in about five minutes.

Whop provides everything you need to run your business end-to-end. Accept money through checkout, store it, and withdraw it through payouts. Around that core you get identity verification, memberships and access control, chat and community, and a surface that distributes apps to businesses already making money. One API key reaches everything, with SDKs in TypeScript, Python, and Ruby.

You can charge a customer, hold a balance, verify a seller, pay out a creator, and drop a checkout or a chat into your product without stitching together five vendors. That's the pitch. The rest of this page shows you where everything lives.

## Two ways to build

<CardGroup cols={2}>
  <Card title="Build Whop into your product" icon="code" href="/developer/quickstart">
    Your app, your domain, your users. Whop handles payments, payouts, identity, and money movement behind the scenes through the API and SDKs. Start with an API key.
  </Card>

  <Card title="Build an app that runs inside Whop" icon="cubes" href="/developer/guides/app-views">
    Ship into Whop's marketplace, where thousands of businesses install apps. Auth, views, and distribution are built in. Start with the CLI, not an API key.
  </Card>
</CardGroup>

## Everything on the platform

Each row is a capability. Each column is how deep you integrate: call the API directly, drop in a prebuilt element, send users to a hosted surface, or listen for webhooks.

| Capability                            | API + SDK                                                                                                                                                  | Elements                                                                                      | Hosted           | Guide                                                   |
| ------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- | ---------------- | ------------------------------------------------------- |
| **Payments & checkout**               | [Payments API](/api-reference/beta/overview)                                                                                                               | [Checkout](/elements/beta/checkout/overview), [card fields](/elements/beta/payments/overview) | Checkout links   | [Accept payments](/developer/guides/accept-payments)    |
| **Subscriptions & memberships**       | [Memberships](/api-reference/beta/memberships/membership), [Plans](/api-reference/beta/plans/plan)                                                         | -                                                                                             | -                | [Memberships](/developer/guides/memberships)            |
| **Payouts & money movement**          | [Payouts](/api-reference/beta/payouts/payout), [Transfers](/api-reference/beta/transfers/transfer), [Ledgers](/api-reference/beta/ledgers/ledger-activity) | [Payout portal](/developer/platforms/render-payout-portal)                                    | -                | [Send payouts](/developer/platforms/manual-payouts)     |
| **Wallets, cards & swaps**            | [Cards](/api-reference/beta/cards/card), [Swaps](/api-reference/beta/swaps/swap), [Deposits](/api-reference/beta/deposits/deposit)                         | [Wallet](/elements/beta/wallet/overview)                                                      | -                | -                                                       |
| **Identity & verification**           | [Verifications](/api-reference/beta/verifications/verification)                                                                                            | [Verify element](/sdk/elements/verify-element)                                                | -                | [Verification](/developer/verification/overview)        |
| **Marketplaces & connected accounts** | [Accounts](/api-reference/beta/accounts/account)                                                                                                           | -                                                                                             | onboarding flows | [Platforms quickstart](/developer/platforms/quickstart) |
| **Chat, DMs & forums**                | Chat API                                                                                                                                                   | [Chat](/developer/guides/chat/chat-element), [DMs](/developer/guides/chat/dms-list-element)   | -                | [Chat quickstart](/developer/guides/chat/quickstart)    |
| **Apps on Whop**                      | [Apps](/api-reference/beta/apps/app), [Permissions](/api-reference/beta/permissions/permission)                                                            | [Frosted UI](/developer/guides/frosted_ui)                                                    | Whop marketplace | [App views](/developer/guides/app-views)                |
| **Ads & attribution**                 | [Ad campaigns](/api-reference/beta/ad-campaigns/ad-campaign)                                                                                               | [Ads](/elements/beta/ads/overview), [Tracking](/elements/beta/tracking/overview)              | -                | [Pixel](/developer/ads/pixel)                           |
| **Data & analytics**                  | [Stats](/api-reference/beta/stats/stats), [Ledger stats](/api-reference/beta/ledger-stats/schema)                                                          | -                                                                                             | -                | [Stats](/developer/guides/stats)                        |
| **Affiliates & partners**             | [Partners](/api-reference/beta/partners/partner)                                                                                                           | -                                                                                             | Affiliate links  | [Affiliates](/developer/guides/affiliates)              |

Everything runs against the [sandbox](/developer/guides/sandbox) before it touches real money. Webhooks fire for the events that matter, like `payment.succeeded` and `membership.activated`, so your server always knows what happened.

## Start with what you came for

<CardGroup cols={2}>
  <Card title="You sell a subscription or a one-time product" icon="credit-card" href="/developer/guides/accept-payments">
    Checkout links or embedded checkout. First payment in under an hour.
  </Card>

  <Card title="You owe money to people who aren't you" icon="money-bill-transfer" href="/developer/platforms/manual-payouts">
    Send payouts, embed the payout portal, top up balances.
  </Card>

  <Card title="You take a cut of what other people sell" icon="store" href="/developer/platforms/quickstart">
    Enroll connected accounts, collect on their behalf, pay them out.
  </Card>

  <Card title="You want distribution, not just infrastructure" icon="cubes" href="/developer/guides/app-views">
    Ship an app to businesses already on Whop. Auth and views included.
  </Card>

  <Card title="Your product needs people talking to each other" icon="messages" href="/developer/guides/chat/quickstart">
    Chat, DMs, and forums, embedded in your product.
  </Card>

  <Card title="You just want to read the reference" icon="code" href="/api-reference/beta/overview">
    The versioned Current API, endpoint by endpoint.
  </Card>
</CardGroup>

## Make your first call

<Card title="Start the quickstart" icon="rocket" href="/developer/quickstart" horizontal arrow>
  Create an API key and make your first authenticated call in about five minutes. Everything above starts here.
</Card>
