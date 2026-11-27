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

# Users API

> A User represents a person on Whop. Users have a public profile and can buy products, join accounts, and access experiences.

Use the Users API to search for users, retrieve or update profiles, and check whether a user has access to an account, product, or experience.

## Endpoints

| Endpoint | Request |
| - | - |
| [List Users](https://docs.whop.com/api-reference/users/list-users.md) | GET `/users` |
| [Retrieve User](https://docs.whop.com/api-reference/users/retrieve-user.md) | GET `/users/{id}` |
| [Check User Access](https://docs.whop.com/api-reference/users/check-user-access.md) | GET `/users/{id}/access/{resource_id}` |
| [Retrieve User Fees](https://docs.whop.com/api-reference/users/retrieve-user-fees.md) | GET `/users/{user_id}/fees` |
| [List Recommended Actions](https://docs.whop.com/api-reference/users/list-recommended-actions.md) | GET `/users/{id}/recommend_actions` |
| [List OAuth Grants](https://docs.whop.com/api-reference/users/list-oauth-grants.md) | GET `/users/me/oauth_grants` |
| [List](https://docs.whop.com/api-reference/users/list.md) | GET `/users/me/passkeys` |
| [Retrieve](https://docs.whop.com/api-reference/users/retrieve.md) | GET `/users/me/preferences` |
| [List Experiences](https://docs.whop.com/api-reference/users/list-experiences.md) | GET `/users/me/preferences/notifications/experiences` |
| [List Topics](https://docs.whop.com/api-reference/users/list-topics.md) | GET `/users/me/preferences/notifications/topics` |
| [Authorize an App](https://docs.whop.com/api-reference/users/authorize-an-app.md) | POST `/users/me/oauth_grants` |
| [Register](https://docs.whop.com/api-reference/users/register.md) | POST `/users/me/passkeys` |
| [Create Challenge](https://docs.whop.com/api-reference/users/create-challenge.md) | POST `/users/me/passkeys/challenge` |
| [Update User](https://docs.whop.com/api-reference/users/update-user.md) | PATCH `/users/{id}` |
| [Update](https://docs.whop.com/api-reference/users/update.md) | PATCH `/users/me/preferences` |
| [Set](https://docs.whop.com/api-reference/users/set.md) | PATCH `/users/me/preferences/notifications` |
| [Delete](https://docs.whop.com/api-reference/users/delete.md) | DELETE `/users/me/passkeys/{id}` |

## Attributes

**id** `string` *required*

User ID, prefixed `user_`.

**badges** `object[]` *required*

Public badges for the user, in display order. Empty when the user has no badges or is suspended.

> [accordion] Properties
>
> **type** `string` *required*
>
> Badge category. `whop_team` identifies a Whop team member. `verified_partner` identifies a verified member of the Whop Partners program.
>
> Available options: `whop_team`, `verified_partner`

**balance** `object | null` *required*

The user's balance: personal cash + other holdings + deposits still in flight, plus account balances for accounts they own. Computed only on the self view (retrieved with the reserved id `me`) for callers with balance-read scope; `null` otherwise, or when `include_balance=false`.

> [accordion] Properties
>
> **businesses** `object[]` *required*
>
> Account balances for accounts the user owns, highest balance first. Excludes accounts with no balance.
>
> > [accordion] Properties
> >
> > **id** `string` *required*
> >
> > The account ID, which looks like biz\_\*\*\*\*\*\*\*\*\*\*\*\*\*.
> >
> > **balance\_usd** `string` *required*
> >
> > The account's total balance in USD.
> >
> > **logo\_url** `string | null` *required*
> >
> > The account's logo URL.
> >
> > **name** `string | null` *required*
> >
> > The account's display name.
>
> **businesses\_total\_usd** `string` *required*
>
> Combined USD balance across every account the user owns.
>
> **cash** `object[]` *required*
>
> Per-currency fiat cash balances.
>
> > [accordion] Properties
> >
> > **balance** `number` *required*
> >
> > Available balance in the native currency.
> >
> > **balance\_usd** `number` *required*
> >
> > Available balance converted to USD.
> >
> > **currency** `string` *required*
> >
> > Lowercase ISO currency code, such as `usd` or `eur`.
> >
> > **in\_transit\_balance\_usd** `number` *required*
> >
> > Balance moving to the user's own wallet or card, converted to USD.
> >
> > **pending\_balance\_usd** `number` *required*
> >
> > Pending balance converted to USD.
> >
> > **price\_usd** `number | null` *required*
> >
> > USD price per native currency unit, or `null` when no exchange rate is
> > available.
> >
> > **reserve\_balance\_usd** `number` *required*
> >
> > Reserved balance converted to USD.
> >
> > **total\_withdrawable\_balance** `number` *required*
> >
> > Withdrawable amount in the native currency.
>
> **cash\_usd** `string` *required*
>
> Fiat cash in USD, including pending, in-transit, and reserve.
>
> **crypto** `object[]` *required*
>
> Per-token crypto holdings in the ledger's own wallet.
>
> > [accordion] Properties
> >
> > **balance** `string` *required*
> >
> > Amount held in native token units, as a decimal string.
> >
> > **breakdown** `object` *required*
> >
> > Balance split into available, pending, in-transit, and reserve amounts, as native-unit decimal strings. Transfers between the user's own wallet and card are reported in `in_transit` until they arrive.
> >
> > > [accordion] Properties
> > >
> > > **available** `string` *required*
> > >
> > > Amount you can spend, send, or withdraw now, in native units, as a decimal
> > > string.
> > >
> > > **finishing\_settlement** `string` *required*
> > >
> > > The part of `pending` that has already settled and is waiting to move into the
> > > account's crypto wallet or Whop Card, where it becomes `available`. The move
> > > usually starts within the hour; while it runs, the amount is reported in
> > > `in_transit` instead. Money moved only to top up the account's retained
> > > minimum balance is not included. In native units, as a decimal string.
> > >
> > > **in\_transit** `string` *required*
> > >
> > > Amount moving between the account's own destinations, such as a card top-up.
> > > In native units, as a decimal string.
> > >
> > > **pending** `string` *required*
> > >
> > > Amount from recent payments still settling, in native units, as a decimal
> > > string.
> > >
> > > **pending\_settlements** `object[]` *required*
> > >
> > > When the pending amount is expected to settle, one entry per day, earliest first. Money without a scheduled settlement day, including `finishing_settlement`, is left out, so these entries plus `finishing_settlement` can sum to less than `pending`, never more.
> > >
> > > > [accordion] Properties
> > > >
> > > > **amount** `string` *required*
> > > >
> > > > Amount expected that day, in native units, as a decimal string.
> > > >
> > > > **date** `string` *required*
> > > >
> > > > The day this money is expected to finish settling, as an ISO 8601 date.
> > >
> > > **perps** `string` *required*
> > >
> > > Amount held for perpetual trading, at what the trading account is worth now,
> > > in native units, as a decimal string. It is not part of `available`.
> > >
> > > **reserve** `string` *required*
> > >
> > > Amount held back, in native units, as a decimal string. Retrieve the account's reserves for why it is held and when it unlocks.
> >
> > **icon\_url** `string | null` *required*
> >
> > Token icon URL.
> >
> > **name** `string | null` *required*
> >
> > The token's display name.
> >
> > **price\_usd** `number | null` *required*
> >
> > USD price per token, or `null` when unknown.
> >
> > **symbol** `string` *required*
> >
> > Token display symbol, such as `USDT`, `XAUT`, or `cbBTC`.
> >
> > **value\_usd** `number` *required*
> >
> > Holding USD value.
>
> **crypto\_usd** `string` *required*
>
> Crypto holdings in USD.
>
> **pending\_usd** `string` *required*
>
> Fiat pending and in-transit balances, plus in-flight treasury deposits, in
> USD.
>
> **total\_usd** `string` *required*
>
> The user's personal balance in USD: cash (available + pending + in-transit +
> reserve) + other holdings + deposits still in flight. Excludes account
> balances (see businesses\_total\_usd).
>
> **treasury\_pending\_usd** `string` *required*
>
> Balance moves into the user's wallet still in flight, in USD.

**balance\_history** `object | null` *required*

The user's cumulative wallet balance over time (USD `\{ t, v }` points plus last/min/max), for the balance chart. Opt in with `include_balance_history=true` when retrieving yourself with the reserved id `me`; populated only for callers with balance-read scope and `null` otherwise. A user with no wallet activity returns an empty series.

> [accordion] Properties
>
> **data** `object[]` *required*
>
> Cumulative balance points over the requested window, oldest first.
>
> > [accordion] Properties
> >
> > **t** `integer` *required*
> >
> > Point timestamp, in Unix seconds.
> >
> > **v** `number` *required*
> >
> > Cumulative wallet balance at this point, in USD.
>
> **last** `number` *required*
>
> Value of the most recent point, in USD.
>
> **max** `number` *required*
>
> Maximum value across the window, in USD.
>
> **min** `number` *required*
>
> Minimum value across the window, in USD.

**balances** `object[]` *required*

The user's personal holdings, each with its USD value and breakdown, in the same shape as an account's `balances`. Empty when `total_usd` is `null`.

> [accordion] Properties
>
> **balance** `string` *required*
>
> Total amount held in native units, as a decimal string.
>
> **breakdown** `object` *required*
>
> Balance split into available, pending, and reserve amounts, as native-unit decimal strings, with the days the pending amount is expected to settle.
>
> > [accordion] Properties
> >
> > **available** `string` *required*
> >
> > Amount you can spend, send, or withdraw now, in native units, as a decimal
> > string.
> >
> > **finishing\_settlement** `string` *required*
> >
> > The part of `pending` that has already settled and is waiting to move into the
> > account's crypto wallet or Whop Card, where it becomes `available`. The move
> > usually starts within the hour; while it runs, the amount is reported in
> > `in_transit` instead. Money moved only to top up the account's retained
> > minimum balance is not included. In native units, as a decimal string.
> >
> > **in\_transit** `string` *required*
> >
> > Amount moving between the account's own destinations, such as a card top-up.
> > In native units, as a decimal string.
> >
> > **pending** `string` *required*
> >
> > Amount from recent payments still settling, in native units, as a decimal
> > string.
> >
> > **pending\_settlements** `object[]` *required*
> >
> > When the pending amount is expected to settle, one entry per day, earliest first. Money without a scheduled settlement day, including `finishing_settlement`, is left out, so these entries plus `finishing_settlement` can sum to less than `pending`, never more.
> >
> > > [accordion] Properties
> > >
> > > **amount** `string` *required*
> > >
> > > Amount expected that day, in native units, as a decimal string.
> > >
> > > **date** `string` *required*
> > >
> > > The day this money is expected to finish settling, as an ISO 8601 date.
> >
> > **perps** `string` *required*
> >
> > Amount held for perpetual trading, at what the trading account is worth now,
> > in native units, as a decimal string. It is not part of `available`.
> >
> > **reserve** `string` *required*
> >
> > Amount held back, in native units, as a decimal string. Retrieve the account's reserves for why it is held and when it unlocks.
>
> **icon\_url** `string | null` *required*
>
> Holding icon URL.
>
> **name** `string` *required*
>
> The holding's display name
>
> **price\_usd** `number | null` *required*
>
> USD price per unit, or `null` when no exchange rate is available.
>
> **symbol** `string` *required*
>
> Holding display symbol, such as `USDT`, `cbBTC`, or `EUR`.
>
> **value\_usd** `string | null` *required*
>
> Holding USD value, or `null` when no exchange rate is available.

**banner** `object | null` *required*

The user's profile banner wrapper. `null` when the user has no banner.

> [accordion] Properties
>
> **url** `string` *required*
>
> Profile banner image URL.

**bio** `string | null` *required*

The user's biography

**cards** `object | null` *required*

Where the user's personal card application stands. Populated only on the self view (retrieved with the reserved id `me`) for callers with balance-read scope; `null` otherwise, or when the user has never applied for a card.

> [accordion] Properties
>
> **decline\_reason** `string | null` *required*
>
> Reason the card issuer declined the application. `unsupported_state` replaces `unsupported_region` for a verified person or business in the United States. `compliance` covers sanctions, politically exposed person, and adverse media checks. `other` covers reasons that are not in this list. `null` when `status` is not `denied`, or when the issuer did not decline the application.
>
> Available options: `compromised_identity`, `duplicate`, `unsupported_region`, `compliance`, `fraud`, `verification_failed`, `unsupported_state`, `other`
>
> **kind** `string | null` *required*
>
> Whether the card application verifies a business (`business`, KYB) or a person (`individual`, consumer identity). `null` when the application is not yet linked to a verification.
>
> Available options: `individual`, `business`
>
> **status** `string` *required*
>
> Where the card application stands. `approved` means cards can be issued. `needs_verification` means the applicant has not completed identity verification yet. `needs_information` means they did, but the documents were rejected for a fixable reason and must be resubmitted. `pending` and `manual_review` are in flight. `denied`, `locked`, and `canceled` are terminal.
>
> Available options: `approved`, `pending`, `manual_review`, `denied`, `locked`, `canceled`, `needs_verification`, `needs_information`

**city** `string | null` *required*

The user's city, derived from their IP address. `null` when unknown or the
user turned off location sharing.

**country** `string | null` *required*

The user's country as an ISO 3166-1 alpha-2 code, derived from their IP
address. `null` when unknown or the user turned off location sharing.

**created\_at** `string` *required*

When the user was created, as an ISO 8601 timestamp

**date\_of\_birth** `string | null` *required*

The user's date of birth as an ISO 8601 date, like `1998-04-23`. Populated
only on the self view (retrieved with the reserved id `me`) for callers with
email-read scope; `null` otherwise, or when the user has not set one.

**earnings\_usd** `object | null` *required*

The user's gross USD income over time, including a Partner commission breakdown. Populated only on single-user self reads for callers with balance-read scope; `null` otherwise.

> [accordion] Properties
>
> **first\_earned\_at** `string | null` *required*
>
> The first time the user earned gross income, as an ISO 8601 timestamp.
>
> **owned\_accounts** `object` *required*
>
> Gross income from accounts the user owns or is owner-authorized on.
>
> > [accordion] Properties
> >
> > **last\_24\_hours** `string` *required*
> >
> > Gross income in USD over the last 24 hours.
> >
> > **last\_30\_days** `string` *required*
> >
> > Gross income in USD over the last 30 days.
> >
> > **last\_7\_days** `string` *required*
> >
> > Gross income in USD over the last 7 days.
> >
> > **lifetime** `string` *required*
> >
> > All-time gross income in USD.
>
> **partners** `object` *required*
>
> Partner commissions posted to the user's wallet. Pending Partner payouts are excluded until they post; later reversals do not reduce gross income.
>
> > [accordion] Properties
> >
> > **last\_24\_hours** `string` *required*
> >
> > Gross income in USD over the last 24 hours.
> >
> > **last\_30\_days** `string` *required*
> >
> > Gross income in USD over the last 30 days.
> >
> > **last\_7\_days** `string` *required*
> >
> > Gross income in USD over the last 7 days.
> >
> > **lifetime** `string` *required*
> >
> > All-time gross income in USD.
>
> **personal** `object` *required*
>
> Gross income from the user's personal wallet.
>
> > [accordion] Properties
> >
> > **last\_24\_hours** `string` *required*
> >
> > Gross income in USD over the last 24 hours.
> >
> > **last\_30\_days** `string` *required*
> >
> > Gross income in USD over the last 30 days.
> >
> > **last\_7\_days** `string` *required*
> >
> > Gross income in USD over the last 7 days.
> >
> > **lifetime** `string` *required*
> >
> > All-time gross income in USD.
>
> **total** `object` *required*
>
> Gross income from the user's personal wallet plus accounts they own or are owner-authorized on.
>
> > [accordion] Properties
> >
> > **last\_24\_hours** `string` *required*
> >
> > Gross income in USD over the last 24 hours.
> >
> > **last\_30\_days** `string` *required*
> >
> > Gross income in USD over the last 30 days.
> >
> > **last\_7\_days** `string` *required*
> >
> > Gross income in USD over the last 7 days.
> >
> > **lifetime** `string` *required*
> >
> > All-time gross income in USD.

**email** `string | null` *required*

The user's email address. Populated only on the self view (retrieved with the
reserved id `me`) for callers with email-read scope; `null` otherwise, or
while the account has no confirmed email yet.

**followers\_count** `integer` *required*

The number of users who follow this user.

**level** `object | null` *required*

The user's Whop Level, counted across every account they own. `current` is public, and is `0` for a user with no points yet. The points and spend fields are populated only when you retrieve yourself with the reserved id `me` and have balance-read scope. `null` on lists and when the user hides their level.

> [accordion] Properties
>
> **current** `integer` *required*
>
> The level that applies: the highest the user reached this month or in the 5
> months before, from `0` up to `10`. Public.
>
> **points** `integer | null` *required*
>
> Points earned this month by the user and their accounts, rounded down. An
> account whose month nets below zero counts as zero. `null` unless you retrieve
> yourself with the reserved id `me` and have balance-read scope.
>
> **points\_by\_source** `object | null` *required*
>
> This month's points, split by where they were earned. Each source is net of its refunds, so one can be negative. `null` unless you retrieve yourself with the reserved id `me` and have balance-read scope.
>
> > [accordion] Properties
> >
> > **actions** `integer` *required*
> >
> > Points earned from eligible actions on Whop, rounded toward zero.
> >
> > **ads** `integer` *required*
> >
> > Points from paid Whop Ads charges, net of refunds: 10 for every dollar,
> > rounded toward zero.
> >
> > **cards** `integer` *required*
> >
> > Points from captured Whop Card spend, net of refunds: 10 for every dollar,
> > rounded toward zero.
> >
> > **partners** `integer` *required*
> >
> > Points from accounts the owner referred as a partner: 1 for every 3 they earn,
> > rounded toward zero.
> >
> > **trading** `integer` *required*
> >
> > Points from perpetual trading volume filled through Whop: 1 for every dollar traded, rounded toward zero.
>
> **resets\_at** `string | null` *required*
>
> When this month's points reset to zero, as an ISO 8601 timestamp. `null` unless you retrieve yourself with the reserved id `me` and have balance-read scope.

**name** `string | null` *required*

The user's display name

**profile\_picture** `object` *required*

Avatar wrapper. Its `url` is always present, and is a generated placeholder when the user set no picture.

> [accordion] Properties
>
> **url** `string` *required*
>
> Avatar image URL. Always present. It is a generated placeholder when the user set no picture.

**social\_links** `object[]` *required*

The social accounts and websites on the user's public profile. Empty when the user has none.

> [accordion] Properties
>
> **url** `string` *required*
>
> The link's URL.
>
> **username** `string | null` *required*
>
> The user's handle on the platform, as entered or as reported by the connected
> account. `null` for links without a handle, such as a personal site.
>
> **verified** `boolean` *required*
>
> Whether Whop verified that the user owns the account.
>
> **website** `string` *required*
>
> The platform the link points to. `website` is a personal site rather than a social platform.
>
> Available options: `x`, `instagram`, `facebook`, `tiktok`, `youtube`, `linkedin`, `twitch`, `website`

**state** `string | null` *required*

The user's state or region, derived from their IP address. `null` when unknown
or the user turned off location sharing.

**total\_usd** `string | null` *required*

Total USD value of the user's personal holdings in `balances`, across those
with known exchange rates. Computed only on the self view (retrieved with the
reserved id `me`) for callers with balance-read scope; `null` otherwise, when
`include_balance=false`, or when the balance source is unavailable.

**trading** `trading_account | null` *required*

The trading account address, a snapshot of its margin and open positions, and its WebSocket subscriptions. Opt in with `include_trading=true` when retrieving `me`. `null` otherwise, without trading permission, or without an Ethereum wallet.

> [accordion] Properties
>
> **id** `string` *required*
>
> The Whop wallet ID backing this trading account, prefixed `cwal_`.
>
> **account\_id** `string | null` *required*
>
> The account that owns this trading account, prefixed `biz_`. `null` when a
> user owns it.
>
> **hyperliquid** `object | null` *required*
>
> Hyperliquid-specific state. Present when `provider` is `hyperliquid`, otherwise `null`.
>
> > [accordion] Properties
> >
> > **address** `string` *required*
> >
> > Lowercase wallet address that holds the Hyperliquid account.
> >
> > **depositable** `string | null` *required*
> >
> > USDT0 in the Whop wallet on Plasma that a `deposit` trade can move into this
> > account, as a decimal string. `null` when the chain could not be read.
> >
> > **margin\_summary** `object | null` *required*
> >
> > Account value and margin across the main exchange and the `xyz` markets, read from Hyperliquid at most once a second. `null` when Hyperliquid could not be read. Use `websocket_subscriptions` for live updates.
> >
> > > [accordion] Properties
> > >
> > > **account\_value** `object` *required*
> > >
> > > Total account value in USD, including unrealized profit and loss.
> > >
> > > > [accordion] Properties
> > > >
> > > > **amount** `string` *required*
> > > >
> > > > The amount in major units, as an exact decimal string. For example, `"10.00"`
> > > > is ten dollars. It is a string so that no float rounds it in transit.
> > > >
> > > > **currency** `string` *required*
> > > >
> > > > Three-letter ISO 4217 currency code, lowercase.
> > > >
> > > > **decimals** `integer` *required*
> > > >
> > > > How many decimal places the amount carries. This is the precision the charge
> > > > itself runs at.
> > > >
> > > > **display\_decimals** `integer` *required*
> > > >
> > > > How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
> > >
> > > **total\_margin\_used** `object` *required*
> > >
> > > Margin allocated across open positions, in USD.
> > >
> > > > [accordion] Properties
> > > >
> > > > **amount** `string` *required*
> > > >
> > > > The amount in major units, as an exact decimal string. For example, `"10.00"`
> > > > is ten dollars. It is a string so that no float rounds it in transit.
> > > >
> > > > **currency** `string` *required*
> > > >
> > > > Three-letter ISO 4217 currency code, lowercase.
> > > >
> > > > **decimals** `integer` *required*
> > > >
> > > > How many decimal places the amount carries. This is the precision the charge
> > > > itself runs at.
> > > >
> > > > **display\_decimals** `integer` *required*
> > > >
> > > > How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
> > >
> > > **total\_position\_notional** `object` *required*
> > >
> > > Combined notional value of open positions, in USD.
> > >
> > > > [accordion] Properties
> > > >
> > > > **amount** `string` *required*
> > > >
> > > > The amount in major units, as an exact decimal string. For example, `"10.00"`
> > > > is ten dollars. It is a string so that no float rounds it in transit.
> > > >
> > > > **currency** `string` *required*
> > > >
> > > > Three-letter ISO 4217 currency code, lowercase.
> > > >
> > > > **decimals** `integer` *required*
> > > >
> > > > How many decimal places the amount carries. This is the precision the charge
> > > > itself runs at.
> > > >
> > > > **display\_decimals** `integer` *required*
> > > >
> > > > How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
> > >
> > > **withdrawable** `object` *required*
> > >
> > > USD that can be withdrawn now without closing positions.
> > >
> > > > [accordion] Properties
> > > >
> > > > **amount** `string` *required*
> > > >
> > > > The amount in major units, as an exact decimal string. For example, `"10.00"`
> > > > is ten dollars. It is a string so that no float rounds it in transit.
> > > >
> > > > **currency** `string` *required*
> > > >
> > > > Three-letter ISO 4217 currency code, lowercase.
> > > >
> > > > **decimals** `integer` *required*
> > > >
> > > > How many decimal places the amount carries. This is the precision the charge
> > > > itself runs at.
> > > >
> > > > **display\_decimals** `integer` *required*
> > > >
> > > > How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
> >
> > **positions** `trading_position[] | null` *required*
> >
> > Open perpetual positions across the main exchange and the `xyz` markets, read from Hyperliquid at most once a second. `null` when Hyperliquid could not be read. Use `websocket_subscriptions` for live updates.
> >
> > > [accordion] Properties
> > >
> > > **id** `string` *required*
> > >
> > > Trading position ID, prefixed `trdpos_`. Stable for a market within one
> > > trading account.
> > >
> > > **entry\_price** `object | null` *required*
> > >
> > > Average entry price in USD, or `null` when the provider omits it.
> > >
> > > > [accordion] Properties
> > > >
> > > > **amount** `string` *required*
> > > >
> > > > The amount in major units, as an exact decimal string. For example, `"10.00"`
> > > > is ten dollars. It is a string so that no float rounds it in transit.
> > > >
> > > > **currency** `string` *required*
> > > >
> > > > Three-letter ISO 4217 currency code, lowercase.
> > > >
> > > > **decimals** `integer` *required*
> > > >
> > > > How many decimal places the amount carries. This is the precision the charge
> > > > itself runs at.
> > > >
> > > > **display\_decimals** `integer` *required*
> > > >
> > > > How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
> > >
> > > **hyperliquid** `object | null` *required*
> > >
> > > Hyperliquid perpetual details. Present on Hyperliquid positions, otherwise `null`.
> > >
> > > > [accordion] Properties
> > > >
> > > > **cumulative\_funding** `object` *required*
> > > >
> > > > Funding paid on the position over several windows, in USD.
> > > >
> > > > > [accordion] Properties
> > > > >
> > > > > **all\_time** `object | null` *required*
> > > > >
> > > > > Funding paid on this market across the account's history, in USD, or `null` when unavailable.
> > > > >
> > > > > > [accordion] Properties
> > > > > >
> > > > > > **amount** `string` *required*
> > > > > >
> > > > > > The amount in major units, as an exact decimal string. For example, `"10.00"`
> > > > > > is ten dollars. It is a string so that no float rounds it in transit.
> > > > > >
> > > > > > **currency** `string` *required*
> > > > > >
> > > > > > Three-letter ISO 4217 currency code, lowercase.
> > > > > >
> > > > > > **decimals** `integer` *required*
> > > > > >
> > > > > > How many decimal places the amount carries. This is the precision the charge
> > > > > > itself runs at.
> > > > > >
> > > > > > **display\_decimals** `integer` *required*
> > > > > >
> > > > > > How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
> > > > >
> > > > > **since\_change** `object | null` *required*
> > > > >
> > > > > Funding paid since the position size last changed, in USD, or `null` when unavailable.
> > > > >
> > > > > > [accordion] Properties
> > > > > >
> > > > > > **amount** `string` *required*
> > > > > >
> > > > > > The amount in major units, as an exact decimal string. For example, `"10.00"`
> > > > > > is ten dollars. It is a string so that no float rounds it in transit.
> > > > > >
> > > > > > **currency** `string` *required*
> > > > > >
> > > > > > Three-letter ISO 4217 currency code, lowercase.
> > > > > >
> > > > > > **decimals** `integer` *required*
> > > > > >
> > > > > > How many decimal places the amount carries. This is the precision the charge
> > > > > > itself runs at.
> > > > > >
> > > > > > **display\_decimals** `integer` *required*
> > > > > >
> > > > > > How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
> > > > >
> > > > > **since\_open** `object | null` *required*
> > > > >
> > > > > Funding paid since the position opened, in USD, or `null` when unavailable.
> > > > >
> > > > > > [accordion] Properties
> > > > > >
> > > > > > **amount** `string` *required*
> > > > > >
> > > > > > The amount in major units, as an exact decimal string. For example, `"10.00"`
> > > > > > is ten dollars. It is a string so that no float rounds it in transit.
> > > > > >
> > > > > > **currency** `string` *required*
> > > > > >
> > > > > > Three-letter ISO 4217 currency code, lowercase.
> > > > > >
> > > > > > **decimals** `integer` *required*
> > > > > >
> > > > > > How many decimal places the amount carries. This is the precision the charge
> > > > > > itself runs at.
> > > > > >
> > > > > > **display\_decimals** `integer` *required*
> > > > > >
> > > > > > How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
> > > >
> > > > **leverage** `object` *required*
> > > >
> > > > Margin mode and multiplier for the position.
> > > >
> > > > > [accordion] Properties
> > > > >
> > > > > **type** `string` *required*
> > > > >
> > > > > `cross` shares margin across positions. `isolated` limits margin to this position.
> > > > >
> > > > > Available options: `cross`, `isolated`
> > > > >
> > > > > **value** `integer` *required*
> > > > >
> > > > > Multiplier applied to the position's margin, such as `10` for 10x.
> > > >
> > > > **liquidation\_price** `object | null` *required*
> > > >
> > > > Estimated liquidation price in USD, or `null` when Hyperliquid reports none.
> > > >
> > > > > [accordion] Properties
> > > > >
> > > > > **amount** `string` *required*
> > > > >
> > > > > The amount in major units, as an exact decimal string. For example, `"10.00"`
> > > > > is ten dollars. It is a string so that no float rounds it in transit.
> > > > >
> > > > > **currency** `string` *required*
> > > > >
> > > > > Three-letter ISO 4217 currency code, lowercase.
> > > > >
> > > > > **decimals** `integer` *required*
> > > > >
> > > > > How many decimal places the amount carries. This is the precision the charge
> > > > > itself runs at.
> > > > >
> > > > > **display\_decimals** `integer` *required*
> > > > >
> > > > > How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
> > > >
> > > > **margin\_used** `object` *required*
> > > >
> > > > Margin allocated to the position, in USD.
> > > >
> > > > > [accordion] Properties
> > > > >
> > > > > **amount** `string` *required*
> > > > >
> > > > > The amount in major units, as an exact decimal string. For example, `"10.00"`
> > > > > is ten dollars. It is a string so that no float rounds it in transit.
> > > > >
> > > > > **currency** `string` *required*
> > > > >
> > > > > Three-letter ISO 4217 currency code, lowercase.
> > > > >
> > > > > **decimals** `integer` *required*
> > > > >
> > > > > How many decimal places the amount carries. This is the precision the charge
> > > > > itself runs at.
> > > > >
> > > > > **display\_decimals** `integer` *required*
> > > > >
> > > > > How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
> > > >
> > > > **return\_on\_equity** `string` *required*
> > > >
> > > > Return on equity as a decimal ratio string, such as `0.1` for 10%.
> > >
> > > **market** `string` *required*
> > >
> > > Market symbol on the provider, such as `ETH`.
> > >
> > > **object** `string` *required*
> > >
> > > **position\_value** `object` *required*
> > >
> > > Current position value in USD.
> > >
> > > > [accordion] Properties
> > > >
> > > > **amount** `string` *required*
> > > >
> > > > The amount in major units, as an exact decimal string. For example, `"10.00"`
> > > > is ten dollars. It is a string so that no float rounds it in transit.
> > > >
> > > > **currency** `string` *required*
> > > >
> > > > Three-letter ISO 4217 currency code, lowercase.
> > > >
> > > > **decimals** `integer` *required*
> > > >
> > > > How many decimal places the amount carries. This is the precision the charge
> > > > itself runs at.
> > > >
> > > > **display\_decimals** `integer` *required*
> > > >
> > > > How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
> > >
> > > **side** `string` *required*
> > >
> > > Available options: `long`, `short`
> > >
> > > **size** `string` *required*
> > >
> > > Absolute position size as a decimal string.
> > >
> > > **unrealized\_pnl** `object` *required*
> > >
> > > Unrealized profit or loss in USD. Negative for a loss.
> > >
> > > > [accordion] Properties
> > > >
> > > > **amount** `string` *required*
> > > >
> > > > The amount in major units, as an exact decimal string. For example, `"10.00"`
> > > > is ten dollars. It is a string so that no float rounds it in transit.
> > > >
> > > > **currency** `string` *required*
> > > >
> > > > Three-letter ISO 4217 currency code, lowercase.
> > > >
> > > > **decimals** `integer` *required*
> > > >
> > > > How many decimal places the amount carries. This is the precision the charge
> > > > itself runs at.
> > > >
> > > > **display\_decimals** `integer` *required*
> > > >
> > > > How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
> >
> > **websocket\_subscriptions** `object[]` *required*
> >
> > Subscriptions for live account updates. Send each `message` unchanged after connecting to `websocket_url`.
> >
> > > [accordion] Properties
> > >
> > > **channel** `string` *required*
> > >
> > > The live update stream this subscription opens.
> > >
> > > Available options: `clearinghouse_state`, `open_orders`, `order_updates`, `user_fills`, `user_events`
> > >
> > > **message** `string` *required*
> > >
> > > JSON subscription message to send unchanged over the Hyperliquid WebSocket.
> >
> > **websocket\_url** `string` *required*
> >
> > Hyperliquid WebSocket URL to connect to directly for live updates.
>
> **object** `string` *required*
>
> **provider** `string` *required*
>
> Trading venue that holds the positions and orders.
>
> Available options: `hyperliquid`
>
> **spot** `object | null` *required*
>
> Spot holdings in the Whop wallet with their entry price and profit and loss. `null` when it could not be read.
>
> > [accordion] Properties
> >
> > **holdings** `object[]` *required*
> >
> > Each spot asset the wallet still holds, with its average entry price and profit and loss. Cost basis counts the oldest units as sold first and includes assets that arrived from outside Whop.
> >
> > > [accordion] Properties
> > >
> > > **average\_entry\_price** `object | null` *required*
> > >
> > > The average price paid per unit of what is still held, in USD, counting the oldest units as sold first. `null` when it is not known.
> > >
> > > > [accordion] Properties
> > > >
> > > > **amount** `string` *required*
> > > >
> > > > The amount in major units, as an exact decimal string. For example, `"10.00"`
> > > > is ten dollars. It is a string so that no float rounds it in transit.
> > > >
> > > > **currency** `string` *required*
> > > >
> > > > Three-letter ISO 4217 currency code, lowercase.
> > > >
> > > > **decimals** `integer` *required*
> > > >
> > > > How many decimal places the amount carries. This is the precision the charge
> > > > itself runs at.
> > > >
> > > > **display\_decimals** `integer` *required*
> > > >
> > > > How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
> > >
> > > **cost\_basis** `object` *required*
> > >
> > > What was paid in USD for the units still held.
> > >
> > > > [accordion] Properties
> > > >
> > > > **amount** `string` *required*
> > > >
> > > > The amount in major units, as an exact decimal string. For example, `"10.00"`
> > > > is ten dollars. It is a string so that no float rounds it in transit.
> > > >
> > > > **currency** `string` *required*
> > > >
> > > > Three-letter ISO 4217 currency code, lowercase.
> > > >
> > > > **decimals** `integer` *required*
> > > >
> > > > How many decimal places the amount carries. This is the precision the charge
> > > > itself runs at.
> > > >
> > > > **display\_decimals** `integer` *required*
> > > >
> > > > How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
> > >
> > > **market** `string` *required*
> > >
> > > The spot market's `id` from `GET /trades/markets`, such as `BTC`, `XAUT`, or a
> > > tokenized stock's ticker such as `AAPL`.
> > >
> > > **realized\_pnl** `object` *required*
> > >
> > > Gains and losses already locked in by selling, in USD.
> > >
> > > > [accordion] Properties
> > > >
> > > > **amount** `string` *required*
> > > >
> > > > The amount in major units, as an exact decimal string. For example, `"10.00"`
> > > > is ten dollars. It is a string so that no float rounds it in transit.
> > > >
> > > > **currency** `string` *required*
> > > >
> > > > Three-letter ISO 4217 currency code, lowercase.
> > > >
> > > > **decimals** `integer` *required*
> > > >
> > > > How many decimal places the amount carries. This is the precision the charge
> > > > itself runs at.
> > > >
> > > > **display\_decimals** `integer` *required*
> > > >
> > > > How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
> > >
> > > **unrealized\_pnl** `object` *required*
> > >
> > > The value of the units still held minus their `cost_basis`, in USD.
> > >
> > > > [accordion] Properties
> > > >
> > > > **amount** `string` *required*
> > > >
> > > > The amount in major units, as an exact decimal string. For example, `"10.00"`
> > > > is ten dollars. It is a string so that no float rounds it in transit.
> > > >
> > > > **currency** `string` *required*
> > > >
> > > > Three-letter ISO 4217 currency code, lowercase.
> > > >
> > > > **decimals** `integer` *required*
> > > >
> > > > How many decimal places the amount carries. This is the precision the charge
> > > > itself runs at.
> > > >
> > > > **display\_decimals** `integer` *required*
> > > >
> > > > How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
> > >
> > > **unrealized\_pnl\_percentage** `number | null` *required*
> > >
> > > `unrealized_pnl` as a percentage of `cost_basis`, where `2` means up 2%. `null` when it is not known.
>
> **user\_id** `string | null` *required*
>
> The user who owns this trading account, prefixed `user_`. `null` when an account owns it.

**username** `string` *required*

The user's unique username

**verification** `object` *required*

Identity verification status for the user's `individual` (KYC) and `business`
(KYB) profiles. Each is `null` until created, otherwise a `status` of
`not_started`, `pending`, `approved`, or `rejected`.

**verified\_at** `string | null` *required*

When the user received a verified badge, as an ISO 8601 timestamp. `null` when
the user is not verified.

**whop\_partner\_enabled\_at** `string | null` *required*

When the user became an enrolled Whop Partner, as an ISO 8601 timestamp.
`null` if never enrolled.

**whop\_partner\_onboarded\_accounts\_count** `integer | null` *required*

Number of accounts the user referred to Whop as a Whop Partner that have
processed more than \$1 in volume attributed to the user. Populated only when
retrieving a single user who is a Verified Whop Partner; `null` otherwise.

**whop\_partner\_verified\_at** `string | null` *required*

When the user became a Verified Whop Partner, as an ISO 8601 timestamp. `null`
for users who are not Verified Whop Partners, including partners who left the
program and suspended users.

```json User theme={null}
{
	"id": "user_alex123",
	"trading": {
		"id": "cwal_xxxxxxxxxxxxx",
		"object": "trading_account",
		"account_id": null,
		"user_id": "user_alex123",
		"provider": "hyperliquid",
		"hyperliquid": {
			"address": "0x1234567890abcdef1234567890abcdef12345678",
			"websocket_url": "wss://api.hyperliquid.xyz/ws",
			"websocket_subscriptions": [
				{
					"channel": "clearinghouse_state",
					"message": "{\"method\":\"subscribe\",\"subscription\":{\"type\":\"clearinghouseState\",\"user\":\"0x1234567890abcdef1234567890abcdef12345678\"}}"
				},
				{
					"channel": "open_orders",
					"message": "{\"method\":\"subscribe\",\"subscription\":{\"type\":\"openOrders\",\"user\":\"0x1234567890abcdef1234567890abcdef12345678\"}}"
				},
				{
					"channel": "order_updates",
					"message": "{\"method\":\"subscribe\",\"subscription\":{\"type\":\"orderUpdates\",\"user\":\"0x1234567890abcdef1234567890abcdef12345678\"}}"
				},
				{
					"channel": "user_fills",
					"message": "{\"method\":\"subscribe\",\"subscription\":{\"type\":\"userFills\",\"user\":\"0x1234567890abcdef1234567890abcdef12345678\"}}"
				},
				{
					"channel": "user_events",
					"message": "{\"method\":\"subscribe\",\"subscription\":{\"type\":\"userEvents\",\"user\":\"0x1234567890abcdef1234567890abcdef12345678\"}}"
				}
			],
			"depositable": "1250.50",
			"margin_summary": {
				"account_value": {
					"currency": "usd",
					"amount": "1520.50",
					"decimals": 2,
					"display_decimals": 2
				},
				"total_position_notional": {
					"currency": "usd",
					"amount": "4350.00",
					"decimals": 2,
					"display_decimals": 2
				},
				"total_margin_used": {
					"currency": "usd",
					"amount": "435.00",
					"decimals": 2,
					"display_decimals": 2
				},
				"withdrawable": {
					"currency": "usd",
					"amount": "500.00",
					"decimals": 2,
					"display_decimals": 2
				}
			},
			"positions": [
				{
					"id": "trdpos_0123456789abcdef",
					"object": "trading_position",
					"market": "ETH",
					"side": "short",
					"size": "1.5",
					"entry_price": {
						"currency": "usd",
						"amount": "3000.00",
						"decimals": 2,
						"display_decimals": 2
					},
					"position_value": {
						"currency": "usd",
						"amount": "4350.00",
						"decimals": 2,
						"display_decimals": 2
					},
					"unrealized_pnl": {
						"currency": "usd",
						"amount": "150.25",
						"decimals": 2,
						"display_decimals": 2
					},
					"hyperliquid": {
						"leverage": {
							"type": "cross",
							"value": 10
						},
						"liquidation_price": {
							"currency": "usd",
							"amount": "4912.40",
							"decimals": 2,
							"display_decimals": 2
						},
						"margin_used": {
							"currency": "usd",
							"amount": "435.00",
							"decimals": 2,
							"display_decimals": 2
						},
						"return_on_equity": "0.33",
						"cumulative_funding": {
							"all_time": {
								"currency": "usd",
								"amount": "1.20",
								"decimals": 2,
								"display_decimals": 2
							},
							"since_open": {
								"currency": "usd",
								"amount": "0.40",
								"decimals": 2,
								"display_decimals": 2
							},
							"since_change": {
								"currency": "usd",
								"amount": "0.10",
								"decimals": 2,
								"display_decimals": 2
							}
						}
					}
				}
			]
		},
		"spot": {
			"holdings": [
				{
					"market": "BTC",
					"average_entry_price": {
						"currency": "usd",
						"amount": "79841.12345678",
						"decimals": 8,
						"display_decimals": 8
					},
					"cost_basis": {
						"currency": "usd",
						"amount": "400.00",
						"decimals": 2,
						"display_decimals": 2
					},
					"unrealized_pnl": {
						"currency": "usd",
						"amount": "38.25",
						"decimals": 2,
						"display_decimals": 2
					},
					"unrealized_pnl_percentage": 9.5625,
					"realized_pnl": {
						"currency": "usd",
						"amount": "3.25",
						"decimals": 2,
						"display_decimals": 2
					}
				}
			]
		}
	},
	"balance": {
		"businesses": [
			{
				"id": "biz_shine123",
				"name": "Shine Time Auto Detailing",
				"logo_url": "https://cdn.whop.com/logos/shine-time-auto-detailing.png",
				"balance_usd": "14325.00"
			}
		],
		"businesses_total_usd": "14325.00",
		"cash": [
			{
				"currency": "usd",
				"balance": 250,
				"balance_usd": 250,
				"pending_balance_usd": 0,
				"in_transit_balance_usd": 0,
				"price_usd": 1,
				"reserve_balance_usd": 0,
				"total_withdrawable_balance": 250
			}
		],
		"cash_usd": "250.00",
		"crypto": [
			{
				"symbol": "USDT",
				"name": "Tether USD",
				"balance": "500.00",
				"breakdown": {
					"available": "450.00",
					"in_transit": "0",
					"perps": "0",
					"pending": "50.00",
					"pending_settlements": [
						{
							"amount": "50.00",
							"date": "2026-08-12"
						}
					],
					"finishing_settlement": "0",
					"reserve": "0"
				},
				"value_usd": 500,
				"price_usd": 1,
				"icon_url": "https://cdn.whop.com/tokens/usdt.png"
			}
		],
		"crypto_usd": "500.00",
		"pending_usd": "0.00",
		"total_usd": "750.00",
		"treasury_pending_usd": "0.00"
	},
	"total_usd": "750.00",
	"balances": [
		{
			"balance": "500.00",
			"breakdown": {
				"available": "450.00",
				"in_transit": "0",
				"perps": "0",
				"pending": "50.00",
				"reserve": "0",
				"pending_settlements": [
					{
						"amount": "50.00",
						"date": "2026-08-12"
					}
				],
				"finishing_settlement": "0"
			},
			"icon_url": "https://cdn.whop.com/tokens/usdt.png",
			"name": "Tether USD",
			"price_usd": 1,
			"symbol": "USDT",
			"value_usd": "500.00"
		},
		{
			"balance": "250.00",
			"breakdown": {
				"available": "240.00",
				"in_transit": "0",
				"perps": "0",
				"pending": "10.00",
				"reserve": "0",
				"pending_settlements": [
					{
						"amount": "10.00",
						"date": "2026-08-12"
					}
				],
				"finishing_settlement": "0"
			},
			"icon_url": null,
			"name": "US Dollar",
			"price_usd": 1,
			"symbol": "USD",
			"value_usd": "250.00"
		}
	],
	"balance_history": {
		"data": [
			{
				"t": 1735689600,
				"v": 500
			},
			{
				"t": 1735776000,
				"v": 750
			}
		],
		"last": 750,
		"min": 500,
		"max": 750
	},
	"bio": "Building communities on Whop.",
	"banner": {
		"url": "https://cdn.whop.com/banner.png"
	},
	"created_at": "2026-06-01T12:00:00Z",
	"earnings_usd": {
		"total": {
			"lifetime": "15075.00",
			"last_30_days": "3425.00",
			"last_7_days": "890.00",
			"last_24_hours": "120.00"
		},
		"personal": {
			"lifetime": "750.00",
			"last_30_days": "250.00",
			"last_7_days": "100.00",
			"last_24_hours": "25.00"
		},
		"owned_accounts": {
			"lifetime": "14325.00",
			"last_30_days": "3175.00",
			"last_7_days": "790.00",
			"last_24_hours": "95.00"
		},
		"partners": {
			"lifetime": "600.00",
			"last_30_days": "200.00",
			"last_7_days": "80.00",
			"last_24_hours": "20.00"
		},
		"first_earned_at": "2026-06-02T15:30:00Z"
	},
	"name": "Marcus Webb",
	"profile_picture": {
		"url": "https://cdn.whop.com/avatar.png"
	},
	"social_links": [
		{
			"website": "instagram",
			"username": "danawhitfield",
			"url": "https://instagram.com/danawhitfield",
			"verified": true
		}
	],
	"username": "marcuswebb",
	"city": "Austin",
	"state": "Texas",
	"country": "US",
	"cards": {
		"kind": "individual",
		"status": "approved",
		"decline_reason": null
	},
	"level": {
		"current": 1,
		"points": 41500,
		"points_by_source": {
			"cards": 40000,
			"ads": 0,
			"trading": 1000,
			"partners": 0,
			"actions": 500
		},
		"resets_at": "2026-11-01T04:00:00.000Z"
	},
	"followers_count": 1284,
	"verified_at": "2026-03-18T15:42:00Z",
	"verification": {
		"business": null,
		"individual": {
			"status": "approved"
		}
	},
	"whop_partner_enabled_at": "2026-06-10T09:00:00Z",
	"whop_partner_verified_at": "2026-07-15T18:30:00Z",
	"whop_partner_onboarded_accounts_count": 12,
	"email": "marcus@shinetime.example",
	"date_of_birth": "1998-04-23",
	"badges": [
		{
			"type": "verified_partner"
		}
	]
}
```


## Related topics

- [Users](https://docs.whop.com/api-reference/legacy/users/user.md)
- [Create API Key](https://docs.whop.com/api-reference/api-keys/create-api-key.md)
- [API Versions](https://docs.whop.com/developer/api/versioning.md)
