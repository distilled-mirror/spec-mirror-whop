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

# iOS Checkout SDK

> Sell in-app purchases and subscriptions in your iOS app, with Whop payments in the US and StoreKit everywhere else.

The Whop iOS Checkout SDK lets you sell in-app purchases and subscriptions in your native iOS app. In the United States, it automatically uses Whop's payment processing (2.7% + \$0.30 versus Apple's 15-30%). Elsewhere, it falls back to Apple's StoreKit for global coverage.

To take payments in person, use the SDK's Apple Pay button to [accept one-time payments](https://docs.whop.com/developer/guides/ios/accept-one-time-payments.md) instead.

> [!TIP]
>
> The SDK is configured with a scoped `iap:read` API key that's safe to ship in your app bundle. See [Create an API key](https://docs.whop.com/developer/guides/ios/installation.md#step-3-create-an-api-key) for setup.

> [!NOTE]
>
> Products and Variants are the current commerce resources. The iOS SDK still exposes compatibility symbols such as `plans`, `CheckoutPlan`, `planMappings`, and `planId`. Those symbols all refer to variants.

## What you can build

> [card] [Build a Paywall](https://docs.whop.com/developer/guides/ios/build-a-paywall.md)
>
> Display subscription variants and handle purchases in your app

> [card] [Check Entitlements](https://docs.whop.com/developer/guides/ios/check-entitlements.md)
>
> Verify subscription status and gate premium content

## Related iOS SDKs

Whop also ships separate SDKs for embedded experiences:

> [card] [Embedded Chat](https://docs.whop.com/developer/guides/chat/quickstart.md)
>
> Add Whop chat channels directly in your app

> [card] [Wallet Elements](https://docs.whop.com/elements/latest/getting-started.md)
>
> Show an account's balances and activity with the Swift wallet elements

## Quick example

Here's a complete example showing the SDK in action:

```swift theme={null}
import SwiftUI
import WhopCheckout

@main
struct MyApp: App {
    var body: some Scene {
        WindowGroup {
            ContentView()
                .environment(Checkout.shared)
                .task {
                    try? await Checkout.shared.configure(
                        // companyId: find in Dashboard URL (starts with biz_)
                        companyId: "biz_xxxxxxxxxxxxxx",
                        // apiKey: create in Dashboard > Developer with iap:read permission
                        apiKey: "your_api_key_here",
                        planMappings: [
                            .init(whopId: "plan_xxxxx", appleId: "monthly_sub")
                        ]
                    )
                }
        }
    }
}

struct ContentView: View {
    @Environment(Checkout.self) var checkout

    var body: some View {
        if checkout.isSubscribed {
            Text("Welcome, premium user!")
        } else {
            Button("Upgrade to Premium") {
                Task {
                    try? await checkout.purchase("plan_xxxxx")
                }
            }
        }
    }
}
```

## Getting started

**1. Install the SDK**

Add the WhopCheckout package to your Xcode project via Swift Package Manager.

[Installation guide →](https://docs.whop.com/developer/guides/ios/installation.md)

**2. Get your API key**

Create an API key with the `iap:read` permission in your [Developer Settings](https://whop.com/dashboard/developer). [Why this is safe to embed →](https://docs.whop.com/developer/guides/ios/installation.md#step-3-create-an-api-key)

**3. Configure the SDK**

Initialize the SDK with your account ID, API key, and variant mappings through the compatibility property `planMappings` on `Checkout.shared.configure()`.

**4. Build your paywall**

Display variants and handle purchases. The SDK automatically uses Whop checkout in the US and StoreKit elsewhere.

[Build a paywall →](https://docs.whop.com/developer/guides/ios/build-a-paywall.md)

## Reference

> [card] [API Reference](https://docs.whop.com/developer/guides/ios/checkout-reference.md)
>
> Complete API documentation for all Checkout classes, methods, and types

## Requirements

* iOS 17.0+
* Xcode 15.0+
* Swift 5.10+


## Related topics

- [iOS Checkout SDK Reference](https://docs.whop.com/developer/guides/ios/checkout-reference.md)
- [Accept Payments](https://docs.whop.com/developer/guides/accept-payments.md)
- [Install the Whop iOS SDKs](https://docs.whop.com/developer/guides/ios/installation.md)
