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

# Create Deposit

> Retrieve the deposit methods for an account, including crypto and bank transfer. Crypto deposits require a $10 minimum.

> [!NOTE]
>
> Crypto deposits require a \$10 minimum.


## OpenAPI

````yaml openapi/api-v1-native.json POST /deposits
openapi: 3.1.0
info:
  description: >-
    The Whop REST API. Please see
    https://docs.whop.com/developer/api/getting-started for more details.
  termsOfService: https://whop.com/tos-developer-api/
  title: Whop API
  version: 1.0.0
  x-api-version-date: '2026-11-25'
servers:
  - description: Production Whop API
    url: https://api.whop.com/api/v1
  - description: Sandbox Whop API
    url: https://sandbox-api.whop.com/api/v1
security: []
tags:
  - description: >
      An Account represents a person or business on Whop that can have its own
      profile, wallet, and account-scoped settings. Use accounts for customers,
      creators, merchants, sellers, or connected businesses your integration
      supports.


      Use the Accounts API to create accounts, list accounts visible to your
      credentials, retrieve or update an account, suspend or delete a subaccount
      managed by your platform, and retrieve the account associated with the
      current API key.
    name: Accounts
    x-whop-summary: 'A business on Whop: profile, wallet, capabilities, settings.'
  - description: >
      A User represents a person on Whop. Users have a public profile and can
      buy products, join accounts, and access experiences.


      Use the Users API to search for users, retrieve or update profiles, and
      check whether a user has access to an account, product, or experience.
    name: Users
    x-whop-summary: 'A person on Whop: profile and connected identities.'
  - description: >
      A Member is one buyer's relationship with an account: one record per
      customer, however many memberships they hold. It tracks whether they have
      joined or left, their access level, and when they last opened the
      account's content.


      Use the Members API to list and search an account's members, retrieve a
      single member, and read a member's activity log. Member rows are created
      and maintained by the membership lifecycle; to grant or revoke access,
      work with memberships instead.
    name: Members
    x-whop-summary: One buyer's relationship with an account, across all their purchases.
  - description: >
      A Team Member is a member of an account's team: the link between a user
      and an account, carrying the role that controls what they can do. Roles
      are either system roles (like `admin` or `moderator`) or `custom` roles
      managed from the dashboard.


      Use the Team Members API to list an account's team, add a user to the team
      with a system role, change a member's role, and remove members. Adding a
      user who has not yet accepted sends an invitation instead.
    name: Team Members
    x-whop-summary: An account's team members and the roles that scope their access.
  - description: >
      Search finds one account's records in one call: members, memberships,
      payments, affiliates, reviews, resolution center cases, promo codes and
      accounts. A pasted identifier finds its records exactly and they come
      first, ahead of ranked text matches.


      Use the Search API behind a search box or a command menu over an account's
      data: find the customer behind an email or a card, open a payment from its
      ID or invoice number, or jump to a member by name. To filter one resource
      and page through every match, use that resource's list endpoint instead.
    name: Search
    x-whop-summary: >-
      Find an account's members, payments and other records in one call, by ID,
      email, card, phone, number, link or text.
  - description: >
      An External Account represents an account on another platform linked to a
      Whop account or user, such as a Facebook page, Instagram account, TikTok
      account, or Klaviyo account. You connect it through OAuth or an API key,
      or Whop creates it for you. External account IDs are prefixed `sacc_`.


      Use the External Accounts API to list connected accounts, [connect a
      profile through
      OAuth](/api-reference/external-accounts/connect-an-external-account),
      [create a Whop-managed Facebook page or TikTok account, or connect a
      Klaviyo account with its API
      key](/api-reference/external-accounts/create-an-external-account), and
      disconnect an external account.
    name: External Accounts
    x-whop-summary: Accounts on other platforms linked to an account or user.
  - description: >
      Social Accounts is the former public name for External Accounts. Existing
      integrations can keep calling these deprecated endpoints while they
      migrate; every response points to the matching External Accounts endpoint.


      Use the External Accounts API for all new integrations. External account
      IDs keep their `sacc_` prefix, and the underlying resource is unchanged.
    name: Social Accounts
    x-whop-summary: Deprecated compatibility endpoints for external accounts.
  - description: >
      A Payment is one charge against a buyer, for one or more variants. Create
      an on-session payment with a `confirmation_token` for the method the buyer
      selected, or an off-session payment with an existing member's stored
      payment method.


      Use the Payments API to charge buyers, list and retrieve payments, and
      refund, void, capture, or retry them. Collection runs in the background,
      so the create response is not the outcome. Poll [Retrieve
      status](/api-reference/payments/retrieve-payment-status) for how far the
      payment has gone and, while it is `requires_action`, what the buyer must
      do next: follow a redirect, complete 3D Secure, display transfer
      instructions, or link a bank account. Update the payment's `return_url` to
      change where they land afterwards, up until they come back.
    name: Payments
    x-whop-summary: A charge against a buyer, and the step they still owe.
  - description: >
      An Invoice is a receipt with an invoice number. Create a draft from
      variant-backed items, set its recipient and due date, and edit it until it
      is sent. Its `pay_` ID is also the payment ID when collected.


      Use the Invoices API to [create](/api-reference/invoices/create-invoice)
      and edit drafts, then [issue](/api-reference/invoices/issue-invoice) or
      [send](/api-reference/invoices/send-invoice) them. You can
      [charge](/api-reference/invoices/charge-invoice) an open invoice to a
      saved payment method, or [void](/api-reference/invoices/void-invoice) an
      invoice the buyer will not pay.


      Draft creation does not create access or charge the buyer. The recipient's
      contact details live on the account's member. Financial fields and caller
      metadata can only be edited in draft.
    name: Invoices
    x-whop-summary: >-
      A payment request that keeps the same receipt from draft through
      collection.
  - description: >
      A Confirmation Token is a single-use, short-lived reference to a payment
      method and billing details collected from a buyer. Its response never
      returns the underlying payment credential.


      Whop Elements mint the token in your buyer-facing collection flow and hand
      you its `ctok_` ID to send to the Payments API from your server. Retrieve
      a token to display its payment method and billing preview or check whether
      it is still usable.
    name: Confirmation Tokens
    x-whop-summary: A short-lived reference to payment details collected from a buyer.
  - description: >
      A Payment Quote prices a purchase exactly as a payment for it will be
      charged: the same line pricing, promo code, and tax, for a buyer located
      by the address, country, or IP address you send. A quote is priced once,
      can be consumed by one payment, and expires.


      Use the Payment Quotes API to show a buyer the total, tax, and currency
      before they pay, then pass the quote's `id` as `quote_id` when you create
      the payment to charge exactly that quote. Quote again whenever the cart,
      promo code, or address changes.
    name: Payment Quotes
    x-whop-summary: 'A purchase priced before it is paid: its lines, promo code and tax.'
  - description: >
      A Setup Intent saves a buyer's payment method for later without taking
      money now. Create one from a confirmation token the payment elements
      collected in setup mode, or from a payment method already on file to
      re-verify it. It runs the same collection flow a payment does, so the
      buyer may still owe a step: 3D Secure on a card, a hosted enrollment, or
      linking a bank account.


      The create response is the setup intent as created, not its outcome. Hand
      its `client_secret` to the elements' `handleNextAction`, or poll [Retrieve
      status](/api-reference/setup-intents/retrieve-setup-status) for how far
      the setup has gone and what is outstanding. Once it reaches `succeeded`,
      `payment_method_id` names the saved method and Create Payment charges it.
    name: Setup Intents
    x-whop-summary: Saving a buyer's payment method without charging it.
  - description: >
      A Refund is one full or partial reversal of a payment, issued with [Refund
      Payment](/api-reference/payments/refund-payment). It records how much
      moved, through which provider, and whether it succeeded.


      Use the Refunds API to list the refunds on a payment or across an account,
      and to retrieve a single refund.
    name: Refunds
    x-whop-summary: Money returned to a buyer from a payment.
  - description: >
      A Resolution Center Case is opened by a buyer when something is wrong with
      a purchase, such as an unwanted renewal, an item that never arrived, or a
      charge they don't recognize. It is the step before a chargeback: the buyer
      and merchant work it out directly, and Whop decides the case if they
      can't.


      Use the Resolution Center Cases API from either side: as the buyer, open,
      reply to, appeal, or withdraw a case; as the merchant, accept it
      (refunding the payment), deny it, or ask the buyer for more information.
    name: Resolution Center Cases
    x-whop-summary: File or respond to a case against a payment, as the buyer or the merchant.
  - description: >
      A Dispute alert is an early warning from a card issuer that a settled
      payment is being questioned, ahead of any chargeback. `type` separates
      fraud reports (`early_fraud_warning`), pre-dispute notices
      (`dispute_alert`), and Visa RDR cases the network already closed by
      refunding (`rapid_dispute_resolution`).


      Use the Dispute alerts API to list alerts for an account, filter them by
      type or payment, and read `actionable` to see whether refunding can still
      avoid the chargeback.
    name: Dispute alerts
    x-whop-summary: Issuer warnings that arrive before a chargeback does.
  - description: >
      A Dispute is a chargeback a customer files against a payment through their
      bank, or an inquiry that may become one. It carries the disputed payment,
      a deadline to respond, your evidence, and the outcome once the processor
      rules.


      Use the Disputes API to list disputes, edit the evidence packet while a
      dispute is still contestable, and submit it for review, or accept a
      chargeback you don't want to contest.
    name: Disputes
    x-whop-summary: Chargebacks filed against an account, with evidence and outcomes.
  - description: >
      A Payment Rule lets an account block, allow, review, or challenge its own
      payments, including payments that fail a CVV or billing address check. A
      challenge asks the buyer for 3D Secure. Most rules run before the payment
      reaches the bank. A rule on the bank's CVV check or address verification
      (AVS) check runs after the bank authorizes the card. Each rule matches on
      a small set of payment attributes, and every condition must hold for it to
      apply.


      A rule's definition is fixed once created, so the payments it decided keep
      naming the rule that decided them. Use
      [Replace](/api-reference/payment-rules/replace-a-payment-rule) to change
      one, and [List fields](/api-reference/payment-rules/list-fields) for the
      attributes, operators and values a condition can use.
    name: Payment Rules
    x-whop-summary: Rules an account writes to decide its own payments.
  - description: >
      A Financing Application is an account's request to accept
      payment-financing methods, such as buy now, pay later. Whop reviews the
      account before it can offer them. It is not a loan or business capital.


      Use the Financing Applications API to
      [create](/api-reference/financing-applications/create-financing-application)
      an application, [save
      answers](/api-reference/financing-applications/update-financing-application)
      to its requirements, and
      [submit](/api-reference/financing-applications/submit-financing-application)
      it for review. You can manage applications for your own account and its
      direct subaccounts.
    name: Financing Applications
    x-whop-summary: An account's request to accept buy-now-pay-later payment methods.
  - description: >
      Deposits describe the ways to add funds to an account or user balance: a
      hosted deposit page, bank transfer instructions, and crypto wallet
      addresses.


      Use the Deposits API to get deposit instructions for an account or user,
      such as a crypto address to send funds to or bank details for a transfer.
    name: Deposits
    x-whop-summary: Add funds to a balance.
  - description: >
      A Transfer moves money between Whop balances: from one account or user to
      another, from an account's wallet to a person or address, or into a claim
      link anyone with the URL can redeem.


      Use the Transfers API to send money, find recipients you can pay, list and
      retrieve transfers when reconciling money movement, and preview or redeem
      claim links.


      Subscribe to `transfer.completed` and `transfer.failed` for outcomes
      instead of polling. Each participating account can subscribe to these
      events. `transfer.created` is also emitted on success, not when processing
      starts. A failed transfer can be retried under the same ID and later
      succeed; retrieve the transfer to reconcile its current status.


      A successful balance transfer credits the recipient's available balance
      unless a release date applies. Transfers funded from pending balance
      retain a release date and credit pending balance; applicable recipient
      reserves or fraud holds can keep funds unavailable. `succeeded` confirms
      the transfer completed, not that all funds are withdrawable.
    name: Transfers
    x-whop-summary: Move funds between Whop accounts and users.
  - description: >
      Payouts represent money sent from an account or user balance to an
      external destination, such as a bank account, wallet, or other saved
      payout method.


      Use the Payouts API to create and track payouts, manage saved payout
      methods, and show expected arrival details for funds leaving Whop.
    name: Payouts
    x-whop-summary: Send money from a balance to a bank or wallet.
  - description: >
      A Financial Activity row is a single movement of money in or out of an
      account's or user's balance: a payment, payout, refund, transfer, on-chain
      deposit, swap, or card transaction. Each row carries a typed `resource`
      and `source` so you can present and link the event without extra lookups.


      Use the Financial Activity API to build a statement or transaction feed
      for an account or user, and financial reports to summarize balance
      activity, income, and balances over a date range, then break a report
      total down into what drove it.
    name: Ledgers
    x-whop-docs-title: Financial Activity
    x-whop-summary: The activity feed behind an account or user's balance.
  - description: >
      Cards represent virtual payment cards that spend from an account or user
      balance. Cards can be assigned to cardholders and configured with spending
      limits for controlled spending.


      Use the Cards API to issue cards, list cards for an account or user,
      retrieve active card details such as the card number and CVC, and list the
      transactions made with them.
    name: Cards
    x-whop-summary: Issue cards that spend from a balance.
  - description: >
      A Cashback Rule pays a platform's direct subaccounts back a share of their
      card spend, funded by the platform. Each rule sets a rate, an eligibility
      window, and optional merchant filters, and can be limited to one
      subaccount.


      Use the Cashback Rules API to create, update, and list the rules your
      platform funds, then pay out cashback on demand from your available USD
      balance.
    name: Cashback Rules
    x-whop-summary: Configure platform-sponsored card cashback.
  - description: >
      A Swap converts value between supported tokens, chains, or wallet
      destinations, or between fiat currencies at the mid-market rate, for an
      account or user. A swap quote previews the expected output, fees, and
      approval requirements before you create the swap.


      Use the Swaps API to quote a conversion, create the swap, list recent
      swaps, and retrieve a swap's status until it completes.
    name: Swaps
    x-whop-summary: Convert a balance between currencies.
  - description: >
      A Trade is one request on an account's or user's Whop-managed wallet. An
      `order` on a perpetual trades from the trading balance, bridging the USDT0
      it needs from the wallet first when the balance is short; an `order` on
      spot swaps between USDT0 and the asset in the wallet. A `close` closes the
      whole position in one market; when the account is then flat, a `withdraw`
      trade sends the trading balance back to the wallet. A `deposit` or
      `withdraw` moves money between the wallet and the trading balance.
      Perpetuals trade on Hyperliquid.


      Use the Trades API to list the markets you can trade, place orders, close
      positions and move funds, and follow each trade until it finishes. Trades
      run in the background; an order that bridged money in and does not fill
      sends that money back to the wallet, and `funds_location` says where the
      money is.


      The trading API, including `include_trading` on accounts and users, is in
      beta. It can change without a new API version date.
    name: Trades
    x-whop-summary: >-
      Place orders, close positions, and move funds for perpetual and spot
      trading from a Whop-managed wallet.
  - description: >
      A Verification represents a legal identity for a person or business.
      Accounts and users complete verification when Whop needs to confirm who
      they are before enabling payouts or compliance-sensitive workflows.


      Use the Verifications API to start or resume a hosted verification
      session, check review status, and submit requested details or documents.
      If `requested_information` contains items, submit answers with [Update
      Verification](/api-reference/verifications/update-verification).
    name: Verifications
    x-whop-summary: Legal identity required before payouts and card issuing.
  - description: >
      A Product is a digital good or service sold on Whop. Products contain
      variants for pricing and may contain experiences for content delivery.


      Use the Products API to search the public marketplace, list an account's
      products, retrieve a product, and create, update, or delete products.
    name: Products
    x-whop-summary: The things you sell. Each owns variants and a store page.
  - description: >
      A Variant is one purchasable configuration of a product. It controls
      price, billing cadence, stock, SKU, attributes, tax behavior, checkout
      fields, and purchase visibility. Variant IDs remain prefixed `plan_`.


      Use the Variants API to create variants for products, list existing
      variants, retrieve or update variant configuration, calculate tax for
      checkout, and delete variants that should no longer be offered.
    name: Variants
    x-whop-summary: Purchasable configurations of a product.
  - description: >
      A Promo Code is a discount a buyer enters at checkout, either a percentage
      or a fixed amount off. It belongs to an account, can be limited to one
      product, and controls who can redeem it, how many times, until when, and
      for how many billing periods the discount lasts.


      Use the Promo Codes API to create codes for an account, list and retrieve
      them, deactivate a code to pause redemptions or activate it again, and
      archive codes you no longer offer.
    name: Promo Codes
    x-whop-summary: Discounts that creators configure for checkout.
  - description: >
      A Checkout Configuration is a reusable checkout link owned by an account.
      In `payment` mode it sells a specific variant; in `setup` mode it collects
      and saves payment details without charging. Each configuration can also
      override which payment methods are accepted and how 3D Secure is enforced
      for that checkout.


      Use the Checkout Configurations API to create checkout links for an
      existing or inline variant, list configurations for an account, retrieve
      the configuration behind a checkout URL, and delete links that should no
      longer be used.
    name: Checkout Configurations
    x-whop-summary: Turn a variant into a shareable, prefilled checkout link.
  - description: >
      A Waitlist Entry is a user's signup for a variant that requires seller
      approval. Joining doesn't grant a membership or charge a payment method;
      the seller approves or denies each signup.


      Use the Waitlist Entries API to join a waitlist, read or cancel your own
      signups, and approve or deny the signups for accounts you operate.
    name: Waitlist Entries
    x-whop-summary: Join waitlists and manage customer signups awaiting approval.
  - description: >
      A Membership is a customer's purchase of a variant: the subscription or
      one-time grant that gives them access to a product. It tracks billing
      state (`active`, `trialing`, `past_due`, and so on), the current period,
      pending cancellations, custom metadata, and the software license key when
      the product includes licensing.


      Use the Memberships API to list an account's memberships or your own,
      retrieve one by ID or license key, invite a recipient to join through a
      free variant, and manage the lifecycle: cancel immediately or at period
      end, reverse a scheduled cancellation, pause and resume payment
      collection, extend or reactivate access, apply a promo code or affiliate,
      change renewal cadence, resync access, generate a transfer link, and
      update metadata.
    name: Memberships
    x-whop-summary: A customer's purchase of a variant, from checkout through cancellation.
  - description: >
      A Shipment attaches a carrier tracking number to a payment and follows the
      package from label creation to delivery, exposing the current delivery
      status and a customer-facing tracking URL.


      Use the Shipments API to list an account's shipments, retrieve one by its
      ID or the payment it fulfills, attach a tracking number to a payment, and
      update the tracking number on an existing shipment.
    name: Shipments
    x-whop-summary: Track the delivery of an order by its carrier tracking number.
  - description: >
      A Review is a customer's star rating and optional written feedback on a
      product, shown on the product's store page.


      Use the Reviews API to list a product's reviews, filtered by star rating
      or creation date, and retrieve a review by ID.
    name: Reviews
    x-whop-summary: Customer star ratings and written feedback on a product.
  - description: >
      A Review Report is a request from an account's admins for Whop staff to
      review a customer's review. Staff remove the review if it breaks the
      guidelines.


      Use the Review Reports API to list the reports filed on an account's
      reviews, narrowed by review, status, or creation date. Check `status` to
      see whether a report is still pending.
    name: Review reports
    x-whop-summary: Reports an account's admins filed against customer reviews.
  - description: >
      A Product Affiliate represents a user's enrollment in a product's
      affiliate program. An enrollment can exist before the user makes a
      referral or earns a reward.


      Use the Product Affiliates API to list product affiliates for an account,
      search by affiliate name or username, and narrow the results by product,
      enrollment status, or signup date.
    name: Product Affiliates
    x-whop-summary: >-
      Users enrolled to promote a product, including affiliates with no
      referrals.
  - description: >
      A Domain is a hostname an account holds on Whop. Its capabilities set what
      Whop does with it: `registration` buys the domain through Whop, runs its
      DNS, and renews it every year; `verification` connects a domain registered
      elsewhere once you publish the DNS records it returns, and expires if
      ownership is not proven within 48 hours; `website` serves one of the
      account's apps on it.


      Use the Domains API to search for a domain to buy, buy or connect one,
      serve a website on it, and check its DNS, payment, and renewal state.
      Thirty days before a bought domain expires, Whop opens a renewal charge at
      `purchase_url` and, while `auto_renew` is on, charges the saved card. An
      unpaid domain stops serving when it expires but stays renewable at
      `purchase_url` until the registry's grace period ends, and is then
      removed.
    name: Domains
    x-whop-summary: Custom domains assigned to hosted apps.
  - description: >
      An App is software you build on Whop. It can be a hosted web app served at
      its domains, such as a `whop.site` address or a custom domain, or an API
      integration installed as an experience, and it belongs to the account that
      owns its credentials, settings, builds, and runtime logs.


      Use the Apps API to manage app configuration, deploy an app's working copy
      and follow the run on the app's `deployment` field, and, for hosted apps,
      read server runtime logs for console output, uncaught exceptions, and
      failed requests.


      Business apps have one App Store category. List the categories with `GET
      /apps/categories`, then set the app's `category_id` to the chosen
      category's ID. Use a category's `route` as `marketplace_category_route`
      when listing apps.


      Apps are also reusable blueprints. List official blueprints with
      `app_type=website&verified=true&order=template_usage`, or community
      blueprints with
      `app_type=website&verified=false&recommended=true&order=template_usage`.
      Pass the returned App `id` as `blueprint_id` when creating an Account.
    name: Apps
    x-whop-summary: 'Apps you build on Whop: metadata, hosted builds, runtime logs.'
  - description: >
      An App Build is a versioned artifact uploaded for an app — a hosted web
      archive, or an iOS/Android bundle. Builds start as drafts, go through
      review, and one approved build per platform is served to users as the
      production build.


      Use the App Builds API to upload a build for an app, list an app's builds
      with platform and status filters, retrieve a build, and promote a draft or
      approved build to production.
    name: App Builds
    x-whop-summary: Versioned build artifacts deployed to an app's platforms.
  - description: >
      An App Installation represents one install of your app on one account. An
      account installs an app once, however many experiences it adds, so an
      installation holds state that belongs to the account rather than to one
      experience, such as a sync cursor or the ID of the account it's linked to
      in another system.


      Use the App Installations API to [list the accounts your app is installed
      on](/api-reference/app-installations/list-app-installations), [retrieve
      one](/api-reference/app-installations/retrieve-app-installation), and
      [update](/api-reference/app-installations/update-app-installation) the
      `metadata` and `external_id` your app keeps on it. An account can also
      list the apps installed on it, and a developer every installation of their
      app. Only the app and its developer see `metadata` and `external_id`, and
      only the app can change them.
    name: App Installations
    x-whop-summary: >-
      Your app's installations on accounts, and the data it keeps about each
      one.
  - description: >
      An Experiment is a feature flag or A/B test owned by an account.
      Treatments take stable percentage ranges of traffic and everyone else gets
      control, so growing an allocation never moves an existing user to another
      arm.


      Use the Experiments API to create a draft, configure its weights,
      targeting, and resource bindings, then activate, pause, or end it, and to
      evaluate which arm a user, account, or anonymous visitor gets. Managing
      experiments requires `experiment:read` or `experiment:manage`; evaluation
      works without authentication.
    name: Experiments
    x-whop-summary: Feature flags and A/B experiments for gradual rollout.
  - description: >
      An Ad Campaign is the top-level container for paid ads. It sets the
      objective and budget strategy shared by its [ad
      groups](/api-reference/ad-groups/ad-group) and ads.


      Use the Ad Campaigns API to create campaigns, list campaigns for an
      account, retrieve or update campaign settings, and pause or resume
      campaign delivery.


      Ads billing combines eligible spend across the account's campaigns. A
      failed payment blocks delivery with `delivery_status: payment_failed`
      while preserving the configured active/paused `status`. Fix the account's
      payment method and [retry its ads
      payment](/api-reference/accounts/retry-failed-ads-payments) once for the
      account. The retry is asynchronous: acceptance does not confirm payment.
      Successful settlement clears the block; active campaigns can resume if
      otherwise eligible, while paused campaigns stay paused. See [billing and
      retries](/developer/ads/overview#paying-for-ads).
    name: Ad Campaigns
    x-whop-summary: Objective and budget for a set of ads.
  - description: >
      An Ad Group sits inside an [ad
      campaign](/api-reference/ad-campaigns/ad-campaign) and controls delivery
      for [ads](/api-reference/ads/ad). It sets the audience, schedule, and
      budget for its ads.


      Use the Ad Groups API to create ad groups in campaigns, list or retrieve
      targeting and delivery settings, update budgets or targeting, delete
      groups that should stop running, and pause or resume delivery.
    name: Ad Groups
    x-whop-summary: Audience, schedule, and budget within a campaign.
  - description: >
      An Ad is the individual creative unit delivered by an [ad
      group](/api-reference/ad-groups/ad-group). It holds the copy, creative
      assets, and destination URL for one ad.


      Use the Ads API to list ads for an account, create ads inside ad groups,
      retrieve or update creative details, delete ads that should stop running,
      and pause or resume delivery.
    name: Ads
    x-whop-summary: 'The creative: copy, assets, and destination URL.'
  - description: >
      An Audience is a reusable group of people to include or exclude when
      targeting ads. Build custom audiences from customer lists or Whop People
      data, and create lookalikes to reach people similar to an existing
      audience.


      Use the Audiences API to create audiences, add people to an uploaded
      customer list, rename or refilter an audience, and monitor asynchronous
      processing. Whop People audiences can refresh automatically or keep a
      snapshot.
    name: Audiences
    x-whop-summary: Reusable targeting lists for ad groups.
  - description: >
      An Ad Pixel connects a pixel you own on an ad platform to a business whose
      campaigns you claimed through the advertiser supplier program. Whop sends
      that pixel the conversions credited to your claimed campaigns for that
      business, using the access token you provide.


      Use the Ad Pixels API to add one pixel per business on each ad platform,
      list or retrieve your pixels, replace an access token the ad platform
      rejected, and remove a pixel to stop deliveries.
    name: Ad Pixels
    x-whop-summary: Your own pixels that receive conversions from campaigns you claimed.
  - description: >
      Stats represent aggregated activity for an account over time. They help
      you understand revenue, transactions, disputes, members, referrals, and
      advertising performance across reporting periods like days, weeks, or
      months.


      Use the Stats API to list the metrics you can chart and retrieve one as a
      time series over a date range, or list reports and retrieve aggregates
      that are not bucketed over time, such as platform-wide payment trends.
    name: Stats
    x-whop-summary: Aggregated financial, audience, and traffic reporting.
  - description: >
      A Person is an identity-linked profile of a visitor or customer of an
      account, assembled from every [event](/api-reference/events/event) the
      person generated, such as pixel page views, ad clicks, leads, and
      payments.


      Use the People API to list and segment the people of an account by
      activity, purchases, traffic source, or marketing touch, and to retrieve
      one person by person ID, user ID, email address, or phone number.
    name: People
    x-whop-summary: >-
      Visitors and customers of an account, with identity, purchase, and traffic
      profiles.
  - description: >
      An Event records conversion or engagement activity for an account, such as
      page views, purchases, or leads. Each event ties the action to the
      [person](/api-reference/people/person) who took it, so activity can be
      attributed to the ads and links that drove it.


      Use the Events API to send new tracking events, list recent
      identity-linked events for an account, and inspect the events recorded for
      a person. The resource also exposes an anonymized read mode — the pulse
      feed — a platform-wide snapshot of recent purchases that carries nothing
      identifying. The pulse feed is public; other Events endpoints require
      authentication and are scoped to an account.


      Events are only as good as the pixel sending them, so [Validate
      Pixel](/api-reference/events/validate-pixel) answers whether an account's
      pixel is working: it reads the events the pixel has sent, and when you
      pass a `url` whose page hasn't sent any lately, it fetches that page and
      looks for the pixel in its source. Use it before launching an ad to
      confirm its destination is tracked, or in a setup flow to tell a merchant
      whether their install is live.
    name: Events
    x-whop-summary: Conversion and engagement events tracked for attribution.
  - description: >
      An Export is a CSV of one of an account's dashboard tables, such as
      members, payments, or disputes. Exports are generated in the background,
      so a new export starts `pending` and becomes `completed` with a download
      link.


      Use the Exports API to start an export, poll it until `download_url` is
      set, and list an account's past exports. Each table requires its own
      export scope, and finished files are deleted after 30 days.
    name: Exports
    x-whop-summary: Asynchronous CSV dumps of an account's dashboard data.
  - description: >
      A recommended action is one change that Whop recommends for an account
      with Economic Intelligence. Its `workflow` lists the API calls it makes
      when it runs.


      Use the Recommended Actions API to list an account's actions, run a
      `ready` action, or dismiss it. A run sends each step in order, as the user
      who runs it. Retrieve the action to follow each step's `status`.
    name: Recommended Actions
    x-whop-summary: Changes Whop recommends for an account, which its owner runs or dismisses.
  - description: >
      A Channel is a conversation on Whop that holds chat messages. A `private`
      channel is a one-to-one or group conversation. A `community` channel is
      the chat in one of an account's Chat experiences. A `support` channel is a
      conversation between an account and one customer.


      Use the Channels API to [create](/api-reference/channels/create-channel)
      and [list](/api-reference/channels/list-channels) channels, and to
      [retrieve](/api-reference/channels/retrieve-channel) one by its own ID or
      its Chat experience's ID. You can also
      [accept](/api-reference/channels/accept-dm-request) or
      [decline](/api-reference/channels/decline-dm-request) direct-message
      requests, [mark a channel
      read](/api-reference/channels/mark-channel-read), and
      [resolve](/api-reference/channels/resolve-support-channel) or
      [reopen](/api-reference/channels/reopen-support-channel) support
      conversations.
    name: Channels
    x-whop-summary: Private, community and support chat channels.
  - description: >
      A Channel Member is a person in a channel. A person added to a private or
      support channel has a stored membership, with a read position and a
      notification preference. A person who reads a channel through their access
      to its experience or support inbox has no stored membership. Their `id` is
      `null`, and their read position is not shown.


      Use the Channel Members API to [list or
      search](/api-reference/channel-members/list-channel-members) the people in
      a channel and [add](/api-reference/channel-members/add-channel-member)
      someone to a private or support channel. You can also [change your
      notification
      preference](/api-reference/channel-members/update-channel-member).
      [Remove](/api-reference/channel-members/remove-channel-member) a member,
      or remove yourself to leave a channel.
    name: Channel Members
    x-whop-summary: Channel participants and personal membership state.
  - description: >
      A Message is one post in a channel. It holds Markdown content and can
      carry attachments, a poll, or a reply to another message.


      Use the Messages API to [send](/api-reference/messages/send-message),
      [edit](/api-reference/messages/update-message),
      [delete](/api-reference/messages/delete-message) and
      [pin](/api-reference/messages/pin-message) messages. [List a channel's
      messages](/api-reference/messages/list-messages) to read its history and
      to sync edits and deletions. You can also
      [react](/api-reference/messages/react-to-message) with an emoji, [list who
      reacted](/api-reference/messages/list-message-reactions), and
      [vote](/api-reference/messages/vote-on-message) once on a poll. Each
      message shows its reaction and vote counts and whether you reacted or
      voted, but not who else voted.
    name: Messages
    x-whop-summary: Chat messages, attachments, reactions and polls.
  - description: >
      A Livestream represents a live video broadcast in an experience. After a
      recorded livestream ends, members can watch the replay, and moderators can
      pin the best recordings to feature them.


      Use the Livestreams API to [list an experience's
      livestreams](/api-reference/livestreams/list-livestreams), [retrieve
      one](/api-reference/livestreams/retrieve-livestream) with its recording,
      and [pin](/api-reference/livestreams/pin-livestream) or
      [unpin](/api-reference/livestreams/unpin-livestream) recordings.
    name: Livestreams
    x-whop-summary: Live video broadcasts in an experience, and their recordings.
  - description: >
      A Notification is a message delivered to a user, such as a new post, a
      payment, or a mention. Each one comes from an experience the user belongs
      to or a team they are on, and falls under a topic, like new sales, that
      users set preferences on.


      Use the Notifications API to list a user's feed, read and clear unread
      badges, send notifications from your app to an experience's users or an
      account's team, and list the topic catalog that notification preferences
      reference.
    name: Notifications
    x-whop-summary: >-
      The user's notification feed: unread badges, mark-read, app sends, and the
      topic catalog.
  - description: >
      An Email address is a mailbox on one of the account's domains, such as
      support@example.com. The domain needs the `email` capability in the
      `ready` state first. A mailbox can belong to one team member, who alone
      can read it and send from it. Or the account can share it, and then
      `email:read` and `email:send` decide who can use it.


      Use the Email addresses API to create, list, reassign and delete
      mailboxes. You can read the password that mail apps sign in with. You can
      also download a configuration profile that adds the mailbox to Mail on
      Apple devices. Deleting an address keeps its mail, and creating it again
      in the same account brings the mailbox back.
    name: Email addresses
    x-whop-summary: Mailboxes on an account's domains, for sending and receiving email.
  - description: >
      An Automation is a chain of steps that members move through one at a time.
      A member enrolls when the automation's `trigger_event` happens to them,
      such as `payment.succeeded`. You can also enroll a member with `POST
      /automations/{id}/trigger`. A `wait` step holds them for a set time. An
      `email` or `message` step runs its `source_code`, a TSX module that reads
      the member's data through the API. The module decides whether to send and
      what to say.


      Steps are versioned. You edit the draft version. `POST
      /automations/{id}/publish` makes it the version new members start on, then
      copies it into the next draft. Each publish moves members already in the
      automation onto the new version. Use the Automations API to create,
      publish and trigger automations, and to update them, including their
      status. Use the Automation steps and Automation enrollments APIs for their
      steps and members.
    name: Automations
    x-whop-summary: >-
      Workflows that a webhook event or a trigger request starts for each
      member, running timed steps that message them.
  - description: >
      An Automation step is one step in a version of an automation. A `wait`
      step holds a member for `config.delay_seconds`. An `email` or `message`
      step runs its `source_code` to decide whether to send and what to say.
      Steps form a chain, and each step names the step before it. Only the draft
      version can change.


      Use the Automation steps API to list a version's steps, and to add, update
      and remove steps in the draft. You can also preview what an `email` or
      `message` step would send a member. A preview sends nothing.


      A step's code reads the account through the API. It holds every
      account-level `:read` permission that both the admin role and Whop AI
      hold, except `payout:account:read`. So it can't read cards. Setting a
      step's `source_code`, or previewing a step, needs every one of those
      permissions. So a credential can't write code that reads more than it can.
    name: Automation Steps
    x-whop-summary: The wait, email and message steps of an automation's versions.
  - description: >
      An Automation enrollment is one member's pass through an automation. It
      records the version the member is on, the step they reach next and when,
      and whether they are still going, finished, or canceled.


      Use the Automation enrollments API to list who is in an automation, or
      which automations a member is in. You can also retrieve one enrollment,
      and cancel one to stop that member's remaining steps.
    name: Automation Enrollments
    x-whop-summary: >-
      The members going through an automation, and the step each one is waiting
      on.
  - description: >
      Get started at [whop.com/network](https://whop.com/network). A Partner is
      a user who refers people and businesses to Whop. The partner profile
      includes enrollment, active direct business referral counts, and default
      payout terms.


      Retrieve your profile with `/partners/me`. Use
      `/partner_referral_requests` to create and manage referral links and their
      rewards. You can also enroll in the partner program, review referred users
      and businesses, track earnings, and see the partner leaderboard.
    name: Partners
    x-whop-summary: >-
      Your partner profile, referral links, payout rates, and referred
      businesses.
  - description: >
      A Partner Referral Request is a partner's referral link, or a request to
      be credited as the referrer of an existing business. An attribution
      request takes effect only when an owner of the business accepts it.


      Use the Partner Referral Requests API to create referral links and send
      attribution requests. You can also list the links and requests you've sent
      or received, and accept, decline, or cancel a pending request.
    name: Partner Referral Requests
    x-whop-summary: Request business or partner attribution and manage approval.
  - description: >
      A Bounty is a paid task posted by an account or user. The reward is held
      in escrow when the bounty publishes, workers submit proof of completed
      work, and each accepted submission is paid out until every winner slot
      fills.


      Use the Bounties API to create and publish a bounty, list an account's
      bounties for reporting or dashboards, list the bounties a user can work or
      has participated in, and retrieve a single bounty by ID.
    name: Bounties
    x-whop-summary: Paid tasks with reviewed submissions and escrowed rewards.
  - description: >
      A Bounty Submission is one worker's attempt on a bounty. It starts as an
      in-progress attempt, enters the review queue when proof is submitted, and
      ends approved (paid from the bounty's escrowed pool) or denied.


      Use the Bounty Submissions API to submit proof of completed work to a
      bounty, list the submissions you authored, and review the submissions on
      your bounties — across every bounty or narrowed to one.
    name: Bounty Submissions
    x-whop-summary: Work submitted to a bounty, from attempt to payout.
  - description: >
      A File is an uploaded document or media object, identified by a `file_`
      ID. Creating a file returns a presigned destination; upload the bytes
      there and the file becomes `ready`.


      Use the Files API to create a file, upload its content directly to storage
      (in one PUT, or in parts for large files), and retrieve it while polling
      for readiness. A ready file's ID can be attached wherever Whop accepts
      files.
    name: Files
    x-whop-summary: Upload files and attach them wherever Whop accepts documents.
  - description: >
      A Media Asset is an AI-generated image or video created from a prompt and
      billed from an account balance. When generation finishes, the asset
      includes a file that can be attached anywhere Whop accepts files.


      Use the Media API to start a generation job and retrieve the asset while
      it processes or after it is ready.
    name: Media
    x-whop-summary: >-
      AI-generated assets, billed from a balance, attachable wherever files are
      accepted.
  - description: >
      An API Key is a programmatic credential owned by an account or app. Each
      key carries its own permissions policy — explicit permission statements or
      an inherited system role — and can be restricted with an expiration date
      and an IP allowlist.


      Use the API Keys API to list an account or app's keys, create a key (the
      full secret is returned once, on creation), inspect a key's effective
      grants, update its name or restrictions, rotate its secret, and revoke it.
      These endpoints require a user session — they cannot be called with an API
      key.
    name: API Keys
    x-whop-summary: Programmatic credentials for an account or app.
  - description: >
      A Webhook is an endpoint on your server that Whop sends signed HTTP POST
      requests to when events happen, such as a payment succeeding or a
      membership going active. A webhook is attached to an account or an app and
      subscribes to the events you choose.


      Use the Webhooks API to create and update endpoints, send a test event,
      inspect the delivery log, and replay deliveries your server missed. See
      [Webhooks & Events](/developer/guides/webhooks) for verifying signatures
      and handling retries.
    name: Webhooks
    x-whop-summary: Event notifications pushed to your server as things happen.
  - description: >
      A Permission is one action, such as `stats:read`, paired with whether your
      credential is granted it on a given resource. It answers for whatever you
      authenticated with, so you can decide what to show or attempt instead of
      discovering a `403`.


      Use the Permissions API to check an account, product, experience, or app,
      narrowing to the actions you care about. It reports only your own access —
      to manage who else can reach an account, use the Team Members API.
    name: Permissions
    x-whop-summary: What your credential is allowed to do on a resource.
  - description: >
      An Api Log is a record of a single request made to Whop's API using one of
      your account's API keys — the programmatic counterpart to the dashboard
      audit log, which only records actions taken by signed-in team members.
      Reads and failed requests are logged too.


      Use the Api Logs API to see what your integrations are doing on Whop: the
      operation, HTTP method and status, outcome, and timing of each request,
      newest first.
    name: Api Logs
    x-whop-summary: Requests made to Whop's API with your account's API keys.
  - description: >
      A feedback submission sends an issue, confusing behavior, documentation
      gap, or unanswered question to Whop. It is recorded under the
      authenticated user, account, or app, with an optional account for context,
      and reviewed by Whop.


      User, account, and app credentials can all submit. The response is a
      receipt with an `fbk_` ID: Whop does not reply to submissions, and they
      cannot be listed or retrieved through the API.
    name: Feedback
    x-whop-summary: Issues and unanswered questions sent to Whop for review.
paths:
  /deposits:
    parameters:
      - $ref: '#/components/parameters/ApiVersionDate'
    post:
      tags:
        - Deposits
      summary: Create Deposit
      description: >-
        Returns the deposit methods for an account or user, including crypto
        addresses, bank transfer instructions, and, for a business, a hosted
        deposit page. Bitcoin and Tron deposits are converted to USDT on Plasma
        in the destination's wallet. Business destinations require no
        authentication. Pass `payment_method_id` to also charge a business's
        saved payment method for `amount` in USD and add it to the business's
        balance. That requires authentication with the `payment:charge` scope.
        The charge has no fees or taxes and does not count as revenue.
      operationId: createDeposit
      parameters:
        - $ref: '#/components/parameters/IdempotencyKey'
      requestBody:
        content:
          application/json:
            schema:
              properties:
                amount:
                  description: >-
                    Amount to prefill on hosted deposit page. Crypto deposits
                    require a $10 minimum. With `payment_method_id`, the amount
                    in USD to charge, and required.
                  example: 50
                  type: number
                currency:
                  default: usd
                  description: >-
                    Bank account currency to set up for the destination. A USD
                    account is always set up. Pass `cad` once to also set up a
                    CAD account. After that, every request returns the details
                    of all set-up currencies.
                  enum:
                    - usd
                    - cad
                  example: usd
                  type: string
                destination:
                  description: >-
                    Account ID to fund, `biz_` or `user_`. Any business resolves
                    without authentication; a user account resolves only for
                    that same authenticated user.
                  example: biz_xxxxxxxxxxxxxx
                  type: string
                metadata:
                  additionalProperties:
                    type:
                      - string
                      - number
                      - boolean
                      - 'null'
                  description: >-
                    Custom metadata to attach to the payment the charge creates.
                    Ignored unless `payment_method_id` is provided.
                  example:
                    attempt_id: att_8f2k1
                  type:
                    - object
                    - 'null'
                origin_bic:
                  description: >-
                    SWIFT/BIC of the bank the funds are sent from, 8 or 11
                    characters. Fills each bank currency's `swift_bic` with the
                    intermediary bank to route the wire through.
                  example: DEUTDEFFXXX
                  type: string
                payment_method_id:
                  description: >-
                    A saved payment method of the business to charge for
                    `amount`, prefixed `payt_`. Save one with Create setup
                    intent and `owner_id` set to the account. Only for a `biz_`
                    destination, and requires authentication with the
                    `payment:charge` scope. A declined charge fails the request
                    with 400.
                  example: payt_xxxxxxxxxxxxxx
                  type: string
              required:
                - destination
              type: object
        required: true
      responses:
        '201':
          content:
            application/json:
              schema:
                properties:
                  account_id:
                    description: Account ID of the destination owner.
                    example: biz_xxxxxxxxxxxxxx
                    type:
                      - string
                      - 'null'
                  amount:
                    description: Requested deposit amount.
                    example: '50.00'
                    type: string
                  hosted_url:
                    description: >-
                      URL of the hosted deposit page. Only present for business
                      destinations.
                    example: https://whop.com/deposit/biz_xxxxxxxxxxxxxx/
                    type:
                      - string
                      - 'null'
                  methods:
                    description: Available deposit methods for destination.
                    properties:
                      bank:
                        description: >-
                          Bank deposit details. Only present when bank deposits
                          are active for the destination account.
                        properties:
                          currencies:
                            description: >-
                              Bank transfer currencies available for this
                              deposit.
                            items:
                              properties:
                                account_number:
                                  description: >-
                                    Bank account number for deposits in this
                                    currency.
                                  example: '8825310472'
                                  type:
                                    - string
                                    - 'null'
                                currency:
                                  description: >-
                                    Currency supported by these bank
                                    instructions.
                                  example: USD
                                  type: string
                                deposit_bank_address:
                                  description: Receiving bank address.
                                  example: >-
                                    2115 Linwood Avenue, Fort Lee, New Jersey
                                    07024
                                  type:
                                    - string
                                    - 'null'
                                deposit_bank_name:
                                  description: Receiving bank name.
                                  example: Cross River Bank
                                  type:
                                    - string
                                    - 'null'
                                deposit_beneficiary_name:
                                  description: Beneficiary name to use for transfer.
                                  example: Shine Time Auto Detailing
                                  type:
                                    - string
                                    - 'null'
                                deposit_reference:
                                  description: Reference to include with bank transfer.
                                  example: SHINE-1042
                                  type:
                                    - string
                                    - 'null'
                                rails:
                                  description: Active deposit rails for this currency.
                                  items:
                                    enum:
                                      - ach
                                      - wire
                                      - sepa
                                      - fps
                                      - chaps
                                      - swift
                                    example: wire
                                    type: string
                                  type: array
                                routing_number:
                                  description: >-
                                    Bank routing number for deposits in this
                                    currency.
                                  example: '021214891'
                                  type:
                                    - string
                                    - 'null'
                                swift_bic:
                                  description: >-
                                    SWIFT/BIC of the intermediary bank the
                                    sending bank routes an international wire
                                    through. Null unless `origin_bic` is
                                    provided and an intermediary is published
                                    for that bank and currency.
                                  example: CHASUS33XXX
                                  type:
                                    - string
                                    - 'null'
                              required:
                                - currency
                                - account_number
                                - routing_number
                                - deposit_bank_name
                                - deposit_bank_address
                                - deposit_beneficiary_name
                                - deposit_reference
                                - swift_bic
                                - rails
                              type: object
                            type: array
                        required:
                          - currencies
                        type:
                          - object
                          - 'null'
                      card:
                        description: >-
                          The charge to a saved payment method. Null unless
                          `payment_method_id` was passed.
                        properties:
                          metadata:
                            additionalProperties:
                              type: string
                            description: >-
                              The custom metadata attached to the payment the
                              charge created. Null if none was passed.
                            example:
                              attempt_id: att_8f2k1
                            type:
                              - object
                              - 'null'
                          payment_id:
                            description: >-
                              The payment the charge created, prefixed `pay_`.
                              Retrieve it, or listen for `payment.succeeded`, to
                              confirm the funds landed.
                            example: pay_xxxxxxxxxxxxxx
                            type: string
                          payment_method_id:
                            description: >-
                              The saved payment method that was charged,
                              prefixed `payt_`.
                            example: payt_xxxxxxxxxxxxxx
                            type: string
                        required:
                          - payment_method_id
                          - payment_id
                          - metadata
                        type:
                          - object
                          - 'null'
                      crypto:
                        description: >-
                          Crypto networks available for this deposit, each with
                          its on-chain deposit address and the tokens accepted
                          on that network. Crypto deposits require a $10
                          minimum.
                        items:
                          properties:
                            deposit_address:
                              description: >-
                                Address to send funds to on this network. Null
                                when the provider has not issued one yet.
                              example: soladdr
                              type:
                                - string
                                - 'null'
                            icon_url:
                              description: Network icon URL.
                              example: >-
                                https://whop.com/_static/images/crypto/solana.svg
                              type:
                                - string
                                - 'null'
                            name:
                              description: Network display name.
                              enum:
                                - Ethereum
                                - Solana
                                - Bitcoin
                                - Tron
                                - Base
                                - BNB Smart Chain
                                - Hyperliquid
                                - Hypercore
                                - MegaETH
                                - Polygon
                                - Plasma
                                - Arbitrum
                              example: Solana
                              type: string
                            supported_currencies:
                              description: Tokens accepted for deposit on this network.
                              items:
                                properties:
                                  icon_url:
                                    description: >-
                                      Token icon URL. Null when no icon is
                                      available.
                                    example: >-
                                      https://whop.com/_static/images/crypto/pyusd.svg
                                    type:
                                      - string
                                      - 'null'
                                  name:
                                    description: Token symbol.
                                    enum:
                                      - ARB
                                      - BNB
                                      - BTC
                                      - ETH
                                      - EURC
                                      - HYPE
                                      - PYUSD
                                      - SOL
                                      - TRX
                                      - USD1
                                      - USDC
                                      - USDC.e
                                      - USDG
                                      - USDT
                                      - USDT0
                                      - USDe
                                      - USDm
                                      - XO
                                      - XPL
                                      - pUSD
                                      - wETH
                                    example: PYUSD
                                    type: string
                                required:
                                  - name
                                  - icon_url
                                type: object
                              type: array
                          required:
                            - name
                            - deposit_address
                            - icon_url
                            - supported_currencies
                          type: object
                        type: array
                    required:
                      - bank
                      - crypto
                      - card
                    type: object
                  object:
                    enum:
                      - deposit
                    example: deposit
                    type: string
                required:
                  - object
                  - account_id
                  - hosted_url
                  - methods
                type: object
          description: deposit created
        '400':
          $ref: '#/components/responses/InvalidParameters'
          description: request is invalid
        '409':
          $ref: '#/components/responses/Conflict'
      security:
        - {}
        - bearerAuth:
            - payout:account:read
        - bearerAuth:
            - payment:charge
      x-codeSamples:
        - lang: TypeScript
          source: >
            import { WhopClient } from "@whop/sdk";


            const client = new WhopClient({ token: "YOUR_TOKEN", apiVersionDate:
            "2026-08-21-1", idempotencyKey: "YOUR_IDEMPOTENCY_KEY" });

            await client.deposits.create({
                destination: "destination"
            });
components:
  parameters:
    ApiVersionDate:
      description: Pins the request to a dated API version.
      in: header
      name: Api-Version-Date
      required: false
      schema:
        example: '2026-11-25'
        type: string
    IdempotencyKey:
      description: >-
        A unique key that makes this request safe to retry. See [Idempotent
        requests](https://docs.whop.com/developer/api/idempotency).
      in: header
      name: Idempotency-Key
      required: false
      schema:
        example: d9105228-4a08-46b1-8b91-42fed586d383
        maxLength: 255
        type: string
  responses:
    InvalidParameters:
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/V1ErrorResponse'
      description: Invalid Parameters
    Conflict:
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/V1ErrorResponse'
      description: Conflict
  schemas:
    V1ErrorResponse:
      properties:
        error:
          properties:
            code:
              description: >-
                Machine-readable reason for this specific refusal, such as
                `bank_warning_not_acknowledged`. Only present when the error
                carries one.
              type: string
            message:
              description: Human-readable error message.
              example: account_id is required
              type: string
            type:
              description: Machine-readable error code.
              example: bad_request
              type: string
          required:
            - type
            - message
          type: object
      required:
        - error
      type: object
  securitySchemes:
    bearerAuth:
      bearerFormat: auth-scheme
      description: >-
        An Account API key, an App API key, an account access token, an
        account-scoped user token, or a user OAuth token. Prepend the key or
        token with `Bearer`, for example `Bearer ***************************`.
        See [Auth & API keys](/developer/guides/auth-scoping) for how to get
        each one.
      scheme: bearer
      type: http

````

## Related topics

- [Deposits API](https://docs.whop.com/api-reference/deposits/deposit.md)
- [API Versions](https://docs.whop.com/developer/api/versioning.md)
- [DepositElement](https://docs.whop.com/elements/latest/wallet/deposit.md)
