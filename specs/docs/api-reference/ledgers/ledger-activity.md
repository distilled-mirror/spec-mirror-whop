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

# Financial Activity API

> A Financial Activity row is a single movement of money in or out of an account's or user's balance: a payment, payout, refund, transfer, on-chain deposit, swap, or card transaction.

Each row carries a typed `resource` and `source` so you can present and link the event without extra lookups.

Use the Financial Activity API to build a statement or transaction feed for an account or user, and financial reports to summarize balance activity, income, and balances over a date range, then break a report total down into what drove it.

## Endpoints

| Endpoint | Request |
| - | - |
| [List Financial Activity](https://docs.whop.com/api-reference/ledgers/list-financial-activity.md) | GET `/financial_activity` |
| [Get Financial Report](https://docs.whop.com/api-reference/ledgers/get-financial-report.md) | GET `/financial_reports` |
| [Get Financial Report Breakdown](https://docs.whop.com/api-reference/ledgers/get-financial-report-breakdown.md) | GET `/financial_reports/breakdown` |

## Attributes

**id** `string` *required*

Ledger activity ID.

**account** `account or user`

The viewer account that owns this row's ledger. Present only when the response
aggregates owned accounts (include\_owned\_accounts=true); omitted otherwise.

**amount** `string` *required*

Signed amount in the currency's smallest precision units.

**available\_at** `string | null` *required*

ISO 8601 timestamp when this activity affects available funds: 00:00:00 UTC on
the scheduled release date for credits and debits in a pending good-funds
release bucket; the posted time for credits and debits to settled available
funds, including refunds, disputes and payouts. Null for activity outside
these paths, including on-chain rows. The available\_after/before filters use
its UTC date; default activity excludes some movements, including opt-in
reserves.

**currency** `object` *required*

Currency for this ledger activity.

> [accordion] Properties
>
> **code** `string` *required*
>
> Currency code.
>
> **precision** `string` *required*
>
> Precision factor for the currency, for example `100000000` for USD.

**ledger\_account\_id** `string | null`

The ledger account (a ldgr\_ identifier) this row belongs to. Present only when
the response aggregates owned accounts (include\_owned\_accounts=true); omitted
otherwise. Pair it with `account` to scope drawers and dashboard links to the
owning business.

**line\_type** `string` *required*

The ledger line category this activity was posted under.

Available options: `psp_payment_receivable`, `payment_gross`, `topup`, `topup_fee`, `payment_gross_reversal`, `topup_reversal`, `payment_refund`, `payment_refund_reversal`, `platform_balance_payment`, `fee_rebate`, `platform_balance_payment_refund`, `ad_spend_purchase`, `psp_refund_payable`, `passthrough_gmv`, `passthrough_gmv_offset`, `installment_default`, `aggregated_fee`, `payment_processing_fixed_fee`, `payment_processing_percentage_fee`, `billing_percentage_fee`, `cross_border_percentage_fee`, `fx_percentage_fee`, `orchestration_percentage_fee`, `revshare_percentage_fee`, `stripe_domestic_processing_fee`, `stripe_international_processing_fee`, `three_ds_fixed_fee`, `whop_processing_fee`, `economic_intelligence_percentage_fee`, `high_risk_merchant_fee`, `fraud_prevention_fee`, `buyer_fee`, `sales_tax_fee`, `payout_fee`, `affiliate_fee`, `marketplace_affiliate_fee`, `application_fee`, `platform_markup_fee`, `platform_markup_fee_payout`, `sales_tax_remittance`, `sales_tax_remittance_reversal`, `sales_tax_collected`, `sales_tax_collected_reversal`, `sales_tax_remitted`, `payment_dispute`, `payment_dispute_adjustment`, `payment_dispute_fee`, `payment_dispute_reversal`, `platform_covered_dispute`, `dispute_alert_fee`, `dispute_representment_fee`, `payment_return_fee`, `dispute_hold_adjustment`, `dispute_hold_blocked`, `psp_dispute_payable`, `withdrawal`, `withdrawal_payable`, `withdrawal_payable_reversal`, `withdrawal_fee`, `withdrawal_fee_reversal`, `withdrawal_markup_fee`, `withdrawal_markup_fee_reversal`, `withdrawal_markup_fee_payout`, `withdrawal_markup_fee_payout_reversal`, `withdrawal_reversal`, `withdrawal_clawback`, `withdrawal_clawback_reversal`, `payout_receivable`, `withdrawal_reclassification`, `clawback_receivable`, `clawback_receivable_reversal`, `capital_withholding`, `capital_withholding_reversal`, `capital_hold`, `capital_hold_release`, `capital_remittance`, `capital_remittance_reversal`, `treasury_payin`, `treasury_payin_receivable`, `onchain_deposit`, `onchain_deposit_offset`, `onchain_withdrawal`, `onchain_withdrawal_offset`, `trading_account_withdrawal`, `trading_account_withdrawal_offset`, `trading_account_deposit`, `trading_account_deposit_offset`, `perps_load_transfer`, `perps_load_deposit`, `perps_unload_transfer`, `perps_unload_deposit`, `perps_bridge_fee`, `perps_bridge_fee_offset`, `perps_volume`, `perps_volume_offset`, `perps_fee`, `perps_fee_offset`, `platform_trading_commission`, `trading_fee`, `perps_realized_pnl`, `perps_realized_pnl_offset`, `onchain_wallet_transfer_outgoing`, `onchain_wallet_transfer_incoming`, `onchain_platform_transfer_outgoing`, `onchain_platform_transfer_incoming`, `onchain_internal_transfer_outgoing`, `onchain_internal_transfer_incoming`, `onchain_swap_source`, `onchain_swap_target`, `savings_revaluation`, `onchain_swap_offset`, `whop_swap_fee_received`, `swap_fee`, `internal_withdrawal`, `internal_withdrawal_reversal`, `internal_withdrawal_payable`, `internal_withdrawal_payable_reversal`, `internal_withdrawal_fee`, `internal_withdrawal_fee_reversal`, `internal_withdrawal_markup_fee`, `internal_withdrawal_markup_fee_reversal`, `internal_withdrawal_markup_fee_payout`, `internal_withdrawal_markup_fee_payout_reversal`, `internal_withdrawal_in_transit`, `internal_withdrawal_complete`, `internal_withdrawal_in_transit_reversal`, `card_load_transfer`, `card_load_deposit`, `card_unload_transfer`, `card_unload_deposit`, `external_card_load_deposit`, `external_card_load_offset`, `card_spend_authorization`, `card_spend_authorization_hold`, `card_spend_capture`, `card_spend_capture_offset`, `card_spend_authorization_void`, `card_spend_authorization_void_release`, `card_spend_refund`, `card_spend_refund_offset`, `cashback`, `cashback_expense`, `cashback_funding`, `card_interchange_receivable`, `card_interchange`, `platform_interchange_share_expense`, `platform_interchange_share_payable`, `account_settlement`, `currency_conversion_outgoing`, `currency_conversion_incoming`, `balance_reservation`, `balance_reservation_hold`, `balance_reservation_reversal`, `balance_reservation_release`, `bad_debt_offset`, `bad_debt_expense`, `platform_credits_payment`, `platform_credits_payment_refund`, `platform_credits_granted`, `levels_points_card`, `levels_points_ads`, `levels_points_trading`, `levels_points_partner`, `levels_points_action`, `levels_points_burned`, `levels_points_issued`, `levels_rebate`, `marketing_expense`, `platform_balance_transfer_outgoing`, `platform_balance_transfer_incoming`, `internal_balance_transfer_outgoing`, `internal_balance_transfer_incoming`, `platform_balance_transfer_fee`, `platform_balance_transfer_fee_reversal`, `platform_balance_transfer_incoming_reversal`, `platform_balance_transfer_outgoing_reversal`, `internal_balance_transfer_incoming_reversal`, `internal_balance_transfer_outgoing_reversal`, `connected_account_clawback`, `connected_account_negative_balance`, `ad_publisher_payout`, `ad_publisher_payout_received`, `company_referral`, `platform_affiliate_payment`, `platform_interchange_share`, `platform_interchange_share_reversal`, `platform_affiliate_payment_reversal`, `onboarding_reward`, `partner_grant_reward`, `payment_revshare`, `payment_revshare_payout`, `payment_revshare_reversal`, `payment_revshare_refund`, `payment_revshare_reinstated`, `payment_revshare_reinstated_payout`, `payment_referral`, `payment_referral_payable`, `payment_referral_reversal`, `payment_referral_refund`, `application_fee_payable`, `application_fee_payout`, `platform_earning`, `airdrop`, `airdrop_reversal`, `airdrop_expense`, `airdrop_expense_reversal`, `airdrop_link_funded`, `airdrop_link_canceled`, `airdrop_link_returned`, `airdrop_link_claimed`, `airdrop_link_redeemed`, `airdrop_link_created`, `resolution_center_refund`, `ad_funding_disbursement`, `ad_campaign_budget`, `ads_card_spread`, `ad_spend_charge`, `ad_network_cost`, `ad_spend_margin`, `ad_income_expense`, `ad_income_receipt`, `ad_budget_release`, `ad_network_settlement`, `ad_affiliate_payout`, `ad_affiliate_payout_received`, `ad_balance_funding_receipt`, `misc_reversal`, `fx_gain_loss`, `fx_markup`, `legacy_crypto_payment`, `legacy_payment`, `legacy_payment_refund`, `license_sale`, `license_sale_commission`, `license_sale_revenue`, `software_rental_revshare`, `software_rental_transaction`, `misc_purchase`, `misc_refund`, `referral_bonus`, `promo_reversal`, `payment_receivable_settlement`, `platform_earning_settlement`, `platform_earning_unreconciled_clearing`, `payment_receivable_clearing`, `payment_receivable_reversal`, `payment_unreconciled_clearing`, `payment_unreconciled_reversal`, `settlement_shortfall_loss`, `psp_error_loss`, `fx_settlement_gain_loss`, `settlement_rounding_variance`, `psp_payout_settlement`, `psp_payout_deposit`, `psp_payout_receivable`, `psp_payout_receivable_clearing`, `payout_unreconciled_clearing`, `psp_currency_conversion_outgoing`, `psp_currency_conversion_incoming`, `psp_currency_conversion_receivable`, `psp_currency_conversion_receivable_clearing`, `psp_fx_adjustment`, `refund_settlement`, `refund_settlement_reversal`, `refund_payable_clearing`, `refund_payable_reversal`, `refund_unreconciled_clearing`, `refund_unreconciled_reversal`, `psp_account_debit`, `payment_refund_fee`, `dispute_settlement`, `dispute_settlement_reversal`, `dispute_payable_clearing`, `dispute_payable_reversal`, `dispute_unreconciled_clearing`, `dispute_unreconciled_reversal`, `psp_reserve_hold`, `psp_reserve_release`, `psp_dispute_fee`, `psp_dispute_alert_fee`, `psp_refund_fee`, `psp_account_updater_fee`, `psp_adjusted_processing_fee`, `psp_authentication_fee`, `psp_commission_fee`, `psp_fixed_fee`, `psp_gateway_fee`, `psp_interchange_fee`, `psp_invoice_tax_fee`, `psp_markup_fee`, `psp_network_token_fee`, `psp_optimization_fee`, `psp_payment_method_fee`, `psp_payout_fee`, `psp_processing_fee`, `psp_risk_fee`, `psp_scheme_fee`, `psp_variable_fee`, `psp_billing_fee`, `psp_tax_service_fee`, `psp_connect_fee`, `external_account_connection_fee`, `psp_external_account_connection_fee`, `psp_invoicing_fee`, `psp_terminal_fee`, `treasury_fee`, `psp_treasury_fee`, `accelerated_settlement_fee`, `psp_accelerated_settlement_fee`, `psp_receivable_pooled`, `psp_pool_settlement`, `psp_pool_clearing`, `psp_pool_refund_settlement`, `psp_pool_refund_clearing`, `psp_correction`, `psp_payin_settlement`, `psp_payin_clearing`, `psp_bank_pull_settlement`, `psp_bank_pull_clearing`, `psp_pool_dispute_settlement`, `psp_payout_consolidation`, `psp_withholding_tax`, `psp_payout_settlement_reversal`, `psp_transfer_settlement`, `psp_transfer_settlement_reversal`, `psp_collection_settlement`, `payout_subsidy`, `withdrawal_topup_adjustment`, `withdrawal_payable_clearing`, `withdrawal_payable_clearing_reversal`, `identity_verification_fee`, `psp_identity_verification_fee`, `tax_filing_fee`, `psp_tax_filing_fee`, `psp_service_fee`, `recipient_wallet_load`, `psp_recipient_wallet_load`, `dispute_management_fee`, `psp_dispute_management_fee`, `psp_clawback_settlement`, `psp_clawback_settlement_reversal`, `clawback_receivable_settlement`, `clawback_receivable_settlement_reversal`, `clawback_fee`, `psp_clawback_fee`, `deposit`, `wallet_transfer_incoming`, `wallet_transfer_outgoing`, `swap_source`, `swap_target`

**object** `string` *required*

**payment** `payment | null`

Payment related to this ledger activity. Included when rich resource hydration is enabled and the movement is tied to a payment.

> [accordion] Properties
>
> **id** `string` *required*
>
> Payment ID, prefixed `pay_`.
>
> **amount** `object | null` *required*
>
> Total charged by the payment.
>
> > [accordion] Properties
> >
> > **amount** `string` *required*
> >
> > The amount in major units, as an exact decimal string. For example, `"10.00"`
> > is ten dollars. It is a string so that no float rounds it in transit.
> >
> > **currency** `string` *required*
> >
> > Three-letter ISO 4217 currency code, lowercase.
> >
> > **decimals** `integer` *required*
> >
> > How many decimal places the amount carries. This is the precision the charge
> > itself runs at.
> >
> > **display\_decimals** `integer` *required*
> >
> > How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
>
> **card\_brand** `string | null` *required*
>
> Card brand, when the customer paid by card.
>
> **card\_last4** `string | null` *required*
>
> Last four digits of the card, when the customer paid by card.
>
> **created\_at** `string` *required*
>
> When the payment was created.
>
> **object** `string` *required*
>
> **payment\_method\_type** `string | null` *required*
>
> How the customer paid, such as `card` or `paypal`.
>
> **payment\_processor** `string | null` *required*
>
> Processor that handled the payment, such as `stripe`.
>
> **plan** `object | null` *required*
>
> Variant associated with the payment, when applicable.
>
> > [accordion] Properties
> >
> > **id** `string` *required*
> >
> > Variant ID, prefixed `plan_`.
> >
> > **name** `string | null` *required*
> >
> > Variant name.
>
> **product** `object | null` *required*
>
> Product associated with the payment, when applicable.
>
> > [accordion] Properties
> >
> > **id** `string` *required*
> >
> > Product ID, prefixed `prod_`.
> >
> > **name** `string` *required*
> >
> > Product name.
>
> **user** `object | null` *required*
>
> Customer associated with the payment. Email requires member:email:read.
>
> > [accordion] Properties
> >
> > **id** `string` *required*
> >
> > Customer ID, prefixed `user_`.
> >
> > **email** `string | null` *required*
> >
> > Customer email, or null without member:email:read.
> >
> > **name** `string` *required*
> >
> > Customer display name.

**payment\_id** `string | null`

Payment ID for any payment-related activity, including refunds and disputes.

**plan\_id** `string | null`

ID of the variant associated with the payment, when applicable.

**plan\_name** `string | null`

Name of the variant associated with the payment, when applicable.

**posted\_at** `string` *required*

When the activity posted to the ledger.

**product\_id** `string | null`

ID of the product associated with the payment, when applicable.

**product\_name** `string | null`

Name of the product associated with the payment, when applicable.

**resource** `account or user or bounty or ledger_account or payment_method or payout_method or card_transaction | null` *required*

Resource associated with this ledger activity.

**source** `object | null` *required*

Source of this ledger activity. Platform markup fees use object platform\_fee and the ledger activity ID. Treasury pay-ins use object `crypto_offramp` and an ID prefixed `cofr_`. With `include_resource=false`, a treasury pay-in's source and a deposit's `bank_deposit_id` can be null.

> [accordion] Properties
>
> **id** `string` *required*
>
> **account\_id** `string | null`
>
> ID of the account that runs the ad campaign (ad\_campaign sources only).
>
> **amount\_float** `number | null`
>
> Payout amount as a decimal number in the destination currency. Present only on
> `withdrawal` sources. Requires `payout:withdrawal:read`.
>
> **bank\_deposit\_id** `string`
>
> ID of the bank deposit that this on-chain deposit delivered, prefixed `vbt_`.
> Present only on onchain\_transaction sources for bank deposits.
>
> **card\_brand** `string | null`
>
> Card brand used by the payment source.
>
> **chain** `string | null`
>
> Chain the deposit landed on, for example plasma (onchain\_transaction sources
> only).
>
> **claim\_url** `string | null`
>
> The airdrop's claim URL. Null unless the caller can manage claim links on the
> funding account or withdraw from the funding personal balance.
>
> **created\_at** `string | null`
>
> Payout creation time as an ISO 8601 timestamp (payout sources only; requires
> payout:withdrawal:read).
>
> **created\_by\_user** `object | null`
>
> User who requested the withdrawal. Null for system-generated withdrawals or without payout:withdrawal:read.
>
> > [accordion] Properties
> >
> > **id** `string` *required*
> >
> > User ID, prefixed `user_`.
> >
> > **badges** `object[]` *required*
> >
> > Public badges for the user, in display order. Empty when the user has no badges or is suspended.
> >
> > > [accordion] Properties
> > >
> > > **type** `string` *required*
> > >
> > > Badge category. `whop_team` identifies a Whop team member. `verified_partner` identifies a verified member of the Whop Partners program.
> > >
> > > Available options: `whop_team`, `verified_partner`
> >
> > **name** `string | null` *required*
> >
> > Display name.
> >
> > **profile\_picture** `object` *required*
> >
> > Avatar wrapper. Its `url` is always present, and is a generated placeholder when the user set no picture.
> >
> > > [accordion] Properties
> > >
> > > **url** `string` *required*
> > >
> > > Avatar image URL. Always present. It is a generated placeholder when the user set no picture.
> >
> > **username** `string` *required*
> >
> > Public username.
>
> **estimated\_arrival** `string | null`
>
> Estimated arrival as an ISO 8601 timestamp (payout sources only; requires
> payout:withdrawal:read).
>
> **fee\_amount** `object | null`
>
> Total withdrawal fees, including markup fees. Requires payout:withdrawal:read.
>
> > [accordion] Properties
> >
> > **amount** `string` *required*
> >
> > The amount in major units, as an exact decimal string. For example, `"10.00"`
> > is ten dollars. It is a string so that no float rounds it in transit.
> >
> > **currency** `string` *required*
> >
> > Three-letter ISO 4217 currency code, lowercase.
> >
> > **decimals** `integer` *required*
> >
> > How many decimal places the amount carries. This is the precision the charge
> > itself runs at.
> >
> > **display\_decimals** `integer` *required*
> >
> > How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
>
> **fee\_kind** `string | null`
>
> Action that generated a platform markup fee: deposit, swap, transfer, card\_spend, or payout. Present for platform\_markup\_fee and platform\_markup\_fee\_payout, including when include\_resource is false. Null when the originating action is unavailable; omitted on other source types.
>
> Available options: `payout`, `transfer`, `deposit`, `swap`, `card_spend`
>
> **fee\_type** `string | null`
>
> Whether withdrawal fees are inclusive or exclusive. Requires payout:withdrawal:read.
>
> Available options: `exclusive`, `inclusive`
>
> **from\_amount** `string | null`
>
> Amount converted out of from\_currency as a decimal string (swap sources only).
>
> **from\_currency** `string | null`
>
> Lowercase currency code converted from (swap sources only).
>
> **notes** `string | null`
>
> Memo attached to the transfer, payout, or bank\_deposit source, or null when
> none was provided (on payout sources requires payout:withdrawal:read).
>
> **object** `string` *required*
>
> **payer\_name** `string | null`
>
> On payout sources, the name of the entity processing the payout (requires
> payout:withdrawal:read). On bank\_deposit sources, the name of the account that
> sent the deposit.
>
> **payment\_amount** `object | null`
>
> Total charged by the payment source.
>
> > [accordion] Properties
> >
> > **amount** `string` *required*
> >
> > The amount in major units, as an exact decimal string. For example, `"10.00"`
> > is ten dollars. It is a string so that no float rounds it in transit.
> >
> > **currency** `string` *required*
> >
> > Three-letter ISO 4217 currency code, lowercase.
> >
> > **decimals** `integer` *required*
> >
> > How many decimal places the amount carries. This is the precision the charge
> > itself runs at.
> >
> > **display\_decimals** `integer` *required*
> >
> > How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
>
> **payment\_method\_type** `string | null`
>
> Payment method used by the payment source. On bank\_deposit sources, the
> transfer rail: `wire`, `ach`, or `rtp`.
>
> **payment\_processor** `string | null`
>
> Processor used by the payment source.
>
> **payout\_amount** `object | null`
>
> Withdrawal principal sent to the destination, excluding fees, in the withdrawal currency. Requires payout:withdrawal:read.
>
> > [accordion] Properties
> >
> > **amount** `string` *required*
> >
> > The amount in major units, as an exact decimal string. For example, `"10.00"`
> > is ten dollars. It is a string so that no float rounds it in transit.
> >
> > **currency** `string` *required*
> >
> > Three-letter ISO 4217 currency code, lowercase.
> >
> > **decimals** `integer` *required*
> >
> > How many decimal places the amount carries. This is the precision the charge
> > itself runs at.
> >
> > **display\_decimals** `integer` *required*
> >
> > How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
>
> **payout\_destination** `object | null`
>
> Payout destination display info (payout sources only).
>
> > [accordion] Properties
> >
> > **icon\_url** `string | null`
> >
> > **payer\_name** `string | null`
> >
> > **security\_answer** `string | null`
> >
> > The security answer the merchant set when adding this payout method (for example an Interac e-Transfer), needed to claim the payout. null when the destination has no custom answer. Requires payout:destination:read.
>
> **payout\_token\_nickname** `string | null`
>
> Saved payout destination nickname (payout sources only).
>
> **reason** `string | null`
>
> Why the activity happened. On transfer sources this is the transfer reason,
> for example pool\_top\_up or bounty\_return. On payout sources it explains why
> the payout was canceled, denied, or failed (requires payout:withdrawal:read);
> null while the payout is progressing normally.
>
> **risk\_review\_hold** `boolean | null`
>
> Whether this payout is currently held for manual risk review (payout sources
> only; requires payout:withdrawal:read).
>
> **sender\_address** `string | null`
>
> On onchain\_transaction sources, the sender wallet address or onramp provider
> identifier. On bank\_deposit sources, the last four digits of the sending bank
> account, such as `••••1547`.
>
> **status** `string | null`
>
> Lifecycle status. On payout sources this is the payout status (requires
> payout:withdrawal:read); on airdrop\_link sources it is the claim-link status
> (ungated); on payment and top-up sources it is the friendly payment status
> such as succeeded/pending/failed (ungated); on bank\_deposit sources it is
> succeeded, reversal\_in\_progress or reversed (ungated).
>
> **title** `string | null`
>
> Name of the ad campaign the spend, payout, or funding belongs to (ad\_campaign
> sources only).
>
> **to\_amount** `string | null`
>
> Amount received in to\_currency as a decimal string (swap sources only).
>
> **to\_currency** `string | null`
>
> Lowercase currency code converted to (swap sources only).
>
> **trace\_code** `string | null`
>
> Bank trace reference for the withdrawal. Requires payout:withdrawal:read.
>
> **tx\_hash** `string | null`
>
> On-chain transaction hash (onchain\_transaction, swap, and crypto\_offramp sources only). On crypto\_offramp sources, this is the wallet send that funded the pay-in.

**usd\_amount** `string | null` *required*

Dollar value of this movement as a decimal string, signed like `amount`.
Converted from the posted amount at the rate that was live when the line
posted — the same pricing the wallet balance chart and the financial reports
use — so a crypto row carries its dollar value too. `null` for a currency Whop
holds no exchange rate for.

**user\_email** `string | null`

Email of the customer associated with the payment. Requires member:email:read.

**user\_id** `string | null`

ID of the customer associated with the payment.

**user\_name** `string | null`

Display name of the customer associated with the payment.

```json LedgerActivity theme={null}
{
	"id": "line_cGF5bWVudF9ncm9zczpsaTpwZXRhbHBvc3Q6YWM6bGVkZ2VyOmxlOjE6djppazpyZWNlaXB0",
	"object": "ledger_activity",
	"line_type": "payment_gross",
	"amount": "4800000000",
	"usd_amount": "48.0",
	"currency": {
		"code": "usd",
		"precision": "100000000"
	},
	"posted_at": "2026-06-14T18:49:49Z",
	"available_at": "2026-06-17T00:00:00Z",
	"resource": {
		"id": "user_4Hk2mZ9qLpXa",
		"object": "user",
		"name": "Dana Whitfield",
		"username": "danawhitfield",
		"profile_picture_url": "https://cdn.whop.com/users/jordan-avery.png"
	},
	"source": {
		"id": "pay_8QcR2mZ9qLpXa",
		"object": "payment"
	},
	"account": {
		"id": "biz_9dCwK2mZ9qLpXa",
		"object": "account",
		"title": "Shine Time Auto Detailing",
		"route": "shine-time-auto-detailing",
		"logo_url": "https://cdn.whop.com/companies/shine-time-auto-detailing.png"
	},
	"ledger_account_id": "ldgr_7bQx2mZ9qLpXa"
}
```

## Glossary of line types

Every activity row carries a `line_type` that names the financial event it represents. Pass one or more `line_types` to [List Financial Activity](https://docs.whop.com/api-reference/ledgers/list-financial-activity.md) to filter the feed to specific events — either as repeated `line_types[]` parameters or as a single comma-separated value (`line_types=payment_gross,payment_refund`). Unknown line types return a `400`.

The term Local Payment Method (LPM) means a non-card method, including bank debits, bank transfers, and wallets.

### Categories

In place of listing individual line types, you can pass a category shorthand. A category expands to every line type it contains.

| Category | Expands to |
| - | - |
| `fees` | All processing-fee line types — every entry in the [Fees](#fees) section below. |

### Payments

| Line type | Description |
| - | - |
| `payment_gross` | A card or LPM sale settling into the wallet — the gross amount before fees. |
| `topup` | Funds the account added to its own wallet balance, rather than a product sale. |
| `platform_balance_payment` | A purchase paid for with existing wallet balance instead of an external card or LPM. |
| `passthrough_gmv` | Recorded Gross Merchandise Value (GMV) for a purchase made through an external processor, such as Stripe Standard, PayPal, or Coinbase Commerce. |
| `installment_default` | A charge to the wallet covering a buyer's defaulted Buy Now, Pay Later installment. |

### Refunds

| Line type | Description |
| - | - |
| `payment_refund` | A refund of a card or LPM payment, deducted from the wallet. |
| `payment_refund_reversal` | Reverses a `payment_refund`, returning the funds to the wallet. |
| `platform_balance_payment_refund` | A refund of a wallet-balance purchase, returned to the original purchaser's balance. |

### Disputes

| Line type | Description |
| - | - |
| `payment_dispute` | A chargeback on a card or LPM payment, debited from the wallet. |
| `payment_dispute_adjustment` | A credit or debit that adjusts a chargeback when the disputed amount differs from the original payment (partial disputes). |
| `payment_dispute_reversal` | Reverses a `payment_dispute` after the merchant wins the dispute. |
| `platform_covered_dispute` | The portion of a chargeback Whop's marketplace covers when the sale carried a buyer fee. It can be a credit or a debit. |
| `dispute_hold_adjustment` | Moves funds into or out of a dispute hold. A debit places the hold, and a credit releases it. |

### Reserves

| Line type | Description |
| - | - |
| `psp_reserve_hold` | A payment processor withholds part of a settlement in reserve, making those funds unavailable until released. |
| `psp_reserve_release` | A payment processor returns previously withheld reserve funds to the available settlement balance. |

### Payouts

| Line type | Description |
| - | - |
| `withdrawal` | A payout of wallet funds to an external bank account (the line type keeps its historical name). |
| `withdrawal_reversal` | Reverses a canceled or rejected payout and returns the funds to the wallet. |
| `withdrawal_clawback` | A credit to the wallet that recovers funds from the linked bank account to cover a negative balance. |
| `withdrawal_clawback_reversal` | Reverses a `withdrawal_clawback` when the clawback later fails or the bank returns the funds. |
| `withdrawal_reclassification` | Returns a payout's net amount to the wallet's available balance when Whop reclassifies it as an internal transfer. |

### Transfers

| Line type | Description |
| - | - |
| `platform_balance_transfer_outgoing` | A transfer of wallet funds out to another account on the platform. |
| `platform_balance_transfer_incoming` | A transfer of funds into the wallet from another account on the platform. |
| `internal_balance_transfer_outgoing` | A transfer out to another wallet owned by the same entity, such as primary to pool. It doesn't count as revenue. |
| `internal_balance_transfer_incoming` | A transfer in from another wallet owned by the same entity, such as a transfer from a parent wallet to a pool wallet. It doesn't count as revenue. |

### Currency conversion

| Line type | Description |
| - | - |
| `currency_conversion_outgoing` | The source-currency debit created when the wallet converts a balance between currencies. |
| `currency_conversion_incoming` | The destination-currency credit created when the wallet converts a balance between currencies. |

### Affiliates & revenue share

| Line type | Description |
| - | - |
| `payment_referral` | An affiliate referral bonus the merchant chose to share from a card or LPM payment, credited to the affiliate's wallet. |
| `payment_referral_reversal` | Reverses a previously paid referral bonus, debited from the affiliate's wallet. |
| `payment_revshare` | A revenue-share amount debited from the seller's wallet on a payment. |
| `payment_revshare_payout` | A revenue-share amount credited to an affiliate's wallet on a payment. |
| `payment_revshare_reversal` | Reverses a previously paid revenue share, debited from the affiliate's wallet. |
| `payment_revshare_refund` | Returns a revenue-share amount to the seller's wallet after a refund or dispute. |
| `application_fee_payout` | A platform fee from a subaccount payment, settled into the parent account. |
| `platform_affiliate_payment` | A referral bonus for referring an account or buyer to Whop: a share of the processing revenue that account generated. |
| `platform_affiliate_payment_reversal` | Reverses a `platform_affiliate_payment` that Whop determined was invalid or needed correction. |
| `ad_publisher_payout_received` | A credit to a publisher's wallet for ad revenue earned. |
| `ad_publisher_payout` | A payout of ad revenue, debited from the Whop Ads wallet. |
| `company_referral` | Legacy: a referral bonus for referring an account to Whop, sharing the processing revenue that account generated. |

### Airdrops

| Line type | Description |
| - | - |
| `airdrop` | A credit of funds into the wallet at Whop's expense, typically promotional or to make good on an issue. |
| `airdrop_reversal` | Reverses an `airdrop` found to be invalid or in need of correction. |
| `airdrop_link_created` | Debits the wallet to fund a newly created airdrop link. |
| `airdrop_link_returned` | Returns an airdrop link's funds to the sender's available balance after the sender cancels the link. |
| `airdrop_link_redeemed` | Credits a claimed airdrop link's funds into the recipient's available balance. |
| `resolution_center_refund` | A refund to a buyer that Whop covers out of pocket after resolving a Resolution Center case in the buyer's favor. |

### Advertising

| Line type | Description |
| - | - |
| `ad_campaign_budget` | Earmarks funds for an ad campaign when funding the campaign. |
| `ad_spend_charge` | A charge to earmarked campaign funds for ad spend, billed at the marked-up rate. |
| `ad_budget_release` | Releases unspent campaign budget, typically the first step of a refund or balance return. |

### On-chain assets and cards

> [!NOTE]
>
> `onchain_deposit` is opt-in: it is excluded from the default feed and only
> returned when you request it explicitly in `line_types`.

| Line type | Description |
| - | - |
| `onchain_deposit` | An inbound on-chain crypto transfer credited to a Whop-managed wallet (including onramps such as MoonPay). Opt-in — only returned when explicitly requested. |
| `onchain_wallet_transfer_outgoing` | An outbound on-chain transfer from a Whop-managed crypto wallet to another Whop-managed wallet. |
| `onchain_wallet_transfer_incoming` | An inbound on-chain transfer to a Whop-managed crypto wallet from another Whop-managed wallet. |
| `onchain_swap_target` | The received side of an on-chain token swap — the output token credited to the wallet. |
| `treasury_payin` | A credit to the cash wallet from a treasury pay-in, such as a crypto off-ramp moving a crypto balance back into cash. |
| `card_spend_authorization` | A card authorization hold that debits the card balance when the issuer authorizes a purchase. |
| `card_spend_authorization_void` | Returns a card authorization to the card balance when it's voided, expired, or declined after auth. |
| `card_spend_refund` | A merchant refund credited back to the card balance. |

### Legacy & miscellaneous

| Line type | Description |
| - | - |
| `legacy_crypto_payment` | A historical Coinbase Commerce payment Whop processed and paid out to the merchant's wallet. |
| `legacy_payment` | A historical express-account payment credited directly to the user's wallet. |
| `legacy_payment_refund` | Reverses a `legacy_payment`, debiting the merchant's wallet. |
| `license_sale` | The merchant payout for a multi-party software license sale — the sale amount net of fees. Commission and revenue shares use the distinct `license_sale_commission` and `license_sale_revenue` line types. |
| `license_sale_commission` | The commission portion of a software license sale, credited to the commission wallet. |
| `license_sale_revenue` | The revenue-share portion of a software license sale, credited to the revenue wallet. |
| `software_rental_revshare` | A revenue-share payout for a software rental, crediting the merchant net of fees. |
| `software_rental_transaction` | The final settlement of a software rental, crediting the merchant net of fees. |
| `misc_purchase` | A purchase made directly from Whop, for example a legacy software rental or proxy. |
| `misc_refund` | A refund of a `misc_purchase`. |
| `referral_bonus` | A bonus for referring a new user to sign up for whop.com. |
| `promo_reversal` | A one-time reversal of promotional, non-withdrawable `referral_bonus` balance. |
| `misc_reversal` | Legacy — a miscellaneous debit for charges negotiated outside the normal transaction lifecycle. |

### Fees

| Line type | Description |
| - | - |
| `payment_processing_fixed_fee` | A fixed fee (≈\$0.30) for processing a card or LPM payment. |
| `payment_processing_percentage_fee` | A percentage fee such as 2.7% for processing a card or LPM payment. |
| `billing_percentage_fee` | A fee on recurring billing transactions covering subscription management and retries. |
| `cross_border_percentage_fee` | A fee on cross-border transactions, charged when the buyer is outside the processing region. |
| `fx_percentage_fee` | A fee for converting a payment processed in a foreign currency at settlement. |
| `orchestration_percentage_fee` | A fee for routing a payment across multiple PSPs (smart routing and failover). |
| `revshare_percentage_fee` | A fee for using revenue-share payouts on a payment. |
| `three_ds_fixed_fee` | A fixed fee for 3DS authentication, charged only when checkout requires 3DS. |
| `whop_processing_fee` | A platform fee for using whop.com, not tied directly to processing cost. |
| `fraud_prevention_fee` | A fee for fraud prevention on card or LPM payments. |
| `buyer_fee` | A buyer fee booked as platform income. It posts only when the configuration treats buyer fees as platform revenue. |
| `sales_tax_fee` | A fee for Whop collecting and remitting sales tax on a payment. |
| `sales_tax_remittance` | Sales tax collected on a transaction that Whop remits to the tax authority. |
| `sales_tax_remittance_reversal` | Reverses a `sales_tax_remittance` after someone refunds or disputes a payment. |
| `affiliate_fee` | The portion of a card or LPM payment earmarked to pay an affiliate referral bonus. |
| `application_fee` | The platform fee from a subaccount payment paid to the parent account. |
| `payout_fee` | A fee covering the cost of paying out to a bank account. |
| `payment_dispute_fee` | A fee (≈\$15) for receiving a chargeback on a card or LPM payment. |
| `dispute_alert_fee` | A fee for receiving an early dispute warning on a card or LPM payment. |
| `stripe_domestic_processing_fee` | Legacy — a fee for processing a card or LPM payment through Stripe domestically. |
| `stripe_international_processing_fee` | Legacy — a fee for processing a card or LPM payment through Stripe internationally. |


## Related topics

- [List Financial Activity](https://docs.whop.com/api-reference/ledgers/list-financial-activity.md)
- [Financial activity funds available](https://docs.whop.com/api-reference/ledgers/financial-activity-funds-available.md)
- [API Versions](https://docs.whop.com/developer/api/versioning.md)
