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

# ReportActivityElement

> Financial activity with date, currency, direction, and movement filters, pagination, and CSV export. Mount independently or open it from the balance report.

> [!NOTE]
>
> This page documents `@whop/elements@1.10.0` and `@whop/elements-react@1.10.0`.

*Since `v1.0.0`.*

Mounts inside [`Reports`](https://docs.whop.com/elements/latest/wallet/reports), in [`Wallet`](https://docs.whop.com/elements/latest/wallet/overview). `accountId` and `accessToken` come from `Wallet`. Pass props and callbacks through the create options or React props. Keep the created handle, or React `ref`, to call `refresh()`.

```tsx React theme={null}
import { WhopElements, Wallet, Reports, ReportActivityElement } from "@whop/elements-react";
import { loadWhop } from "@whop/elements";

function Example() {
  return (
    <WhopElements elements={loadWhop()}>
      <Wallet /* options */>
        <Reports>
          <ReportActivityElement onExportReady={(e) => console.log(e)} />
        </Reports>
      </Wallet>
    </WhopElements>
  );
}
```

```html JavaScript theme={null}
<script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>
<script type="module">
  const wallet = window.WhopElements().wallet.create({ /* options */ });
  const reports = wallet.create('reports', { /* options */ });
  reports.create('reportActivity', { onExportReady: (e) => console.log(e) }).mount('#wallet-reports-reportActivity');
</script>
```

Example data. [Open the Playground](https://docs.whop.com/elements/latest/wallet/overview#playground).

## Props

**filters** `ReportActivityFilters`

Initial filters. Changing these resets the table to the requested activity. Defaults to `{}`.

**properties**

**currency** `string`

**postedAfter** `string`

**postedBefore** `string`

**direction** `"money_in" | "money_out"`

**lineTypes** `string[]`

**values**

Each item is one of `"fees"`, `"withdrawal"`, `"account_settlement"`, `"ad_affiliate_payout_received"`, `"ad_budget_release"`, `"ad_campaign_budget"`, `"ad_publisher_payout"`, `"ad_publisher_payout_received"`, `"ad_spend_charge"`, `"affiliate_fee"`, `"airdrop"`, `"airdrop_link_created"`, `"airdrop_link_redeemed"`, `"airdrop_link_returned"`, `"airdrop_reversal"`, `"application_fee"`, `"application_fee_payout"`, `"balance_reservation"`, `"balance_reservation_reversal"`, `"billing_percentage_fee"`, `"buyer_fee"`, `"card_interchange"`, `"card_load_deposit"`, `"card_load_transfer"`, `"card_spend_authorization"`, `"card_spend_authorization_void"`, `"card_spend_refund"`, `"card_unload_deposit"`, `"card_unload_transfer"`, `"cashback"`, `"cashback_funding"`, `"company_referral"`, `"connected_account_negative_balance"`, `"cross_border_percentage_fee"`, `"currency_conversion_incoming"`, `"currency_conversion_outgoing"`, `"dispute_alert_fee"`, `"dispute_hold_adjustment"`, `"dispute_representment_fee"`, `"economic_intelligence_percentage_fee"`, `"external_card_load_deposit"`, `"fraud_prevention_fee"`, `"fx_percentage_fee"`, `"high_risk_merchant_fee"`, `"installment_default"`, `"internal_balance_transfer_incoming"`, `"internal_balance_transfer_outgoing"`, `"internal_withdrawal"`, `"internal_withdrawal_complete"`, `"internal_withdrawal_fee"`, `"internal_withdrawal_fee_reversal"`, `"internal_withdrawal_in_transit"`, `"internal_withdrawal_in_transit_reversal"`, `"internal_withdrawal_markup_fee"`, `"internal_withdrawal_markup_fee_payout"`, `"internal_withdrawal_markup_fee_payout_reversal"`, `"internal_withdrawal_markup_fee_reversal"`, `"internal_withdrawal_reversal"`, `"legacy_crypto_payment"`, `"legacy_payment"`, `"legacy_payment_refund"`, `"levels_rebate"`, `"license_sale"`, `"license_sale_commission"`, `"license_sale_revenue"`, `"marketplace_affiliate_fee"`, `"misc_purchase"`, `"misc_refund"`, `"misc_reversal"`, `"onboarding_reward"`, `"onchain_deposit"`, `"onchain_internal_transfer_incoming"`, `"onchain_internal_transfer_outgoing"`, `"onchain_platform_transfer_incoming"`, `"onchain_platform_transfer_outgoing"`, `"onchain_swap_source"`, `"onchain_swap_target"`, `"onchain_wallet_transfer_incoming"`, `"onchain_wallet_transfer_outgoing"`, `"onchain_withdrawal"`, `"orchestration_percentage_fee"`, `"partner_grant_reward"`, `"passthrough_gmv"`, `"payment_dispute"`, `"payment_dispute_adjustment"`, `"payment_dispute_fee"`, `"payment_dispute_reversal"`, `"payment_gross"`, `"payment_gross_reversal"`, `"payment_processing_fixed_fee"`, `"payment_processing_percentage_fee"`, `"payment_referral"`, `"payment_referral_refund"`, `"payment_referral_reversal"`, `"payment_refund"`, `"payment_refund_reversal"`, `"payment_return_fee"`, `"payment_revshare"`, `"payment_revshare_payout"`, `"payment_revshare_refund"`, `"payment_revshare_reversal"`, `"payout_fee"`, `"platform_affiliate_payment"`, `"platform_affiliate_payment_reversal"`, `"platform_balance_payment"`, `"platform_balance_payment_refund"`, `"platform_balance_transfer_fee"`, `"platform_balance_transfer_incoming"`, `"platform_balance_transfer_outgoing"`, `"platform_covered_dispute"`, `"platform_credits_granted"`, `"platform_credits_payment"`, `"platform_credits_payment_refund"`, `"platform_earning"`, `"platform_markup_fee"`, `"platform_markup_fee_payout"`, `"promo_reversal"`, `"referral_bonus"`, `"resolution_center_refund"`, `"revshare_percentage_fee"`, `"sales_tax_fee"`, `"sales_tax_remittance"`, `"sales_tax_remittance_reversal"`, `"software_rental_revshare"`, `"software_rental_transaction"`, `"stripe_domestic_processing_fee"`, `"stripe_international_processing_fee"`, `"swap_fee"`, `"three_ds_fixed_fee"`, `"topup"`, `"topup_fee"`, `"topup_reversal"`, `"treasury_payin"`, `"whop_processing_fee"`, `"withdrawal_clawback"`, `"withdrawal_clawback_reversal"`, `"withdrawal_fee"`, `"withdrawal_fee_reversal"`, `"withdrawal_markup_fee"`, `"withdrawal_markup_fee_payout"`, `"withdrawal_markup_fee_payout_reversal"`, `"withdrawal_markup_fee_reversal"`, `"withdrawal_reclassification"`, `"withdrawal_reversal"`, or `"withdrawal_topup_adjustment"`.

**resourceId** `string`

## Events

Pass callbacks in the create options or React props.

### `onExportReady`

The CSV is ready and its download has started.

**payload** `ReportExportReady`

**properties**

**exportId** `string` *required*

**downloadUrl** `string` *required*

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

### `refresh`

Refresh the report data after activity changes.

**Returns** `Promise<void>`

### `mount`

Mounts the element in `target` and starts loading. React components mount themselves.

**Parameters**

**target** `string | HTMLElement` *required*

### `destroy`

Removes the element and releases its frame and subscriptions. You can call it more than once. React removes the element automatically.

### `update`

Merges new props into the mounted element. In React, change the component props instead.

**Parameters**

**options** `Partial\<ReportActivityElementProps\>` *required*

## Styling

Style these parts through `appearance.classes`. Use camel case or kebab case for property names and include units. Page stylesheets can't reach the element's frame. The framework validates each declaration before injecting it.

| Class | Targets |
| - | - |
| `.whop-ReportActivity` | The report activity table and filters. |

```ts theme={null}
const wallet = whop.wallet.create({
  appearance: {
    classes: {
      'whop-ReportActivity': { borderRadius: '8px', fontWeight: '600' }
    }
  }
});

wallet.update({
  appearance: {
    classes: { 'whop-ReportActivity': { fontWeight: '700' } }
  }
});
```

In React, pass `appearance` to `<Wallet>`. Set it globally with `WhopElements({ appearance })`.


## Related topics

- [Reports](https://docs.whop.com/elements/latest/wallet/reports.md)
- [ActivityElement](https://docs.whop.com/elements/latest/wallet/activity.md)
- [BalanceReportElement](https://docs.whop.com/elements/latest/wallet/reports-balanceReport.md)
