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

# Wallet

> Drives an account's money surfaces. `actions` renders the Deposit, Accept, Send, and Convert controls: Deposit, Send, and Convert open their Wallet overlays, while Accept opens Whop's checkout-link creator for a business account or company creation for a personal account. `deposit` returns live funding rails; `convert` swaps USD with Gold, Bitcoin, or a stock token and exposes FX currencies when the host enables `fxEnabled`; `send` moves money to a recipient or creates a public claim link; `withdraw` lists payout methods and creates the payout when the host does not already drive that API; `balances` holds two faces — the holdings list, and the balance block drawing value over a window; `cards` is the compact issued-card list; `cardsTable` renders the full sortable card roster; `cardsChart` plots card spend; `whopCard` renders one revealable card; `activity` lists ledger movements; `activityDetail` renders a prefetched movement, or retrieves one by activity ID or card transaction ID, in a standalone drawer. The outstanding-action bar and the identity nudge live on the `dashboard` controller. Every data-backed surface can use the viewer's session when no token is provided.

> [!NOTE]
>
> This page documents `@whop/elements@1.10.0` and `@whop/elements-react@1.10.0`.

*Since `v1.0.0`.*

## Playground

Assemble the elements with example data. Drive the controls, add and arrange elements, and watch events fire live:

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
<script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>
<script type="module">
  const wallet = window.WhopElements().wallet.create({ /* options */ });
</script>
```

## Options

Pass these to `whop.wallet.create({ … })`, or as props on `<Wallet>` in React.

**currency** `string`

Three-letter ISO 4217 currency code for amount fields. An unknown code falls back to `usd`. Defaults to `"usd"`.

**accessToken** `string`

Optional scoped token projected to data-backed wallet children. Without it, reads use the viewer session.

**openHoldingOnSelect** `boolean`

Show a holding's balance page in place of the balances chart and list when a row is clicked. Off by default so a host that routes on `balanceSelected` is not covered by a second screen. Defaults to `false`.

**accountId** `string` *required*

Account or user ID whose money these surfaces read. Account IDs are prefixed `biz_`; user IDs are prefixed `user_` and can read only the viewer's own balance.

**appearance** `Appearance`

Visual customization for this group's elements. Overrides the global `WhopElements({ appearance })`. Change it live with `update({ appearance })`.

**locale** `WhopElementsLocale`

Locale for this group's element UI text. Set it to one of the app's built locales to override the global configuration. Any other value falls back to the default locale.

## Events

Pass callbacks in the create options or React props.

### `onIdentityVerificationRequested`

A deposit requires identity verification. Handle this callback to open your settings or verification flow; Wallet does not navigate.

**payload** `object`

**properties**

**accountId** `string` *required*

### `onLoadingChange`

Runs when the grouped loading state changes. The value is `true` while any mounted element is still loading.

**loading** `boolean`

## Methods

Call these on the Wallet handle from `whop.wallet.create({ … })` or `useWallet()`.

### `update`

Merges new handle options into every mounted element. In React, change the namespace props instead.

**Parameters**

**options** `Partial\<WalletOptions\>` *required*

### `destroy`

Destroys every element and sub-controller this handle created, removes the controller frame, and releases its subscriptions. You can call it more than once, but a destroyed handle refuses any other call — create a new handle to start over. React removes the group automatically when the provider unmounts.

## Elements

The elements this group mounts. Each has its own page:

> [card] [ActionsElement](https://docs.whop.com/elements/latest/wallet/actions.md)
>
> The account action row from Whop's balance dashboard. Deposit, Send, Withdraw, and Convert open the Wallet controller's built-in overlays. Accept opens Whop's checkout-link creator for a business account or company creation for a personal account. Each button also emits its requested event so the embedding page can observe the action.

> [card] [Balances](https://docs.whop.com/elements/latest/wallet/balances)
>
> Three views of an account's money. The balance view shows the total, a chart of how it changed, and a picker for the time range. The list view shows the holdings that make up that total, valued in dollars. The breakdown view splits one currency into available, pending, reserve, and negative amounts without money-movement controls. When `openHoldingOnSelect` is on, a list row replaces this unit's canvas — the chart and the list — with that holding's balance page instead of only reporting the click. *(sub-controller, 4 elements)*

> [card] [Reports](https://docs.whop.com/elements/latest/wallet/reports)
>
> Balance history and the financial activity behind it, including CSV exports. Account statements are available in the Whop dashboard. *(sub-controller, 2 elements)*

> [card] [ActivityElement](https://docs.whop.com/elements/latest/wallet/activity.md)
>
> An account's ledger activity: every movement of money in or out, newest first. The list pages as the viewer scrolls, and rows report which one was tapped instead of navigating, so you can open your own detail screen.

> [card] [ActivityDetailElement](https://docs.whop.com/elements/latest/wallet/activityDetail.md)
>
> A standalone detail drawer for one movement. Point it at a prefetched financial-activity row, a ledger activity ID, or a card transaction ID — a prefetched row takes precedence, otherwise the element retrieves the record itself. A card transaction opens the card receipt: merchant, amount, status, the card it was charged to, its cardholder on a company account, the settlement date, category, currency conversion and cashback.

> [card] [DepositElement](https://docs.whop.com/elements/latest/wallet/deposit.md)
>
> Funds a Whop account. Renders an amount field and the account's live funding rails — crypto (a per-network deposit address with its QR) and bank transfer (the wire fields for each settlement currency). A business account's rails resolve with no credentials, so they work on any page; a personal (`user_`) account only reveals its rails to itself, so pass `accessToken` for it — omitted, the viewer's own same-origin session covers it. Cards and platform balance are opt-in: pass `savedCards`, `allowNewCard`, or `showPlatformBalance` and the element collects the amount and the choice, then emits `cardDepositRequested` / `addCardRequested` / `platformBalanceSelected` and waits for you to call `showStep({ step: 'amount' })` when your own screen is done.

> [card] [ConvertElement](https://docs.whop.com/elements/latest/wallet/convert.md)
>
> Converts an account's USD balance to Gold, Coinbase Wrapped Bitcoin, or a stock token, and back, through Whop's public swaps API. A cross-origin mount needs an `accessToken` scoped to `company:balance:read` plus `crypto_wallet:swap` or `crypto_wallet:manage`.

> [card] [WithdrawElement](https://docs.whop.com/elements/latest/wallet/withdraw.md)
>
> Collects a payout amount and saved payout method, groups standard and instant delivery choices with live fees and arrival estimates, and collects a new payout method when needed. When the host does not drive the flow, the element lists methods and creates the payout itself — pass `accessToken` (or rely on the viewer's same-origin session). A host that already talks to the payouts API can still supply methods, limits, and loading flags; confirming then emits `withdrawalRequested` for that host to create.

> [card] [SendElement](https://docs.whop.com/elements/latest/wallet/send.md)
>
> Sends money from an account to a recipient — a user, another account, or a public claim link anyone can redeem. Direct transfers use the source account's configured fiat or crypto rail. Renders its own recipient search resolved from the account ID with no credentials beyond the account's own token. Needs an `accessToken` scoped to `payout:transfer_funds` for direct transfers and `payout:withdraw_funds` for recipient lists; account recipient search additionally needs `company:authorized_user:read` and `member:basic:read`, and account claim links need `airdrop_link:manage` — a host without one of those scopes should turn off the matching prop rather than leave a row that will 403.

> [card] [CardDetailsElement](https://docs.whop.com/elements/latest/wallet/cardDetails)
>
> One card and what has been spent on it: the card itself, what it has spent against its limit, and its latest transactions. Opens as a drawer. The card face, its reveal and its lock come from the `whopCard` element composed inside, so a host gets one surface rather than assembling three. Pressing a transaction, or asking for the full list, reports the request — the drawer never navigates.

> [card] [Cards](https://docs.whop.com/elements/latest/wallet/cards)
>
> An account's card surfaces, mounted from one place: the compact issued-card list, the full sortable roster, the spend chart, and a single revealable card. Mount the faces the page needs — they share the account and the credential this unit is minted with, so a page showing a chart above a roster wires them once. *(sub-controller, 5 elements)*


## Related topics

- [Open a Wallet Sheet from Your Own Button](https://docs.whop.com/developer/guides/payment-request.md)
- [Add Express Checkout](https://docs.whop.com/developer/guides/express-checkout.md)
- [ActivityElement](https://docs.whop.com/elements/latest/wallet/activity.md)
