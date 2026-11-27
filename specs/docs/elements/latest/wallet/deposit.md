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

# DepositElement

> Funds a Whop account. Renders an amount field and the account's live funding rails — crypto (a per-network deposit address with its QR) and bank transfer (the wire fields for each settlement currency). A business account's rails resolve with no credentials, so they work on any page; a personal (`user_`) account only reveals its rails to itself, so pass `accessToken` for it — omitted, the viewer's own same-origin session covers it. Cards and platform balance are opt-in: pass `savedCards`, `allowNewCard`, or `showPlatformBalance` and the element collects the amount and the choice, then emits `cardDepositRequested` / `addCardRequested` / `platformBalanceSelected` and waits for you to call `showStep({ step: 'amount' })` when your own screen is done.

> [!NOTE]
>
> This page documents `@whop/elements@1.10.0` and `@whop/elements-react@1.10.0`.

*Since `v1.0.0`.*

Mounts inside [`Wallet`](https://docs.whop.com/elements/latest/wallet/overview). `accountId` and `accessToken` come from there. Pass props and callbacks through the create options or React props. Keep the created handle, or React `ref`, to call `showStep()` and `refresh()`.

> [!NOTE]
>
> You can mount this element **inline** (`create`) or open it as a **modal** overlay (`createOverlay`).

```tsx React theme={null}
import { WhopElements, Wallet, DepositElement } from "@whop/elements-react";
import { loadWhop } from "@whop/elements";

function Example() {
  return (
    <WhopElements elements={loadWhop()}>
      <Wallet /* options */>
        <DepositElement onDepositInitiated={(e) => console.log(e)} onCardDepositRequested={(e) => console.log(e)} onAddCardRequested={(e) => console.log(e)} onBankSelected={(e) => console.log(e)} onIdentityVerificationRequested={(e) => console.log(e)} onPlatformBalanceSelected={(e) => console.log(e)} onDepositConfirmed={(e) => console.log(e)} onStepChanged={(e) => console.log(e)} onDismissed={(e) => console.log(e)} />
      </Wallet>
    </WhopElements>
  );
}
```

```html JavaScript theme={null}
<script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>
<script type="module">
  const wallet = window.WhopElements().wallet.create({ /* options */ });
  wallet.create('deposit', {
    onDepositInitiated: (e) => console.log(e),
    onCardDepositRequested: (e) => console.log(e),
    onAddCardRequested: (e) => console.log(e),
    onBankSelected: (e) => console.log(e),
    onIdentityVerificationRequested: (e) => console.log(e),
    onPlatformBalanceSelected: (e) => console.log(e),
    onDepositConfirmed: (e) => console.log(e),
    onStepChanged: (e) => console.log(e),
    onDismissed: (e) => console.log(e)
  }).mount('#wallet-deposit');
</script>
```

Example data. [Open the Playground](https://docs.whop.com/elements/latest/wallet/overview#playground).

## Props

**accessToken** `string`

A scoped token that lets the rails resolution read a personal (`user_`) account — needs `payout:account:read`, plus `payout:account:update` for the bank rail to provision. Mint it on your server with `POST /api/v1/access_tokens`. Omitted, the call carries the viewer's own session, which only answers same-origin; business accounts resolve without any credential.

**amount** `string`

Prefill the amount and make it read-only — for flows that already know what is owed. An amount at or below zero is ignored, as is one longer than 12 whole digits; the rest is rounded to the currency’s decimal places. Defaults to `""`.

**showCrypto** `boolean`

Offer the crypto rail when the account has a deposit address. Defaults to `true`.

**showBank** `boolean`

Offer the bank-transfer rail. Shown even before the account has bank details — choosing it then asks the viewer to verify their identity when that is what the account still owes, and reports the rail as not available yet when it is not. Defaults to `true`.

**savedCards** `DepositSavedCard[]`

Cards you already hold for this payer, rendered as funding rows. Choosing one emits `cardDepositRequested` with the amount — the element never collects a card payment. Defaults to `[]`.

**properties**

**id** `string` *required*

Payment-method ID echoed back in `cardDepositRequested`.

**label** `string` *required*

Row label, for example, `Visa •••• 4242`.

**cardBrand** `string | null`

Brand key used to pick the row icon — `visa`, `mastercard`, `amex`, `discover`, `jcb`.

**allowNewCard** `boolean`

Add an "Add card" row that emits `addCardRequested` so you can open your own card-collection flow. Defaults to `false`.

**showPlatformBalance** `boolean`

Offer a "Platform balance" row that emits `platformBalanceSelected` immediately, with no amount step. Defaults to `false`.

**cardFee** `DepositCardFee | null`

Processing fees to preview under the amount when a card row is selected. `percentageFee` is in percentage POINTS (`2.9` is 2.9%); `fixedFee` and `radarFee` are major units (`0.3` is \$0.30). Defaults to `null`.

**properties**

**percentageFee** `number` *required*

Percentage POINTS, not a fraction: `2.9` means 2.9%.

**fixedFee** `number` *required*

Flat fee in major units: `0.3` means \$0.30.

**radarFee** `number` *required*

Fraud-screening fee in major units.

**depositRailFees** `Partial\<Record\<"crypto" | "bank", DepositRailFee\>\>`

Override the account’s deposit fees under the rail picker, keyed by rail (`bank`, `crypto`). Omitted rails use the fees returned by the account. `percentageFee` is in percentage POINTS (`1.5` is 1.5%); `fixedFeeUsd` is USD major units (`2` is \$2.00) whatever the deposit currency. Shows the rate before an amount is typed and the charged amount after. Defaults to `{}`.

**properties**

**crypto** `DepositRailFee`

**properties**

**percentageFee** `number` *required*

Percentage POINTS, not a fraction: `1.5` means 1.5%.

**fixedFeeUsd** `number` *required*

Flat fee in USD major units: `2` means \$2.00, whatever the deposit currency.

**bank** `DepositRailFee`

**properties**

**percentageFee** `number` *required*

Percentage POINTS, not a fraction: `1.5` means 1.5%.

**fixedFeeUsd** `number` *required*

Flat fee in USD major units: `2` means \$2.00, whatever the deposit currency.

**preferredMethodId** `string`

Preselect a rail by id (`bank`, `crypto`, `platform_balance`, or a saved card id). Continue stays disabled while that rail is not yet offered, rather than funding through another one. Defaults to `""`.

**deferBankToHost** `boolean`

Emit `bankSelected` and stay on the amount screen instead of showing the wire fields — for hosts that run their own step after the account already has a virtual bank account. Defaults to `false`.

**confirmCryptoDeposit** `boolean`

Show an "I have deposited my funds" button on the crypto screen that emits `depositConfirmed`, so you can start watching for the transfer. Defaults to `false`.

**initialStep** `"amount" | "crypto" | "bank"`

Open straight onto a rail instead of the picker. `crypto` and `bank` need no amount; their back button emits `dismissed` because there is no picker behind them. Defaults to `"amount"`.

## Events

Pass callbacks in the create options or React props.

### `onDepositInitiated`

The payer confirmed an amount and a rail. Fires for every rail, before any instructions render — the analytics/funnel hook.

**payload** `object`

**properties**

**method** `"crypto" | "bank" | "card" | "platform_balance"` *required*

**amount** `number` *required*

**currency** `string` *required*

**paymentMethodId** `string`

### `onCardDepositRequested`

A saved card was chosen. Collect the payment on your side, then call `showStep({ step: "amount" })` to return the element to the picker.

**payload** `object`

**properties**

**amount** `number` *required*

**currency** `string` *required*

**paymentMethodId** `string` *required*

### `onAddCardRequested`

The "Add card" row was picked — open your card-collection flow.

### `onBankSelected`

The bank rail was chosen while `deferBankToHost` is set — run your step, then show the fields yourself or call `showStep({ step: "bank" })`.

### `onIdentityVerificationRequested`

The viewer chose bank transfer before this account has a virtual bank account. The element stays on the amount screen and never navigates — mount your own verification, such as the `verifications` controller, in answer to this.

**payload** `object`

**properties**

**accountId** `string` *required*

### `onPlatformBalanceSelected`

The platform-balance row was picked. It carries no amount — your own screen collects that.

### `onDepositConfirmed`

The payer said they sent the crypto. `since` is a unix-seconds floor to start matching incoming transfers from (it looks slightly back in time, so a transfer sent moments before the click still matches).

**payload** `object`

**properties**

**network** `string` *required*

**token** `string` *required*

**networkLabel** `string` *required*

**since** `number` *required*

### `onStepChanged`

The visible screen changed.

**payload** `object`

**properties**

**step** `"amount" | "crypto" | "bank"` *required*

### `onDismissed`

The payer backed out of a rail the element was opened directly onto — close the surface holding it.

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

### `showStep`

Move the element to a screen. `{ step: "amount" }` is how you hand control back after settling a card or platform-balance deposit on your side.

**Parameters**

**input** `object` *required*

**properties**

**step** `"amount" | "crypto" | "bank"` *required*

**Returns** `Promise<void>`

### `refresh`

Re-resolve the account's funding rails. Call it after anything on your side changes what the account can offer — finishing bank onboarding, for instance — so the new rail appears without waiting for the cache to expire.

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

**options** `Partial\<DepositElementProps\>` *required*

## Styling

Style these parts through `appearance.classes`. Use camel case or kebab case for property names and include units. Page stylesheets can't reach the element's frame. The framework validates each declaration before injecting it.

| Class | Targets |
| - | - |
| `.whop-DepositSurface` | The deposit element root — one screen at a time |

```ts theme={null}
const wallet = whop.wallet.create({
  appearance: {
    classes: {
      'whop-DepositSurface': { borderRadius: '8px', fontWeight: '600' }
    }
  }
});

wallet.update({
  appearance: {
    classes: { 'whop-DepositSurface': { fontWeight: '700' } }
  }
});
```

In React, pass `appearance` to `<Wallet>`. Set it globally with `WhopElements({ appearance })`.


## Related topics

- [Wallet](https://docs.whop.com/elements/latest/wallet/overview.md)
- [ActionsElement](https://docs.whop.com/elements/latest/wallet/actions.md)
- [RequiredActionsElement](https://docs.whop.com/elements/latest/dashboard/required-actions.md)
