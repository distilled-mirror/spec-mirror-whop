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

# Whop API Core Concepts

> The objects Whop's API is built from: what each one is, what it's called, and how they connect.

Whop's API is a small number of objects that connect in predictable ways. This page defines each one, and says so plainly where an object goes by more than one name.

```mermaid theme={null}
graph LR
  A[account<br/>biz_] --> P[product<br/>prod_]
  A --> K[API keys]
  P --> L[variant<br/>plan_]
  P --> E[experience<br/>exp_]
  U[user<br/>user_] --> M[membership<br/>mem_]
  M --> P
  PAY[payment<br/>pay_] --> LG[ledger<br/>ldgr_]
  LG --> PO[payout]
  LG --> T[transfer]
```

## Accounts and identity

**account** `biz_`

A business on Whop. Owns API keys, products, webhooks, and money. See [Accounts](https://docs.whop.com/api-reference/accounts/account).

**user** `user_`

A person with a Whop login. A user can buy from many accounts, and can also help run one. See [Users](https://docs.whop.com/api-reference/users/user).

**team member**

A user granted access to operate an account. Their role decides what they can do. See [Team members](https://docs.whop.com/api-reference/team-members/team-member).

**member**

A user seen as a customer: someone who holds or once held a membership to something the account sells. See [Members](https://docs.whop.com/api-reference/members/member).

### Subaccount

An account you enroll and act for when you run a marketplace. You collect payments on its behalf and pay it out. Platform guides sometimes call this a sub-merchant. The two words mean the same object. The [platform quickstart](https://docs.whop.com/developer/platforms/quickstart.md) walks the whole flow.

**verification**

The identity check an account clears before it can receive payouts. Individuals complete Know Your Customer (KYC) checks. Businesses complete Know Your Business (KYB) checks. See [Verifications](https://docs.whop.com/api-reference/verifications/verification).

**identity profile** `idpf_`

The record of verified identity data a verification produces.

## What you sell

An account sells products. A product carries one or more variants, which set the price, and connects to the experiences a buyer actually gets.

**product** `prod_`

A thing an account sells. Holds the description, the media, and the variants a buyer can choose from. See [Products](https://docs.whop.com/api-reference/products/product).

**variant** `plan_`

One purchasable configuration of a product, with its own price, billing shape, stock, SKU, and attributes. Variant IDs retain the `plan_` prefix. See [Variants](https://docs.whop.com/api-reference/variants/variant).

**membership** `mem_`

What a buyer gets when they pay. Ties a user to a variant and carries a status telling you whether access is valid right now. See [Memberships](https://docs.whop.com/api-reference/memberships/membership).

### Experience

A unit of content or functionality inside what an account sells. A product connects to one or more experiences, and a membership to that product is what grants access to them.

This is Whop's most platform-specific noun and the one you can't guess from its name. If you are building an app that runs inside Whop, the experience is the thing your app renders into. See [App views](https://docs.whop.com/developer/guides/app-views.md).

### Entitlement

The record that says a user may use a specific thing right now. Access is the question, and the entitlement is the stored answer. Check it against an account, a product, or an experience. See [Authentication](https://docs.whop.com/developer/guides/authentication.md).

## How money moves

Money arrives, sits somewhere, then leaves. Every object below is one step along that path, in the order it happens.

**checkout configuration** `ch_`

The reusable definition of a purchase: which variant, at what price, with what redirect. Produces a live checkout page. See [Checkout configurations](https://docs.whop.com/api-reference/checkout-configurations/checkout-configuration).

**payment** `pay_`

One attempt to move money from a buyer to an account, carrying a status that says whether it worked.

**charge**

The same event described from the card network's side. The docs use both words. The API object is the payment.

### Ledger

The running record of every credit and debit on an account's money. It's the source of truth for what an account holds and what it's owed. Money lands in a ledger before it goes anywhere else. See [Ledger activity](https://docs.whop.com/api-reference/ledgers/ledger-activity).

**balance**

The amount sitting on a ledger right now that you can spend or withdraw.

**reserve**

A slice of a balance Whop holds back against future refunds and disputes. It releases on a schedule.

**fee**

Whop's cut of a payment, taken before the money reaches a balance.

**deposit**

Money added to a balance from outside Whop. See [Deposits](https://docs.whop.com/api-reference/deposits/deposit).

**payout**

Money leaving a balance for a bank account or wallet outside Whop. See [Payouts](https://docs.whop.com/api-reference/payouts/payout).

**transfer**

Money moving between two accounts inside Whop, without touching the outside world. See [Transfers](https://docs.whop.com/api-reference/transfers/transfer).

**swap**

A conversion of a balance between supported tokens, chains, or wallet destinations. See [Swaps](https://docs.whop.com/api-reference/swaps/swap).

**card** `icrd_`

A Whop-issued virtual card that spends from an account or user balance. See [Cards](https://docs.whop.com/api-reference/cards/card).

**refund**

A completed payment returned to the buyer, initiated by you.

**dispute**

A payment reversed by the buyer's bank on their behalf. You can contest it. See [Disputes](https://docs.whop.com/api-reference/disputes/dispute).

## Building on Whop

> [!NOTE]
>
> **Platform** means two things in these docs. Whop-the-platform is the product you are integrating with. A platform in the marketplace sense is *you*, once you enroll subaccounts and take a cut of what they sell. The surrounding sentence tells you which is meant.

**app** `app_`

Software that runs inside Whop and reaches an audience through its marketplace. See [Apps](https://docs.whop.com/api-reference/apps/app).

**app view**

One screen an app renders inside Whop: the customer-facing experience view, or the creator-facing dashboard view. See [App views](https://docs.whop.com/developer/guides/app-views.md).

**iframe**

How Whop embeds your app's pages in its own interface. Your app runs on your servers and renders inside Whop's chrome. See [iframe](https://docs.whop.com/developer/guides/iframe.md).

**permission**

A single capability an API key is allowed to exercise. See [Permissions](https://docs.whop.com/developer/guides/permissions.md).

### Scope

The same idea as a permission, but on an OAuth token: the set of things a user has agreed your app may do on their behalf. A permission is what your key can do. A scope is what a user let you do. See the [OAuth guide](https://docs.whop.com/developer/guides/oauth.md).

**OAuth grant**

The record of a user approving your app for a set of scopes.

**element**

A prebuilt UI component Whop ships that you drop into your own product, so you don't build the interface yourself.

**event**

Something that happened in an account, like a payment succeeding.

**webhook**

The HTTP request Whop sends your server to tell you an event happened. See [Webhooks](https://docs.whop.com/developer/guides/webhooks.md).

## Community

**chat channel**

A room where members send messages to each other. See [Chat](https://docs.whop.com/developer/guides/chat/quickstart.md).

**message**

One post in a chat channel or a direct message.

**forum**

A threaded space for longer posts, separate from live chat. See [Forums](https://docs.whop.com/developer/guides/forums.md).

**forum post**

One thread in a forum, with its replies, and its reactions.

**notification**

A message pushed to a user inside Whop. Each send is scoped to one experience or one user. See [Notifications](https://docs.whop.com/developer/guides/notifications.md).

## Advertising

**ad campaign** `adcamp_`

The top-level container for paid ads on an ad network. Sets the platform, the objective, and the budget strategy. See [Ad campaigns](https://docs.whop.com/api-reference/ad-campaigns/ad-campaign).

**ad group** `adgrp_`

Sits inside a campaign and controls delivery: targeting, the optimization goal, and the schedule. See [Ad groups](https://docs.whop.com/api-reference/ad-groups/ad-group).

**ad** `ad_`

The creative unit an ad group delivers: copy, assets, and the destination URL. See [Ads](https://docs.whop.com/api-reference/ads/ad).

**audience**

A customer list uploaded to Whop and used to target or exclude people in an ad group. See [Audiences](https://docs.whop.com/api-reference/audiences/audience).

## Referrals and rewards

**affiliate** `aff_`

Someone who earns commission for sending buyers to a product. See [Affiliates](https://docs.whop.com/developer/guides/affiliates.md).

**partner**

A user enrolled in Whop's partner program, earning on the businesses they refer onto Whop. See [Partners](https://docs.whop.com/developer/partners/overview.md).

**referred business** `coma_`

A business a partner brought onto Whop, carrying the commission rates and the referral window that apply to it.

**partner earning**

One commission line for a referred business, naming the income source it came from and its status.

**bounty** `bnty_`

A paid task with its reward held in escrow from the moment it publishes. See [Bounties](https://docs.whop.com/developer/bounties/overview.md).

**bounty submission** `btys_`

One worker's attempt on a bounty, from the attempt through review, and on to payout.

## Environments and keys

Whop runs two environments. They share nothing.

| | Sandbox | Production |
| - | - | - |
| Money | Never moves | Real |
| Dashboard | Separate | Separate |
| Keys | Own set | Own set |
| Data | Test data | Live data |

See [Test in the sandbox](https://docs.whop.com/developer/guides/sandbox.md) for how to switch.

> [!WARNING]
>
> Sandbox and production keys look identical. Nothing in the key string tells you which environment it belongs to, so store them under different names and check before running anything that moves money.

Five credential types reach the API. Pick by whose data you need.

| Credential | Acts as | Use when |
| - | - | - |
| API key | Your account, or your app | Your server works on your own business, or on every account that installed your app |
| Account-scoped user token | One user, inside one account | Your product has its own users and they reach Whop without a Whop sign-in |
| Account access token | Your account, with narrowed permissions | A client needs short-lived access to your account without holding your API key |
| iframe user token | The user viewing your app | Your app renders inside Whop and you need to know who is looking at it |
| OAuth token | A user who signed in to Whop | A user signed in and approved your app |

See [Auth & API keys](https://docs.whop.com/developer/guides/auth-scoping.md) for how to choose and scope each one.

**Api-Version-Date**

The header that pins your integration to a dated version of the API, so later changes don't alter the responses you built against. See [API versioning](https://docs.whop.com/developer/api/versioning.md).

**idempotency key**

A value you attach to a `POST` so retrying it replays the original response instead of doing the work twice. See [Idempotent requests](https://docs.whop.com/developer/api/idempotency.md).

## ID reference

Every object carries a prefixed ID. When you are staring at a payload, this table tells you what you are holding.

| Prefix | Object | See |
| - | - | - |
| `biz_` | Account | [Accounts](https://docs.whop.com/api-reference/accounts/account) |
| `user_` | User | [Users](https://docs.whop.com/api-reference/users/user) |
| `prod_` | Product | [Products](https://docs.whop.com/api-reference/products/product) |
| `plan_` | Variant | [Variants](https://docs.whop.com/api-reference/variants/variant) |
| `mem_` | Membership | [Memberships](https://docs.whop.com/api-reference/memberships/membership) |
| `exp_` | Experience | [App views](https://docs.whop.com/developer/guides/app-views.md) |
| `app_` | App | [Apps](https://docs.whop.com/api-reference/apps/app) |
| `pay_` | Payment | [Overview](https://docs.whop.com/api-reference/overview.md) |
| `ch_` | Checkout configuration | [Checkout configurations](https://docs.whop.com/api-reference/checkout-configurations/checkout-configuration) |
| `ldgr_` | Ledger account | [Ledger activity](https://docs.whop.com/api-reference/ledgers/ledger-activity) |
| `idpf_` | Identity profile | [Verifications](https://docs.whop.com/api-reference/verifications/verification) |
| `sacc_` | External account | [External accounts](https://docs.whop.com/api-reference/external-accounts/external-account) |
| `aff_` | Affiliate | [Affiliates](https://docs.whop.com/developer/guides/affiliates.md) |
| `adcamp_` | Ad campaign | [Ad campaigns](https://docs.whop.com/api-reference/ad-campaigns/ad-campaign) |
| `adgrp_` | Ad group | [Ad groups](https://docs.whop.com/api-reference/ad-groups/ad-group) |
| `ad_` | Ad | [Ads](https://docs.whop.com/api-reference/ads/ad) |
| `bnty_` | Bounty | [Bounties](https://docs.whop.com/developer/bounties/overview.md) |
| `btys_` | Bounty submission | [Bounties](https://docs.whop.com/developer/bounties/overview.md) |
| `coma_` | Referred business | [Partners](https://docs.whop.com/developer/partners/overview.md) |
| `icrd_` | Card | [Cards](https://docs.whop.com/api-reference/cards/card) |
| `course_` | Course | [Overview](https://docs.whop.com/api-reference/overview.md) |
| `file_` | File | [Overview](https://docs.whop.com/api-reference/overview.md) |

## Next steps

> [card] [Quickstart](https://docs.whop.com/developer/quickstart.md)
>
> Make your first API call in a few minutes.

> [card] [Test in sandbox](https://docs.whop.com/developer/guides/sandbox.md)
>
> Try everything against test data before going live.

> [card] [API overview](https://docs.whop.com/api-reference/overview.md)
>
> How requests, versioning, and pagination work.

> [card] [What you can build](https://docs.whop.com/developer/start.md)
>
> The full capability map, and the guide for what you're building.


## Related topics

- [Whop Partners API](https://docs.whop.com/developer/partners/overview.md)
- [Whop API Overview](https://docs.whop.com/api-reference/overview.md)
- [Accounts API](https://docs.whop.com/api-reference/accounts/account.md)
