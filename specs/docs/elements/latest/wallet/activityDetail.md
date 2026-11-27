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

# ActivityDetailElement

> A standalone detail drawer for one movement. Point it at a prefetched financial-activity row, a ledger activity ID, or a card transaction ID — a prefetched row takes precedence, otherwise the element retrieves the record itself. A card transaction opens the card receipt: merchant, amount, status, the card it was charged to, its cardholder on a company account, the settlement date, category, currency conversion and cashback.

> [!NOTE]
>
> This page documents `@whop/elements@1.10.0` and `@whop/elements-react@1.10.0`.

*Since `v1.0.0`.*

Opens as a modal from [`Wallet`](https://docs.whop.com/elements/latest/wallet/overview): `wallet.createOverlay('activityDetail')`. Pass props and callbacks in the create options.

> [!NOTE]
>
> This element is **modal-only**. Open it with `createOverlay`; it has no inline mount.

```tsx React theme={null}
import { WhopElements, Wallet, ActivityDetailElement } from "@whop/elements-react";
import { loadWhop } from "@whop/elements";

function Example() {
  return (
    <WhopElements elements={loadWhop()}>
      <Wallet /* options */>
        <ActivityDetailElement onCardSelected={(e) => console.log(e)} onCardholderSelected={(e) => console.log(e)} onCardTransactionIssueRequested={(e) => console.log(e)} />
      </Wallet>
    </WhopElements>
  );
}
```

```html JavaScript theme={null}
<script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>
<script type="module">
  const wallet = window.WhopElements().wallet.create({ /* options */ });
  wallet.createOverlay('activityDetail', {
    onCardSelected: (e) => console.log(e),
    onCardholderSelected: (e) => console.log(e),
    onCardTransactionIssueRequested: (e) => console.log(e)
  }).open();
</script>
```

Example data. [Open the Playground](https://docs.whop.com/elements/latest/wallet/overview#playground).

## Props

**activity** `LedgerActivity | null`

Optional prefetched ledger activity row. When provided, the element renders it without another request. Defaults to `null`.

**properties**

**object** `"ledger_activity"` *required*

**id** `string` *required*

Ledger activity ID.

**line\_type** `string` *required*

The ledger line category this activity was posted under.

**values**

One of `"withdrawal"`, `"accelerated_settlement_fee"`, `"account_settlement"`, `"ad_affiliate_payout"`, `"ad_affiliate_payout_received"`, `"ad_balance_funding_receipt"`, `"ad_budget_release"`, `"ad_campaign_budget"`, `"ad_funding_disbursement"`, `"ad_income_expense"`, `"ad_income_receipt"`, `"ad_network_cost"`, `"ad_network_settlement"`, `"ad_publisher_payout"`, `"ad_publisher_payout_received"`, `"ad_spend_charge"`, `"ad_spend_margin"`, `"ad_spend_purchase"`, `"ads_card_spread"`, `"affiliate_fee"`, `"aggregated_fee"`, `"airdrop"`, `"airdrop_expense"`, `"airdrop_expense_reversal"`, `"airdrop_link_canceled"`, `"airdrop_link_claimed"`, `"airdrop_link_created"`, `"airdrop_link_funded"`, `"airdrop_link_redeemed"`, `"airdrop_link_returned"`, `"airdrop_reversal"`, `"application_fee"`, `"application_fee_payable"`, `"application_fee_payout"`, `"bad_debt_expense"`, `"bad_debt_offset"`, `"balance_reservation"`, `"balance_reservation_hold"`, `"balance_reservation_release"`, `"balance_reservation_reversal"`, `"billing_percentage_fee"`, `"buyer_fee"`, `"card_interchange"`, `"card_interchange_receivable"`, `"card_load_deposit"`, `"card_load_transfer"`, `"card_spend_authorization"`, `"card_spend_authorization_hold"`, `"card_spend_authorization_void"`, `"card_spend_authorization_void_release"`, `"card_spend_capture"`, `"card_spend_capture_offset"`, `"card_spend_refund"`, `"card_spend_refund_offset"`, `"card_unload_deposit"`, `"card_unload_transfer"`, `"cashback"`, `"cashback_expense"`, `"cashback_funding"`, `"clawback_fee"`, `"clawback_receivable"`, `"clawback_receivable_reversal"`, `"clawback_receivable_settlement"`, `"clawback_receivable_settlement_reversal"`, `"company_referral"`, `"connected_account_clawback"`, `"connected_account_negative_balance"`, `"cross_border_percentage_fee"`, `"currency_conversion_incoming"`, `"currency_conversion_outgoing"`, `"dispute_alert_fee"`, `"dispute_hold_adjustment"`, `"dispute_hold_blocked"`, `"dispute_management_fee"`, `"dispute_payable_clearing"`, `"dispute_payable_reversal"`, `"dispute_representment_fee"`, `"dispute_settlement"`, `"dispute_settlement_reversal"`, `"dispute_unreconciled_clearing"`, `"dispute_unreconciled_reversal"`, `"economic_intelligence_percentage_fee"`, `"external_account_connection_fee"`, `"external_card_load_deposit"`, `"external_card_load_offset"`, `"fraud_prevention_fee"`, `"fx_gain_loss"`, `"fx_markup"`, `"fx_percentage_fee"`, `"fx_settlement_gain_loss"`, `"high_risk_merchant_fee"`, `"identity_verification_fee"`, `"installment_default"`, `"internal_balance_transfer_incoming"`, `"internal_balance_transfer_outgoing"`, `"internal_withdrawal"`, `"internal_withdrawal_complete"`, `"internal_withdrawal_fee"`, `"internal_withdrawal_fee_reversal"`, `"internal_withdrawal_in_transit"`, `"internal_withdrawal_in_transit_reversal"`, `"internal_withdrawal_markup_fee"`, `"internal_withdrawal_markup_fee_payout"`, `"internal_withdrawal_markup_fee_payout_reversal"`, `"internal_withdrawal_markup_fee_reversal"`, `"internal_withdrawal_payable"`, `"internal_withdrawal_payable_reversal"`, `"internal_withdrawal_reversal"`, `"legacy_crypto_payment"`, `"legacy_payment"`, `"legacy_payment_refund"`, `"levels_points_ads"`, `"levels_points_burned"`, `"levels_points_card"`, `"levels_points_issued"`, `"levels_points_partner"`, `"levels_points_trading"`, `"levels_rebate"`, `"license_sale"`, `"license_sale_commission"`, `"license_sale_revenue"`, `"marketing_expense"`, `"marketplace_affiliate_fee"`, `"misc_purchase"`, `"misc_refund"`, `"misc_reversal"`, `"onboarding_reward"`, `"onchain_deposit"`, `"onchain_deposit_offset"`, `"onchain_internal_transfer_incoming"`, `"onchain_internal_transfer_outgoing"`, `"onchain_platform_transfer_incoming"`, `"onchain_platform_transfer_outgoing"`, `"onchain_swap_offset"`, `"onchain_swap_source"`, `"onchain_swap_target"`, `"onchain_wallet_transfer_incoming"`, `"onchain_wallet_transfer_outgoing"`, `"onchain_withdrawal"`, `"onchain_withdrawal_offset"`, `"orchestration_percentage_fee"`, `"partner_grant_reward"`, `"passthrough_gmv"`, `"passthrough_gmv_offset"`, `"payment_dispute"`, `"payment_dispute_adjustment"`, `"payment_dispute_fee"`, `"payment_dispute_reversal"`, `"payment_gross"`, `"payment_gross_reversal"`, `"payment_processing_fixed_fee"`, `"payment_processing_percentage_fee"`, `"payment_receivable_clearing"`, `"payment_receivable_reversal"`, `"payment_receivable_settlement"`, `"payment_referral"`, `"payment_referral_payable"`, `"payment_referral_refund"`, `"payment_referral_reversal"`, `"payment_refund"`, `"payment_refund_fee"`, `"payment_refund_reversal"`, `"payment_return_fee"`, `"payment_revshare"`, `"payment_revshare_payout"`, `"payment_revshare_refund"`, `"payment_revshare_reversal"`, `"payment_unreconciled_clearing"`, `"payment_unreconciled_reversal"`, `"payout_fee"`, `"payout_receivable"`, `"payout_subsidy"`, `"payout_unreconciled_clearing"`, `"platform_affiliate_payment"`, `"platform_affiliate_payment_reversal"`, `"platform_balance_payment"`, `"platform_balance_payment_refund"`, `"platform_balance_transfer_fee"`, `"platform_balance_transfer_incoming"`, `"platform_balance_transfer_outgoing"`, `"platform_covered_dispute"`, `"platform_credits_granted"`, `"platform_credits_payment"`, `"platform_credits_payment_refund"`, `"platform_earning"`, `"platform_earning_settlement"`, `"platform_earning_unreconciled_clearing"`, `"platform_markup_fee"`, `"platform_markup_fee_payout"`, `"promo_reversal"`, `"psp_accelerated_settlement_fee"`, `"psp_account_debit"`, `"psp_account_updater_fee"`, `"psp_adjusted_processing_fee"`, `"psp_authentication_fee"`, `"psp_bank_pull_clearing"`, `"psp_bank_pull_settlement"`, `"psp_billing_fee"`, `"psp_clawback_fee"`, `"psp_clawback_settlement"`, `"psp_clawback_settlement_reversal"`, `"psp_collection_settlement"`, `"psp_commission_fee"`, `"psp_connect_fee"`, `"psp_correction"`, `"psp_currency_conversion_incoming"`, `"psp_currency_conversion_outgoing"`, `"psp_currency_conversion_receivable"`, `"psp_currency_conversion_receivable_clearing"`, `"psp_dispute_alert_fee"`, `"psp_dispute_fee"`, `"psp_dispute_management_fee"`, `"psp_dispute_payable"`, `"psp_external_account_connection_fee"`, `"psp_fixed_fee"`, `"psp_fx_adjustment"`, `"psp_gateway_fee"`, `"psp_identity_verification_fee"`, `"psp_interchange_fee"`, `"psp_invoice_tax_fee"`, `"psp_invoicing_fee"`, `"psp_markup_fee"`, `"psp_network_token_fee"`, `"psp_optimization_fee"`, `"psp_payin_clearing"`, `"psp_payin_settlement"`, `"psp_payment_method_fee"`, `"psp_payment_receivable"`, `"psp_payout_consolidation"`, `"psp_payout_deposit"`, `"psp_payout_fee"`, `"psp_payout_receivable"`, `"psp_payout_receivable_clearing"`, `"psp_payout_settlement"`, `"psp_payout_settlement_reversal"`, `"psp_pool_clearing"`, `"psp_pool_dispute_settlement"`, `"psp_pool_refund_clearing"`, `"psp_pool_refund_settlement"`, `"psp_pool_settlement"`, `"psp_processing_fee"`, `"psp_receivable_pooled"`, `"psp_recipient_wallet_load"`, `"psp_refund_fee"`, `"psp_refund_payable"`, `"psp_reserve_hold"`, `"psp_reserve_release"`, `"psp_risk_fee"`, `"psp_scheme_fee"`, `"psp_service_fee"`, `"psp_tax_filing_fee"`, `"psp_tax_service_fee"`, `"psp_terminal_fee"`, `"psp_transfer_settlement"`, `"psp_transfer_settlement_reversal"`, `"psp_treasury_fee"`, `"psp_variable_fee"`, `"psp_withholding_tax"`, `"recipient_wallet_load"`, `"referral_bonus"`, `"refund_payable_clearing"`, `"refund_payable_reversal"`, `"refund_settlement"`, `"refund_settlement_reversal"`, `"refund_unreconciled_clearing"`, `"refund_unreconciled_reversal"`, `"resolution_center_refund"`, `"revshare_percentage_fee"`, `"sales_tax_collected"`, `"sales_tax_collected_reversal"`, `"sales_tax_fee"`, `"sales_tax_remittance"`, `"sales_tax_remittance_reversal"`, `"sales_tax_remitted"`, `"settlement_rounding_variance"`, `"settlement_shortfall_loss"`, `"software_rental_revshare"`, `"software_rental_transaction"`, `"stripe_domestic_processing_fee"`, `"stripe_international_processing_fee"`, `"swap_fee"`, `"tax_filing_fee"`, `"three_ds_fixed_fee"`, `"topup"`, `"topup_fee"`, `"topup_reversal"`, `"trading_account_deposit"`, `"trading_account_deposit_offset"`, `"trading_account_withdrawal"`, `"trading_account_withdrawal_offset"`, `"treasury_fee"`, `"treasury_payin"`, `"treasury_payin_receivable"`, `"whop_processing_fee"`, `"whop_swap_fee_received"`, `"withdrawal_clawback"`, `"withdrawal_clawback_reversal"`, `"withdrawal_fee"`, `"withdrawal_fee_reversal"`, `"withdrawal_markup_fee"`, `"withdrawal_markup_fee_payout"`, `"withdrawal_markup_fee_payout_reversal"`, `"withdrawal_markup_fee_reversal"`, `"withdrawal_payable"`, `"withdrawal_payable_clearing"`, `"withdrawal_payable_clearing_reversal"`, `"withdrawal_payable_reversal"`, `"withdrawal_reclassification"`, `"withdrawal_reversal"`, `"withdrawal_topup_adjustment"`, `"deposit"`, `"wallet_transfer_incoming"`, `"wallet_transfer_outgoing"`, `"swap_source"`, or `"swap_target"`.

**amount** `string` *required*

Signed amount in the currency's smallest precision units.

**usd\_amount** `string | null` *required*

Dollar value of this movement as a decimal string, signed like `amount`. Converted from the posted amount at the rate that was live when the line posted — the same pricing the wallet balance chart and the financial reports use — so a crypto row carries its dollar value too. `null` for a currency Whop holds no exchange rate for.

**currency** `object` *required*

Currency for this ledger activity.

**properties**

**code** `string` *required*

**precision** `string` *required*

**posted\_at** `string` *required*

When the activity posted to the ledger.

**available\_at** `string | null` *required*

ISO 8601 timestamp when this activity affects available funds: 00:00:00 UTC on the scheduled release date for credits and debits in a pending good-funds release bucket; the posted time for credits and debits to settled available funds, including refunds, disputes and payouts. Null for activity outside these paths, including on-chain rows. The available\_after/before filters use its UTC date; default activity excludes some movements, including opt-in reserves.

**resource** `object | null` *required*

Resource associated with this ledger activity.

**shape 1**

**object** `"account"` *required*

**id** `string` *required*

**title** `string | null` *required*

**route** `string | null` *required*

**logo\_url** `string | null` *required*

**shape 2**

**object** `"user"` *required*

**id** `string` *required*

**name** `string | null` *required*

**username** `string | null` *required*

**profile\_picture\_url** `string | null` *required*

**shape 3**

**object** `"bounty"` *required*

**id** `string` *required*

**title** `string` *required*

**status** `string` *required*

**shape 4**

**object** `"ledger_account"` *required*

**id** `string` *required*

**owner** `object | null` *required*

**shape 1**

**object** `"account"` *required*

**id** `string` *required*

**title** `string | null` *required*

**route** `string | null` *required*

**logo\_url** `string | null` *required*

**shape 2**

**object** `"user"` *required*

**id** `string` *required*

**name** `string | null` *required*

**username** `string | null` *required*

**profile\_picture\_url** `string | null` *required*

**shape 5**

**object** `"payment_method"` *required*

**id** `string` *required*

**payment\_method\_type** `string | null` *required*

**gateway\_type** `string | null` *required*

**card** `object | null` *required*

**properties**

**brand** `string | null` *required*

**last4** `string | null` *required*

**exp\_month** `number | null` *required*

**exp\_year** `number | null` *required*

**bank** `object | null` *required*

**properties**

**bank\_name** `string | null` *required*

**account\_name** `string | null` *required*

**last4** `string | null` *required*

**account\_type** `string | null` *required*

**email\_identifier** `string | null` *required*

**shape 6**

**object** `"payout_method"` *required*

**id** `string` *required*

**nickname** `string | null` *required*

**institution\_name** `string | null` *required*

**account\_reference** `string | null` *required*

**provider** `string | null` *required*

**destination\_currency\_code** `string | null` *required*

**shape 7**

**object** `"card_transaction"` *required*

**id** `string` *required*

**card\_id** `string | null` *required*

**merchant\_name** `string | null` *required*

**merchant\_icon\_url** `string | null` *required*

**merchant\_category** `string | null` *required*

**status** `string | null` *required*

**usd\_amount** `string | null` *required*

**local\_amount** `string | null` *required*

**local\_currency** `string | null` *required*

**cashback\_usd** `string | null` *required*

**levels\_points** `string | null` *required*

**authorized\_at** `string | null` *required*

**posted\_at** `string | null` *required*

**declined\_reason** `string | null` *required*

**source** `object | null` *required*

Source of this ledger activity. Platform markup fees use object platform\_fee and the ledger activity ID.

**properties**

**object** `string` *required*

**id** `string` *required*

**fee\_kind** `string | null`

One of `"deposit"`, `"transfer"`, `"card_spend"`, `"payout"`, `"swap"`, or `null`.

**status** `string | null`

**payment\_amount** `Money | null`

**properties**

**currency** `string` *required*

Three-letter ISO 4217 currency code, lowercase.

**amount** `string` *required*

The amount in major units, as an exact decimal string — `"10.00"` is ten dollars. A string so no float rounds it in transit.

**decimals** `number` *required*

How many decimal places the amount CARRIES — the precision the charge itself runs at.

**display\_decimals** `number` *required*

How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.

**payment\_method\_type** `string | null`

**payment\_processor** `string | null`

**card\_brand** `string | null`

**reason** `string | null`

**notes** `string | null`

**risk\_review\_hold** `boolean | null`

**claim\_url** `string | null`

**amount\_float** `number | null`

**payout\_amount** `Money | null`

**properties**

**currency** `string` *required*

Three-letter ISO 4217 currency code, lowercase.

**amount** `string` *required*

The amount in major units, as an exact decimal string — `"10.00"` is ten dollars. A string so no float rounds it in transit.

**decimals** `number` *required*

How many decimal places the amount CARRIES — the precision the charge itself runs at.

**display\_decimals** `number` *required*

How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.

**fee\_amount** `Money | null`

**properties**

**currency** `string` *required*

Three-letter ISO 4217 currency code, lowercase.

**amount** `string` *required*

The amount in major units, as an exact decimal string — `"10.00"` is ten dollars. A string so no float rounds it in transit.

**decimals** `number` *required*

How many decimal places the amount CARRIES — the precision the charge itself runs at.

**display\_decimals** `number` *required*

How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.

**fee\_type** `"exclusive" | "inclusive" | null`

**created\_by\_user** `UserSummary | null`

**properties**

**id** `string` *required*

User ID, prefixed `user_`.

**username** `string` *required*

Public username.

**name** `string | null` *required*

Display name.

**profile\_picture** `UserProfilePicture` *required*

Avatar wrapper; its `url` is always present, using a generated placeholder when the user set no picture.

**properties**

**url** `string` *required*

Avatar image URL. Always present — a generated placeholder when the user set no picture.

**trace\_code** `string | null`

**created\_at** `string | null`

**estimated\_arrival** `string | null`

**payer\_name** `string | null`

**payout\_token\_nickname** `string | null`

**tx\_hash** `string | null`

**sender\_address** `string | null`

**chain** `string | null`

**from\_amount** `string | null`

**from\_currency** `string | null`

**to\_amount** `string | null`

**to\_currency** `string | null`

**payout\_destination** `object | null`

**properties**

**payer\_name** `string | null`

**icon\_url** `string | null`

**security\_answer** `string | null`

**\[key: string]** `unknown` *required*

**payment** `object | null`

Payment related to this ledger activity. Included when rich resource hydration is enabled and the movement is tied to a payment.

**properties**

**object** `"payment"` *required*

**id** `string` *required*

**amount** `Money | null` *required*

**properties**

**currency** `string` *required*

Three-letter ISO 4217 currency code, lowercase.

**amount** `string` *required*

The amount in major units, as an exact decimal string — `"10.00"` is ten dollars. A string so no float rounds it in transit.

**decimals** `number` *required*

How many decimal places the amount CARRIES — the precision the charge itself runs at.

**display\_decimals** `number` *required*

How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.

**payment\_method\_type** `string | null` *required*

**payment\_processor** `string | null` *required*

**card\_brand** `string | null` *required*

**card\_last4** `string | null` *required*

**user** `object | null` *required*

**properties**

**id** `string` *required*

**name** `string` *required*

**email** `string | null` *required*

**product** `object | null` *required*

**properties**

**id** `string` *required*

**name** `string` *required*

**plan** `object | null` *required*

**properties**

**id** `string` *required*

**name** `string | null` *required*

**created\_at** `string` *required*

**payment\_id** `string | null`

Payment ID for any payment-related activity, including refunds and disputes.

**user\_id** `string | null`

ID of the customer associated with the payment.

**user\_name** `string | null`

Display name of the customer associated with the payment.

**user\_email** `string | null`

Email of the customer associated with the payment. Requires member:email:read.

**product\_id** `string | null`

ID of the product associated with the payment, when applicable.

**product\_name** `string | null`

Name of the product associated with the payment, when applicable.

**plan\_id** `string | null`

ID of the variant associated with the payment, when applicable.

**plan\_name** `string | null`

Name of the variant associated with the payment, when applicable.

**account** `object`

The viewer account that owns this row's ledger. Present only when the response aggregates owned accounts (include\_owned\_accounts=true); omitted otherwise.

**shape 1**

**object** `"account"` *required*

**id** `string` *required*

**title** `string | null` *required*

**route** `string | null` *required*

**logo\_url** `string | null` *required*

**shape 2**

**object** `"user"` *required*

**id** `string` *required*

**name** `string | null` *required*

**username** `string | null` *required*

**profile\_picture\_url** `string | null` *required*

**ledger\_account\_id** `string | null`

The ledger account (a ldgr\_ identifier) this row belongs to. Present only when the response aggregates owned accounts (include\_owned\_accounts=true); omitted otherwise. Pair it with `account` to scope drawers and dashboard links to the owning business.

**activityId** `string | null`

Optional ledger activity ID to retrieve when no prefetched activity row is provided. Defaults to `null`.

**cardTransactionId** `string | null`

Optional card transaction ID (`citx_`) to retrieve and show as a card receipt — what a card table hands back when a row is pressed. Takes precedence over `activityId`, and is ignored when a prefetched row is passed. Defaults to `null`.

**showCardholder** `boolean`

Show the cardholder row on a card receipt. Company accounts only — a personal wallet has one cardholder, so the row never appears there. Reading the name needs `company:authorized_user:read`; without it the row reads Unavailable. Defaults to `true`.

**showCardTransactionReportIssueAction** `boolean`

Show the report-an-issue action for eligible card transactions. The action emits an event for the host. Defaults to `false`.

## Events

Pass callbacks in the create options or React props.

### `onCardSelected`

The card row was clicked.

**payload** `object`

**properties**

**cardId** `string` *required*

### `onCardholderSelected`

The cardholder row was clicked. Open your own profile surface for that user.

**payload** `object`

**properties**

**userId** `string` *required*

### `onCardTransactionIssueRequested`

Report an issue was clicked. Open the host-owned card dispute flow.

**payload** `object`

**properties**

**transactionId** `string` *required*

### `onLoaderStart`

Runs after the loading skeleton first paints and before `onReady`.

### `onReady`

Runs after the element's first complete paint.

### `onError`

Runs when the element fails to load or crashes. The fallback remains visible. Use `code` for programmatic handling. `sourceKey` identifies a failed host-state source.

**e** `object`

**properties**

**message** `string` *required*

**code** `string`

**sourceKey** `string`

## Methods

Call these on the handle returned by `create`, or through a React `ref`.

### `mount`

Mounts the element in `target` and starts loading. React components mount themselves.

**Parameters**

**target** `string | HTMLElement` *required*

### `destroy`

Removes the element and releases its frame and subscriptions. You can call it more than once. React removes the element automatically.

### `update`

Merges new props into the mounted element. In React, change the component props instead.

**Parameters**

**options** `Partial\<ActivityDetailElementProps\>` *required*

## Styling

This element doesn't expose class names for styling. Use `appearance` (theme, accent color, variables) to restyle it.


## Related topics

- [Wallet](https://docs.whop.com/elements/latest/wallet/overview.md)
- [ActivityElement](https://docs.whop.com/elements/latest/wallet/activity.md)
- [PaymentDetailElement](https://docs.whop.com/elements/latest/dashboard/paymentDetail.md)
