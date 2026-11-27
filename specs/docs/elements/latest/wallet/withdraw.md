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

# WithdrawElement

> Collects a payout amount and saved payout method, groups standard and instant delivery choices with live fees and arrival estimates, and collects a new payout method when needed. When the host does not drive the flow, the element lists methods and creates the payout itself — pass `accessToken` (or rely on the viewer's same-origin session). A host that already talks to the payouts API can still supply methods, limits, and loading flags; confirming then emits `withdrawalRequested` for that host to create.

> [!NOTE]
>
> This page documents `@whop/elements@1.10.0` and `@whop/elements-react@1.10.0`.

*Since `v1.0.0`.*

Mounts inside [`Wallet`](https://docs.whop.com/elements/latest/wallet/overview). `accountId` and `accessToken` come from there. Pass props and callbacks through the create options or React props. Keep the created handle, or React `ref`, to call `refresh()`.

> [!NOTE]
>
> You can mount this element **inline** (`create`) or open it as a **modal** overlay (`createOverlay`).

```tsx React theme={null}
import { WhopElements, Wallet, WithdrawElement } from "@whop/elements-react";
import { loadWhop } from "@whop/elements";

function Example() {
  return (
    <WhopElements elements={loadWhop()}>
      <Wallet /* options */>
        <WithdrawElement onAmountChanged={(e) => console.log(e)} onCountryChanged={(e) => console.log(e)} onSupportedMethodChanged={(e) => console.log(e)} onAddMethodRequested={(e) => console.log(e)} onPlaidLinkRequested={(e) => console.log(e)} onMethodVerificationCompleted={(e) => console.log(e)} onRenameMethodRequested={(e) => console.log(e)} onRemoveMethodRequested={(e) => console.log(e)} onPayoutQuoteRequested={(e) => console.log(e)} onWithdrawalRequested={(e) => console.log(e)} onDone={(e) => console.log(e)} />
      </Wallet>
    </WhopElements>
  );
}
```

```html JavaScript theme={null}
<script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>
<script type="module">
  const wallet = window.WhopElements().wallet.create({ /* options */ });
  wallet.create('withdraw', {
    onAmountChanged: (e) => console.log(e),
    onCountryChanged: (e) => console.log(e),
    onSupportedMethodChanged: (e) => console.log(e),
    onAddMethodRequested: (e) => console.log(e),
    onPlaidLinkRequested: (e) => console.log(e),
    onMethodVerificationCompleted: (e) => console.log(e),
    onRenameMethodRequested: (e) => console.log(e),
    onRemoveMethodRequested: (e) => console.log(e),
    onPayoutQuoteRequested: (e) => console.log(e),
    onWithdrawalRequested: (e) => console.log(e),
    onDone: (e) => console.log(e)
  }).mount('#wallet-withdraw');
</script>
```

Example data. [Open the Playground](https://docs.whop.com/elements/latest/wallet/overview#playground).

## Props

**accessToken** `string`

A scoped token for listing payout methods and creating the payout — needs `payout:withdraw_funds`, `payout:destination:read`, and `payout:create_destination` to add a bank. Omitted, the calls carry the viewer's own session, which only answers same-origin.

**succeeded** `boolean`

Whether the confirmed payout was created successfully. Defaults to `false`.

**loading** `boolean`

Whether payout methods and limits are loading. Defaults to `false`.

**methods** `WithdrawalMethod[]`

Saved payout methods with quotes for the current amount. Defaults to `[]`.

**properties**

**id** `string` *required*

**createdAt** `string | null`

**nickname** `string | null`

**accountReference** `string | null`

**payerName** `string | null`

**destinationCurrency** `string` *required*

**isDefault** `boolean` *required*

**iconUrl** `string | null`

**name** `string | null`

**deliveryType** `string` *required*

**supportsStandard** `boolean` *required*

**supportsInstant** `boolean` *required*

**bankVerificationState** `string | null`

**unavailableReason** `string | null`

**feeStructure** `object | null`

**properties**

**percentage** `number` *required*

**fixedAmount** `number` *required*

**currency** `string` *required*

**standardEstimatedArrival** `string | null`

**instantEstimatedArrival** `string | null`

**quote** `WithdrawalQuote | null`

**properties**

**amount** `number` *required*

**currency** `string` *required*

**destinationCurrency** `string` *required*

**exchangeRate** `number` *required*

**minLimit** `number` *required*

**maxLimit** `number | null`

**instantUnavailableReason** `"minimum_crypto_sales_not_met" | null`

**standard** `WithdrawalQuoteSpeed | null`

**properties**

**estimatedArrival** `string | null`

**fee** `number` *required*

**totalReceived** `number` *required*

**currency** `string`

**destinationCurrency** `string`

**exchangeRate** `number`

**minLimit** `number`

**maxLimit** `number | null`

**instant** `WithdrawalQuoteSpeed | null`

**properties**

**estimatedArrival** `string | null`

**fee** `number` *required*

**totalReceived** `number` *required*

**currency** `string`

**destinationCurrency** `string`

**exchangeRate** `number`

**minLimit** `number`

**maxLimit** `number | null`

**error** `string`

A host-side error to show inside the current screen. Defaults to `""`.

**availableBalance** `number`

Settled balance available to standard payout methods. Defaults to `0`.

**pendingBalance** `number`

Pending balance that may be available to instant payout methods. Defaults to `0`.

**payoutCountry** `string`

The payout account's ISO 3166-1 alpha-2 country. Defaults to `"US"`.

**supportedMethods** `WithdrawalSupportedMethod[]`

Payout methods available to add in the selected country, including required fields. Defaults to `[]`.

**properties**

**id** `string` *required*

**name** `string | null`

**iconUrl** `string | null`

**deliveryType** `string` *required*

**supportsStandard** `boolean` *required*

**supportsInstant** `boolean` *required*

**supportsPlaid** `boolean`

**feeStructure** `object | null`

**properties**

**percentage** `number` *required*

**fixedAmount** `number` *required*

**currency** `string` *required*

**quotes** `WithdrawalQuote[]` *required*

**properties**

**amount** `number` *required*

**currency** `string` *required*

**destinationCurrency** `string` *required*

**exchangeRate** `number` *required*

**minLimit** `number` *required*

**maxLimit** `number | null`

**instantUnavailableReason** `"minimum_crypto_sales_not_met" | null`

**standard** `WithdrawalQuoteSpeed | null`

**properties**

**estimatedArrival** `string | null`

**fee** `number` *required*

**totalReceived** `number` *required*

**currency** `string`

**destinationCurrency** `string`

**exchangeRate** `number`

**minLimit** `number`

**maxLimit** `number | null`

**instant** `WithdrawalQuoteSpeed | null`

**properties**

**estimatedArrival** `string | null`

**fee** `number` *required*

**totalReceived** `number` *required*

**currency** `string`

**destinationCurrency** `string`

**exchangeRate** `number`

**minLimit** `number`

**maxLimit** `number | null`

**requiredFields** `WithdrawalRequiredField[]` *required*

**properties**

**id** `string` *required*

**label** `string` *required*

**inputType** `string` *required*

**required** `boolean` *required*

**sensitive** `boolean` *required*

**placeholder** `string | null`

**validation** `string | null`

**options** `string[] | null`

**supportedMethodsLoading** `boolean`

Whether supported payout methods are loading for the selected country. Defaults to `false`.

**limits** `WithdrawalLimits`

Live standard and instant payout limits. Defaults to `{"standard":{"maxAmount":0},"instant":{"maxAmount":0}}`.

**properties**

**standard** `object` *required*

**properties**

**maxAmount** `number` *required*

**errorCode** `string | null`

**errorMessage** `string | null`

**instant** `object` *required*

**properties**

**maxAmount** `number` *required*

**dailyAmountRemaining** `number | null`

**resetsAt** `string | null`

**errorCode** `string | null`

**errorMessage** `string | null`

**quoteLoading** `boolean`

Whether the current amount is being repriced. Defaults to `false`.

**payoutQuoteRequired** `boolean`

Whether this payout must use an exact provider-backed quote before confirmation. Defaults to `false`.

**payoutQuote** `WithdrawalPayoutQuote | null`

The exact short-lived payout quote for confirmation. Include serverTime so the quote is timed by its remaining lifetime rather than the device clock. Include recommendedMethod to offer a cheaper method through the connection flow without changing the selected method. Defaults to `null`.

**properties**

**requestId** `string` *required*

**payoutMethodId** `string` *required*

**speed** `"standard" | "instant"` *required*

**id** `string` *required*

**amount** `WithdrawalMoney` *required*

**properties**

**currency** `string` *required*

**amount** `string` *required*

**decimals** `number` *required*

**display\_decimals** `number` *required*

**fee** `WithdrawalMoney` *required*

**properties**

**currency** `string` *required*

**amount** `string` *required*

**decimals** `number` *required*

**display\_decimals** `number` *required*

**netAmount** `WithdrawalMoney` *required*

**properties**

**currency** `string` *required*

**amount** `string` *required*

**decimals** `number` *required*

**display\_decimals** `number` *required*

**destinationAmount** `WithdrawalMoney` *required*

**properties**

**currency** `string` *required*

**amount** `string` *required*

**decimals** `number` *required*

**display\_decimals** `number` *required*

**exchangeRate** `number` *required*

**serverTime** `string`

**expiresAt** `string` *required*

**quoteToken** `string` *required*

**recommendedMethod** `object | null`

**properties**

**supportedPayoutMethodId** `string` *required*

**name** `string` *required*

**country** `string` *required*

**destinationCurrency** `string` *required*

**estimatedSavings** `WithdrawalMoney` *required*

**properties**

**currency** `string` *required*

**amount** `string` *required*

**decimals** `number` *required*

**display\_decimals** `number` *required*

**estimatedFee** `WithdrawalMoney` *required*

**properties**

**currency** `string` *required*

**amount** `string` *required*

**decimals** `number` *required*

**display\_decimals** `number` *required*

**estimatedArrival** `string | null`

**payoutQuoteLoading** `boolean`

Whether an exact payout quote is being created. Defaults to `false`.

**payoutQuoteError** `string`

The error from the current exact payout quote request, if any. Defaults to `""`.

**payoutQuoteRetryRequired** `boolean`

Whether the last quote request was inconclusive and must be retried with the same request ID. Defaults to `false`.

**withdrawalRetryRequired** `boolean`

Whether the last payout result was inconclusive and must be retried exactly. Defaults to `false`.

**unresolvedWithdrawalRequest** `WithdrawalRequest | null`

The exact unresolved payout request to retry after the element remounts. Defaults to `null`.

**properties**

**amount** `number` *required*

**currency** `string` *required*

**payoutMethodId** `string` *required*

**presentedFee** `number` *required*

**quoteId** `string`

**quoteToken** `string`

**speed** `"standard" | "instant"` *required*

**notes** `string`

**acknowledgeBankWarning** `boolean`

**savingMethod** `boolean`

Whether a new payout method is being saved. Defaults to `false`.

**submitting** `boolean`

Whether the confirmed payout is being created. Defaults to `false`.

**bankWarning** `boolean`

True when the last payout was refused because the bank could not confirm the account holder name. Shows a fix-or-confirm choice; confirming re-emits withdrawalRequested with acknowledgeBankWarning set. Set this back to false when handling each withdrawalRequested — the choice appears on the change to true, so a flag left true hides every later refusal. Defaults to `false`.

**createdMethodId** `string`

The most recently created method, selected when it appears in methods. Defaults to `""`.

**plaidUnavailable** `boolean`

Set when the payer backed out of Plaid Link or it failed to open. Unlocks manual entry on a rail that otherwise requires linking. Defaults to `false`.

**linkingBank** `boolean`

Whether a bank link is in progress — the Plaid session is open, or the linked method is being saved. Defaults to `false`.

## Events

Pass callbacks in the create options or React props.

### `onAmountChanged`

The amount changed and should be repriced.

**payload** `object`

**properties**

**amount** `number` *required*

### `onCountryChanged`

The add-method country changed and supported methods should be refreshed.

**payload** `object`

**properties**

**country** `string` *required*

### `onSupportedMethodChanged`

A supported payout method was selected and its exact required fields should be loaded.

**payload** `object`

**properties**

**country** `string` *required*

**supportedPayoutMethodId** `string` *required*

**destinationCurrency** `string` *required*

### `onAddMethodRequested`

The payer submitted the dynamic add-payout-method form.

**payload** `WithdrawalCreateMethodInput`

**properties**

**country** `string` *required*

**supportedPayoutMethodId** `string` *required*

**destinationCurrency** `string` *required*

**nickname** `string` *required*

**fields** `Record\<string, string\>` *required*

### `onPlaidLinkRequested`

The payer chose to link their bank through Plaid instead of typing details. Open Plaid Link on receipt.

**payload** `object`

**properties**

**country** `string` *required*

**supportedPayoutMethodId** `string` *required*

**destinationCurrency** `string` *required*

### `onMethodVerificationCompleted`

The newly added payout method finished its verification step.

### `onRenameMethodRequested`

The payer requested to rename a saved payout method.

**payload** `object`

**properties**

**methodId** `string` *required*

### `onRemoveMethodRequested`

The payer requested to remove a saved payout method.

**payload** `object`

**properties**

**methodId** `string` *required*

### `onPayoutQuoteRequested`

The payer requested an exact short-lived quote before confirming a payout.

**payload** `WithdrawalPayoutQuoteRequest`

**properties**

**requestId** `string` *required*

**amount** `number` *required*

**currency** `string` *required*

**payoutMethodId** `string` *required*

**speed** `"standard" | "instant"` *required*

### `onWithdrawalRequested`

The payer confirmed the payout. When the host is driving the flow, create the payout on receipt. When the element created it, this fires after success so the host can refresh balances — do not create a second payout.

**payload** `WithdrawalRequest`

**properties**

**amount** `number` *required*

**currency** `string` *required*

**payoutMethodId** `string` *required*

**presentedFee** `number` *required*

**quoteId** `string`

**quoteToken** `string`

**speed** `"standard" | "instant"` *required*

**notes** `string`

**acknowledgeBankWarning** `boolean`

### `onDone`

The payer finished reviewing a successful payout.

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

Re-fetch saved payout methods and limits. Call it after anything on your side changes what the account can pay out to.

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

**options** `Partial\<WithdrawElementProps\>` *required*

## Styling

This element doesn't expose class names for styling. Use `appearance` (theme, accent color, variables) to restyle it.


## Related topics

- [Wallet](https://docs.whop.com/elements/latest/wallet/overview.md)
- [ActionsElement](https://docs.whop.com/elements/latest/wallet/actions.md)
- [SendElement](https://docs.whop.com/elements/latest/wallet/send.md)
