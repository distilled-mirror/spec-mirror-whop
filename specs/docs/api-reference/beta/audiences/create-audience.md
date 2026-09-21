> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Create

> Create an audience from a customer list, your account's Whop People data, or engagement with videos, lead forms, Instagram profiles, or Facebook pages. Create lookalike audiences to reach people similar to an existing audience. Processing runs asynchronously. Custom creation returns one audience; lookalike creation returns the requested similarity bands in `data`.

Upload the customer CSV with [`POST /files`](/api-reference/files/create-file) on the Legacy API, then pass the returned `file_...` ID as `file_id`.

`column_mapping` tells Whop which CSV header contains each identity field. Headers can be custom, but Whop skips rows that lack both email and phone. After creating the audience, poll [List Audiences](/api-reference/beta/audiences/list-audiences) until `status` is `ready`, `partial`, or `failed`.

Map `ltv` to a column of per-customer lifetime values to build a value-based audience. Lookalikes created from it favor people similar to your highest-value customers.

<RequestExample>
  ```csv CSV file theme={null}
  Email,Phone,First Name,Last Name,Country,LTV
  jenny.nuo@example.com,+14155550123,Jenny,Nuo,US,249.50
  ```

  ```json Request body theme={null}
  {
  	"account_id": "biz_xxxxxxxxxxxxxx",
  	"name": "Past purchasers",
  	"column_mapping": {
  		"email": "Email",
  		"phone": "Phone",
  		"first_name": "First Name",
  		"last_name": "Last Name",
  		"country": "Country",
  		"ltv": "LTV"
  	},
  	"file_id": "file_xxxxxxxxxxxxx"
  }
  ```
</RequestExample>


## OpenAPI

````yaml openapi/api-v1-native.json POST /audiences
openapi: 3.1.0
info:
  description: >-
    The Whop REST API. Please see
    https://docs.whop.com/developer/api/getting-started for more details.
  termsOfService: https://whop.com/tos-developer-api/
  title: Whop API
  version: 1.0.0
  x-api-version-date: '2026-09-15'
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
      credentials, retrieve or update an account, suspend a connected account
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
      A Member is one buyer's relationship with an account — one record per
      customer regardless of how many memberships they hold. It carries
      relationship-level state: whether they have joined or left, their access
      level (`customer`, `admin`, or `no_access`), when they joined, and when
      they last opened the account's content.


      Use the Members API to list an account's members with filtering by access
      level, status, join date, and name or username search, and to retrieve a
      single member. Member rows are created and maintained by the membership
      lifecycle; to grant or revoke access, work with memberships instead.
    name: Members
    x-whop-summary: One buyer's relationship with an account, across all their purchases.
  - description: >
      Economic Intelligence is Whop's recommendation engine for an account. Each
      recommendation is a single action: a title the owner sees, a step-by-step
      brief Whop AI carries out, and the bet it makes on the account's ledger.
      Whop generates them from the account's sales, site, ads, and what its
      owner has said.


      Use the Economic Intelligence API to list recommendations and to request
      actions for a specific goal with POST. For callers with company:update
      permission, listing automatically queues generation when no actions are
      ready or in progress, with a ten-minute cooldown after an unsuccessful
      request from the current pipeline version. A new request returns a
      recommendation with status `queued`; the engine moves it through `pending`
      to `ready`. Unsuccessful requests are omitted from the list. A `ready`
      recommendation becomes `executed` once the owner runs it from the
      dashboard, or `superseded` when a newer one replaces it.
    name: Economic Intelligence
    x-whop-summary: What an account should do next to grow, generated from its own data.
  - name: Webhooks
    x-whop-summary: Event notifications pushed to your server as things happen.
  - description: >
      Stats represent aggregated activity for an account over time. They help
      you understand revenue, transactions, disputes, members, referrals, and
      advertising performance across reporting periods like days, weeks, or
      months.


      Use the Stats API to list available metrics and their filterable
      properties, then retrieve time-series values for a date range.
    name: Stats
    x-whop-summary: Aggregated financial, audience, and traffic reporting.
  - description: >
      A Verification represents a legal identity for a person or business.
      Accounts and users complete verification when Whop needs to confirm who
      they are before enabling payouts or compliance-sensitive workflows.


      Use the Verifications API to start or resume a hosted verification
      session, check review status, and submit requested details or documents.
      If `requested_information` contains items, submit answers with [Update
      Verification](/api-reference/beta/verifications/update-verification).
    name: Verifications
    x-whop-summary: Legal identity required before payouts and card issuing.
  - description: >
      An Export is an asynchronous CSV of one resource for one account —
      members, payments, disputes, ads, and the other tables the Whop dashboard
      can export. Generating a full table takes longer than a request, so an
      export is created in `pending`, moves through `processing`, and lands on
      `completed` with a download link. Each resource requires that resource's
      own export scope.


      Use the Exports API to start an export, poll it until `download_url` is
      set, and list the exports already requested for an account. Finished CSVs
      are retained for 30 days, after which the file is deleted and the export
      moves to `expired`.
    name: Exports
    x-whop-summary: Asynchronous CSV dumps of an account's dashboard data.
  - description: >
      A Notification is a message delivered to a user — a new post, a payment, a
      mention. Every notification comes from an experience the user belongs to
      or a team they are on, and users control what they receive with
      notification preferences.


      Every notification belongs to a topic: the category it falls under, such
      as new sales or account activity. Topics carry a default, so a user only
      needs a preference row where they diverge from it. `GET
      /notifications/topics` lists the platform's visible topics, and a topic's
      `id` is what the notification preference endpoints take as `topic_id` —
      the catalog is the only place those ids come from, so read it rather than
      hardcoding. Each topic also carries an `identifier` such as
      `new-follower`, which is stable across environments and is the value to
      match on in code.


      Use the Notifications API to list the authenticated user's feed, read
      per-experience unread badges, mark an experience (or everything) as read,
      send notifications from your app to an experience's users or an account's
      team, and list the topic catalog.
    name: Notifications
    x-whop-summary: >-
      The user's notification feed: unread badges, mark-read, app sends, and the
      topic catalog.
  - description: >
      A Payment is one charge against a buyer. Create an on-session payment with
      a `confirmation_token` for the method the buyer selected, or an
      off-session payment with an existing member's stored payment method.


      Collection runs in the background, so the create response is not the
      outcome. Poll [Retrieve
      status](/api-reference/beta/payments/retrieve-status) for how far the
      payment has got and, while it is `requires_action`, what the buyer must do
      next — follow a redirect, complete 3D Secure, display transfer
      instructions, or link a bank account. Use the return_url operation to
      change where they land afterwards, up until they come back.
    name: Payments
    x-whop-summary: A charge against a buyer, and the step they still owe.
  - description: >
      A Refund is one reversal of a payment, full or partial. Refunds are issued
      with `POST /payments/{id}/refund`; this resource is the record of each one
      — how much moved, through which provider, and where it stands (`pending`,
      `succeeded`, `failed`).


      List a payment's refunds with `?payment_id=`, or every refund an account
      issued with `?account_id=`. `amount` is stated in the payment's settlement
      currency so it nets against the payment's `total`; `original_amount` is
      what the processor moved.
    name: Refunds
    x-whop-summary: Money returned to a buyer from a payment.
  - description: >
      A Confirmation Token is a single-use, short-lived reference to a payment
      method and billing details collected from a buyer. Its response contains
      only a display-safe preview and never returns the underlying payment
      credential.


      Whop Elements mint the token in your buyer-facing collection flow and hand
      you its `ctok_` ID to send to the Payments API from your server. Retrieve
      a token to display its payment method and billing preview or check whether
      it is still usable.
    name: Confirmation Tokens
    x-whop-summary: A short-lived reference to payment details collected from a buyer.
  - description: >
      A Setup Intent saves a buyer's payment method for later without taking
      money now. It runs the same collection flow a payment does, so the buyer
      may still owe a step — 3D Secure on a card, a hosted enrollment, or
      linking a bank account.


      Poll [Retrieve status](/api-reference/beta/setup-intents/retrieve-status)
      for how far the setup has got and what is outstanding. Once it reaches
      `succeeded` the method is on file and can be charged.
    name: Setup Intents
    x-whop-summary: Saving a buyer's payment method without charging it.
  - description: >
      A Payment Rule lets an account act on its own payments before they reach
      the bank: block them, let them through, send them to review, or ask the
      buyer for 3D Secure. Each rule matches on a small set of payment
      attributes, and every condition must hold for it to apply.


      A rule's definition is fixed once created, so the payments it decided keep
      naming the rule that decided them. Use
      [Replace](/api-reference/beta/payment-rules/replace-a-payment-rule) to
      change one, and [List
      fields](/api-reference/beta/payment-rules/list-fields) for the attributes,
      operators and values a condition can use.
    name: Payment Rules
    x-whop-summary: Rules an account writes to decide its own payments.
  - description: >
      A Dispute is a chargeback a customer files against a payment through their
      bank, or an inquiry that may become one. It carries the disputed payment,
      a deadline to respond, your evidence, and the outcome once the processor
      rules.


      Use the Disputes API to list disputes, edit the evidence packet while a
      dispute is still contestable, and submit it for review.
    name: Disputes
    x-whop-summary: Chargebacks filed against an account, with evidence and outcomes.
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
      A Resolution Center Case is opened by a buyer when something is wrong with
      a purchase — an unwanted renewal, an item that never arrived, or a charge
      they don't recognize. It is the step before a chargeback: the two sides
      work it out directly, and Whop decides the case if they can't. Each case
      carries a reason, a status naming which side it is waiting on, a timeline
      of events, and the actions available to whoever is reading it.


      Use the Resolution Center Cases API from either side: as the buyer, open a
      case, reply, appeal a decision, or withdraw it; as the merchant, accept it
      (refunding the payment), deny it, or ask the buyer for more information.
      Both sides read the same case, page its timeline, and summarize the cases
      they can see.
    name: Resolution Center Cases
    x-whop-summary: File or respond to a case against a payment, as the buyer or the merchant.
  - description: >
      A Ledger Activity row is a single financial event on an account's ledger —
      a payment, payout, refund, transfer, on-chain deposit, swap, or card
      transaction. Each row is derived from the underlying ledger lines and
      carries a typed `resource` and `source` so you can present and link the
      event without extra lookups.


      Use Ledger Activity to build a statement or transaction feed for an
      account or user. Reconcile against your own records with `amount` (signed,
      in the currency's smallest precision units) and `posted_at`, and use
      `available_at` to know when inflows became withdrawable.
    name: Ledgers
    x-whop-docs-title: Financial Activity
    x-whop-summary: The activity feed behind an account or user's balance.
  - description: >
      Payouts represent money sent from an account or user balance to an
      external destination, such as a bank account, wallet, or other saved
      payout method.


      Use the Payouts API to create and track payouts, manage saved payout
      methods, and show expected arrival details for funds leaving Whop.
    name: Payouts
    x-whop-summary: Send money from a balance to a bank or wallet.
  - description: >
      Cards represent Whop-issued virtual payment cards that spend from an
      account or user balance. Cards can be assigned to cardholders and
      configured with spending limits for controlled spending.


      Use the Cards API to issue cards, list cards for an account or user, and
      retrieve active card details such as the card number and CVC.
    name: Cards
    x-whop-summary: Issue cards that spend from a balance.
  - description: >
      Cashback rules designate a funding platform, a merchant name and category,
      a rate, and an eligibility window. An optional account ID limits the rule
      to one of the platform's direct connected accounts.


      Use the Cashback Rules API to create future-dated rules, update their
      merchant name, MCC, description, or expiration, and list every rule funded
      by the authenticated platform, including expired and discarded rules.
      Discarded rules cannot be updated. Creating or updating a rule does not
      transfer funds.
    name: Cashback Rules
    x-whop-summary: Configure platform-sponsored card cashback.
  - description: >
      Transfers move value between identities on Whop. They are used for
      account-to-account money movement, user payouts inside Whop, crypto
      transfers, and claim links depending on the destination type.


      Use the Transfers API to create a transfer, list previous transfers, and
      retrieve a transfer by ID when reconciling money movement between accounts
      or users.
    name: Transfers
    x-whop-summary: Move funds between Whop accounts and users.
  - description: >
      Deposits describe ways to add funds to an account balance, including
      hosted deposit pages, bank deposit instructions, and supported crypto
      wallet addresses.


      Use the Deposits API to create deposit instructions for an account. Crypto
      deposits require a $10 minimum.
    name: Deposits
    x-whop-summary: Add funds to a balance.
  - description: >
      Swaps convert value between supported tokens, chains, or wallet
      destinations for an account. A swap quote describes the expected output,
      fees, and approval requirements before you create the swap.


      Use the Swaps API to quote a conversion, create the swap, list recent
      swaps, and retrieve status until the transaction completes.
    name: Swaps
    x-whop-summary: Convert a balance between currencies.
  - description: >
      A Product is a digital good or service sold on Whop. Products may contain
      plans for pricing and/or experiences for content delivery.


      Use the Products API to search the public marketplace, list an account's
      products, retrieve a product, and create, update, or delete products.
    name: Products
    x-whop-summary: The things you sell. Each owns plans and a store page.
  - description: >
      A Plan defines how customers buy a product. It controls pricing, billing
      cadence, availability, tax behavior, checkout fields, and purchase
      visibility.


      Use the Plans API to create plans for products, list existing plans,
      retrieve or update plan configuration, calculate tax for checkout, and
      delete plans that should no longer be offered.
    name: Plans
    x-whop-summary: 'Pricing for a product: one-time, recurring, trials, stock.'
  - name: Promo Codes
    x-whop-summary: Discounts that creators configure for checkout.
  - description: >
      A Membership is a customer's purchase of a plan: the subscription or
      one-time grant that gives them access to a product. It tracks billing
      state (`active`, `trialing`, `past_due`, and so on), the current period,
      pending cancellations, custom metadata, and the software license key when
      the product includes licensing.


      Use the Memberships API to list an account's memberships or the caller's
      own, retrieve one by ID or license key, invite a recipient to join through
      a free plan, and manage the lifecycle: cancel immediately or at period
      end, reverse a scheduled period-end cancellation, pause and resume payment
      collection, extend with free days, generate a transfer link, and update
      metadata.
    name: Memberships
    x-whop-summary: A customer's purchase of a plan, from checkout through cancellation.
  - description: >
      A Checkout Configuration is a reusable checkout link owned by an account.
      In `payment` mode it sells a specific plan; in `setup` mode it collects
      and saves payment details without charging. Each configuration can also
      override which payment methods are accepted and how 3D Secure is enforced
      for that checkout.


      Use the Checkout Configurations API to create checkout links for an
      existing or inline plan, list configurations for an account, retrieve the
      configuration behind a checkout URL, and delete links that should no
      longer be used.
    name: Checkout Configurations
    x-whop-summary: Turn a plan into a shareable, prefilled checkout link.
  - description: >
      A Payment Method Domain registers a hostname with a wallet provider so its
      payment methods can appear at a checkout served from that domain. The
      domain proves ownership by hosting the provider's association file — for
      Apple Pay, at `/.well-known/apple-developer-merchantid-domain-association`
      — and `status` reports whether verification has completed.


      Use the Payment Method Domains API to register domains for your account or
      its connected accounts, retry verification once the association file is
      hosted, and remove domains that should no longer serve wallet payments. A
      domain a platform shares with its connected accounts at checkout is listed
      on the platform's account, not on each connected account.


      Wallet buttons at checkout depend on this: embedded surfaces like the
      [Express Checkout element](/elements/beta/checkout/expressCheckout) only
      render Apple Pay on a `verified` domain (first-party whop.com pages are
      pre-approved). To verify a domain, [create
      it](/api-reference/beta/payment-method-domains/create-payment-method-domain),
      host the association file at the path above, then [retry
      verification](/api-reference/beta/payment-method-domains/verify-payment-method-domain)
      until `status` is `verified`.
    name: Payment Method Domains
    x-whop-summary: >-
      Domains verified to show wallet payment methods like Apple Pay at
      checkout.
  - description: >
      A Shipment attaches a carrier tracking number to a payment and follows the
      package from label creation to delivery, exposing the current delivery
      status and a customer-facing tracking URL.


      Use the Shipments API to list an account's shipments, retrieve one by its
      id or the payment it fulfills, attach a tracking number to a payment, and
      update the tracking number on an existing shipment.
    name: Shipments
    x-whop-summary: Track the delivery of an order by its carrier tracking number.
  - description: >
      A Partner Referral Request records a partner's request for a business to
      attribute them as its referring partner. Manual requests start pending and
      require a business owner's acceptance before attribution takes effect.


      Enrolled, verified Whop partners can create, view, and cancel their
      requests. Business owners can accept or decline incoming requests. List
      requests by business, partner, request type, or status.


      Authenticate with your Whop login or an account API key created by that
      account's current owner. Account API keys act as their account owner when
      creating or cancelling requests; that owner must be enrolled, verified,
      and not suspended. Keys can view their owner's sent requests and incoming
      requests for the key's account, and can accept or decline requests only
      for that account. API keys require the corresponding
      `partner:referral_request:read`, `partner:referral_request:create`,
      `partner:referral_request:accept`, `partner:referral_request:decline`, or
      `partner:referral_request:cancel` permission.
    name: Partner Referral Requests
    x-whop-summary: Request business attribution and manage owner approval.
  - description: >
      Get started at [whop.com/network](https://whop.com/network). A Partner is
      a user who refers people and businesses to Whop. The partner profile
      includes enrollment, active direct business referral counts, and default
      payout terms.


      Retrieve your profile with `/partners/{id}`. Use `/partners/links` for
      your standard referral URL and paginated promotion links, including reward
      amounts, requirements, redemption counts, and availability. You can also
      enroll in the partner program, review referred users and businesses, track
      earnings, and see the partner leaderboard.
    name: Partners
    x-whop-summary: >-
      Your partner profile, referral links, payout rates, and referred
      businesses.
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
      A Person is an identity-linked profile of a visitor or customer of an
      account, assembled from every [event](/api-reference/beta/events/event)
      the person generated — pixel page views, ad clicks, leads, identifies, and
      payments. Each profile carries the person's known identities (names,
      emails, phones, user IDs), purchase history and LTV, geo/device profile,
      traffic sources, and the first and last marketing touches that reached
      them.


      Use the People API to list and segment the people of an account — filter
      by activity, purchases, traffic source, location, or marketing touch, and
      sort by value — or retrieve one person by person ID, user ID, email
      address, or phone number.
    name: People
    x-whop-summary: >-
      Visitors and customers of an account, with identity, purchase, and traffic
      profiles.
  - description: >
      An Event records conversion or engagement activity for an account, such as
      page views, purchases, or leads. Each event ties the action to the
      [person](/api-reference/beta/people/person) who took it, so activity can
      be attributed to the ads and links that drove it.


      Use the Events API to send new tracking events, list recent
      identity-linked events for an account, and inspect the events recorded for
      a person. The resource also exposes an anonymized read mode — the pulse
      feed — a platform-wide snapshot of recent purchases that carries nothing
      identifying. The pulse feed is public; other Events endpoints require
      authentication and are scoped to an account.


      Events are only as good as the pixel sending them, so [Validate
      Pixel](/api-reference/beta/events/validate-pixel) answers whether an
      account's pixel is working: it reads the events the pixel has sent, and
      when you pass a `url` whose page hasn't sent any lately, it fetches that
      page and looks for the pixel in its source. Use it before launching an ad
      to confirm its destination is tracked, or in a setup flow to tell a
      merchant whether their install is live.
    name: Events
    x-whop-summary: Conversion and engagement events tracked for attribution.
  - description: >
      An Ad is the individual creative unit delivered by an [ad
      group](/api-reference/beta/ad-groups/ad-group). It holds the copy,
      creative assets, and destination URL for one ad.


      Use the Ads API to list ads for an account, create ads inside ad groups,
      retrieve or update creative details, delete ads that should stop running,
      and pause or resume delivery.
    name: Ads
    x-whop-summary: 'The creative: copy, assets, and destination URL.'
  - description: >
      An Ad Campaign is the top-level container for paid ads on an ad network.
      It sets the platform, objective, and budget strategy shared by its [ad
      groups](/api-reference/beta/ad-groups/ad-group) and ads.


      Use the Ad Campaigns API to create campaigns, list campaigns for an
      account, retrieve or update campaign settings, and pause or resume
      campaign delivery.
    name: Ad Campaigns
    x-whop-summary: Platform, objective, and budget for a set of ads.
  - description: >
      An Ad Group sits inside an [ad
      campaign](/api-reference/beta/ad-campaigns/ad-campaign) and controls
      delivery for [ads](/api-reference/beta/ads/ad). It sets the audience,
      placements, schedule, budget, and optimization goal for its ads.


      Use the Ad Groups API to create ad groups in campaigns, list or retrieve
      targeting and delivery settings, update budgets or targeting, delete
      groups that should stop running, and pause or resume delivery. It can also
      search the ad platform's targeting taxonomy for options to target and
      estimate how many people a draft targeting spec can reach.
    name: Ad Groups
    x-whop-summary: Audience, placements, and schedule within a campaign.
  - description: >
      An Audience is a reusable group of people to include or exclude when
      targeting ads. Build custom audiences from customer lists, Whop People
      data, or social engagement, and create lookalikes to reach people similar
      to an existing audience.


      Use the Audiences API to create, list, and delete audiences and monitor
      asynchronous processing. Meta engagement sources include videos, lead
      forms, Instagram profiles, and Facebook pages. Engagement membership
      updates on Meta; Whop People audiences can refresh automatically or keep a
      snapshot.
    name: Audiences
    x-whop-summary: Reusable targeting lists for ad groups.
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
      A Social Account represents an external profile connected to a Whop
      account or user, such as a Facebook page or Instagram account. Connecting
      a social account lets Whop run [ads](/api-reference/beta/ads/ad) under
      that profile's identity and promote its existing posts.


      Use the Social Accounts API to list connected accounts, create a
      Whop-managed Facebook page, start an OAuth connection, disconnect a social
      account, and list a connected profile's posts or a Facebook page's lead
      forms.
    name: Social Accounts
    x-whop-summary: Connected Facebook and Instagram accounts that run ads.
  - description: >
      An App is software you build on Whop. It can be a hosted web app served at
      `<route>.whop.site` or an API integration installed as an experience, and
      it belongs to the account that owns its credentials, settings, builds, and
      runtime logs.


      Use the Apps API to manage app configuration, deploy an app's working copy
      and follow the run on the app's `deployment` field, and, for hosted apps,
      read server runtime logs for console output, uncaught exceptions, and
      failed requests. Logs are retained for 7 days and can be filtered by
      build, level, time window, and message text.


      Apps are also reusable blueprints. List official blueprints with
      `app_type=website&verified=true&order=template_usage`, or community
      blueprints with
      `app_type=website&verified=false&recommended=true&order=template_usage`.
      Pass the returned App `id` as `blueprint_id` when creating an Account.
    name: Apps
    x-whop-summary: 'Apps you build on Whop: metadata, hosted builds, runtime logs.'
  - description: >
      A Domain is an account's claim to a hostname and its app assignment.
      Publish the returned ownership TXT and routing DNS records. Verification
      and certificate provisioning run automatically; unverified claims expire
      after 48 hours. Only verified domains with active hostname and certificate
      status resolve through the Apps API.


      An unverified claim does not reserve a hostname globally. Transferring
      ownership requires a fresh TXT proof and an explicit replacement request.
      Removing a domain stops app resolution immediately while Cloudflare
      cleanup finishes in the background.
    name: Domains
    x-whop-summary: Custom domains assigned to hosted apps.
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
      Experiments belong to an account. Use `account_id` to select the owning
      account, or `internal` for Whop's platform experiments. Reading and
      managing account experiments requires `experiment:read` or
      `experiment:manage`; internal configuration requires Whop internal access.
      Exposure is callable without authentication.


      Create a draft, configure treatment weights and targeting, then activate,
      pause, or end it. Treatments occupy stable percentage ranges; the
      remainder is control. Growing an allocation preserves existing treatment
      assignments. Optional `related_resource` references attach experiments,
      control, and variants to resources owned by the account. Bindings cannot
      change after first activation.


      `GET /experiments/exposures` evaluates and records exposure. Ownership is
      separate from `subject` identity: `subject[user_id]`,
      `subject[account_id]`, and `subject[anonymous_id]` supply the experiment's
      bucketing unit. Internal user identity comes from the authenticated
      session. Resolved authentication is recorded on the event separately from
      the subject. Pass a flag key and its account, or a globally unique
      experiment ID. Without a flag key, evaluation returns active experiments
      in the account and related resource scope.


      Account experiments run without a reporting provider. Statistical results
      and the metric catalog currently remain internal. Configuration responses
      include an assignment seed and revision for consumers that cache
      experiment definitions.
    name: Experiments
    x-whop-summary: >-
      Feature flags and A/B experiments for gradual rollout and statistical
      measurement.
paths:
  /audiences:
    parameters:
      - $ref: '#/components/parameters/ApiVersionDate'
    post:
      tags:
        - Audiences
      summary: Create Audience
      description: >-
        Create an audience from a customer list, your account's Whop People
        data, or engagement with videos, lead forms, Instagram profiles, or
        Facebook pages. Create lookalike audiences to reach people similar to an
        existing audience. Processing runs asynchronously. Custom creation
        returns one audience; lookalike creation returns the requested
        similarity bands in `data`.
      operationId: createAudience
      parameters:
        - $ref: '#/components/parameters/IdempotencyKey'
      requestBody:
        content:
          application/json:
            schema:
              example:
                account_id: biz_xxxxxxxxxxxxxx
                engagement:
                  include:
                    - event: engaged
                      object: facebook_page
                      retention_days: 30
                      social_account_id: sacc_xxxxxxxxxxxxxx
                  platform: meta
                name: Page engagers
                source_type: engagement
              properties:
                account_id:
                  description: Account ID, prefixed `biz_`.
                  example: biz_xxxxxxxxxxxxxx
                  type: string
                audience_type:
                  description: Audience type. Defaults to `custom`.
                  enum:
                    - custom
                    - lookalike
                  example: lookalike
                  type: string
                auto_refresh:
                  description: >-
                    Filter audiences only, and set only at creation. `true` (the
                    default) rebuilds membership from the filters twice a day.
                    `false` keeps whoever matched at creation and never
                    rebuilds.
                  example: true
                  type: boolean
                column_mapping:
                  description: >-
                    CSV audiences only. Maps supported identity fields to CSV
                    column headers. Map at least one of `email` or `phone`.
                  properties:
                    country:
                      description: >-
                        CSV header for ISO 3166-1 alpha-2 country codes, such as
                        `US`.
                      example: Country
                      type: string
                    email:
                      description: CSV header for email addresses.
                      example: Email
                      type: string
                    first_name:
                      description: CSV header for first names.
                      example: First Name
                      type: string
                    last_name:
                      description: CSV header for last names.
                      example: Last Name
                      type: string
                    ltv:
                      description: >-
                        CSV header for each customer's lifetime value — a
                        non-negative number, currency symbols allowed. When
                        mapped, Meta creates the audience as value-based, so
                        lookalikes built from it favor people similar to the
                        highest-value customers.
                      example: Lifetime Value
                      type: string
                    phone:
                      description: CSV header for phone numbers.
                      example: Phone
                      type: string
                  type: object
                count:
                  description: >-
                    Lookalikes only. Number of lookalike audiences to create
                    (1–6).
                  example: 3
                  type: integer
                engagement:
                  additionalProperties: false
                  description: >-
                    Rules for membership based on social engagement. Requires a
                    connected social account with advertising access.
                  properties:
                    exclude:
                      description: >-
                        Exclude anyone matching any exclusion rule. Defaults to
                        an empty array. Video audiences do not support
                        exclusions; use a separate audience in ad-group
                        exclusions.
                      items:
                        $ref: '#/components/schemas/AudienceEngagementRule'
                      maxItems: 10
                      type: array
                    include:
                      description: >-
                        Match any inclusion rule. Video rules must share a
                        retention window and cannot be combined with other
                        sources.
                      items:
                        $ref: '#/components/schemas/AudienceEngagementRule'
                      maxItems: 10
                      minItems: 1
                      type: array
                    platform:
                      description: Ad platform that maintains membership.
                      enum:
                        - meta
                      example: meta
                      type: string
                  required:
                    - platform
                    - include
                  type: object
                file_id:
                  description: >-
                    CSV audiences only. The uploaded customer CSV — a file id
                    (`file_...`) returned by `POST /files`.
                  example: >-
                    eyJfcmFpbHMiOnsiZGF0YSI6MSwicHVyIjoiYmxvYl9pZCJ9fQ==--xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
                  type: string
                filters:
                  description: >-
                    Filter audiences only. The People filters that define
                    membership, keyed exactly as `GET /people` accepts them —
                    for example `{"os": "iOS", "country": "US"}`. Activity dates
                    `event_from` and `event_to` are inclusive and remain fixed
                    on refresh. Use `event_within_days`,
                    `first_seen_within_days` or `last_seen_within_days` for a
                    rolling window. Source values are canonical source paths
                    (`whop:<campaign>:<group>:<ad>`, `ext:<platform>:...`,
                    `referrer:<domain>`, `direct`), exact or with a trailing
                    `:*` wildcard.
                  example:
                    country: US
                    last_seen_within_days: 30
                  type: object
                name:
                  description: >-
                    Audience display name. Required for custom audiences;
                    lookalike names are generated from the source audience.
                  example: Page engagers
                  type: string
                percentage:
                  description: >-
                    Lookalikes only. Total similarity reach as a whole percent
                    (1–20), sliced evenly across `count` — must be divisible by
                    `count`. For example, 3 audiences at 6% creates 0–2%, 2–4%,
                    and 4–6% bands.
                  example: 6
                  type: integer
                source_audience_id:
                  description: >-
                    Lookalikes only. The ready custom audience (`adaud_`) to
                    build from; uploaded and People audiences need at least 100
                    matched people. Meta validates engagement audience
                    eligibility when creating the lookalike.
                  example: adaud_xxxxxxxxxxxxxx
                  type: string
                source_type:
                  description: >-
                    Custom audience source. Inferred from `engagement`, then
                    `filters`, otherwise defaults to `csv_upload`. Supply only
                    the fields for the selected source.
                  enum:
                    - csv_upload
                    - people_filter
                    - engagement
                  example: engagement
                  type: string
              required:
                - account_id
              type: object
        required: true
      responses:
        '201':
          content:
            application/json:
              schema:
                oneOf:
                  - $ref: '#/components/schemas/Audience'
                  - properties:
                      data:
                        items:
                          $ref: '#/components/schemas/Audience'
                        type: array
                    required:
                      - data
                    type: object
          description: >-
            Audience created. Custom creation returns one audience; lookalike
            creation returns an array in `data`.
        '400':
          $ref: '#/components/responses/InvalidParameters'
          description: Conflicting audience sources.
        '401':
          $ref: '#/components/responses/Unauthorized'
          description: Missing or invalid authentication.
        '409':
          $ref: '#/components/responses/Conflict'
      security:
        - bearerAuth:
            - audience:update
      x-codeSamples:
        - lang: TypeScript
          source: >
            import { WhopClient } from "@whop/sdk";


            const client = new WhopClient({ token: "YOUR_TOKEN", apiVersionDate:
            "2026-08-21-1", idempotencyKey: "YOUR_IDEMPOTENCY_KEY" });

            await client.audiences.create({
                account_id: "biz_xxxxxxxxxxxxxx"
            });
components:
  parameters:
    ApiVersionDate:
      description: Pins the request to a dated API version.
      in: header
      name: Api-Version-Date
      required: false
      schema:
        example: '2026-09-15'
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
  schemas:
    AudienceEngagementRule:
      discriminator:
        mapping:
          facebook_page:
            $ref: '#/components/schemas/AudienceEngagementFacebookPageRule'
          instagram_profile:
            $ref: '#/components/schemas/AudienceEngagementInstagramProfileRule'
          lead_form:
            $ref: '#/components/schemas/AudienceEngagementLeadFormRule'
          video:
            $ref: '#/components/schemas/AudienceEngagementVideoRule'
        propertyName: object
      oneOf:
        - $ref: '#/components/schemas/AudienceEngagementVideoRule'
        - $ref: '#/components/schemas/AudienceEngagementLeadFormRule'
        - $ref: '#/components/schemas/AudienceEngagementInstagramProfileRule'
        - $ref: '#/components/schemas/AudienceEngagementFacebookPageRule'
    Audience:
      properties:
        audience_type:
          description: >-
            Whether the audience targets a defined group of people or people
            similar to an existing audience.
          enum:
            - custom
            - lookalike
          example: lookalike
          type: string
        auto_refresh:
          description: >-
            Whether Whop rebuilds membership from saved People filters twice a
            day. When `false`, People audiences keep the members matched at
            creation. Always `false` for uploaded lists, lookalikes, and
            engagement audiences. Engagement membership is maintained by Meta.
          example: false
          type: boolean
        created_at:
          description: When the audience was created, as an ISO 8601 timestamp.
          example: '2026-01-01T12:00:00.000Z'
          type: string
        engagement:
          description: >-
            Social engagement rules maintained by the ad platform. `null` for
            other audience sources.
          oneOf:
            - $ref: '#/components/schemas/AudienceEngagement'
            - type: 'null'
        error_message:
          description: >-
            Processing error message. `null` unless processing is partial or
            failed.
          example: >-
            412 of 1,000 rows had no email or phone number, so the list could
            not be matched.
          type:
            - string
            - 'null'
        filters:
          description: >-
            Saved Whop People filters that define membership, using the same
            keys as `GET /people`. `null` for uploaded lists, engagement
            audiences, and lookalikes.
          example:
            country: US
            last_seen_within_days: 30
          type:
            - object
            - 'null'
        id:
          description: Audience ID, prefixed `adaud_`.
          example: adaud_xxxxxxxxxxxxxx
          type: string
        last_refreshed_at:
          description: >-
            When the audience membership was last rebuilt, as an ISO 8601
            timestamp. `null` until the first build completes.
          example: '2026-01-01T12:00:00.000Z'
          type:
            - string
            - 'null'
        lookalike_ratio:
          description: >-
            For lookalikes: the upper bound of the similarity band as a fraction
            (0.02 = top 2%). `null` for custom audiences.
          example: 0.04
          type:
            - number
            - 'null'
        lookalike_starting_ratio:
          description: >-
            For lookalikes: the lower bound of the similarity band as a
            fraction. `null` for custom audiences and first-tier lookalikes.
          example: 0.02
          type:
            - number
            - 'null'
        match_rates:
          items:
            $ref: '#/components/schemas/AudienceMatchRate'
            description: >-
              Estimated match rates by ad platform. Empty for engagement
              audiences and audiences not sent to a supported platform.
          type: array
        matched_rows:
          description: >-
            Members successfully uploaded to connected ad accounts. Always 0 for
            lookalikes and engagement audiences.
          example: 0
          type: number
        name:
          description: Audience display name.
          example: Past purchasers Lookalike 2–4%
          type: string
        platform_audience_ids:
          items:
            description: >-
              External audience IDs created on connected ad platforms, such as
              Meta.
            example: '120246230799130687'
            type: string
          type: array
        processed_rows:
          description: >-
            Members processed from the source so far. Always 0 for lookalikes
            and engagement audiences.
          example: 0
          type: number
        progress_percent:
          description: Processing progress from 0 to 100.
          example: 0
          type: number
        source_audience_id:
          description: >-
            For lookalikes: the audience this lookalike was built from. `null`
            for custom audiences.
          example: adaud_xxxxxxxxxxxxxx
          type:
            - string
            - 'null'
        source_type:
          description: >-
            Membership source: an uploaded CSV, Whop People filters, or social
            engagement.
          enum:
            - csv_upload
            - people_filter
            - engagement
          example: csv_upload
          type: string
        status:
          description: >-
            Current state of audience creation. For engagement audiences,
            `ready` means the rules were created on Meta; membership may still
            be populating. `syncing` means Whop is sending matched rows to
            connected ad accounts. When status is `partial` or `failed`,
            `error_message` explains what went wrong.
          enum:
            - pending
            - processing
            - syncing
            - ready
            - partial
            - failed
          type: string
        total_rows:
          description: >-
            Total members detected in the source — CSV rows for uploaded lists,
            matching people for automatic audiences. Always 0 for lookalikes and
            engagement audiences.
          example: 0
          type: number
        updated_at:
          description: When the audience was last updated, as an ISO 8601 timestamp.
          example: '2026-01-01T12:00:00.000Z'
          type: string
      required:
        - id
        - name
        - audience_type
        - source_type
        - status
        - total_rows
        - processed_rows
        - matched_rows
        - progress_percent
        - error_message
        - platform_audience_ids
        - source_audience_id
        - lookalike_ratio
        - lookalike_starting_ratio
        - engagement
        - filters
        - auto_refresh
        - last_refreshed_at
        - created_at
        - updated_at
        - match_rates
      type: object
    AudienceEngagementFacebookPageRule:
      properties:
        event:
          description: Interaction that qualifies a person for this rule.
          enum:
            - engaged
            - visited
            - liked
            - messaged
            - cta_clicked
            - saved
            - post_interaction
          example: engaged
          type: string
        object:
          description: Engagement source.
          enum:
            - facebook_page
          example: facebook_page
          type: string
        retention_days:
          description: >-
            Rolling membership window in days, from 1 to 730. Use 0 for `liked`,
            which tracks current likes and cannot be combined with other events.
          example: 30
          type: integer
        social_account_id:
          description: >-
            Connected social account ID, prefixed `sacc_`, with advertising
            access.
          example: sacc_xxxxxxxxxxxxxx
          type: string
      required:
        - object
        - social_account_id
        - event
        - retention_days
      type: object
    AudienceEngagementInstagramProfileRule:
      properties:
        event:
          description: Interaction that qualifies a person for this rule.
          enum:
            - all
            - engaged
            - visited
            - messaged
            - saved
            - ad_liked
            - ad_commented
            - ad_shared
            - ad_saved
            - ad_cta_clicked
            - ad_carousel_swiped
            - organic_liked
            - organic_commented
            - organic_shared
            - organic_saved
            - organic_swiped
            - organic_carousel_swiped
          example: all
          type: string
        object:
          description: Engagement source.
          enum:
            - instagram_profile
          example: instagram_profile
          type: string
        retention_days:
          description: Rolling membership window in days, from 1 to 730.
          example: 30
          type: integer
        social_account_id:
          description: >-
            Connected social account ID, prefixed `sacc_`, with advertising
            access.
          example: sacc_xxxxxxxxxxxxxx
          type: string
      required:
        - object
        - social_account_id
        - event
        - retention_days
      type: object
    AudienceEngagementLeadFormRule:
      properties:
        event:
          description: Interaction that qualifies a person for this rule.
          enum:
            - opened
            - submitted
            - not_submitted
          example: opened
          type: string
        object:
          description: Engagement source.
          enum:
            - lead_form
          example: lead_form
          type: string
        platform_form_ids:
          items:
            description: >-
              Numeric platform lead form IDs belonging to the selected social
              account. Supply 1–50 IDs.
            example: '333'
            type: string
          type: array
        retention_days:
          description: Rolling membership window in days, from 1 to 90.
          example: 30
          type: integer
        social_account_id:
          description: >-
            Connected social account ID, prefixed `sacc_`, with advertising
            access.
          example: sacc_xxxxxxxxxxxxxx
          type: string
      required:
        - object
        - social_account_id
        - event
        - retention_days
        - platform_form_ids
      type: object
    AudienceEngagementVideoRule:
      properties:
        event:
          description: Interaction that qualifies a person for this rule.
          enum:
            - watched_3_seconds
            - watched_10_seconds
            - watched_15_seconds
            - watched_25_percent
            - watched_50_percent
            - watched_75_percent
            - watched_95_percent
          example: watched_50_percent
          type: string
        object:
          description: Engagement source.
          enum:
            - video
          example: video
          type: string
        platform_video_ids:
          items:
            description: >-
              Numeric platform video IDs belonging to the selected social
              account. Supply 1–50 IDs.
            example: '444'
            type: string
          type: array
        retention_days:
          description: Rolling membership window in days, from 1 to 365.
          example: 30
          type: integer
        social_account_id:
          description: >-
            Connected social account ID, prefixed `sacc_`, with advertising
            access.
          example: sacc_xxxxxxxxxxxxxx
          type: string
      required:
        - object
        - social_account_id
        - event
        - retention_days
        - platform_video_ids
      type: object
    AudienceEngagement:
      properties:
        exclude:
          description: >-
            Exclude anyone matching any exclusion rule. Supply 0–10 rules. Video
            audiences do not support exclusions; use a separate audience in
            ad-group exclusions.
          items:
            $ref: '#/components/schemas/AudienceEngagementRule'
          type: array
        include:
          description: >-
            Match any inclusion rule. Supply 1–10 rules. Video rules must share
            a retention window and cannot be combined with other sources.
          items:
            $ref: '#/components/schemas/AudienceEngagementRule'
          type: array
        platform:
          description: Ad platform that maintains membership.
          enum:
            - meta
          example: meta
          type: string
      required:
        - platform
        - include
        - exclude
      type: object
    AudienceMatchRate:
      properties:
        lower_bound:
          description: >-
            Lower bound of the estimated match rate percentage. `null` until
            available.
          example: 40
          type:
            - number
            - 'null'
        platform:
          description: The ad platform that provided the match-rate estimate.
          enum:
            - meta
          example: meta
          type: string
        status:
          description: Availability of the estimated match rate.
          enum:
            - calculating
            - available
            - unavailable
            - null
          example: available
          type:
            - string
            - 'null'
        upper_bound:
          description: >-
            Upper bound of the estimated match rate percentage. `null` until
            available.
          example: 50
          type:
            - number
            - 'null'
      required:
        - platform
        - status
        - lower_bound
        - upper_bound
      type: object
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
  responses:
    InvalidParameters:
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/V1ErrorResponse'
      description: Invalid Parameters
    Unauthorized:
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/V1ErrorResponse'
      description: Unauthorized
    Conflict:
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/V1ErrorResponse'
      description: Conflict
  securitySchemes:
    bearerAuth:
      bearerFormat: auth-scheme
      description: >-
        An Account API key, account-scoped JWT, App API key, or user OAuth
        token. Prepend the key or token with `Bearer`, for example `Bearer
        ***************************`.
      scheme: bearer
      type: http

````