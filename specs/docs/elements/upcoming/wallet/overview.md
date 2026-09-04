> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Wallet

> Drives an account's money surfaces. `required-actions` is the outstanding-action bar from Whop's balance dashboard. `actions` renders the Deposit, Accept, Send, and Convert controls: Deposit, Send, and Convert open their Wallet overlays, while Accept opens Whop's checkout-link creator for a business account or company creation for a personal account. `deposit` returns live funding rails; `convert` swaps USD with Gold or Bitcoin and exposes FX currencies only to gated first-party users; `send` moves money to a recipient or creates a public claim link; `withdraw` lists payout methods and creates the payout when the host does not already drive that API; `balances` holds two faces — the holdings list, and the balance block drawing value over a window; `cards` is the compact issued-card list; `cardsTable` renders the full sortable card roster; `cardsChart` plots card spend; `whopCard` renders one revealable card; `activity` lists ledger movements; `activityDetail` renders a prefetched movement, or retrieves one by activity ID or card transaction ID, in a standalone drawer; and `verification` is the identity-only nudge. Every data-backed surface can use the viewer's session when no token is provided.

## Playground

Assemble the elements with example data. Drive the controls, add and arrange elements, and watch events fire live:

<div data-whop-demo-shell style={{ position: "relative", minHeight: "480px", transition: "min-height 200ms ease" }}>
  <div data-whop-demo-skeleton style={{ position: "absolute", inset: "0", borderRadius: "12px", background: "rgba(140, 140, 140, 0.12)", pointerEvents: "none", transition: "opacity 200ms ease" }} />

  <div data-whop-demo-native="playground:wallet" data-whop-elements-version="" style={{ position: "relative" }} />
</div>

<div data-whop-usage="wallet/playground">
  <CodeGroup>
    ```tsx React theme={null}
    import { WhopElements, Wallet } from "@whop/elements-react";
    import { loadWhop } from "@whop/elements";

    function Example() {
      return (
        <WhopElements elements={loadWhop()}>
          <Wallet /* options */>
            {/* mount elements here */}
          </Wallet>
        </WhopElements>
      );
    }
    ```

    ```html JavaScript theme={null}
    <script src="https://js.whop.cloud/elements/amber/elements.js" data-whop-elements></script>
    <script type="module">
      const wallet = window.WhopElements().wallet.create({ /* options */ });
    </script>
    ```
  </CodeGroup>
</div>

## Options

Pass these to `whop.wallet.create({ … })`, or as props on `<Wallet>` in React.

<ResponseField name="currency" type="string">
  Three-letter ISO 4217 currency code for amount fields. An unknown code falls back to `usd`. Defaults to `"usd"`.
</ResponseField>

<ResponseField name="accessToken" type="string">
  Optional scoped token projected to data-backed wallet children. Without it, reads use the viewer session.
</ResponseField>

<ResponseField name="openHoldingOnSelect" type="boolean">
  Show a holding's balance page in place of the balances chart and list when a row is clicked. Off by default so a host that routes on `balanceSelected` is not covered by a second screen. Defaults to `false`.
</ResponseField>

<ResponseField name="accountId" type="string" required>
  Account or user ID whose money these surfaces read. Account IDs are prefixed `biz_`; user IDs are prefixed `user_` and can read only the viewer's own balance.
</ResponseField>

<ResponseField name="appearance" type="Appearance">
  Visual customization for this group's elements. Overrides the global `WhopElements({ appearance })`. Change it live with `update({ appearance })`.
</ResponseField>

<ResponseField name="locale" type="WhopElementsLocale">
  Locale for this group's element UI text. Set it to one of the app's built locales to override the global configuration. Any other value falls back to the default locale.
</ResponseField>

## Events

Pass callbacks in the create options or React props.

### `onLoadingChange`

Runs when the grouped loading state changes. The value is `true` while any mounted element is still loading.

**Signature:** `((loading: boolean) => void)`

## Methods

Call these on the Wallet handle from `whop.wallet.create({ … })` or `useWallet()`.

### `update`

Merges new handle options into every mounted element. In React, change the namespace props instead.

**Signature:** `(options: Partial<WalletOptions>) => void`

### `destroy`

Destroys every element and sub-controller this handle created, removes the controller frame, and releases its subscriptions. You can call it more than once, but a destroyed handle refuses any other call — create a new handle to start over. React removes the group automatically when the provider unmounts.

**Signature:** `() => void`

## Types

Named types used throughout this page.

## `ActivityDateRangeOverride`

Fields on `ActivityDateRangeOverride`.

### `start`

**Signature:** `string`

### `end`

**Signature:** `string`

### `includeTime`

**Signature:** `boolean | undefined`

### `postedAfter`

**Signature:** `string | undefined`

### `postedBefore`

**Signature:** `string | undefined`

### `displayPostedAfter`

**Signature:** `string | undefined`

### `displayPostedBefore`

**Signature:** `string | undefined`

## `Money`

Fields on `Money`.

### `currency`

Three-letter ISO 4217 currency code, lowercase.

**Signature:** `string`

### `amount`

The amount in major units, as an exact decimal string — `"10.00"` is ten dollars. A string so no float rounds it in transit.

**Signature:** `string`

### `decimals`

How many decimal places the amount CARRIES — the precision the charge itself runs at.

**Signature:** `number`

### `display_decimals`

How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.

**Signature:** `number`

## `LedgerActivity`

Fields on `LedgerActivity`.

### `object`

**Signature:** `"ledger_activity"`

### `id`

Ledger activity ID.

**Signature:** `string`

### `line_type`

The ledger line category this activity was posted under.

**Signature:** `"platform_balance_transfer_outgoing" | "payment_gross" | "withdrawal" | "topup" | "payment_dispute" | "payment_revshare" | "card_spend_authorization" | "airdrop" | "onchain_swap_target" | "payment_gross_reversal" | "payment_refund" | "payment_refund_reversal" | "payment_dispute_reversal" | "payment_dispute_adjustment" | "dispute_hold_adjustment" | "topup_reversal" | "platform_balance_payment" | "platform_balance_payment_refund" | "platform_balance_transfer_incoming" | "internal_balance_transfer_incoming" | "internal_balance_transfer_outgoing" | "onchain_wallet_transfer_incoming" | "onchain_wallet_transfer_outgoing" | "withdrawal_reversal" | "withdrawal_clawback" | "withdrawal_clawback_reversal" | "payment_revshare_refund" | "payment_revshare_reversal" | "payment_referral" | "payment_referral_reversal" | "application_fee_payout" | "airdrop_reversal" | "airdrop_link_created" | "airdrop_link_returned" | "card_spend_authorization_void" | "card_spend_refund" | "onchain_deposit" | "bank_transfer" | "currency_conversion_outgoing" | "currency_conversion_incoming" | "airdrop_link_redeemed" | "resolution_center_refund" | "withdrawal_reclassification" | "payment_revshare_payout" | "platform_affiliate_payment" | "platform_affiliate_payment_reversal" | "onboarding_reward" | "platform_covered_dispute" | "treasury_payin" | "passthrough_gmv" | "promo_reversal" | "misc_reversal" | "ad_spend_charge" | "ad_campaign_budget" | "ad_budget_release" | "ad_publisher_payout" | "ad_publisher_payout_received" | "affiliate_fee" | "application_fee" | "billing_percentage_fee" | "buyer_fee" | "cross_border_percentage_fee" | "dispute_alert_fee" | "fraud_prevention_fee" | "fx_percentage_fee" | "high_risk_merchant_fee" | "orchestration_percentage_fee" | "payment_dispute_fee" | "payment_processing_fixed_fee" | "payment_processing_percentage_fee" | "payout_fee" | "revshare_percentage_fee" | "sales_tax_fee" | "sales_tax_remittance" | "sales_tax_remittance_reversal" | "stripe_domestic_processing_fee" | "stripe_international_processing_fee" | "three_ds_fixed_fee" | "whop_processing_fee" | "balance_reservation" | "balance_reservation_reversal" | "card_interchange" | "card_load_deposit" | "card_load_transfer" | "card_unload_deposit" | "card_unload_transfer" | "company_referral" | "connected_account_negative_balance" | "dispute_representment_fee" | "external_card_load_deposit" | "installment_default" | "internal_withdrawal" | "internal_withdrawal_complete" | "internal_withdrawal_fee" | "internal_withdrawal_fee_reversal" | "internal_withdrawal_in_transit" | "internal_withdrawal_in_transit_reversal" | "internal_withdrawal_markup_fee" | "internal_withdrawal_markup_fee_payout" | "internal_withdrawal_markup_fee_payout_reversal" | "internal_withdrawal_markup_fee_reversal" | "internal_withdrawal_reversal" | "legacy_crypto_payment" | "legacy_payment" | "legacy_payment_refund" | "license_sale" | "license_sale_commission" | "license_sale_revenue" | "marketplace_affiliate_fee" | "misc_purchase" | "misc_refund" | "onchain_swap_source" | "onchain_withdrawal" | "payment_referral_refund" | "platform_balance_transfer_fee" | "platform_earning" | "referral_bonus" | "software_rental_revshare" | "software_rental_transaction" | "swap_fee" | "topup_fee" | "withdrawal_fee" | "withdrawal_fee_reversal" | "withdrawal_markup_fee" | "withdrawal_markup_fee_payout" | "withdrawal_markup_fee_payout_reversal" | "withdrawal_markup_fee_reversal" | "withdrawal_topup_adjustment" | "deposit" | "wallet_transfer_incoming" | "wallet_transfer_outgoing" | "swap_source" | "swap_target"`

### `amount`

Signed amount in the currency's smallest precision units.

**Signature:** `string`

### `usd_amount`

Dollar value of this movement as a decimal string, signed like `amount`. Converted from the posted amount at the rate that was live when the line posted — the same pricing the wallet balance chart and the financial reports use — so a crypto row carries its dollar value too. `null` for a currency Whop holds no exchange rate for.

**Signature:** `string | null`

### `currency`

Currency for this ledger activity.

**Signature:** `{ code: string; precision: string; }`

### `posted_at`

When the activity posted to the ledger.

**Signature:** `string`

### `available_at`

ISO 8601 timestamp these funds became (or are scheduled to become) withdrawable: the posted time for already-settled funds, or 00:00:00 UTC on the scheduled release date for pending funds. Present only on inflows entering the balance (payments, top-ups, incoming transfers/affiliate); null on payouts, refunds, disputes and on-chain rows. The available\_after/before filters window on its UTC settlement date.

**Signature:** `string | null`

### `resource`

Resource associated with this ledger activity.

**Signature:** `{ object: "account"; id: string; title: string | null; route: string | null; logo_url: string | null; } | { object: "user"; id: string; name: string | null; username: string | null; profile_picture_url: string | null; } | { object: "bounty"; id: string; title: string; status: string; } | { object: "ledger_account"; id: string; owner: { object: "account"; id: string; title: string | null; route: string | null; logo_url: string | null; } | { object: "user"; id: string; name: string | null; username: string | null; profile_picture_url: string | null; } | null; } | { object: "payment_method"; id: string; payment_method_type: string | null; gateway_type: string | null; card: { brand: string | null; last4: string | null; exp_month: number | null; exp_year: number | null; } | null; bank: { bank_name: string | null; account_name: string | null; last4: string | null; account_type: string | null; } | null; email_identifier: string | null; } | { object: "payout_method"; id: string; nickname: string | null; institution_name: string | null; account_reference: string | null; provider: string | null; destination_currency_code: string | null; } | { object: "card_transaction"; id: string; card_id: string | null; merchant_name: string | null; merchant_icon_url: string | null; merchant_category: string | null; status: string | null; usd_amount: string | null; local_amount: string | null; local_currency: string | null; cashback_usd: string | null; authorized_at: string | null; posted_at: string | null; declined_reason: string | null; } | null`

### `source`

Source of this ledger activity.

**Signature:** `{ object: string; id: string; status?: string | null | undefined; payment_amount?: Money | null | undefined; payment_method_type?: string | null | undefined; payment_processor?: string | null | undefined; card_brand?: string | null | undefined; reason?: string | null | undefined; notes?: string | null | undefined; risk_review_hold?: boolean | null | undefined; claim_url?: string | null | undefined; amount_float?: number | null | undefined; created_at?: string | null | undefined; estimated_arrival?: string | null | undefined; payer_name?: string | null | undefined; payout_token_nickname?: string | null | undefined; tx_hash?: string | null | undefined; sender_address?: string | null | undefined; chain?: string | null | undefined; from_amount?: string | null | undefined; from_currency?: string | null | undefined; to_amount?: string | null | undefined; to_currency?: string | null | undefined; payout_destination?: { payer_name?: string | null | undefined; icon_url?: string | null | undefined; } | null | undefined; [key: string]: unknown; } | null`

### `payment`

Payment related to this ledger activity. Included when rich resource hydration is enabled and the movement is tied to a payment.

**Signature:** `{ object: "payment"; id: string; amount: Money | null; payment_method_type: string | null; payment_processor: string | null; card_brand: string | null; card_last4: string | null; user: { id: string; name: string; email: string | null; } | null; product: { id: string; name: string; } | null; plan: { id: string; name: string | null; } | null; created_at: string; } | null | undefined`

### `payment_id`

Payment ID for any payment-related activity, including refunds and disputes.

**Signature:** `string | null | undefined`

### `user_id`

ID of the customer associated with the payment.

**Signature:** `string | null | undefined`

### `user_name`

Display name of the customer associated with the payment.

**Signature:** `string | null | undefined`

### `user_email`

Email of the customer associated with the payment. Requires member:email:read.

**Signature:** `string | null | undefined`

### `product_id`

ID of the product associated with the payment, when applicable.

**Signature:** `string | null | undefined`

### `product_name`

Name of the product associated with the payment, when applicable.

**Signature:** `string | null | undefined`

### `plan_id`

ID of the plan associated with the payment, when applicable.

**Signature:** `string | null | undefined`

### `plan_name`

Name of the plan associated with the payment, when applicable.

**Signature:** `string | null | undefined`

### `account`

The viewer account that owns this row's ledger. Present only when the response aggregates owned accounts (include\_owned\_accounts=true); omitted otherwise.

**Signature:** `{ object: "account"; id: string; title: string | null; route: string | null; logo_url: string | null; } | { object: "user"; id: string; name: string | null; username: string | null; profile_picture_url: string | null; } | undefined`

### `ledger_account_id`

The ledger account (a ldgr\_ identifier) this row belongs to. Present only when the response aggregates owned accounts (include\_owned\_accounts=true); omitted otherwise. Pair it with `account` to scope drawers and dashboard links to the owning business.

**Signature:** `string | null | undefined`

## `DepositSavedCard`

A card the consumer already holds for this account — the element renders it as a funding row and hands the choice back through `cardDepositRequested`; it never collects a card payment itself.

### `id`

Payment-method ID echoed back in `cardDepositRequested`.

**Signature:** `string`

### `label`

Row label, for example, `Visa •••• 4242`.

**Signature:** `string`

### `cardBrand`

Brand key used to pick the row icon — `visa`, `mastercard`, `amex`, `discover`, `jcb`.

**Signature:** `string | null | undefined`

## `DepositCardFee`

Fields on `DepositCardFee`.

### `percentageFee`

Percentage POINTS, not a fraction: `2.9` means 2.9%.

**Signature:** `number`

### `fixedFee`

Flat fee in major units: `0.3` means \$0.30.

**Signature:** `number`

### `radarFee`

Fraud-screening fee in major units.

**Signature:** `number`

## `WithdrawalQuoteSpeed`

Fields on `WithdrawalQuoteSpeed`.

### `estimatedArrival`

**Signature:** `string | null | undefined`

### `fee`

**Signature:** `number`

### `totalReceived`

**Signature:** `number`

### `currency`

**Signature:** `string | undefined`

### `destinationCurrency`

**Signature:** `string | undefined`

### `exchangeRate`

**Signature:** `number | undefined`

### `minLimit`

**Signature:** `number | undefined`

### `maxLimit`

**Signature:** `number | null | undefined`

## `WithdrawalQuote`

Fields on `WithdrawalQuote`.

### `amount`

**Signature:** `number`

### `currency`

**Signature:** `string`

### `destinationCurrency`

**Signature:** `string`

### `exchangeRate`

**Signature:** `number`

### `minLimit`

**Signature:** `number`

### `maxLimit`

**Signature:** `number | null | undefined`

### `instantUnavailableReason`

**Signature:** `"minimum_crypto_sales_not_met" | null | undefined`

### `standard`

**Signature:** `WithdrawalQuoteSpeed | null | undefined`

### `instant`

**Signature:** `WithdrawalQuoteSpeed | null | undefined`

## `WithdrawalMethod`

Fields on `WithdrawalMethod`.

### `id`

**Signature:** `string`

### `createdAt`

**Signature:** `string | null | undefined`

### `nickname`

**Signature:** `string | null | undefined`

### `accountReference`

**Signature:** `string | null | undefined`

### `payerName`

**Signature:** `string | null | undefined`

### `destinationCurrency`

**Signature:** `string`

### `isDefault`

**Signature:** `boolean`

### `iconUrl`

**Signature:** `string | null | undefined`

### `name`

**Signature:** `string | null | undefined`

### `deliveryType`

**Signature:** `string`

### `supportsStandard`

**Signature:** `boolean`

### `supportsInstant`

**Signature:** `boolean`

### `bankVerificationState`

**Signature:** `string | null | undefined`

### `unavailableReason`

**Signature:** `string | null | undefined`

### `feeStructure`

**Signature:** `{ percentage: number; fixedAmount: number; currency: string; } | null | undefined`

### `standardEstimatedArrival`

**Signature:** `string | null | undefined`

### `instantEstimatedArrival`

**Signature:** `string | null | undefined`

### `quote`

**Signature:** `WithdrawalQuote | null | undefined`

## `WithdrawalRequiredField`

Fields on `WithdrawalRequiredField`.

### `id`

**Signature:** `string`

### `label`

**Signature:** `string`

### `inputType`

**Signature:** `string`

### `required`

**Signature:** `boolean`

### `sensitive`

**Signature:** `boolean`

### `placeholder`

**Signature:** `string | null | undefined`

### `validation`

**Signature:** `string | null | undefined`

### `options`

**Signature:** `string[] | null | undefined`

## `WithdrawalSupportedMethod`

Fields on `WithdrawalSupportedMethod`.

### `id`

**Signature:** `string`

### `name`

**Signature:** `string | null | undefined`

### `iconUrl`

**Signature:** `string | null | undefined`

### `deliveryType`

**Signature:** `string`

### `supportsStandard`

**Signature:** `boolean`

### `supportsInstant`

**Signature:** `boolean`

### `supportsPlaid`

**Signature:** `boolean | undefined`

### `quotes`

**Signature:** `WithdrawalQuote[]`

### `requiredFields`

**Signature:** `WithdrawalRequiredField[]`

## `WithdrawalLimits`

Fields on `WithdrawalLimits`.

### `standard`

**Signature:** `{ maxAmount: number; errorCode?: string | null | undefined; errorMessage?: string | null | undefined; }`

### `instant`

**Signature:** `{ maxAmount: number; dailyAmountRemaining?: number | null | undefined; errorCode?: string | null | undefined; errorMessage?: string | null | undefined; }`

## `WithdrawalMoney`

Fields on `WithdrawalMoney`.

### `currency`

**Signature:** `string`

### `amount`

**Signature:** `string`

### `decimals`

**Signature:** `number`

### `display_decimals`

**Signature:** `number`

## `WithdrawalPayoutQuote`

Fields on `WithdrawalPayoutQuote`.

### `requestId`

**Signature:** `string`

### `payoutMethodId`

**Signature:** `string`

### `speed`

**Signature:** `"standard" | "instant"`

### `id`

**Signature:** `string`

### `amount`

**Signature:** `WithdrawalMoney`

### `fee`

**Signature:** `WithdrawalMoney`

### `netAmount`

**Signature:** `WithdrawalMoney`

### `destinationAmount`

**Signature:** `WithdrawalMoney`

### `exchangeRate`

**Signature:** `number`

### `expiresAt`

**Signature:** `string`

### `quoteToken`

**Signature:** `string`

## `WithdrawalRequest`

Fields on `WithdrawalRequest`.

### `amount`

**Signature:** `number`

### `currency`

**Signature:** `string`

### `payoutMethodId`

**Signature:** `string`

### `presentedFee`

**Signature:** `number`

### `quoteId`

**Signature:** `string | undefined`

### `quoteToken`

**Signature:** `string | undefined`

### `speed`

**Signature:** `"standard" | "instant"`

### `notes`

**Signature:** `string | undefined`

### `acknowledgeBankWarning`

**Signature:** `boolean | undefined`

## `WithdrawalCreateMethodInput`

Fields on `WithdrawalCreateMethodInput`.

### `country`

**Signature:** `string`

### `supportedPayoutMethodId`

**Signature:** `string`

### `destinationCurrency`

**Signature:** `string`

### `nickname`

**Signature:** `string`

### `fields`

**Signature:** `Record<string, string>`

## `WithdrawalPayoutQuoteRequest`

Fields on `WithdrawalPayoutQuoteRequest`.

### `requestId`

**Signature:** `string`

### `amount`

**Signature:** `number`

### `currency`

**Signature:** `string`

### `payoutMethodId`

**Signature:** `string`

### `speed`

**Signature:** `"standard" | "instant"`

## `SendRecipient`

Fields on `SendRecipient`.

### `id`

**Signature:** `string`

### `name`

**Signature:** `string | null`

### `username`

**Signature:** `string | null`

### `profilePicUrl`

**Signature:** `string | null`

### `kind`

**Signature:** `"business" | "user" | "email"`

## `Billing`

The billing address.

### `city`

Billing city.

**Signature:** `string | null`

### `country_code`

Billing country code.

**Signature:** `string | null`

### `line1`

Street address line 1.

**Signature:** `string | null`

### `line2`

Street address line 2.

**Signature:** `string | null`

### `postal_code`

Billing postal code.

**Signature:** `string | null`

### `region`

Billing region or state.

**Signature:** `string | null`

## Elements

The elements this group mounts. Each has its own page:

<CardGroup cols={2}>
  <Card title="RequiredActionsElement" href="/elements/upcoming/wallet/required-actions">
    The outstanding-action banners from Whop's balance dashboard — identity verification, deposits, tax, and the rest — in the same order the API returns them. An account with nothing outstanding renders nothing at all, so the element can sit permanently in a layout. Copy comes from the API. Pressing Verify starts a hosted identity session and leaves for it; Add money asks the Wallet controller to open deposit; every other button follows the action's own link. Needs an `accessToken`. A failed read renders nothing rather than an error — a banner should never become the loudest thing on someone else's page.
  </Card>

  <Card title="ActionsElement" href="/elements/upcoming/wallet/actions">
    The account action row from Whop's balance dashboard. Deposit, Send, Withdraw, and Convert open the Wallet controller's built-in overlays. Accept opens Whop's checkout-link creator for a business account or company creation for a personal account. Each button also emits its requested event so the embedding page can observe the action.
  </Card>

  <Card title="Balances" href="/elements/upcoming/wallet/balances">
    Three views of an account's money. The balance view shows the total, a chart of how it changed, and a picker for the time range. The list view shows the holdings that make up that total, valued in dollars. The breakdown view splits one currency into available, pending, reserve, and negative amounts without money-movement controls. When `openHoldingOnSelect` is on, a list row replaces this unit's canvas — the chart and the list — with that holding's balance page instead of only reporting the click. *(sub-controller, 4 elements)*
  </Card>

  <Card title="ActivityElement" href="/elements/upcoming/wallet/activity">
    An account's ledger activity: every movement of money in or out, newest first. The list pages as the viewer scrolls, and rows report which one was tapped instead of navigating, so you can open your own detail screen.
  </Card>

  <Card title="ActivityDetailElement" href="/elements/upcoming/wallet/activityDetail">
    A standalone detail drawer for one movement. Point it at a prefetched financial-activity row, a ledger activity ID, or a card transaction ID — a prefetched row takes precedence, otherwise the element retrieves the record itself. A card transaction opens the card receipt: merchant, amount, status, the card it was charged to, its cardholder on a company account, the settlement date, category, currency conversion and cashback.
  </Card>

  <Card title="DepositElement" href="/elements/upcoming/wallet/deposit">
    Funds a Whop account. Renders an amount field and the account's live funding rails — crypto (a per-network deposit address with its QR) and bank transfer (the wire fields for each settlement currency). A business account's rails resolve with no credentials, so they work on any page; a personal (`user_`) account only reveals its rails to itself, so pass `accessToken` for it — omitted, the viewer's own same-origin session covers it. Cards and platform balance are opt-in: pass `savedCards`, `allowNewCard`, or `showPlatformBalance` and the element collects the amount and the choice, then emits `cardDepositRequested` / `addCardRequested` / `platformBalanceSelected` and waits for you to call `showStep({ step: 'amount' })` when your own screen is done.
  </Card>

  <Card title="ConvertElement" href="/elements/upcoming/wallet/convert">
    Converts an account's USD balance to Gold or Coinbase Wrapped Bitcoin, and back, through Whop's public swaps API. A cross-origin mount needs an `accessToken` scoped to `company:balance:read` plus `crypto_wallet:swap` or `crypto_wallet:manage`.
  </Card>

  <Card title="WithdrawElement" href="/elements/upcoming/wallet/withdraw">
    Collects a payout amount and saved payout method, groups standard and instant delivery choices with live fees and arrival estimates, and collects a new payout method when needed. When the host does not drive the flow, the element lists methods and creates the payout itself — pass `accessToken` (or rely on the viewer's same-origin session). A host that already talks to the payouts API can still supply methods, limits, and loading flags; confirming then emits `withdrawalRequested` for that host to create.
  </Card>

  <Card title="SendElement" href="/elements/upcoming/wallet/send">
    Sends money from an account to a recipient — a user, another account, or a public claim link anyone can redeem. Direct transfers use the source account's configured fiat or crypto rail. Renders its own recipient search resolved from the account ID with no credentials beyond the account's own token. Needs an `accessToken` scoped to `payout:transfer_funds` for direct transfers and `payout:withdraw_funds` for recipient lists; account recipient search additionally needs `company:authorized_user:read` and `member:basic:read`, and account claim links need `airdrop_link:manage` — a host without one of those scopes should turn off the matching prop rather than leave a row that will 403.
  </Card>

  <Card title="CardDetailsElement" href="/elements/upcoming/wallet/cardDetails">
    One card and what has been spent on it: the card itself, what it has spent against its limit, and its latest transactions. Opens as a drawer. The card face, its reveal and its lock come from the `whopCard` element composed inside, so a host gets one surface rather than assembling three. Pressing a transaction, or asking for the full list, reports the request — the drawer never navigates.
  </Card>

  <Card title="Cards" href="/elements/upcoming/wallet/cards">
    An account's card surfaces, mounted from one place: the compact issued-card list, the full sortable roster, the spend chart, and a single revealable card. Mount the faces the page needs — they share the account and the credential this unit is minted with, so a page showing a chart above a roster wires them once. *(sub-controller, 5 elements)*
  </Card>

  <Card title="VerificationElement" href="/elements/upcoming/wallet/verification">
    A banner asking the account holder to verify their identity, shown only while verification is outstanding — an account that has already verified renders nothing at all, so the element can sit permanently in a layout. The headline and body come from the API, so they track the account's actual state: an unstarted account is invited to unlock cards and payouts, one under review reads as pending, and a failed or flagged one says so. Pressing the button reports `verificationRequested` and stays put, so the host mounts its own verification — the `verifications` controller's `kyc` element, say. Needs an `accessToken`. A failed read renders nothing rather than an error — a nudge should never become the loudest thing on the page.
  </Card>
</CardGroup>
