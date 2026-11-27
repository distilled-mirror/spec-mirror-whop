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

# Accept One-time Payments

> Accept one-time Apple Pay payments for physical goods and real-world services in your iOS app.

[Accept one-time payments on iOS with Whop](https://www.youtube.com/embed/K2e_sht1P1I?rel=0)

Use `Checkout.ApplePayButton` to accept one-time payments for physical goods or real-world services using Apple Pay. Whop processes payments at **2.7% + \$0.30** instead of Apple's 15–30% in-app purchase fees.

> [!TIP]
>
> For web sales, use [checkout links or the Checkout element](https://docs.whop.com/developer/guides/accept-payments.md) for **100+ payment methods** across **195 countries**.

> [!WARNING]
>
> **Apple Pay isn't for digital content.** Only use `Checkout.ApplePayButton` if your app sells physical goods (merch, food, hardware) or real-world services (event tickets, deliveries). Apps selling digital content — premium features, subscriptions, in-app currency, content unlocks — must use StoreKit. See the [Apple Pay eligibility](https://docs.whop.com/developer/guides/ios/checkout-reference.md#apple-pay-eligibility) section for details.

## What you'll build

* A product detail screen with a native Apple Pay button
* A purchase flow that handles success, failure, and cancellation
* Variant configuration in the Whop dashboard for your physical goods

## Prerequisites

* [Install the WhopCheckout SDK](https://docs.whop.com/developer/guides/ios/installation.md)
* An API key scoped to `iap:read` — see [Create an API key](https://docs.whop.com/developer/guides/ios/installation.md#step-3-create-an-api-key)
* A Whop product with a one-time variant (see [Step 5](#step-5-set-up-your-variant))

## Step 1: Configure the SDK

Initialize the SDK at app launch. For one-time physical goods you don't need the compatibility property `planMappings` — that's only used for StoreKit fallback on subscriptions.

```swift theme={null}
import SwiftUI
import WhopCheckout

@main
struct DropsApp: App {
    var body: some Scene {
        WindowGroup {
            ContentView()
                .environment(Checkout.shared)
                .task {
                    try? await Checkout.shared.configure(
                        // companyId: find in Dashboard URL (starts with biz_)
                        companyId: "biz_xxxxxxxxxxxxxx",
                        // apiKey: create in Dashboard > Developer with iap:read permission
                        apiKey: "your_api_key_here"
                    )
                }
        }
    }
}
```

See the [installation guide](https://docs.whop.com/developer/guides/ios/installation.md) for the full setup and security notes on the API key.

## Step 2: Add the `Apple Pay` button

Drop `Checkout.ApplePayButton` into your product view. The component renders Apple's official, Human Interface Guidelines-compliant Apple Pay button — you can't substitute a custom-styled button.

```swift theme={null}
import SwiftUI
import WhopCheckout

struct ProductDetailView: View {
    let product: MerchItem
    @State private var purchaseComplete = false

    var body: some View {
        ScrollView {
            // product image, title, price, size picker...

            Checkout.ApplePayButton(
                // planId: find in Dashboard > Products > [product] > Variants
                planId: product.planId,
                label: .buy,
                onCompletion: { result in
                    switch result {
                    case .success(let purchase):
                        // purchase.receiptId, purchase.membership
                        purchaseComplete = true
                    case .failure(let error):
                        print("Purchase failed: \(error.localizedDescription)")
                    }
                }
            )
            .frame(height: 48)
            .padding(.horizontal)
        }
    }
}
```

When the user taps, they get the native Apple Pay sheet. Face ID or double-click confirms, and the payment flows through Whop.

> [!NOTE]
>
> `onCompletion` **isn't called** when the user cancels — the sheet just dismisses. No cleanup needed.

## Step 3: Choose a button label

The `label` parameter controls the text on the button. Pick the one that fits the action:

| Label | Button text | Use for |
| - | - | - |
| `.buy` | Buy with Apple Pay | One-time merchandise and physical goods |
| `.checkout` | Check out with… | Cart-style checkout flows |
| `.order` | Order with Apple Pay | Food, deliveries, made-to-order items |
| `.book` | Book with Apple Pay | Reservations, appointments, events |
| `.continue` | Continue with… | Multi-step flows where this isn't final |
| `.plain` | (Apple Pay logo) | Fallback when no verb fits |

The full list comes from Apple's `PayWithApplePayButtonLabel` — see [Apple's documentation](https://developer.apple.com/documentation/passkit/paywithapplepaybuttonlabel) for every option.

## Step 4: Handle the purchase result

`onCompletion` receives a `Result<CheckoutPurchaseResult, Error>`. On success you get back:

| Property | Type | Description |
| - | - | - |
| `receiptId` | `String` | Unique receipt ID for this purchase |
| `membership` | `CheckoutMembership?` | The membership Whop created for the purchase |

Use this to show a confirmation screen, kick off shipping, or store the receipt for your own records.

```swift theme={null}
onCompletion: { result in
    switch result {
    case .success(let purchase):
        // Persist the receipt for your fulfillment flow
        OrderStore.shared.record(
            receiptId: purchase.receiptId,
            membershipId: purchase.membership?.id,
            product: product
        )
        showConfirmation = true
    case .failure(let error):
        showError(error.localizedDescription)
    }
}
```

## Step 5: Set up your variant

Each purchasable item needs a variant in your Whop dashboard:

**1. Create a variant**

Go to your [Dashboard](https://whop.com/dashboard) > Products > select your product > **Variants** > **Create variant**.

**2. Set it to one-time**

Choose **One-time payment** and set the price.

**3. (Optional) Set inventory limits**

For limited drops, set a stock cap. The checkout enforces it automatically — when stock runs out, purchases stop.

**4. Copy the variant ID**

The ID retains the `plan_` prefix. Pass it through the compatibility parameter `Checkout.ApplePayButton(planId:)`.

## Handle fulfillment with webhooks

For physical goods, you'll need a server-side webhook to trigger shipping after a successful purchase. Listen for the `payment.succeeded` event:

```typescript theme={null}
import { unwrapWebhook } from "@whop/sdk/helpers";

export async function POST(request: Request) {
  const body = await request.text();
  const headers = Object.fromEntries(request.headers);
  const event = unwrapWebhook(body, { headers, key: process.env.WHOP_WEBHOOK_SECRET });

  if (event.type === "payment.succeeded") {
    // Trigger your shipping/fulfillment flow
    await fulfillOrder(event.data);
  }

  return new Response("OK", { status: 200 });
}
```

> [!NOTE]
>
> The client-side `onCompletion` callback confirms the payment to the user, but your **server** should handle fulfillment via webhooks — don't rely on the client alone. See the full [webhooks guide](https://docs.whop.com/developer/guides/webhooks.md) for setup.

## Complete example

A minimal merchandise detail view with size selection and Apple Pay:

```swift theme={null}
import SwiftUI
import WhopCheckout

struct ProductDetailView: View {
    let product: MerchItem
    @State private var selectedSize: String?
    @State private var showConfirmation = false

    var body: some View {
        ScrollView {
            VStack(alignment: .leading, spacing: 16) {
                AsyncImage(url: product.imageURL) { image in
                    image.resizable().scaledToFit()
                } placeholder: {
                    Color.gray.opacity(0.1)
                }
                .frame(maxWidth: .infinity)

                Text(product.title)
                    .font(.title)
                    .bold()

                Text(product.price, format: .currency(code: "USD"))
                    .font(.title2)

                // Size picker
                HStack {
                    ForEach(product.sizes, id: \.self) { size in
                        Button(size) {
                            selectedSize = size
                        }
                        .buttonStyle(.bordered)
                        .tint(selectedSize == size ? .accentColor : .secondary)
                    }
                }

                Checkout.ApplePayButton(
                    planId: product.planId,
                    label: .buy,
                    onCompletion: { result in
                        switch result {
                        case .success(let purchase):
                            print("Receipt: \(purchase.receiptId)")
                            showConfirmation = true
                        case .failure(let error):
                            print("Purchase failed: \(error.localizedDescription)")
                        }
                    }
                )
                .frame(height: 48)
                .disabled(selectedSize == nil)
            }
            .padding()
        }
        .sheet(isPresented: $showConfirmation) {
            OrderConfirmationView(product: product, size: selectedSize ?? "")
        }
    }
}
```

## Next steps

> [card] [Webhooks](https://docs.whop.com/developer/guides/webhooks.md)
>
> Handle payment events and trigger fulfillment

> [card] [Apple Pay eligibility](https://docs.whop.com/developer/guides/ios/checkout-reference.md#apple-pay-eligibility)
>
> Confirm your app qualifies under App Store Review Guidelines

> [card] [Build a paywall](https://docs.whop.com/developer/guides/ios/build-a-paywall.md)
>
> Sell subscriptions instead of one-time purchases

> [card] [API reference](https://docs.whop.com/developer/guides/ios/checkout-reference.md)
>
> Full SDK documentation


## Related topics

- [iOS Checkout SDK](https://docs.whop.com/developer/guides/ios/overview.md)
- [Accept Payments](https://docs.whop.com/developer/guides/accept-payments.md)
- [Payments Overview](https://docs.whop.com/developer/payments/overview.md)
