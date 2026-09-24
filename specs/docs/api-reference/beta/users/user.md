> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# User

A User represents a person on Whop. Users have a public profile and can buy products, join accounts, and access experiences.

Use the Users API to search for users, retrieve or update profiles, and check whether a user has access to an account, product, or experience.

## Endpoints

| Endpoint                                                                       | Request                                                                                            |
| ------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------- |
| [List Users](/api-reference/beta/users/list-users)                             | <Badge color="blue" size="sm" stroke>GET</Badge> `/users`                                          |
| [Retrieve User](/api-reference/beta/users/retrieve-user)                       | <Badge color="blue" size="sm" stroke>GET</Badge> `/users/{id}`                                     |
| [Check User Access](/api-reference/beta/users/check-user-access)               | <Badge color="blue" size="sm" stroke>GET</Badge> `/users/{id}/access/{resource_id}`                |
| [List Recommended Actions](/api-reference/beta/users/list-recommended-actions) | <Badge color="blue" size="sm" stroke>GET</Badge> `/users/{id}/recommend_actions`                   |
| [List OAuth Grants](/api-reference/beta/users/list-oauth-grants)               | <Badge color="blue" size="sm" stroke>GET</Badge> `/users/me/oauth_grants`                          |
| [List](/api-reference/beta/users/list)                                         | <Badge color="blue" size="sm" stroke>GET</Badge> `/users/me/passkeys`                              |
| [Retrieve](/api-reference/beta/users/retrieve)                                 | <Badge color="blue" size="sm" stroke>GET</Badge> `/users/me/preferences`                           |
| [List Experiences](/api-reference/beta/users/list-experiences)                 | <Badge color="blue" size="sm" stroke>GET</Badge> `/users/me/preferences/notifications/experiences` |
| [List Topics](/api-reference/beta/users/list-topics)                           | <Badge color="blue" size="sm" stroke>GET</Badge> `/users/me/preferences/notifications/topics`      |
| [Authorize an App](/api-reference/beta/users/authorize-an-app)                 | <Badge color="green" size="sm" stroke>POST</Badge> `/users/me/oauth_grants`                        |
| [Register](/api-reference/beta/users/register)                                 | <Badge color="green" size="sm" stroke>POST</Badge> `/users/me/passkeys`                            |
| [Create Challenge](/api-reference/beta/users/create-challenge)                 | <Badge color="green" size="sm" stroke>POST</Badge> `/users/me/passkeys/challenge`                  |
| [Update User](/api-reference/beta/users/update-user)                           | <Badge color="orange" size="sm" stroke>PATCH</Badge> `/users/{id}`                                 |
| [Update](/api-reference/beta/users/update)                                     | <Badge color="orange" size="sm" stroke>PATCH</Badge> `/users/me/preferences`                       |
| [Set](/api-reference/beta/users/set)                                           | <Badge color="orange" size="sm" stroke>PATCH</Badge> `/users/me/preferences/notifications`         |
| [Delete](/api-reference/beta/users/delete)                                     | <Badge color="red" size="sm" stroke>DELETE</Badge> `/users/me/passkeys/{id}`                       |

## Attributes

<Columns cols={2}>
  <Column>
    <ResponseField name="id" type="string" required>
      User ID, prefixed `user_`.
    </ResponseField>

    <ResponseField name="balance" type="object | null" required>
      The user's balance: personal cash + crypto + in-flight treasury deposits, plus account balances for accounts they own. Computed only on the self view (retrieved with the reserved id `me`) for callers with balance-read scope; `null` otherwise, or when `include_balance=false`.

      <Accordion title="Properties" defaultOpen={true}>
        <ResponseField name="businesses" type="object[]" required>
          Account balances for accounts the user owns, highest balance first. Excludes accounts with no balance.

          <Accordion title="Properties" defaultOpen={true}>
            <ResponseField name="id" type="string" required>
              The account ID, which looks like biz\_\*\*\*\*\*\*\*\*\*\*\*\*\*.
            </ResponseField>

            <ResponseField name="balance_usd" type="string" required>
              The account's total balance in USD.
            </ResponseField>

            <ResponseField name="logo_url" type="string | null" required>
              The account's logo URL.
            </ResponseField>

            <ResponseField name="name" type="string | null" required>
              The account's display name.
            </ResponseField>
          </Accordion>
        </ResponseField>

        <ResponseField name="businesses_total_usd" type="string" required>
          Combined USD balance across every account the user owns.
        </ResponseField>

        <ResponseField name="cash" type="object[]" required>
          Per-currency fiat cash balances.

          <Accordion title="Properties" defaultOpen={true}>
            <ResponseField name="balance" type="number" required>
              Available balance in the native currency.
            </ResponseField>

            <ResponseField name="balance_usd" type="number" required>
              Available balance converted to USD.
            </ResponseField>

            <ResponseField name="currency" type="string" required>
              Lowercase ISO currency code, such as `usd` or `eur`.
            </ResponseField>

            <ResponseField name="in_transit_balance_usd" type="number" required>
              Balance moving to the user's own wallet or card, converted to USD.
            </ResponseField>

            <ResponseField name="pending_balance_usd" type="number" required>
              Pending balance converted to USD.
            </ResponseField>

            <ResponseField name="price_usd" type="number | null" required>
              USD price per native currency unit, or `null` when no exchange rate is
              available.
            </ResponseField>

            <ResponseField name="reserve_balance_usd" type="number" required>
              Reserved balance converted to USD.
            </ResponseField>

            <ResponseField name="total_withdrawable_balance" type="number" required>
              Withdrawable amount in the native currency.
            </ResponseField>
          </Accordion>
        </ResponseField>

        <ResponseField name="cash_usd" type="string" required>
          Fiat cash in USD, including pending, in-transit, and reserve.
        </ResponseField>

        <ResponseField name="crypto" type="object[]" required>
          Per-token crypto holdings in the ledger's own wallet.

          <Accordion title="Properties" defaultOpen={true}>
            <ResponseField name="balance" type="string" required>
              Amount held in native token units, as a decimal string.
            </ResponseField>

            <ResponseField name="breakdown" type="object" required>
              Balance split into available, pending, in-transit, and reserve amounts, as native-unit decimal strings. Transfers between the user's own wallet and card are reported in `in_transit` until they arrive.

              <Accordion title="Properties" defaultOpen={true}>
                <ResponseField name="available" type="string" required>
                  Amount you can spend, send, or withdraw now, in native units, as a decimal
                  string.
                </ResponseField>

                <ResponseField name="in_transit" type="string" required>
                  Amount moving between the account's own destinations, such as a treasury sweep
                  to its crypto wallet or a card top-up. In native units, as a decimal string.
                </ResponseField>

                <ResponseField name="pending" type="string" required>
                  Amount from recent payments still settling, in native units, as a decimal
                  string.
                </ResponseField>

                <ResponseField name="pending_settlements" type="object[]" required>
                  When the pending amount is expected to settle, one entry per day, earliest first. Money with no scheduled settlement day, such as a transfer in flight, is left out — so these can sum to less than `pending`, never more.

                  <Accordion title="Properties" defaultOpen={true}>
                    <ResponseField name="amount" type="string" required>
                      Amount expected that day, in native units, as a decimal string.
                    </ResponseField>

                    <ResponseField name="date" type="string" required>
                      The day this money is expected to finish settling, as an ISO 8601 date.
                    </ResponseField>
                  </Accordion>
                </ResponseField>

                <ResponseField name="reserve" type="string" required>
                  Amount held back, in native units, as a decimal string. Retrieve the account's reserves for why it is held and when it unlocks.
                </ResponseField>
              </Accordion>
            </ResponseField>

            <ResponseField name="icon_url" type="string | null" required>
              Token icon URL.
            </ResponseField>

            <ResponseField name="name" type="string | null" required>
              The token's display name.
            </ResponseField>

            <ResponseField name="price_usd" type="number | null" required>
              USD price per token, or `null` when unknown.
            </ResponseField>

            <ResponseField name="symbol" type="string" required>
              Token display symbol, such as `USDT`, `XAUT`, or `cbBTC`.
            </ResponseField>

            <ResponseField name="value_usd" type="number" required>
              Holding USD value.
            </ResponseField>
          </Accordion>
        </ResponseField>

        <ResponseField name="crypto_usd" type="string" required>
          Crypto holdings in USD.
        </ResponseField>

        <ResponseField name="pending_usd" type="string" required>
          Fiat pending and in-transit balances, plus in-flight treasury deposits, in
          USD.
        </ResponseField>

        <ResponseField name="total_usd" type="string" required>
          The user's personal balance in USD: cash (available + pending + in-transit +
          reserve) + crypto + in-flight treasury deposits. Excludes account balances
          (see businesses\_total\_usd).
        </ResponseField>

        <ResponseField name="treasury_pending_usd" type="string" required>
          Balance-to-wallet USDT0 payouts still in flight, in USD.
        </ResponseField>
      </Accordion>
    </ResponseField>

    <ResponseField name="balance_history" type="object | null" required>
      The user's cumulative wallet balance over time (USD `\{ t, v }` points plus last/min/max), for the balance chart. Opt in with `include_balance_history=true` when retrieving yourself with the reserved id `me`; populated only for callers with balance-read scope and `null` otherwise. A user with no wallet activity returns an empty series.

      <Accordion title="Properties" defaultOpen={true}>
        <ResponseField name="data" type="object[]" required>
          Cumulative balance points over the requested window, oldest first.

          <Accordion title="Properties" defaultOpen={true}>
            <ResponseField name="t" type="integer" required>
              Point timestamp, in Unix seconds.
            </ResponseField>

            <ResponseField name="v" type="number" required>
              Cumulative wallet balance at this point, in USD.
            </ResponseField>
          </Accordion>
        </ResponseField>

        <ResponseField name="last" type="number" required>
          Value of the most recent point, in USD.
        </ResponseField>

        <ResponseField name="max" type="number" required>
          Maximum value across the window, in USD.
        </ResponseField>

        <ResponseField name="min" type="number" required>
          Minimum value across the window, in USD.
        </ResponseField>
      </Accordion>
    </ResponseField>

    <ResponseField name="banner" type="object | null" required>
      The user's profile banner wrapper. `null` when the user has no banner.

      <Accordion title="Properties" defaultOpen={true}>
        <ResponseField name="url" type="string" required>
          Profile banner image URL.
        </ResponseField>
      </Accordion>
    </ResponseField>

    <ResponseField name="bio" type="string | null" required>
      The user's biography
    </ResponseField>

    <ResponseField name="created_at" type="string" required>
      When the user was created, as an ISO 8601 timestamp
    </ResponseField>

    <ResponseField name="earnings_usd" type="object | null" required>
      The user's gross USD income over time, including a Partner commission breakdown. Populated only on single-user self reads for callers with balance-read scope; `null` otherwise.

      <Accordion title="Properties" defaultOpen={true}>
        <ResponseField name="first_earned_at" type="string | null" required>
          The first time the user earned gross income, as an ISO 8601 timestamp.
        </ResponseField>

        <ResponseField name="owned_accounts" type="object" required>
          Gross income from accounts the user owns or is owner-authorized on.

          <Accordion title="Properties" defaultOpen={true}>
            <ResponseField name="last_24_hours" type="string" required>
              Gross income in USD over the last 24 hours.
            </ResponseField>

            <ResponseField name="last_30_days" type="string" required>
              Gross income in USD over the last 30 days.
            </ResponseField>

            <ResponseField name="last_7_days" type="string" required>
              Gross income in USD over the last 7 days.
            </ResponseField>

            <ResponseField name="lifetime" type="string" required>
              All-time gross income in USD.
            </ResponseField>
          </Accordion>
        </ResponseField>

        <ResponseField name="partners" type="object" required>
          Partner commissions posted to the user's wallet. Pending Partner payouts are excluded until they post; later reversals do not reduce gross income.

          <Accordion title="Properties" defaultOpen={true}>
            <ResponseField name="last_24_hours" type="string" required>
              Gross income in USD over the last 24 hours.
            </ResponseField>

            <ResponseField name="last_30_days" type="string" required>
              Gross income in USD over the last 30 days.
            </ResponseField>

            <ResponseField name="last_7_days" type="string" required>
              Gross income in USD over the last 7 days.
            </ResponseField>

            <ResponseField name="lifetime" type="string" required>
              All-time gross income in USD.
            </ResponseField>
          </Accordion>
        </ResponseField>

        <ResponseField name="personal" type="object" required>
          Gross income from the user's personal wallet.

          <Accordion title="Properties" defaultOpen={true}>
            <ResponseField name="last_24_hours" type="string" required>
              Gross income in USD over the last 24 hours.
            </ResponseField>

            <ResponseField name="last_30_days" type="string" required>
              Gross income in USD over the last 30 days.
            </ResponseField>

            <ResponseField name="last_7_days" type="string" required>
              Gross income in USD over the last 7 days.
            </ResponseField>

            <ResponseField name="lifetime" type="string" required>
              All-time gross income in USD.
            </ResponseField>
          </Accordion>
        </ResponseField>

        <ResponseField name="total" type="object" required>
          Gross income from the user's personal wallet plus accounts they own or are owner-authorized on.

          <Accordion title="Properties" defaultOpen={true}>
            <ResponseField name="last_24_hours" type="string" required>
              Gross income in USD over the last 24 hours.
            </ResponseField>

            <ResponseField name="last_30_days" type="string" required>
              Gross income in USD over the last 30 days.
            </ResponseField>

            <ResponseField name="last_7_days" type="string" required>
              Gross income in USD over the last 7 days.
            </ResponseField>

            <ResponseField name="lifetime" type="string" required>
              All-time gross income in USD.
            </ResponseField>
          </Accordion>
        </ResponseField>
      </Accordion>
    </ResponseField>

    <ResponseField name="email" type="string | null" required>
      The user's email address. Populated only on the self view (retrieved with the
      reserved id `me`) for callers with email-read scope; `null` otherwise, or
      while the account has no confirmed email yet.
    </ResponseField>

    <ResponseField name="name" type="string | null" required>
      The user's display name
    </ResponseField>

    <ResponseField name="profile_picture" type="object" required>
      Avatar wrapper; its `url` is always present, using a generated placeholder when the user set no picture.

      <Accordion title="Properties" defaultOpen={true}>
        <ResponseField name="url" type="string" required>
          Avatar image URL. Always present — a generated placeholder when the user set no picture.
        </ResponseField>
      </Accordion>
    </ResponseField>

    <ResponseField name="social_accounts" type="object[]" required>
      Social accounts linked to the user (Discord, X/Twitter, Telegram), oldest first. Reading your own profile returns every linked account; other profiles only include what is public on Whop (the primary Discord and the X account). Empty when none are linked.

      <Accordion title="Properties" defaultOpen={true}>
        <ResponseField name="id" type="string" required>
          Unique identifier for the social account.
        </ResponseField>

        <ResponseField name="error" type="string | null" required>
          Why this social account currently can't be used for advertising — a failed
          share or a Meta-side restriction. Null when the account is healthy.
        </ResponseField>

        <ResponseField name="external_id" type="string | null" required>
          The platform-specific ID for this social account.
        </ResponseField>

        <ResponseField name="name" type="string | null" required>
          The display name of the social account on the platform.
        </ResponseField>

        <ResponseField name="parent_social_account" type="object | null" required>
          The social account this one belongs to on the platform, such as the Facebook page that owns an Instagram account. Null when the social account stands on its own.

          <Accordion title="Properties" defaultOpen={true}>
            <ResponseField name="id" type="string" required>
              Social account ID, prefixed `sacc_`.
            </ResponseField>

            <ResponseField name="external_id" type="string | null" required>
              The platform-specific ID for the parent social account.
            </ResponseField>

            <ResponseField name="name" type="string | null" required>
              The display name of the parent social account on the platform.
            </ResponseField>

            <ResponseField name="platform" type="string" required>
              The platform the parent social account exists on.

              Available options: `x`, `instagram`, `youtube`, `tiktok`, `facebook`, `discord`, `telegram`, `linkedin`
            </ResponseField>

            <ResponseField name="profile_picture_url" type="string | null" required>
              The URL where the profile picture of the parent social account can be
              accessed.
            </ResponseField>

            <ResponseField name="username" type="string | null" required>
              The username of the parent social account on the platform.
            </ResponseField>

            <ResponseField name="verified" type="boolean" required>
              Whether the parent social account is verified on the platform.
            </ResponseField>
          </Accordion>
        </ResponseField>

        <ResponseField name="platform" type="string" required>
          The platform the social account exists on.

          Available options: `x`, `instagram`, `youtube`, `tiktok`, `facebook`, `discord`, `telegram`, `linkedin`
        </ResponseField>

        <ResponseField name="profile_picture_url" type="string | null" required>
          The URL where the profile picture of the social account can be accessed.
        </ResponseField>

        <ResponseField name="scopes" type="string[]" required>
          Capabilities Whop retains specific to this social account. For example, Whop
          may request the ability to run advertisements that use this social account's
          identity, reflected by the presence of `advertise` in this value.
        </ResponseField>

        <ResponseField name="url" type="string | null" required>
          The URL where the social account can be accessed on the platform. Null while a
          Whop-owned account is still being provisioned.
        </ResponseField>

        <ResponseField name="username" type="string | null" required>
          The username of the social account on the platform. Null while a Whop-owned
          account is still being provisioned.
        </ResponseField>

        <ResponseField name="verified" type="boolean" required>
          Whether the social account is verified on the platform.
        </ResponseField>
      </Accordion>
    </ResponseField>

    <ResponseField name="staff" type="object | null" required>
      Whop staff access flags. Populated only on the self view (retrieved with the reserved id `me`) for callers with staff-read scope; `null` there for every user who is not Whop staff, and always `null` elsewhere.

      <Accordion title="Properties" defaultOpen={true}>
        <ResponseField name="admin" type="boolean" required>
          Whether the user holds the admin staff role with a valid second factor.
        </ResponseField>

        <ResponseField name="investigation_access" type="boolean" required>
          Whether the user can open Whop-internal investigation tooling right now: a
          qualifying staff role plus their investigation toggle switched on.
        </ResponseField>

        <ResponseField name="manager" type="boolean" required>
          Whether the user holds the manager staff role with a valid second factor.
        </ResponseField>

        <ResponseField name="support" type="boolean" required>
          Whether the user holds the support staff role with a valid second factor.
        </ResponseField>
      </Accordion>
    </ResponseField>

    <ResponseField name="trading" type="trading_account | null" required>
      Live trading state. Opt in with `include_trading=true` when retrieving `me`; `null` otherwise, without trading permission, or without an Ethereum wallet. Provider failures return an error, not a zero balance.

      <Accordion title="Properties" defaultOpen={true}>
        <ResponseField name="id" type="string" required>
          The Whop wallet ID backing this trading account, prefixed `cwal_`.
        </ResponseField>

        <ResponseField name="account_id" type="string | null" required>
          The account that owns this trading account, prefixed `biz_`. `null` when a
          user owns it.
        </ResponseField>

        <ResponseField name="hyperliquid" type="object | null" required>
          Hyperliquid-specific state. Present when `provider` is `hyperliquid`, otherwise `null`.

          <Accordion title="Properties" defaultOpen={true}>
            <ResponseField name="address" type="string" required>
              Lowercase wallet address that holds the Hyperliquid account.
            </ResponseField>

            <ResponseField name="builder_fee_bps" type="string | null" required>
              Builder fee Whop charges on orders, in basis points as a decimal string, or
              `null` when no fee is configured.
            </ResponseField>

            <ResponseField name="margin_summary" type="object" required>
              Account value, margin, and withdrawable balance, all in USD.

              <Accordion title="Properties" defaultOpen={true}>
                <ResponseField name="account_value" type="object" required>
                  Total account value in USD, including unrealized profit and loss.

                  <Accordion title="Properties" defaultOpen={true}>
                    <ResponseField name="amount" type="string" required>
                      The amount in major units, as an exact decimal string — `"10.00"` is ten
                      dollars. A string so no float rounds it in transit.
                    </ResponseField>

                    <ResponseField name="currency" type="string" required>
                      Three-letter ISO 4217 currency code, lowercase.
                    </ResponseField>

                    <ResponseField name="decimals" type="integer" required>
                      How many decimal places the amount CARRIES — the precision the charge itself
                      runs at.
                    </ResponseField>

                    <ResponseField name="display_decimals" type="integer" required>
                      How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
                    </ResponseField>
                  </Accordion>
                </ResponseField>

                <ResponseField name="total_margin_used" type="object" required>
                  Margin allocated across open positions, in USD.

                  <Accordion title="Properties" defaultOpen={true}>
                    <ResponseField name="amount" type="string" required>
                      The amount in major units, as an exact decimal string — `"10.00"` is ten
                      dollars. A string so no float rounds it in transit.
                    </ResponseField>

                    <ResponseField name="currency" type="string" required>
                      Three-letter ISO 4217 currency code, lowercase.
                    </ResponseField>

                    <ResponseField name="decimals" type="integer" required>
                      How many decimal places the amount CARRIES — the precision the charge itself
                      runs at.
                    </ResponseField>

                    <ResponseField name="display_decimals" type="integer" required>
                      How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
                    </ResponseField>
                  </Accordion>
                </ResponseField>

                <ResponseField name="total_position_notional" type="object" required>
                  Combined notional value of open positions, in USD.

                  <Accordion title="Properties" defaultOpen={true}>
                    <ResponseField name="amount" type="string" required>
                      The amount in major units, as an exact decimal string — `"10.00"` is ten
                      dollars. A string so no float rounds it in transit.
                    </ResponseField>

                    <ResponseField name="currency" type="string" required>
                      Three-letter ISO 4217 currency code, lowercase.
                    </ResponseField>

                    <ResponseField name="decimals" type="integer" required>
                      How many decimal places the amount CARRIES — the precision the charge itself
                      runs at.
                    </ResponseField>

                    <ResponseField name="display_decimals" type="integer" required>
                      How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
                    </ResponseField>
                  </Accordion>
                </ResponseField>

                <ResponseField name="total_raw_usd" type="object" required>
                  Raw USD balance as Hyperliquid reports it, excluding position value.

                  <Accordion title="Properties" defaultOpen={true}>
                    <ResponseField name="amount" type="string" required>
                      The amount in major units, as an exact decimal string — `"10.00"` is ten
                      dollars. A string so no float rounds it in transit.
                    </ResponseField>

                    <ResponseField name="currency" type="string" required>
                      Three-letter ISO 4217 currency code, lowercase.
                    </ResponseField>

                    <ResponseField name="decimals" type="integer" required>
                      How many decimal places the amount CARRIES — the precision the charge itself
                      runs at.
                    </ResponseField>

                    <ResponseField name="display_decimals" type="integer" required>
                      How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
                    </ResponseField>
                  </Accordion>
                </ResponseField>

                <ResponseField name="withdrawable" type="object" required>
                  USD that can be withdrawn now without closing positions.

                  <Accordion title="Properties" defaultOpen={true}>
                    <ResponseField name="amount" type="string" required>
                      The amount in major units, as an exact decimal string — `"10.00"` is ten
                      dollars. A string so no float rounds it in transit.
                    </ResponseField>

                    <ResponseField name="currency" type="string" required>
                      Three-letter ISO 4217 currency code, lowercase.
                    </ResponseField>

                    <ResponseField name="decimals" type="integer" required>
                      How many decimal places the amount CARRIES — the precision the charge itself
                      runs at.
                    </ResponseField>

                    <ResponseField name="display_decimals" type="integer" required>
                      How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
                    </ResponseField>
                  </Accordion>
                </ResponseField>
              </Accordion>
            </ResponseField>

            <ResponseField name="websocket_subscriptions" type="object[]" required>
              Subscriptions for live account updates. Send each `message` unchanged after connecting to `websocket_url`.

              <Accordion title="Properties" defaultOpen={true}>
                <ResponseField name="channel" type="string" required>
                  The live update stream this subscription opens.

                  Available options: `clearinghouse_state`, `open_orders`, `order_updates`, `user_fills`, `user_events`
                </ResponseField>

                <ResponseField name="message" type="string" required>
                  JSON subscription message to send unchanged over the Hyperliquid WebSocket.
                </ResponseField>
              </Accordion>
            </ResponseField>

            <ResponseField name="websocket_url" type="string" required>
              Hyperliquid WebSocket URL to connect to directly for live updates.
            </ResponseField>
          </Accordion>
        </ResponseField>

        <ResponseField name="object" type="string" required />

        <ResponseField name="open_orders" type="trading_order[]" required>
          Resting orders that have not filled or been canceled.

          <Accordion title="Properties" defaultOpen={true}>
            <ResponseField name="id" type="string" required>
              Trading order ID, prefixed `trdord_`.
            </ResponseField>

            <ResponseField name="client_order_id" type="string | null" required>
              Client order ID, prefixed `trdcloid_`, or `null` when the order was placed
              without one.
            </ResponseField>

            <ResponseField name="created_at" type="string | null" required>
              When the order was placed, as an ISO 8601 timestamp, or `null` when the
              provider omits it.
            </ResponseField>

            <ResponseField name="hyperliquid" type="object | null" required>
              Hyperliquid-specific order details. Present on Hyperliquid orders, otherwise `null`.

              <Accordion title="Properties" defaultOpen={true}>
                <ResponseField name="reduce_only" type="boolean | null" required>
                  Whether the order can only reduce an existing position, or `null` when
                  Hyperliquid omits it.
                </ResponseField>

                <ResponseField name="trigger_price" type="object | null" required>
                  Trigger price in USD for take-profit and stop-loss orders, or `null` for orders without a trigger.

                  <Accordion title="Properties" defaultOpen={true}>
                    <ResponseField name="amount" type="string" required>
                      The amount in major units, as an exact decimal string — `"10.00"` is ten
                      dollars. A string so no float rounds it in transit.
                    </ResponseField>

                    <ResponseField name="currency" type="string" required>
                      Three-letter ISO 4217 currency code, lowercase.
                    </ResponseField>

                    <ResponseField name="decimals" type="integer" required>
                      How many decimal places the amount CARRIES — the precision the charge itself
                      runs at.
                    </ResponseField>

                    <ResponseField name="display_decimals" type="integer" required>
                      How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
                    </ResponseField>
                  </Accordion>
                </ResponseField>
              </Accordion>
            </ResponseField>

            <ResponseField name="market" type="string" required>
              Market symbol on the provider, such as `ETH`.
            </ResponseField>

            <ResponseField name="object" type="string" required />

            <ResponseField name="order_type" type="string" required>
              Available options: `limit`, `market`, `take_profit`, `stop_loss`
            </ResponseField>

            <ResponseField name="original_size" type="string | null" required>
              Size when the order was placed, as a decimal string, or `null` when the
              provider omits it.
            </ResponseField>

            <ResponseField name="price" type="object" required>
              Limit price in USD.

              <Accordion title="Properties" defaultOpen={true}>
                <ResponseField name="amount" type="string" required>
                  The amount in major units, as an exact decimal string — `"10.00"` is ten
                  dollars. A string so no float rounds it in transit.
                </ResponseField>

                <ResponseField name="currency" type="string" required>
                  Three-letter ISO 4217 currency code, lowercase.
                </ResponseField>

                <ResponseField name="decimals" type="integer" required>
                  How many decimal places the amount CARRIES — the precision the charge itself
                  runs at.
                </ResponseField>

                <ResponseField name="display_decimals" type="integer" required>
                  How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
                </ResponseField>
              </Accordion>
            </ResponseField>

            <ResponseField name="provider_order_id" type="string | null" required>
              The provider's own order ID, as a string.
            </ResponseField>

            <ResponseField name="side" type="string" required>
              Available options: `buy`, `sell`
            </ResponseField>

            <ResponseField name="size" type="string" required>
              Remaining order size as a decimal string.
            </ResponseField>

            <ResponseField name="status" type="string" required>
              Available options: `open`, `filled`, `canceled`, `triggered`, `rejected`
            </ResponseField>

            <ResponseField name="status_updated_at" type="string | null" required>
              When the status last changed, as an ISO 8601 timestamp, or `null` when the
              provider omits it.
            </ResponseField>

            <ResponseField name="time_in_force" type="string | null" required>
              How long the order stays active. `null` when the provider omits it or reports a policy outside the supported values.

              Available options: `add_liquidity_only`, `good_til_canceled`, `immediate_or_cancel`
            </ResponseField>
          </Accordion>
        </ResponseField>

        <ResponseField name="positions" type="trading_position[]" required>
          Open positions. Positions with zero size are omitted.

          <Accordion title="Properties" defaultOpen={true}>
            <ResponseField name="id" type="string" required>
              Trading position ID, prefixed `trdpos_`. Stable for a market within one
              trading account.
            </ResponseField>

            <ResponseField name="entry_price" type="object | null" required>
              Average entry price in USD, or `null` when the provider omits it.

              <Accordion title="Properties" defaultOpen={true}>
                <ResponseField name="amount" type="string" required>
                  The amount in major units, as an exact decimal string — `"10.00"` is ten
                  dollars. A string so no float rounds it in transit.
                </ResponseField>

                <ResponseField name="currency" type="string" required>
                  Three-letter ISO 4217 currency code, lowercase.
                </ResponseField>

                <ResponseField name="decimals" type="integer" required>
                  How many decimal places the amount CARRIES — the precision the charge itself
                  runs at.
                </ResponseField>

                <ResponseField name="display_decimals" type="integer" required>
                  How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
                </ResponseField>
              </Accordion>
            </ResponseField>

            <ResponseField name="hyperliquid" type="object | null" required>
              Hyperliquid perpetual details. Present on Hyperliquid positions, otherwise `null`.

              <Accordion title="Properties" defaultOpen={true}>
                <ResponseField name="cumulative_funding" type="object" required>
                  Funding paid on the position over several windows, in USD.

                  <Accordion title="Properties" defaultOpen={true}>
                    <ResponseField name="all_time" type="object | null" required>
                      Funding paid on this market across the account's history, in USD, or `null` when unavailable.

                      <Accordion title="Properties" defaultOpen={true}>
                        <ResponseField name="amount" type="string" required>
                          The amount in major units, as an exact decimal string — `"10.00"` is ten
                          dollars. A string so no float rounds it in transit.
                        </ResponseField>

                        <ResponseField name="currency" type="string" required>
                          Three-letter ISO 4217 currency code, lowercase.
                        </ResponseField>

                        <ResponseField name="decimals" type="integer" required>
                          How many decimal places the amount CARRIES — the precision the charge itself
                          runs at.
                        </ResponseField>

                        <ResponseField name="display_decimals" type="integer" required>
                          How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
                        </ResponseField>
                      </Accordion>
                    </ResponseField>

                    <ResponseField name="since_change" type="object | null" required>
                      Funding paid since the position size last changed, in USD, or `null` when unavailable.

                      <Accordion title="Properties" defaultOpen={true}>
                        <ResponseField name="amount" type="string" required>
                          The amount in major units, as an exact decimal string — `"10.00"` is ten
                          dollars. A string so no float rounds it in transit.
                        </ResponseField>

                        <ResponseField name="currency" type="string" required>
                          Three-letter ISO 4217 currency code, lowercase.
                        </ResponseField>

                        <ResponseField name="decimals" type="integer" required>
                          How many decimal places the amount CARRIES — the precision the charge itself
                          runs at.
                        </ResponseField>

                        <ResponseField name="display_decimals" type="integer" required>
                          How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
                        </ResponseField>
                      </Accordion>
                    </ResponseField>

                    <ResponseField name="since_open" type="object | null" required>
                      Funding paid since the position opened, in USD, or `null` when unavailable.

                      <Accordion title="Properties" defaultOpen={true}>
                        <ResponseField name="amount" type="string" required>
                          The amount in major units, as an exact decimal string — `"10.00"` is ten
                          dollars. A string so no float rounds it in transit.
                        </ResponseField>

                        <ResponseField name="currency" type="string" required>
                          Three-letter ISO 4217 currency code, lowercase.
                        </ResponseField>

                        <ResponseField name="decimals" type="integer" required>
                          How many decimal places the amount CARRIES — the precision the charge itself
                          runs at.
                        </ResponseField>

                        <ResponseField name="display_decimals" type="integer" required>
                          How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
                        </ResponseField>
                      </Accordion>
                    </ResponseField>
                  </Accordion>
                </ResponseField>

                <ResponseField name="leverage" type="object" required>
                  Margin mode and multiplier for the position.

                  <Accordion title="Properties" defaultOpen={true}>
                    <ResponseField name="type" type="string" required>
                      `cross` shares margin across positions; `isolated` limits margin to this position.

                      Available options: `cross`, `isolated`
                    </ResponseField>

                    <ResponseField name="value" type="integer" required>
                      Multiplier applied to the position's margin, such as `10` for 10x.
                    </ResponseField>
                  </Accordion>
                </ResponseField>

                <ResponseField name="liquidation_price" type="object | null" required>
                  Estimated liquidation price in USD, or `null` when Hyperliquid reports none.

                  <Accordion title="Properties" defaultOpen={true}>
                    <ResponseField name="amount" type="string" required>
                      The amount in major units, as an exact decimal string — `"10.00"` is ten
                      dollars. A string so no float rounds it in transit.
                    </ResponseField>

                    <ResponseField name="currency" type="string" required>
                      Three-letter ISO 4217 currency code, lowercase.
                    </ResponseField>

                    <ResponseField name="decimals" type="integer" required>
                      How many decimal places the amount CARRIES — the precision the charge itself
                      runs at.
                    </ResponseField>

                    <ResponseField name="display_decimals" type="integer" required>
                      How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
                    </ResponseField>
                  </Accordion>
                </ResponseField>

                <ResponseField name="margin_used" type="object" required>
                  Margin allocated to the position, in USD.

                  <Accordion title="Properties" defaultOpen={true}>
                    <ResponseField name="amount" type="string" required>
                      The amount in major units, as an exact decimal string — `"10.00"` is ten
                      dollars. A string so no float rounds it in transit.
                    </ResponseField>

                    <ResponseField name="currency" type="string" required>
                      Three-letter ISO 4217 currency code, lowercase.
                    </ResponseField>

                    <ResponseField name="decimals" type="integer" required>
                      How many decimal places the amount CARRIES — the precision the charge itself
                      runs at.
                    </ResponseField>

                    <ResponseField name="display_decimals" type="integer" required>
                      How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
                    </ResponseField>
                  </Accordion>
                </ResponseField>

                <ResponseField name="return_on_equity" type="string" required>
                  Return on equity as a decimal ratio string, such as `0.1` for 10%.
                </ResponseField>
              </Accordion>
            </ResponseField>

            <ResponseField name="market" type="string" required>
              Market symbol on the provider, such as `ETH`.
            </ResponseField>

            <ResponseField name="object" type="string" required />

            <ResponseField name="position_value" type="object" required>
              Current position value in USD.

              <Accordion title="Properties" defaultOpen={true}>
                <ResponseField name="amount" type="string" required>
                  The amount in major units, as an exact decimal string — `"10.00"` is ten
                  dollars. A string so no float rounds it in transit.
                </ResponseField>

                <ResponseField name="currency" type="string" required>
                  Three-letter ISO 4217 currency code, lowercase.
                </ResponseField>

                <ResponseField name="decimals" type="integer" required>
                  How many decimal places the amount CARRIES — the precision the charge itself
                  runs at.
                </ResponseField>

                <ResponseField name="display_decimals" type="integer" required>
                  How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
                </ResponseField>
              </Accordion>
            </ResponseField>

            <ResponseField name="side" type="string" required>
              Available options: `long`, `short`
            </ResponseField>

            <ResponseField name="size" type="string" required>
              Absolute position size as a decimal string.
            </ResponseField>

            <ResponseField name="unrealized_pnl" type="object" required>
              Unrealized profit or loss in USD. Negative for a loss.

              <Accordion title="Properties" defaultOpen={true}>
                <ResponseField name="amount" type="string" required>
                  The amount in major units, as an exact decimal string — `"10.00"` is ten
                  dollars. A string so no float rounds it in transit.
                </ResponseField>

                <ResponseField name="currency" type="string" required>
                  Three-letter ISO 4217 currency code, lowercase.
                </ResponseField>

                <ResponseField name="decimals" type="integer" required>
                  How many decimal places the amount CARRIES — the precision the charge itself
                  runs at.
                </ResponseField>

                <ResponseField name="display_decimals" type="integer" required>
                  How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
                </ResponseField>
              </Accordion>
            </ResponseField>
          </Accordion>
        </ResponseField>

        <ResponseField name="provider" type="string" required>
          Trading venue that holds the positions and orders.

          Available options: `hyperliquid`
        </ResponseField>

        <ResponseField name="user_id" type="string | null" required>
          The user who owns this trading account, prefixed `user_`. `null` when an account owns it.
        </ResponseField>
      </Accordion>
    </ResponseField>

    <ResponseField name="username" type="string" required>
      The user's unique username
    </ResponseField>

    <ResponseField name="verification" type="object" required>
      Identity verification status for the user's `individual` (KYC) and `business`
      (KYB) profiles. Each is `null` until created, otherwise a `status` of
      `not_started`, `pending`, `approved`, or `rejected`.
    </ResponseField>

    <ResponseField name="whop_partner_enabled_at" type="string | null" required>
      When the user became an enrolled Whop Partner, as an ISO 8601 timestamp.
      `null` if never enrolled.
    </ResponseField>
  </Column>

  <Column>
    <div className="api-resource-sticky-example">
      ```json User theme={null}
      {
      	"id": "user_alex123",
      	"trading": {
      		"id": "cwal_xxxxxxxxxxxxx",
      		"object": "trading_account",
      		"account_id": null,
      		"user_id": "user_alex123",
      		"provider": "hyperliquid",
      		"positions": [
      			{
      				"id": "trdpos_9f2c4e1a7b3d5f60",
      				"object": "trading_position",
      				"market": "ETH",
      				"side": "long",
      				"size": "0.5",
      				"entry_price": {
      					"currency": "usd",
      					"amount": "3000.0",
      					"decimals": 2,
      					"display_decimals": 2
      				},
      				"position_value": {
      					"currency": "usd",
      					"amount": "1525.0",
      					"decimals": 2,
      					"display_decimals": 2
      				},
      				"unrealized_pnl": {
      					"currency": "usd",
      					"amount": "25.0",
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
      						"amount": "2750.50",
      						"decimals": 2,
      						"display_decimals": 2
      					},
      					"margin_used": {
      						"currency": "usd",
      						"amount": "152.50",
      						"decimals": 2,
      						"display_decimals": 2
      					},
      					"return_on_equity": "0.1639",
      					"cumulative_funding": {
      						"all_time": {
      							"currency": "usd",
      							"amount": "1.25",
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
      							"amount": "0.00",
      							"decimals": 2,
      							"display_decimals": 2
      						}
      					}
      				}
      			}
      		],
      		"open_orders": [
      			{
      				"id": "trdord_123456789",
      				"object": "trading_order",
      				"client_order_id": null,
      				"provider_order_id": "123456789",
      				"market": "ETH",
      				"side": "buy",
      				"size": "0.25",
      				"original_size": "0.25",
      				"price": {
      					"currency": "usd",
      					"amount": "2900.0",
      					"decimals": 2,
      					"display_decimals": 2
      				},
      				"order_type": "limit",
      				"time_in_force": "good_til_canceled",
      				"status": "open",
      				"created_at": "2026-09-23T15:04:05.000Z",
      				"status_updated_at": null,
      				"hyperliquid": {
      					"reduce_only": false,
      					"trigger_price": null
      				}
      			}
      		],
      		"hyperliquid": {
      			"address": "0x1234567890abcdef1234567890abcdef12345678",
      			"builder_fee_bps": null,
      			"margin_summary": {
      				"account_value": {
      					"currency": "usd",
      					"amount": "1025.00",
      					"decimals": 2,
      					"display_decimals": 2
      				},
      				"total_position_notional": {
      					"currency": "usd",
      					"amount": "1525.00",
      					"decimals": 2,
      					"display_decimals": 2
      				},
      				"total_raw_usd": {
      					"currency": "usd",
      					"amount": "-500.00",
      					"decimals": 2,
      					"display_decimals": 2
      				},
      				"total_margin_used": {
      					"currency": "usd",
      					"amount": "152.50",
      					"decimals": 2,
      					"display_decimals": 2
      				},
      				"withdrawable": {
      					"currency": "usd",
      					"amount": "872.50",
      					"decimals": 2,
      					"display_decimals": 2
      				}
      			},
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
      			]
      		}
      	},
      	"balance": {
      		"businesses": [
      			{
      				"id": "biz_acme123",
      				"name": "Acme Inc",
      				"logo_url": "https://cdn.whop.com/logos/acme.png",
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
      					"pending": "50.00",
      					"pending_settlements": [
      						{
      							"amount": "50.00",
      							"date": "2026-08-12"
      						}
      					],
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
      	"social_accounts": [
      		{
      			"id": "discord_123456789012345678",
      			"platform": "discord",
      			"username": "alex",
      			"name": "alex",
      			"url": null,
      			"profile_picture_url": "https://cdn.discordapp.com/avatars/123456789012345678/abcdef.png",
      			"verified": false,
      			"external_id": "123456789012345678",
      			"scopes": [],
      			"error": null,
      			"parent_social_account": null
      		},
      		{
      			"id": "telegram_987654321",
      			"platform": "telegram",
      			"username": "alex_tg",
      			"name": "alex_tg",
      			"url": null,
      			"profile_picture_url": null,
      			"verified": false,
      			"external_id": "987654321",
      			"scopes": [],
      			"error": null,
      			"parent_social_account": null
      		},
      		{
      			"id": "x_111222333",
      			"platform": "x",
      			"username": "alex",
      			"name": "alex",
      			"url": null,
      			"profile_picture_url": "https://pbs.twimg.com/profile_images/alex.jpg",
      			"verified": true,
      			"external_id": "111222333",
      			"scopes": [],
      			"error": null,
      			"parent_social_account": null
      		}
      	],
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
      	"name": "Alex Rivera",
      	"profile_picture": {
      		"url": "https://cdn.whop.com/avatar.png"
      	},
      	"username": "alex",
      	"verification": {
      		"business": null,
      		"individual": {
      			"status": "approved"
      		}
      	},
      	"whop_partner_enabled_at": "2026-06-10T09:00:00Z",
      	"email": "jack@whop.com",
      	"staff": null
      }
      ```
    </div>
  </Column>
</Columns>
