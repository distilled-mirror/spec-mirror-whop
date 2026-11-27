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

# SendElement

> Sends money from an account to a recipient — a user, another account, or a public claim link anyone can redeem. Direct transfers use the source account's configured fiat or crypto rail. Renders its own recipient search resolved from the account ID with no credentials beyond the account's own token. Needs an `accessToken` scoped to `payout:transfer_funds` for direct transfers and `payout:withdraw_funds` for recipient lists; account recipient search additionally needs `company:authorized_user:read` and `member:basic:read`, and account claim links need `airdrop_link:manage` — a host without one of those scopes should turn off the matching prop rather than leave a row that will 403.

> [!NOTE]
>
> This page documents `@whop/elements@1.10.0` and `@whop/elements-react@1.10.0`.

*Since `v1.0.0`.*

Mounts inside [`Wallet`](https://docs.whop.com/elements/latest/wallet/overview). `accountId` and `accessToken` come from there. Pass props and callbacks through the create options or React props. Keep the created handle, or React `ref`, to call `refresh()`.

> [!NOTE]
>
> You can mount this element **inline** (`create`) or open it as a **modal** overlay (`createOverlay`).

```tsx React theme={null}
import { WhopElements, Wallet, SendElement } from "@whop/elements-react";
import { loadWhop } from "@whop/elements";

function Example() {
  return (
    <WhopElements elements={loadWhop()}>
      <Wallet /* options */>
        <SendElement onStepChanged={(e) => console.log(e)} onSendCompleted={(e) => console.log(e)} onLinkCreated={(e) => console.log(e)} onDone={(e) => console.log(e)} />
      </Wallet>
    </WhopElements>
  );
}
```

```html JavaScript theme={null}
<script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>
<script type="module">
  const wallet = window.WhopElements().wallet.create({ /* options */ });
  wallet.create('send', {
    onStepChanged: (e) => console.log(e),
    onSendCompleted: (e) => console.log(e),
    onLinkCreated: (e) => console.log(e),
    onDone: (e) => console.log(e)
  }).mount('#wallet-send');
</script>
```

Example data. [Open the Playground](https://docs.whop.com/elements/latest/wallet/overview#playground).

## Props

**accessToken** `string`

A scoped token for the privileged read/write — needs `payout:transfer_funds` for direct transfers, `payout:withdraw_funds` for recipient lists, and `airdrop_link:manage` for claim links. Omitted, the read carries the viewer's own session, which only answers same-origin.

**title** `string`

Overrides the recipient-picker header. Omitted, the header is "Choose recipient".

**canSearchRecipients** `boolean`

Enables recipient lookup. Suggests people and businesses with fewer than six saved recipients, and searches from Add recipient after typing. Defaults to `true`.

**canCreateLink** `boolean`

Shows the claim-link action in the recipient picker. Turn off for a host without `airdrop_link:manage`. Defaults to `true`.

**initialRecipient** `SendRecipient | null`

Skip the picker and open straight onto this recipient's amount screen — for a "pay this person" action row. No back button is shown in that case, matching there being no picker behind it. Defaults to `null`.

**properties**

**id** `string` *required*

**name** `string | null` *required*

**username** `string | null` *required*

**profilePicUrl** `string | null` *required*

**kind** `"email" | "business" | "user"` *required*

**initialCurrency** `string`

Preselect a currency in the amount screen's picker. Falls back to `currency`, then USD. Defaults to `""`.

## Events

Pass callbacks in the create options or React props.

### `onStepChanged`

The visible screen changed.

**payload** `object`

**properties**

**step** `string` *required*

One of `"success"`, `"enter_amount"`, `"select_recipient"`, `"add_recipient"`, `"create_link"`, or `"link_success"`.

### `onSendCompleted`

A transfer succeeded. Refresh anything on your side reading the account's balance or activity.

**payload** `object`

**properties**

**recipient** `SendRecipient | Omit\<SendRecipient, "kind"\> & object` *required*

**SendRecipient**

**id** `string` *required*

**name** `string | null` *required*

**username** `string | null` *required*

**profilePicUrl** `string | null` *required*

**kind** `"email" | "business" | "user"` *required*

**shape 2**

**id** `string` *required*

**name** `string | null` *required*

**username** `string | null` *required*

**profilePicUrl** `string | null` *required*

**kind** `"bill_pay"` *required*

**nickname** `string | null`

**after** `string`

**methodName** `string` *required*

**destinationCurrency** `string` *required*

**amount** `number` *required*

**currency** `string` *required*

### `onLinkCreated`

A claim link was created. Refresh anything on your side reading the account's balance.

**payload** `object`

**properties**

**claimUrl** `string` *required*

**amount** `number` *required*

**redeemableCount** `number` *required*

### `onDone`

The viewer clicked "Done" on a success screen — close the surface holding it.

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

Re-fetch the account's balances and recipient list. Call it after anything on your side changes what it can offer.

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

**options** `Partial\<SendElementProps\>` *required*

## Styling

Style these parts through `appearance.classes`. Use camel case or kebab case for property names and include units. Page stylesheets can't reach the element's frame. The framework validates each declaration before injecting it.

| Class | Targets |
| - | - |
| `.whop-AddressSuggestion` | One suggestion row in the autocomplete overlay |
| `.whop-AddressSuggestionActive` | Added to the keyboard/pointer-active suggestion row |
| `.whop-AddressSuggestionManual` | The "Enter address manually" row closing the suggestions list |
| `.whop-AddressSuggestionsEmpty` | The quiet line shown when the query settled with no address matches |
| `.whop-SendSurface` | The send element root — one screen at a time |

```ts theme={null}
const wallet = whop.wallet.create({
  appearance: {
    classes: {
      'whop-AddressSuggestion': { borderRadius: '8px', fontWeight: '600' },
      'whop-AddressSuggestionActive': { borderRadius: '8px', fontWeight: '600' },
      'whop-AddressSuggestionManual': { borderRadius: '8px', fontWeight: '600' }
    }
  }
});

// 5 classes use this shape
wallet.update({
  appearance: {
    classes: { 'whop-AddressSuggestion': { fontWeight: '700' } }
  }
});
```

In React, pass `appearance` to `<Wallet>`. Set it globally with `WhopElements({ appearance })`.


## Related topics

- [Wallet](https://docs.whop.com/elements/latest/wallet/overview.md)
- [ActionsElement](https://docs.whop.com/elements/latest/wallet/actions.md)
- [Chat Element](https://docs.whop.com/developer/guides/chat/chat-element.md)
