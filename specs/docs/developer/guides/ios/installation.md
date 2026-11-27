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

# Install the Whop iOS SDKs

> Install the WhopCheckout and WhopElements SDKs with Swift Package Manager, and create the API key WhopCheckout needs.

Whop distributes its iOS SDKs as separate Swift packages — **WhopCheckout** for payments and **WhopElements** for embedded chat and payouts. Install only what you need.

## WhopCheckout (in-app purchases)

The WhopCheckout package handles subscriptions and payments in your app.

### Step 1: Add package dependency

In Xcode, go to **File** → **Add Package Dependencies...**.

Enter the package URL:

```
https://github.com/whopio/whopsdk-checkout-swift
```

### Step 2: Select version

Choose the latest version or a version range:

* **Up to Next Major**: Recommended for production
* **Exact Version**: For stability

### Step 3: Create an API key

The SDK requires an API key with only the `iap:read` permission. Whop designed this permission for client-side use, so you can embed the key in your app bundle.

**1. Open Developer Settings**

Go to [Whop Developer Settings](https://whop.com/dashboard/developer) and select your app.

**2. Create the API key**

Select **Create API Key** and then select **only** the `iap:read` permission.

**3. Copy the key**

Copy the generated key — you'll pass it to `Checkout.shared.configure()` in the next step.

> [!WARNING]
>
> Only grant the `iap:read` permission. It's the only permission designed for client-side use and limits the key to in-app purchase operations. Never ship a key with broader permissions inside your app.

### Step 4: Import and configure

```swift theme={null}
import SwiftUI
import WhopCheckout

@main
struct YourApp: App {
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
```

> [card] [Build a Paywall](https://docs.whop.com/developer/guides/ios/build-a-paywall.md)
>
> Continue with the full setup guide

***

## WhopElements (embedded chat and wallet)

Use WhopElements to embed Whop chat channels in your app.

### Step 1: Add package dependency

In Xcode, go to **File** → **Add Package Dependencies...**.

Enter the package URL:

```
https://github.com/whopio/whopsdk-elements-swift
```

### Step 2: Select version

Choose the latest version or a version range.

### Step 3: Import and Configure the specific element

```swift theme={null}
import SwiftUI
import WhopElements

@main
struct YourApp: App {
    var body: some Scene {
        WindowGroup {
            NavigationStack {
                WhopChatView(channelId: "chat_feed_XXXXXXXXXXXXXX")
            }
            .task {
                await WhopSDK.configureWithOAuth(appId: "app_XXXXXXXXXXXXXX")
            }
        }
    }
}
```

> [card] [Embedded Chat](https://docs.whop.com/developer/guides/chat/quickstart.md)
>
> Continue with the chat integration guide

> [card] [Wallet Elements](https://docs.whop.com/elements/latest/getting-started.md)
>
> Show an account's balances and activity with the Swift wallet elements

***

## Requirements

* iOS 17.0+
* Xcode 15.0+
* Swift 5.10+

## Next steps

> [card] [Build a Paywall](https://docs.whop.com/developer/guides/ios/build-a-paywall.md)
>
> Add subscriptions and payments

> [card] [Check Entitlements](https://docs.whop.com/developer/guides/ios/check-entitlements.md)
>
> Gate content based on subscription status

> [card] [Embedded Chat](https://docs.whop.com/developer/guides/chat/quickstart.md)
>
> Add Whop chat to your app

> [card] [Wallet Elements](https://docs.whop.com/elements/latest/getting-started.md)
>
> Show an account's balances and activity with the Swift wallet elements

> [card] [API Reference](https://docs.whop.com/developer/guides/ios/checkout-reference.md)
>
> Full SDK documentation


## Related topics

- [iOS Checkout SDK](https://docs.whop.com/developer/guides/ios/overview.md)
- [Whop CLI](https://docs.whop.com/cli/overview.md)
- [Getting Started with Elements](https://docs.whop.com/elements/latest/getting-started.md)
