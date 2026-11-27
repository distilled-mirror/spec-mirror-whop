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

# AddressElement

> Collects a billing or shipping address. Fields, order, and validation follow the selected country. Includes street autocomplete (full screen on phones) and methods to read or validate the address.

> [!NOTE]
>
> This page documents `@whop/elements@1.10.0` and `@whop/elements-react@1.10.0`.

*Since `v1.0.0`.*

Mounts inside [`Payments`](https://docs.whop.com/elements/latest/payments/overview). Pass props and callbacks through the create options or React props. Keep the created handle, or React `ref`, to call `validate()` and `getValues()`.

> [!NOTE]
>
> **One per `mode`.** Mount one `<AddressElement />` for each `mode` value under one Payments handle. A second mount with the same `mode` is refused. An element's `mode` is set when you create it; to change it, destroy the element and create a new one.

Mounts inside a `WhopPayments` scope. Collects the billing address in the selected country's own format, and the controller reads it for the mint whether or not you bind it.

Mount inside `<Payments>`, which owns the charge and the confirmation token. `<Payments>` itself mounts inside `<WhopElements>`. It renders the billing address in the buyer's own country format and publishes it to the provider, so `createConfirmationToken` picks it up without being passed anything.

```tsx React theme={null}
import { WhopElements, Payments, AddressElement } from "@whop/elements-react";
import { loadWhop } from "@whop/elements";

function Example() {
  return (
    <WhopElements elements={loadWhop()}>
      <Payments /* options */>
        <AddressElement onChange={(e) => console.log(e)} />
      </Payments>
    </WhopElements>
  );
}
```

```tsx React Native theme={null}
import { ScrollView } from 'react-native';
import { AddressElement, Payments } from '@whop/elements-react-native';

export function BillingStep() {
  return (
    <Payments accountId="biz_xxxxxxxx" plan="plan_xxxxxxxx">
      <ScrollView>
        <AddressElement
          layout="full"
          scope="full"
          name="combined"
          line2="toggle"
          onChange={({ complete, address }) => console.log(complete, address.country)}
        />
      </ScrollView>
    </Payments>
  );
}
```

```html JavaScript theme={null}
<script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>
<script type="module">
  const payments = window.WhopElements().payments.create({ /* options */ });
  payments.create('address', { onChange: (e) => console.log(e) }).mount('#payments-address');
</script>
```

```swift Swift theme={null}
import Elements
import SwiftUI

// .whopElements(environment:) runs once at the app root. See Getting started.
struct CheckoutScreen: View {
    @State private var address = PostalAddress()
    @State private var addressComplete = false

    var body: some View {
        WhopPayments(accountID: "biz_xxxx", charge: .plan(id: "plan_xxxx")) { payments in
            ScrollView {
                VStack(alignment: .leading, spacing: 20) {
                WhopAddressElement(address: $address, isComplete: $addressComplete)
                    WhopBrandingElement()
                }
                .padding()
            }
        }
    }
}
```

Example data. [Open the Playground](https://docs.whop.com/elements/latest/payments/overview#playground).

[AddressElement running on Android, in the React Native example app](https://app.revyl.ai/embed/d5ebb0fa-b4cb-48b7-8fd4-033f844518c0?controls=0)

## Props

**mode** `"billing" | "shipping"`

Which address this collects: `billing` (default) or `shipping`. Also sets the browser autocomplete purpose. You can mount one of each under one handle. Billing details come from the billing element, or from the shipping element when no billing element is mounted. Defaults to `"billing"`.

**name** `"split" | "none" | "combined"`

The name row: one full-name field (default), split first/last fields, or none. Defaults to `"combined"`.

**line2** `"never" | "toggle" | "always"`

Address line 2: always visible (default), revealed by a text button (`toggle`), or never collected. Defaults to `"always"`.

**defaultValues** `object`

Seed values applied once before first paint (`address.country` is an ISO 3166-1 alpha-2 country code). Takes precedence over IP-country detection.

**properties**

**name** `string`

**address** `object`

**properties**

**name** `string`

**first\_name** `string`

**last\_name** `string`

**organization** `string`

**organization\_type** `"business" | "individual"`

**line1** `string`

**line2** `string`

**city** `string`

**state** `string`

**postal\_code** `string`

**country** `string`

**layout** `"full" | "compact" | "wrap"`

`full` (default) stacks labeled fields. `compact` groups placeholder-labeled fields within one border. `wrap` keeps `full`'s separate fields and also puts city, state and postal code side by side. Those and the pairs `full` already sets side by side (first and last name, organization) fit as many to a row as the width allows and wrap as it narrows. Defaults to `"full"`.

**labelMode** `"above" | "floating"`

`above` (default) keeps each label outside its field: above it in the `full` layout, as its placeholder in `compact`. `floating` uses each label as the placeholder of its field, in either layout, then shows a small label above the typed text once the field has a value. Defaults to `"above"`. Since `v1.5.0`.

**scope** `"minimal" | "full"`

`full` (default) follows the selected country's complete address format. `minimal` collects only country and postal code. `name` independently adds required name fields. Set `name` to `none` to omit them. This element does not add fields required by a payment method. Card confirmation requires a name, country, and postal code, so keep `name` enabled or pass `billingDetails.name`. Defaults to `"full"`.

**organization** `"none" | "name" | "name_with_type"`

Organization fields: none (default), name only, or name with a business/individual selector. Defaults to `"none"`.

**detectCountry** `boolean`

Default the country from the buyer's IP (resolved before first paint). Falls back to the controller's `countryHint`, then US. Defaults to `true`.

**allowedCountries** `string[]`

Allowed ISO 3166-1 alpha-2 country codes. With one code, the selector remains visible but offers only that country.

**autocomplete** `boolean`

Enables Google Places suggestions for the street address. If no match appears or Places is blocked, the list reports no matches and lets buyers enter the address manually. On a phone, the suggestions open full screen from a search button in the street field instead of under it, where the keyboard would cover them. Set `false` for a plain input. Defaults to `true`.

**expanded** `boolean`

Shows every address field from the start. By default (`false`) the form opens with the street address and reveals the rest once the buyer picks a suggestion, types an address, or chooses to enter it manually. Defaults to `false`. Since `v1.6.0`.

**customFields** `object[]`

Additional fields rendered with the address form. Values are validated and emitted in the separate `custom` map.

**properties**

**key** `string` *required*

**label** `string` *required*

**type** `"phone" | "text" | "select" | "date"` *required*

**position** `string` *required*

One of `"after_name"`, `"after_organization"`, `"before_country"`, or `"after_address"`.

**required** `boolean`

**options** `string[]`

**format** `string`

**autocomplete** `string`

**countryHint** `string`

Fallback country after `defaultValues` and IP detection. Use an ISO 3166-1 alpha-2 code. An empty value falls back to `US`. Defaults to `""`.

## Events

Pass callbacks in the create options or React props.

### `onChange`

Fires when the address changes. `complete` is true when all country-specific fields and required custom fields are valid. `address` follows confirmation-token `billing_details` names, including `postal_code`. `country` is an ISO 3166-1 alpha-2 code. Custom values are in `custom`.

**payload** `object`

**properties**

**complete** `boolean` *required*

**address** `object` *required*

**properties**

**name** `string`

**first\_name** `string`

**last\_name** `string`

**organization** `string`

**organization\_type** `"business" | "individual"`

**line1** `string`

**line2** `string`

**city** `string`

**state** `string`

**postal\_code** `string`

**country** `string` *required*

**custom** `Record\<string, string\>` *required*

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

### `validate`

Validates the form, reveals all inline errors, and never throws. Returns `complete`, `address`, and `errors`. `errors` is empty exactly when `complete` is true. Keys use contract names such as `postal_code` and `line1`, `custom:<key>` for custom fields, and transient `form` while loading.

**Returns** `Promise<object>`

**complete** `boolean` *required*

**address** `object` *required*

**properties**

**name** `string`

**first\_name** `string`

**last\_name** `string`

**organization** `string`

**organization\_type** `"business" | "individual"`

**line1** `string`

**line2** `string`

**city** `string`

**state** `string`

**postal\_code** `string`

**country** `string` *required*

**errors** `Record\<string, string\>` *required*

### `getValues`

Returns `address` and `custom` without validation or revealing errors.

**Returns** `Promise<object>`

**address** `object` *required*

**properties**

**name** `string`

**first\_name** `string`

**last\_name** `string`

**organization** `string`

**organization\_type** `"business" | "individual"`

**line1** `string`

**line2** `string`

**city** `string`

**state** `string`

**postal\_code** `string`

**country** `string` *required*

**custom** `Record\<string, string\>` *required*

### `mount`

Mounts the element in `target` and starts loading. React components mount themselves.

**Parameters**

**target** `string | HTMLElement` *required*

### `destroy`

Removes the element and releases its frame and subscriptions. You can call it more than once. React removes the element automatically.

### `update`

Merges new props into the mounted element. In React, change the component props instead.

**Parameters**

**options** `Partial\<AddressElementProps\>` *required*

## Styling

Style these parts through `appearance.classes`. Use camel case or kebab case for property names and include units. Page stylesheets can't reach the element's frame. The framework validates each declaration before injecting it.

| Class | Targets |
| - | - |
| `.whop-Address` | The address form root |
| `.whop-AddressErrorSummary` | The summary line shown when validation reveals missing or invalid fields |
| `.whop-AddressField` | One field cell in the address form |
| `.whop-AddressFieldError` | The error line for an address field: under it in the full layout, under the group for a composer message in the compact layout |
| `.whop-AddressFieldInput` | A text input in the address form |
| `.whop-AddressFieldInputInvalid` | Added to an address input while it fails validation |
| `.whop-AddressFieldInvalid` | Added to a compact field cell while it fails validation |
| `.whop-AddressFieldLabel` | Address field label: above the field, or inside it with floating labels |
| `.whop-AddressFieldSelect` | A select (country, state, organization type) in the address form |
| `.whop-AddressGroup` | The bordered group holding every field in the compact layout |
| `.whop-AddressLine2Toggle` | Collapsed address line 2 toggle |
| `.whop-AddressManualEntry` | The "Enter address manually" text button below the collapsed form — expands the full country format |
| `.whop-AddressSearchButton` | The search button at the trailing edge of the street field on a phone |
| `.whop-AddressSuggestion` | One suggestion row in the autocomplete overlay |
| `.whop-AddressSuggestionActive` | Added to the keyboard/pointer-active suggestion row |
| `.whop-AddressSuggestionManual` | The "Enter address manually" row closing the suggestions list |
| `.whop-AddressSuggestions` | The autocomplete suggestions overlay anchored to the address line 1 field |
| `.whop-AddressSuggestionsEmpty` | The quiet line shown when the query settled with no address matches |

```ts theme={null}
const payments = whop.payments.create({
  appearance: {
    classes: {
      'whop-Address': { borderRadius: '8px', fontWeight: '600' },
      'whop-AddressErrorSummary': { borderRadius: '8px', fontWeight: '600' },
      'whop-AddressField': { borderRadius: '8px', fontWeight: '600' }
    }
  }
});

// 18 classes use this shape
payments.update({
  appearance: { classes: { 'whop-Address': { fontWeight: '700' } } }
});
```

In React, pass `appearance` to `<Payments>`. Set it globally with `WhopElements({ appearance })`.

## Parameters

**address** `Binding\<PostalAddress\>?`

Reads the collected address back out. The controller collects it either way.

**isComplete** `Binding\<Bool\>?`

True once every field the country requires is filled and valid.

**mode** `WhopAddressElement.Mode`

`.billing` or `.shipping`. Shipping asks the keyboard for shipping fields. Defaults to `.billing`.

**layout** `WhopAddressElement.Layout`

`.full` labels every field and stacks them; `.compact` moves the labels into placeholders. Defaults to `.full`.

**scope** `WhopAddressElement.Scope`

`.full` collects the country's whole format; `.minimal` collects country and postal code only. Defaults to `.full`.

**name** `WhopAddressElement.NameFields`

`.combined` for one full-name field, `.split` for first and last, `.none` to leave the name out. Defaults to `.combined`.

**organization** `WhopAddressElement.OrganizationFields`

`.none`, `.name` for an organization name, or `.nameWithType` to also ask whether it is a business or an individual. Defaults to `.none`.

**line2** `WhopAddressElement.Line2Field`

`.always` shows the second line, `.toggle` reveals it with a button, `.never` leaves it out. Defaults to `.toggle`.

**defaultValues** `PostalAddress?`

Values to start from. Its `country` is an ISO 3166-1 alpha-2 code and takes precedence over detection.

**detectCountry** `Bool`

Starts on the buyer's own country when true. Falls back to `countryHint`, then the charge currency's country, then US. Defaults to `true`.

**allowedCountries** `[String]?`

Restricts the picker to these ISO 3166-1 alpha-2 codes, intersected with the selected method's own constraint.

**countryHint** `String?`

ISO 3166-1 alpha-2 fallback for the country chain.

**autocomplete** `Bool`

Street suggestions as the buyer types. With it off there is nothing to pick, so every field shows from the start. Defaults to `true`.

## `PostalAddress`

What a selection hands back:

* `country: String`: ISO 3166-1 alpha-2, uppercased
* `line1` / `line2` / `city` / `state` / `postalCode`: `String?`, in the country's own format
* `name` / `firstName` / `lastName`: `String?`, per the `name` mode
* `organization` / `organizationType`: `String?`, per the `organization` mode

## States

The form renders immediately. With autocomplete on at `.full` scope, the locality fields appear after a suggestion or a manual entry; turn it off and every field shows at once. Errors reveal on the first confirm attempt, then live.

## Good to know

* MapKit provides the street suggestions on-device. Whop never sees the buyer's query as they type.
* `defaultValues.country` beats detection. Detection falls back to `countryHint`, then the charge currency's country, then US.
* `allowedCountries` is intersected with the selected method's own billing-country constraint, so a method that only takes one country narrows the picker on its own.
* `PostalAddress` uses the confirmation-token `billing_details` keys, so the same JSON works across platforms.

## Install

```swift theme={null}
dependencies: [
    .package(url: "https://github.com/whopio/elements-swift.git", from: "0.1.0")
]
```

> [!NOTE]
>
> Mount it inside a `WhopPayments(accountID:charge:)` scope, which creates the controller and hands it to its content. `payments.buyer` is the signed-in buyer once an email sign-in has proven one. `WhopBrandingElement` has to be on screen too, because Whop is merchant of record on these sales and `createConfirmationToken` refuses without it. Style with `.whopElementsAppearance(_:)`. The module is `Elements`, not the wallet SDK's `WhopElements`. See [Getting started](https://docs.whop.com/elements/latest/getting-started.md) and [Appearance](https://docs.whop.com/elements/latest/appearance.md).

## Props

**mode** `'billing' | 'shipping'`

Autocomplete purpose. Defaults to `billing`.

**layout** `'full' | 'compact'`

`full` labels every field and stacks them. `compact` moves the labels into placeholders. Defaults to `full`.

**scope** `'full' | 'minimal'`

`full` follows the country's complete format. `minimal` collects country and postal code only. Defaults to `full`.

**name** `'combined' | 'split' | 'none'`

One full-name field, first and last, or no name field. Defaults to `combined`.

**organization** `'none' | 'name' | 'name_with_type'`

Whether to collect an organization, and whether to also ask if it is a business or an individual. Defaults to `none`.

**line2** `'toggle' | 'always' | 'never'`

How the second address line appears. Defaults to `toggle`, which shows an "Add address line 2" button.

**allowedCountries** `string[]`

Restrict the country selector to these ISO 3166-1 alpha-2 codes. A selected payment method's own country list narrows this further, never widens it.

**detectCountry** `boolean`

Default the country from the buyer's IP.

**countryHint** `string`

Fallback country after `defaultValues` and IP detection. ISO 3166-1 alpha-2.

**defaultValues** `\{ name?: string; address?: Partial\<EmittedAddress\> \}`

Seed values applied once at mount, for a resumed session.

**customFields** `CustomFieldSpec[]`

Extra fields rendered at a declared position (`after_name`, `after_organization`, `before_country`, `after_address`). A required one gates `complete`.

**onChange** `(payload: AddressChangePayload) =\> void`

Fires on every edit with the current snapshot.

**style** `StyleProp\<ViewStyle\>`

Applied to the element's outer `View`. For theming, prefer `appearance.parts` on the provider, which covers every element on this surface. Note the React Native part names are their own set today, not the web's `whop-*` class names, so a web appearance object does not port across unchanged.

**fallback** `ReactNode`

Rendered instead of the built-in skeleton while the element loads.

**onReady** `() =\> void`

Fires once the element is interactive. `<Payments>` groups these, so its own `onLoadingChange` is usually the one you want.

**onError** `(error: \{ message: string; code?: string \}) =\> void`

A load or configuration failure for this element. The element renders its own error face either way.

## `AddressChangePayload`

What `onChange` hands back:

* `complete: boolean`: every field the country requires is filled and valid
* `address: EmittedAddress`: what the buyer has entered so far
* `custom: Record<string, string>`: values for any `customFields` you declared

## States

A three-field skeleton while the country catalog loads, then the form. Changing the country re-plans the fields, because the format is the country's, not a fixed set. `onChange` reports `complete` when every field the country requires is filled and valid.

## Good to know

* The country defaults from the buyer's IP when `detectCountry` is on, then `countryHint`, then US. `defaultValues.address.country` overrides all three.
* `autocomplete` is on by default, as on web, but it is served by the **platform geocoder** rather than Google Places, so it needs no API key, no account and no extra dependency. iOS uses `MKLocalSearchCompleter`, a real as-you-type API that answers a fragment with several candidates. Android uses `android.location.Geocoder`, which resolves a *complete* address rather than predicting from a partial one: expect roughly one result, and a coarser one on a half-typed street. Android ships no keyless typeahead, and the alternative — the Places SDK — would add several megabytes and force location permissions on every app embedding this SDK.
* Either platform falling short is silent by design: a geocoder with no backend, no network or no matches shows nothing and the buyer types the address by hand, which is where they started. Autocomplete never blocks a checkout.
* The emitted address uses the same keys as the web element and the confirmation token's `billing_details`, so one payload shape works across platforms.
* A mounted address element is the provider's billing source. Unmount it and `createConfirmationToken` falls back to whatever you pass it directly.

> [!NOTE]
>
> Wrap your app in `<WhopElements getToken={…}>` once, then mount `<Payments>` around the elements. See [Getting started](https://docs.whop.com/elements/latest/getting-started.md) and [Appearance](https://docs.whop.com/elements/latest/appearance.md).


## Related topics

- [PaymentElement](https://docs.whop.com/elements/latest/payments/payment.md)
- [Getting Started with Elements](https://docs.whop.com/elements/latest/getting-started.md)
- [BrandingElement](https://docs.whop.com/elements/latest/payments/branding.md)
