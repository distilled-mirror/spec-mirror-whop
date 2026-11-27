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

# Dashboard

> An account's own dashboard surfaces, embedded on your site. Scope it to an account with `accountId`, then mount `paymentsTable` for the account's payments, `paymentDetail` for one payment, `required-actions` for the outstanding-action banners Whop's own dashboard shows above the balance — identity verification, deposits, tax, and the rest — and `verification` for the identity-only nudge. Every surface reads with the same credential, so two side by side always show the same account. The banners render nothing once the account has nothing outstanding, so they can sit permanently in a layout, and they report the presses they cannot answer themselves — Add money and Verify — so the host mounts its own deposit or verification flow, such as the `wallet` controller's `deposit` element or the `verifications` controller's `kyc` element.

> [!NOTE]
>
> This page documents `@whop/elements@1.10.0` and `@whop/elements-react@1.10.0`.

*Since `v1.0.0`.*

## Playground

Assemble the elements with example data. Drive the controls, add and arrange elements, and watch events fire live:

```tsx React theme={null}
import { WhopElements, Dashboard } from "@whop/elements-react";
import { loadWhop } from "@whop/elements";

function Example() {
  return (
    <WhopElements elements={loadWhop()}>
      <Dashboard /* options */>
        {/* mount elements here */}
      </Dashboard>
    </WhopElements>
  );
}
```

```html JavaScript theme={null}
<script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>
<script type="module">
  const dashboard = window.WhopElements().dashboard.create({ /* options */ });
</script>
```

## Options

Pass these to `whop.dashboard.create({ … })`, or as props on `<Dashboard>` in React.

**accessToken** `string`

A scoped token every surface under this handle reads with. Mint one token for the whole handle on your server with `POST /api/v1/access_tokens`, and set a fresh one with `update({ accessToken })` before it expires. Reading payments needs `payment:basic:read`; buyer emails additionally need `member:email:read`, the customer journey `member:basic:read`, the tracking column `shipment:basic:read`, and the product and plan names on a payment `access_pass:basic:read` and `plan:basic:read`. The banners need `payout:account:read`, and starting verification from the action bar `identity:write`. Omitted, the reads carry the viewer's own session, which only answers same-origin.

**accountId** `string` *required*

Account or user ID whose records these surfaces read. Account IDs are prefixed `biz_`; user IDs are prefixed `user_`, can read only the viewer's own outstanding actions, and have no payments.

**appearance** `Appearance`

Visual customization for this group's elements. Overrides the global `WhopElements({ appearance })`. Change it live with `update({ appearance })`.

**locale** `WhopElementsLocale`

Locale for this group's element UI text. Set it to one of the app's built locales to override the global configuration. Any other value falls back to the default locale.

## Events

Pass callbacks in the create options or React props.

### `onLoadingChange`

Runs when the grouped loading state changes. The value is `true` while any mounted element is still loading.

**loading** `boolean`

## Methods

Call these on the Dashboard handle from `whop.dashboard.create({ … })` or `useDashboard()`.

### `update`

Merges new handle options into every mounted element. In React, change the namespace props instead.

**Parameters**

**options** `Partial\<DashboardOptions\>` *required*

### `destroy`

Destroys every element and sub-controller this handle created, removes the controller frame, and releases its subscriptions. You can call it more than once, but a destroyed handle refuses any other call — create a new handle to start over. React removes the group automatically when the provider unmounts.

## Elements

The elements this group mounts. Each has its own page:

> [card] [RequiredActionsElement](https://docs.whop.com/elements/latest/dashboard/required-actions.md)
>
> The outstanding-action banners from Whop's balance dashboard — identity verification, deposits, tax, and the rest — in the same order the API returns them. An account with nothing outstanding renders nothing at all, so the element can sit permanently in a layout. Copy comes from the API. Pressing Verify starts a hosted identity session and leaves for it; Add money reports `depositRequested` and stays put, so the host mounts its own deposit flow — the `wallet` controller's `deposit` element, say; every other button follows the action's own link. Reads with the Dashboard handle's `accessToken`, which needs `payout:account:read`, plus `identity:write` to start verification. A failed read renders nothing rather than an error — a banner should never become the loudest thing on someone else's page.

> [card] [VerificationElement](https://docs.whop.com/elements/latest/dashboard/verification.md)
>
> A banner asking the account holder to verify their identity, shown only while verification is outstanding — an account that has already verified renders nothing at all, so the element can sit permanently in a layout. The headline and status messages come from the API, with a shorter description when inviting the account holder to start verification, so they track the account's actual state: an unstarted account is invited to unlock cards and payouts, one under review reads as pending, and a failed or flagged one says so. Pressing the button reports `verificationRequested` and stays put, so the host mounts its own verification — the `verifications` controller's `kyc` element, say. Reads with the Dashboard handle's `accessToken`, which needs `payout:account:read`. A failed read renders nothing rather than an error — a nudge should never become the loudest thing on the page.

> [card] [PaymentsTableElement](https://docs.whop.com/elements/latest/dashboard/paymentsTable.md)
>
> The dashboard payments table with status cards, search, filters, sorting, row selection, CSV export, column settings, and pagination. Reads all payment pages to compute complete counts and filter locally; intended for accounts with modest payment histories. Customer details and refunds are handed to your application through events.

> [card] [PaymentDetailElement](https://docs.whop.com/elements/latest/dashboard/paymentDetail.md)
>
> A payment detail page with the dashboard breakdown, activity, customer, details, and customer journey. Each section can be hidden. Reads payment:basic:read; customer email needs member:email:read and journey needs member:basic:read. Action events let your application confirm and authorize changes; this element never refunds, retries, or voids a payment itself.


## Related topics

- [Enable Apple Pay and Google Pay](https://docs.whop.com/payments/apple-pay.md)
- [Preferred Settlement Currency](https://docs.whop.com/developer/payouts/preferred-settlement-currency.md)
- [Webhooks](https://docs.whop.com/developer/guides/webhooks.md)
