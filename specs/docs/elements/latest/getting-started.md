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

# Getting Started with Elements

> Install Whop Elements and mount your first element in React, JavaScript, Swift, or React Native.

> [!NOTE]
>
> This page documents `@whop/elements@1.10.0` and `@whop/elements-react@1.10.0`.

Whop Elements are hosted, themeable UI components you embed in your own site. Each element renders in an isolated frame served from Whop's CDN. You install a thin, fully typed package and the element code stays up to date on its own.

Whop Elements are themeable UI components you can import in your iOS app. They ship as a single Swift SPM package. Each one is a plain SwiftUI view that fetches its own data and manages its own state, so a balance screen is a couple of views in a `VStack` instead of a networking layer and a view model. Add the package, tell the SDK how to fetch an access token, and put the elements in your view hierarchy.

Whop Elements are themeable UI components you can drop into a React Native app. They are real React Native components, not web views: the payment method tiles, the address form and the card fields are all native input, and Apple Pay and Google Pay open the platform's own sheet. Install the package, tell the SDK how to fetch an access token, and mount the elements.

## Install

Add the package to your `Package.swift` (or **File → Add Package Dependencies** in Xcode):

Install the SDK:

```bash React theme={null}
npm install @whop/elements-react @whop/elements
```

```bash React Native theme={null}
npm install @whop/elements-react-native
```

```bash JavaScript theme={null}
npm install @whop/elements
```

```swift Swift theme={null}
dependencies: [
    .package(url: "https://github.com/whopio/whopsdk-elements-swift.git", from: "0.1.12")
]
```

## Requirements

| | |
| - | - |
| React | `^17.0.0 \|\| ^18.0.0 \|\| ^19.0.0`, for `@whop/elements-react`. |
| Content Security Policy | Your policy needs every source in the [Content Security Policy table](#content-security-policy) below. |

### Content security policy

| Requirement | Directive | Production source | Sandbox source |
| - | - | - | - |
| Elements frames | `frame-src` | `https://cdn.whop.com` | Same |
| The script tag, if you load it | `script-src` | `https://cdn.whop.com` | Same |
| The Whop API | `connect-src` | `https://api.whop.com` | `https://sandbox-api.whop.com` |
| The Whop pixel | `script-src` | `https://t.whop.tw` | None |
| The Whop pixel | `connect-src` | `https://t.whop.tw` | None |
| Apple Pay, Google Pay, and PayPal | `frame-src` | `https://checkout.paypal.com` `https://assets.braintreegateway.com` `https://www.paypal.com` | `https://checkout.paypal.com` `https://assets.braintreegateway.com` `https://www.sandbox.paypal.com` |
| Apple Pay, Google Pay, and PayPal | `script-src` | `https://pay.google.com` `https://applepay.cdn-apple.com` `https://js.braintreegateway.com` `https://assets.braintreegateway.com` `https://www.paypal.com` `https://c.paypal.com` | `https://pay.google.com` `https://applepay.cdn-apple.com` `https://js.braintreegateway.com` `https://assets.braintreegateway.com` `https://www.sandbox.paypal.com` `https://c.paypal.com` |
| Apple Pay, Google Pay, and PayPal | `connect-src` | `https://pay.google.com` `https://google.com` `https://account.google.com` `https://www.google.com` `https://api.basistheory.com` `https://payments.braintree-api.com` `https://api.braintreegateway.com` `https://api-m.paypal.com` `https://www.paypal.com` | `https://pay.google.com` `https://google.com` `https://account.google.com` `https://www.google.com` `https://api.basistheory.com` `https://payments.sandbox.braintree-api.com` `https://api.sandbox.braintreegateway.com` `https://api-m.sandbox.paypal.com` `https://www.sandbox.paypal.com` |
| Apple Pay, Google Pay, and PayPal | `img-src` | `https://www.paypalobjects.com` | Same |
| Plaid bank account linking | `frame-src` | `https://cdn.plaid.com` | Same |
| Plaid bank account linking | `script-src` | `https://cdn.plaid.com` | Same |
| Plaid bank account linking | `connect-src` | `https://production.plaid.com` | `https://sandbox.plaid.com` |

`contentSecurityPolicy()` from `@whop/elements` returns every source in this table, for the environment you pass. The rows for page-side work run on your page, not in a frame. The scripts elements load for Apple Pay, Google Pay, and PayPal and Plaid bank account linking come from other companies and can load resources beyond these rows. Check each provider's Content Security Policy guidance too.

> [!WARNING]
>
> Elements render in frames served from `cdn.whop.com`. Some of their work runs on your page and calls `api.whop.com` from it, like Apple Pay, Google Pay, and PayPal and Plaid bank account linking. If your CSP blocks either origin, you get blank space instead of the element or a failed request, and the error appears only in the browser console.

> [!NOTE]
>
> A modal element dims your page behind it with an inline style. A `style-src` that doesn't allow inline styles leaves the page undimmed, but the modal still works.

## Before you start

You need two things:

1. **An `accountId`.** An account ID, prefixed `biz_`, or a user ID, prefixed `user_`, for the user's own data. Payment surfaces take a plan or amount instead, and each element page says which it needs.
2. **A server endpoint that creates an access token**, scoped to the elements you mount.

> [!WARNING]
>
> Never put a Whop API key in browser code. It creates tokens, so anyone who opens the browser's developer tools can read the account. Create the token on your server and send only the token.

Your server calls [Create Access Token](https://docs.whop.com/api-reference/access-tokens/create-access-token.md) with the account and the scopes the elements need:

```http theme={null}
POST https://api.whop.com/api/v1/access_tokens
Authorization: Bearer <api_key>
Content-Type: application/json

{ "company_id": "biz_xxxxxxxx", "scoped_actions": ["company:balance:read"] }
```

## Requirements

| | |
| - | - |
| Platform | iOS 18.0+ |
| Toolchain | Swift 6.0 |
| Install | Swift Package Manager |

## Before you start

You need two things:

1. **An `accountId`.** An account ID, prefixed `biz_`, or a user ID, prefixed `user_`, for the user's own balance.
2. **A server endpoint that creates an access token** for the account. Never ship a Whop API key in the app. It creates tokens, so anyone who extracts it can read the account. Create the token on your server and return only that.

A `user_…` tag reads the viewer's own data, so it uses viewer OAuth (`WhopSDK.configureWithOAuth`) instead of a token. The chat views too.

Your server calls [Create Access Token](https://docs.whop.com/api-reference/access-tokens/create-access-token.md) with the account and the scope the elements need:

```http theme={null}
POST https://api.whop.com/api/v1/access_tokens
Authorization: Bearer <api_key>
Content-Type: application/json

{ "company_id": "biz_xxxxxxxx", "scoped_actions": ["company:balance:read"] }
```

## Requirements

| | |
| - | - |
| React Native | `0.87+` on Swift Package Manager, `0.86+` on CocoaPods. New Architecture (Fabric) enabled |
| React | `19.2.3+` |
| iOS | 15.1+ |
| Android | `minSdk 24`, `compileSdk 37` |

> [!NOTE]
>
> **Either iOS dependency manager works.** The package ships both an `ios/Package.swift` and a podspec, and the toolchain picks one. React Native's Swift Package Manager autolinker prefers the `Package.swift`, so an app already on SPM is unaffected by the podspec. An app on CocoaPods, which includes every Expo app today, links through the podspec and needs nothing else. The React Native floor is higher on SPM because `Package.swift` links two header-only XCFrameworks that first shipped in 0.87.

> [!NOTE]
>
> Native code is Swift and Kotlin generated through React Native Codegen: nothing here asks you to hand-edit Objective-C or Java, and there are no native peer dependencies to install.

## Before you start

You need two things:

1. **An `accountId`.** A company ID, prefixed `biz_`.
2. **A server endpoint that mints an access token** for that account. Never ship a Whop API key in the app: it creates tokens, so anyone who extracts it can act as the account. Mint the token on your server and return only that.

Your server calls [Create Access Token](https://docs.whop.com/api-reference/access-tokens/create-access-token.md) and hands the result back to `getToken`.

## Mount your first element

A complete app. `configure` runs once at launch, and the elements render a spinner until it lands, so nothing races it:

A complete checkout. `<WhopElements>` holds the token resolver, `<Payments>` holds the charge, and the elements between them collect it:

```tsx React theme={null}
import { WhopElements, Payments, PaymentElement, AddressElement, CardElement, EmailElement, TaxIdElement, BrandingElement } from "@whop/elements-react";
import { loadWhop } from "@whop/elements";

function Example() {
  return (
    <WhopElements elements={loadWhop()}>
      <Payments /* options */>
        <PaymentElement />
        <AddressElement />
        <CardElement />
        <EmailElement />
        <TaxIdElement />
        <BrandingElement />
      </Payments>
    </WhopElements>
  );
}
```

```tsx React Native theme={null}
import { useRef, useState } from 'react';
import { Button, ScrollView } from 'react-native';
import {
  AddressElement,
  BrandingElement,
  EmailElement,
  PaymentElement,
  Payments,
  WhopElements,
  type PaymentsHandle,
} from '@whop/elements-react-native';

export function Checkout() {
  const payments = useRef<PaymentsHandle | null>(null);
  const [ready, setReady] = useState(false);

  return (
    // Called again before every request, so fetch rather than cache.
    <WhopElements getToken={() => myBackend.whopAccessToken()}>
      <Payments ref={payments} accountId="biz_xxxxxxxx" plan="plan_xxxxxxxx">
        <ScrollView>
          <EmailElement />
          <AddressElement />
          <PaymentElement onChange={(p) => setReady(p.complete)} />
          <BrandingElement />
        </ScrollView>
        <Button
          title="Pay"
          disabled={!ready}
          onPress={async () => {
            const token = await payments.current!.createConfirmationToken();
            // Confirm server-side, then hand the payment's client_secret back:
            // await payments.current!.handleNextAction({ clientSecret });
            console.log(token.confirmationToken);
          }}
        />
      </Payments>
    </WhopElements>
  );
}
```

```html JavaScript theme={null}
<script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>
<script type="module">
  const payments = window.WhopElements().payments.create({ /* options */ });
  payments.create('payment').mount('#payments-payment');
  payments.create('address').mount('#payments-address');
  payments.create('card').mount('#payments-card');
  payments.create('email').mount('#payments-email');
  payments.create('taxId').mount('#payments-taxId');
  payments.create('branding').mount('#payments-branding');
</script>
```

```swift Swift theme={null}
import SwiftUI
import WhopElements

/// The SDK calls this again before the token expires, so fetch rather than cache.
final class WalletTokenProvider: WhopTokenProvider {
    func getToken() async -> WhopTokenResponse {
        WhopTokenResponse(accessToken: await myBackend.whopAccessToken())
    }
}

@main
struct MyApp: App {
    var body: some Scene {
        WindowGroup {
            WalletScreen()
                .task { await WhopSDK.configure(tokenProvider: WalletTokenProvider()) }
        }
    }
}

struct WalletScreen: View {
    var body: some View {
        ScrollView {
            VStack(spacing: 24) {
                BalanceElement(accountId: "biz_xxxxxxxx")
                ListElement(accountId: "biz_xxxxxxxx")
            }
            .padding()
        }
    }
}
```

Prefer npm over the script tag? `loadWhop()` injects the same hosted script and resolves the global constructor:

```ts theme={null}
import { loadWhop } from "@whop/elements";

const whop = (await loadWhop())({ locale: "en" });
const payments = whop.payments.create({ /* options */ });
```

## Authentication

Pass an `accessToken` when you create the handle. **Use one token for the whole handle, not one per element.** The handle sends that token to every element under it, so ask for all the scopes you need on that one token. Each element page lists the scopes it reads.

```tsx React theme={null}
<Ads accountId="biz_xxxxxxxx" accessToken={token}>
  {/* elements */}
</Ads>
```

```ts JavaScript theme={null}
const ads = whop.ads.create({ accountId: "biz_xxxxxxxx", accessToken });
```

The token is a value you set, not a callback the SDK calls. Set a new one before it expires:

```ts theme={null}
ads.update({ accessToken: await createAccessToken() });
```

> [!NOTE]
>
> If you omit `accessToken`, requests use the viewer's session cookie instead. This works **only on whop.com**. The API does not send `Access-Control-Allow-Credentials` on cross-origin preflights, so a page on your own domain has no session to fall back to and must pass a token.

## Global configuration

Everything you pass at construction applies to every element group created from that instance (a handle's own options can override per group):

**appearance** `Appearance`

Visual customization for every element — `theme` (light/dark + palettes), `variables` (CSS custom properties), and `classes` (per-part style declarations). The color scheme is applied before an element's first paint, so dark pages never flash light. See [Appearance](https://docs.whop.com/elements/latest/appearance.md).

**locale** `string`

Locale for element UI text — one of the app's built locales; any other value falls back to the default locale. Defaults to `"en"`.

One of `"en"`, `"es"`, `"zh"`, `"nl"`, `"pt"`, `"de"`, `"hu"`, `"it"`, `"fr"`, `"ja"`, `"pl"`, or `"tr"`.

**environment** `"production" | "sandbox"`

Which Whop API environment the elements talk to — `"sandbox"` targets the sandbox API (test data; no real money moves). Choosing an environment is the only way to change where the elements send what a buyer types, and both environments are Whop's own. The sandbox environment is not yet generally available. Defaults to `"production"`.

**toasts** `boolean`

Whether elements may show toast notifications on your page — brief status messages ("Payment method added", "Payment failed") rendered in the bottom-right corner. Set `false` to turn them off entirely. Defaults to `true`.

**skipPixel** `boolean`

Skip the Whop pixel. By default, elements load the Whop ads pixel onto a page that does not already run it, scoped to the account they work for (an element group's `accountId`, or the seller of a checkout's plan), and link element analytics to the pixel's visitor id, a one-time identity link that makes element activity joinable to your ad-attribution data in Whop's identity graph. Set `true` to do neither. Usage analytics themselves are unaffected (turn those off per handle with `analytics: false`). Defaults to `false`.

```ts theme={null}
const whop = WhopElements({
  locale: 'es',
  appearance: { theme: { appearance: 'dark', accentColor: 'blue' } }
});
```

In React, the same object rides `<WhopElements appearance={…} locale={…}>`.

The `toasts` global is live on this page — the button below sends one through the same rail elements use, themed by this page's `appearance`:

## What the elements handle, and what you own

Each element fetches its own data and renders its own loading, empty, and error states.

You handle **navigation and storage**. Elements report selections and results as events, and never write to your database.

The exception is a flow that leaves the page. Where an element takes a `returnUrl`, it navigates the tab there, so the buyer can end up off your page. The buyer arrives there after a failed or abandoned off-site step as well as after a successful one, with `status` (`succeeded`, `failed`, or `canceled`) and the attempt's ID appended to the URL, so the page at `returnUrl` has to read `status` before it shows a success message. Fulfill from webhooks rather than from a browser callback.

Elements run in frames, so only serializable values cross the boundary. There is no shared DOM, and object identity is not preserved.

## Troubleshooting

| What you see | Why |
| - | - |
| Blank space instead of the element | Your CSP blocks `cdn.whop.com`. Add it to `frame-src`, and to `script-src` if you use the script tag. |
| The console shows a Content Security Policy violation | Your policy is missing a source. Compare it with the Requirements table. `contentSecurityPolicy()` returns every source in that table, and another company's script can need more of its own. |
| `mount()` does nothing | The selector matched no node. The target must already be in the DOM. |
| The element loads but stays empty | The token is missing a scope that element reads. Check the scopes listed on the element's page. |
| Requests fail on your own domain | No token was passed. The session fallback works only on whop.com. |
| It worked, then stopped | The token expired. Create a new one and pass it with `update({ accessToken })`. |

## Available elements

> [card] [Payments](https://docs.whop.com/elements/latest/payments/overview)
>
> Collect a payment from a `plan_` ID or inline currency and amount, or save a payment method without charging with `mode: "setup"`.

> [card] [Verifications](https://docs.whop.com/elements/latest/verifications/overview)
>
> Collects and verifies identity information for an account.

> [card] [Checkout](https://docs.whop.com/elements/latest/checkout/overview)
>
> Drives a full hosted checkout for one or more plans — itemized price summary, promo codes, the currency the buyer pays in, and the whole payment collection surface (the payments elements, composed inside) — against the Whop checkout sessions API.

> [card] [Ads](https://docs.whop.com/elements/latest/ads/overview)
>
> An advertising account.

> [card] [Tracking](https://docs.whop.com/elements/latest/tracking/overview)
>
> An account's tracked audience: everyone its pixel has seen, and every event they performed.

> [card] [Wallet](https://docs.whop.com/elements/latest/wallet/overview)
>
> Drives an account's money surfaces.

> [card] [Websites](https://docs.whop.com/elements/latest/websites/overview)
>
> An account's websites: every site built on whop.site plus every domain the Whop Pixel reports, with traffic and attributed revenue per domain.

> [card] [Dashboard](https://docs.whop.com/elements/latest/dashboard/overview)
>
> An account's own dashboard surfaces, embedded on your site.

> [card] [Delivery](https://docs.whop.com/elements/latest/delivery/overview)
>
> Customer delivery tracking for a business.

> [card] [Chat](https://docs.whop.com/elements/latest/chat/overview)
>
> Real-time chat with a channel list and message search.

## Next

* [Appearance](https://docs.whop.com/elements/latest/appearance.md): theming, CSS variables, and per-part restyling
* [Create Access Token](https://docs.whop.com/api-reference/access-tokens/create-access-token.md): the endpoint your server mints from

## Authentication

`getToken()` is your only integration point, and the SDK calls it **again before the token expires**, so return a fresh token each time rather than caching one:

```swift theme={null}
final class WalletTokenProvider: WhopTokenProvider {
    func getToken() async -> WhopTokenResponse {
        // your backend, not the Whop API directly
        WhopTokenResponse(accessToken: await myBackend.whopAccessToken())
    }
}
```

Configure once, anywhere above the elements. `WhopSDK` is process-wide, so a second call is only needed when the account changes:

```swift theme={null}
await WhopSDK.configure(tokenProvider: WalletTokenProvider())
```

Chat and DMs authenticate the **viewer** instead of an account, through OAuth: `WhopSDK.configureWithOAuth(appId:)`.

## Theming

`.whopTheme(_:)` propagates through the SwiftUI environment, so one call themes every element beneath it. Six roles, each a `WhopTint`:

```swift theme={null}
ListElement(accountId: "biz_xxxxxxxx")
    .whopTheme(WhopTheme(accent: .blue, danger: .red, success: .green))
```

Defaults are `accent: .blue`, `neutral: .gray`, `danger: .red`, `info: .sky`, `success: .green`, `warning: .amber`. `WhopTheme.default` is all six. Views follow the system light/dark appearance on their own.

## What the elements handle, and what you own

Each view fetches its own data and renders its own loading, empty and error states, including a retry. You own **navigation**: the selection callbacks hand you a value and never push a screen.

```swift theme={null}
ListElement(accountId: account) { balance in
    path.append(Route.asset(balance.symbol))   // your router, your screen
}
```

## Troubleshooting

| What you see | Why |
| - | - |
| `Authentication failed` | The token is missing, expired, or is an **API key** rather than an access token. `getToken()` must return a token minted by [Create Access Token](https://docs.whop.com/api-reference/access-tokens/create-access-token.md). |
| A view stays empty | The token is valid but not scoped for that read. Balances need `company:balance:read`. |
| `Couldn't load this account` | The `accountId` is not one this token can read. |
| A chart with no line | Balance history is available only for account IDs prefixed `biz_`, not personal accounts prefixed `user_`. |

## Available on iOS

The wallet views, each with a live simulator on its page:

* [`BalanceElement`](https://docs.whop.com/elements/latest/wallet/balances-balance#swift): the total balance, its trend, and the range picker
* [`ListElement`](https://docs.whop.com/elements/latest/wallet/balances-list#swift): the holdings list
* [`ActivityElement`](https://docs.whop.com/elements/latest/wallet/activity#swift): the ledger activity feed

`WhopChatView` and `WhopDMsListView` ship in the same package for viewer-authenticated chat. See the [README](https://github.com/whopio/whopsdk-elements-swift).

## Payments on iOS

The payments elements ship from their own package, [`whopio/elements-swift`](https://github.com/whopio/elements-swift), vending an `Elements` module. The wallet SDK's module is `WhopElements`, so the two install side by side:

```swift theme={null}
dependencies: [
    .package(url: "https://github.com/whopio/elements-swift.git", from: "0.1.0")
]
```

The module is `Elements`, not `WhopElements`, so `import Elements` is the one line that changes. Setup differs too, and it splits the way SwiftUI splits: configuration inherits from the app root, and one charge is a scope. Nothing needs a token, because a confirmation token is minted from what the buyer typed and confirmed on your server:

```swift theme={null}
// once, at the app root
WindowGroup {
    CheckoutScreen()
        .whopElements(environment: .production)
}

// one charge, and the elements that collect for it
WhopPayments(accountID: "biz_xxxxxxxx", charge: .plan(id: "plan_xxxxxxxx")) { payments in
    WhopEmailElement()
    WhopPaymentElement()
    WhopBrandingElement()
    Button("Pay") { Task { try await payments.createConfirmationToken() } }
        .disabled(!payments.isComplete)
}
```

* [`WhopPaymentElement`](https://docs.whop.com/elements/latest/payments/payment#swift): the method tiles and whatever the selected method collects
* [`WhopCardElement`](https://docs.whop.com/elements/latest/payments/card#swift) and [`WhopCardFields`](https://docs.whop.com/elements/latest/payments/cardFields#swift): the hosted card inputs, as one unit or placed one by one
* [`WhopAddressElement`](https://docs.whop.com/elements/latest/payments/address#swift): the billing address, in the country's own format
* [`WhopEmailElement`](https://docs.whop.com/elements/latest/payments/email#swift) and [`WhopTaxIDElement`](https://docs.whop.com/elements/latest/payments/taxId#swift)
* [`WhopBrandingElement`](https://docs.whop.com/elements/latest/payments/branding#swift): required beside any payment surface
* [`WhopPaymentRequest`](https://docs.whop.com/elements/latest/payments/paymentRequest#swift): Apple Pay with no element mounted

Card data is captured by PCI-isolated hosted inputs and never enters your process. Style everything with `.whopElementsAppearance(_:)` rather than `.whopTheme(_:)`.

## Authentication

`getToken` is your only integration point, and the SDK calls it **before every request**, so return a fresh token rather than caching one:

```tsx theme={null}
<WhopElements getToken={async () => (await fetch('/api/whop-token')).text()}>
  {/* your app */}
</WhopElements>
```

An element that needs no credentials works without it, so you can render a card form before wiring your backend.

## Taking a payment

Collection and confirmation are separate, and the split is deliberate: the app never sees a card number and your server never trusts the client.

1. The elements collect. Card numbers go straight from PCI-isolated inputs to a token, never through your code.
2. `createConfirmationToken()` returns a **confirmation token**, which is safe to send to your own backend.
3. Your backend confirms the payment with the Whop API.
4. If the API answers that the payment needs more from the buyer, hand its `client_secret` to `handleNextAction()` and the SDK runs the 3D Secure or redirect step.

```tsx theme={null}
const token = await payments.current!.createConfirmationToken();
const { status, clientSecret } = await myBackend.confirm(token.confirmationToken);
if (status === 'requires_action') await payments.current!.handleNextAction({ clientSecret });
```

## Redirects and 3D Secure

Off-app steps open the **system browser** (`ASWebAuthenticationSession` on iOS, Custom Tabs on Android), never a WebView, so the issuer's page stays outside your app's trust boundary and its cookies stay outside your app's jar.

Set `returnUrl` on `<Payments>` to an **https** URL you host. The API refuses anything but https or loopback (`PaymentsApi::ValidateReturnUrl`), so a custom app scheme will not work here — and you do not need one: `handleNextAction` polls the payment to rest and closes the browser itself, rather than waiting on a deep link back into your app.

```tsx theme={null}
<Payments accountId="biz_xxxxxxxx" plan="plan_xxxxxxxx" returnUrl="https://example.com/checkout/return">
```

## Apple Pay and Google Pay

Wallet tiles appear once the platform can present a sheet, and there is nothing to set up for either. Apple Pay uses the merchant registered on the Whop account, so your app needs no merchant identifier and no Xcode capability. `googlePayMerchantName` on `<Payments>` only sets the name shown in the sheet.

## Theming

`appearance` on `<WhopElements>` themes every element beneath it, with the same `theme` object the web elements take. See [Appearance](https://docs.whop.com/elements/latest/appearance.md).

## What the elements handle, and what you own

Each element fetches its own data and renders its own loading, empty and error states. You own **navigation and submission**: the elements report through `onChange` and never push a screen or call your backend.

## Troubleshooting

| What you see | Why |
| - | - |
| `BrandingRequiredError` on confirm | No `BrandingElement` is mounted. Mount one inside `<Payments>`. |
| `ChargeConfigError` | `<Payments>` has neither a `plan` nor an `amount` and `currency` pair. |
| No wallet tile | The account has no Apple Pay merchant registered, or the device cannot present a sheet. |
| Method tiles never arrive | `getToken` is returning an API key rather than an access token, or the token is not scoped for the account. |
| A blank card field | The native build predates the package. `@basis-theory/react-native-elements` installs with it, so rebuild rather than adding it by hand. |

## Available in React Native

The payments elements:

* [`PaymentElement`](https://docs.whop.com/elements/latest/payments/payment): the method tiles and everything the selected method needs
* [`AddressElement`](https://docs.whop.com/elements/latest/payments/address): the billing address in the buyer's own country format
* [`CardElement`](https://docs.whop.com/elements/latest/payments/card): number, expiry and security code as one component
* [`CardFields`](https://docs.whop.com/elements/latest/payments/cardFields): the same three fields, placed individually
* [`EmailElement`](https://docs.whop.com/elements/latest/payments/email): the buyer's email
* [`TaxIdElement`](https://docs.whop.com/elements/latest/payments/taxId): a registration type and number
* [`BrandingElement`](https://docs.whop.com/elements/latest/payments/branding): the Powered by Whop mark, required on every form

The checkout elements:

* [`ExpressCheckoutElement`](https://docs.whop.com/elements/latest/checkout/expressCheckout): one Apple Pay or Google Pay press that pays a whole checkout, under `<Checkout>`


## Related topics

- [Enable Subaccount Payouts](https://docs.whop.com/developer/platforms/render-payout-portal.md)
- [Enroll Subaccounts](https://docs.whop.com/developer/platforms/enroll-subaccounts.md)
- [iOS Checkout SDK](https://docs.whop.com/developer/guides/ios/overview.md)
