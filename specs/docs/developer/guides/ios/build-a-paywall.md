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

# Build an iOS Paywall

> Build a SwiftUI paywall with the WhopCheckout SDK that shows your subscription variants and handles purchases.

This guide shows you how to build a paywall that displays your subscription variants and handles the purchase flow. By the end, you'll have a working paywall that lets users subscribe to your product.

## What you'll build

* A paywall screen that displays available variants with pricing
* A purchase flow that handles payments through Whop
* Error handling for cancelled or failed purchases

## Prerequisites

* [Install the SDK](https://docs.whop.com/developer/guides/ios/installation.md)
* A Whop account with at least one product and variant
* An API key scoped to `iap:read` — see [Create an API key](https://docs.whop.com/developer/guides/ios/installation.md#step-3-create-an-api-key) for setup

> [!NOTE]
>
> The `iap:read` permission is designed for client-side use and only grants access to in-app purchase operations. It's safe to embed in your app bundle. Never ship a key with broader permissions.

## Step 1: Configure the SDK

Initialize the SDK in your app's entry point with your API key:

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
                            .init(whopId: "plan_xxxxx", appleId: "monthly_sub"),
                            .init(whopId: "plan_yyyyy", appleId: "yearly_sub")
                        ]
                    )
                }
        }
    }
}
```

> [!NOTE]
>
> Find your account ID in the URL of your [Whop
> Dashboard](https://whop.com/dashboard) (starts with `biz_`). Find your variant
> IDs in the Products tab under each variant (IDs retain the `plan_` prefix).

## Step 2: Display available variants

Use the compatibility property `plans` to show available subscription variants:

```swift theme={null}
struct PaywallView: View {
    @Environment(Checkout.self) var checkout

    var body: some View {
        VStack(spacing: 20) {
            Text("Choose a Variant")
                .font(.title)
                .bold()

            ForEach(checkout.plans) { plan in
                PlanCard(plan: plan)
            }
        }
        .padding()
    }
}

struct PlanCard: View {
    let plan: CheckoutPlan
    @Environment(Checkout.self) var checkout

    var body: some View {
        VStack(alignment: .leading, spacing: 8) {
            Text(plan.title ?? "Variant")
                .font(.headline)

            Text(plan.initialPrice, format: .currency(code: plan.baseCurrency))
                .font(.title2)
                .bold()

            Button("Subscribe") {
                Task {
                    await purchase(plan)
                }
            }
            .buttonStyle(.borderedProminent)
        }
        .padding()
        .background(.ultraThinMaterial)
        .cornerRadius(12)
    }

    func purchase(_ plan: CheckoutPlan) async {
        do {
            let result = try await checkout.purchase(plan.id)
            // Purchase successful!
            print("Purchased: \(result.receiptId)")
        } catch WhopCheckoutError.cancelled {
            // User dismissed the checkout
        } catch {
            // Handle other errors
            print("Purchase failed: \(error)")
        }
    }
}
```

## Step 3: Handle the purchase flow

When a user selects **Subscribe**, the SDK automatically:

1. Presents a checkout sheet with the Whop payment flow
2. Handles payment processing
3. Dismisses the sheet when complete
4. Returns the result or throws an error

```swift theme={null}
func purchase(_ planId: String) async {
    do {
        let result = try await checkout.purchase(planId)
        // Success - user now has an active membership
        print("Receipt ID: \(result.receiptId)")
    } catch WhopCheckoutError.cancelled {
        // User dismissed the checkout sheet
    } catch WhopCheckoutError.paymentFailed(let message) {
        // Payment failed - show error to user
        showError(message)
    } catch {
        // Other errors
        showError("Something went wrong. Please try again.")
    }
}
```

## Complete example

Here's a full paywall implementation:

```swift theme={null}
struct PaywallView: View {
    @Environment(Checkout.self) var checkout
    @Environment(\.dismiss) var dismiss
    @State private var selectedPlanId: String?
    @State private var isPurchasing = false

    var body: some View {
        NavigationStack {
            ScrollView {
                VStack(spacing: 24) {
                    // Header
                    VStack(spacing: 8) {
                        Image(systemName: "crown.fill")
                            .font(.system(size: 48))
                            .foregroundStyle(.yellow)

                        Text("Unlock Premium")
                            .font(.title)
                            .bold()

                        Text("Get access to all features")
                            .foregroundStyle(.secondary)
                    }
                    .padding(.top, 32)

                    // Variants
                    ForEach(checkout.plans) { plan in
                        Button {
                            selectedPlanId = plan.id
                        } label: {
                            HStack {
                                VStack(alignment: .leading, spacing: 4) {
                                    Text(plan.title ?? "Variant")
                                        .font(.headline)
                                    Text(plan.initialPrice, format: .currency(code: plan.baseCurrency))
                                        .foregroundStyle(.secondary)
                                }
                                Spacer()
                            }
                            .padding()
                            .background(plan.id == selectedPlanId ? Color.accentColor.opacity(0.1) : Color(.secondarySystemBackground))
                            .cornerRadius(12)
                            .overlay {
                                RoundedRectangle(cornerRadius: 12)
                                    .stroke(plan.id == selectedPlanId ? Color.accentColor : .clear, lineWidth: 2)
                            }
                        }
                        .buttonStyle(.plain)
                    }

                    // Purchase button
                    Button {
                        guard let planId = selectedPlanId else { return }
                        Task { await purchase(planId) }
                    } label: {
                        Group {
                            if isPurchasing {
                                ProgressView().tint(.white)
                            } else {
                                Text("Continue")
                            }
                        }
                        .font(.headline)
                        .foregroundStyle(.white)
                        .frame(maxWidth: .infinity)
                        .padding(.vertical, 14)
                        .background(Color.accentColor, in: RoundedRectangle(cornerRadius: 12))
                    }
                    .disabled(selectedPlanId == nil || isPurchasing)
                    .opacity(selectedPlanId == nil ? 0.5 : 1)
                }
                .padding()
            }
            .navigationTitle("Subscribe")
            .navigationBarTitleDisplayMode(.inline)
            .toolbar {
                ToolbarItem(placement: .cancellationAction) {
                    Button("Close") { dismiss() }
                }
            }
        }
    }

    func purchase(_ planId: String) async {
        isPurchasing = true
        defer { isPurchasing = false }

        do {
            _ = try await checkout.purchase(planId)
            dismiss()
        } catch WhopCheckoutError.cancelled {
            // User cancelled
        } catch {
            print("Purchase failed: \(error.localizedDescription)")
        }
    }
}
```

## Next steps

> [card] [Check Entitlements](https://docs.whop.com/developer/guides/ios/check-entitlements.md)
>
> Learn how to gate content based on subscription status

> [card] [API Reference](https://docs.whop.com/developer/guides/ios/checkout-reference.md)
>
> Explore all available methods and properties


## Related topics

- [iOS Checkout SDK](https://docs.whop.com/developer/guides/ios/overview.md)
- [iOS Checkout SDK Reference](https://docs.whop.com/developer/guides/ios/checkout-reference.md)
- [Install the Whop iOS SDKs](https://docs.whop.com/developer/guides/ios/installation.md)
