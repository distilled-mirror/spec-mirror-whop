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

# Accept Payments

> Accept one-time and recurring payments across 195 countries with 100+ payment methods

✨ Build this with AI

Copy and paste our templates using your favorite editor

Claude
Cursor
Codex
Lovable

* more

[Open](https://whop.com/network/examples?filter=checkouts)

Accept one-time and recurring payments with a checkout link, or on your own site with Whop Elements. Whop supports **100+ payment methods** across **195 countries**, and the right ones appear automatically based on the buyer's location. [See all payment methods](https://docs.whop.com/payments-and-billing/local-payment-methods.md).

> [!TIP]
>
> For an iOS app, the [Whop iOS Checkout SDK](https://docs.whop.com/developer/guides/ios/overview.md) handles checkout natively with lower fees (2.7% + \$0.30 vs Apple's 15–30%). The SDK uses a scoped `iap:read` API key that's [safe to embed in your app](https://docs.whop.com/developer/guides/ios/installation.md#step-3-create-an-api-key).

## Choose your integration

Every path on your own site runs on [Whop Elements](https://docs.whop.com/elements/latest/getting-started.md). Pick by how much of the checkout you want to own. Each row links to a step-by-step quickstart with the client, the server, and the return page side by side.

| | Checkout link | [Checkout element](https://docs.whop.com/developer/guides/embed-checkout.md) | [Payment elements](https://docs.whop.com/developer/guides/payment-elements.md) | [Express checkout](https://docs.whop.com/developer/guides/express-checkout.md) | [Payment request](https://docs.whop.com/developer/guides/payment-request.md) |
| - | - | - | - | - | - |
| **What you get** | A Whop-hosted checkout page | Whop's whole checkout on your page | PCI-isolated fields for a form you design | Apple Pay and Google Pay buttons for a variant | The wallet sheet behind your own button |
| **Effort** | Low | Low | High | Low | High |
| **Customization** | Branding | Theme and layout | Your own form and pay button | Wallets and layout | Everything but the sheet |
| **Price** | From the variant | From the variant | From the variant, or an amount you pass | From the variant | An amount you pass |
| **Server code in the purchase path** | No | No | Yes, to charge the confirmation token | No | Yes, to charge the confirmation token |
| **Off-site steps** | Handled | Handled, returns to `returnUrl` | You run `handleNextAction` | Handled, returns to `returnUrl` | You run `handleNextAction` |
| **Best for** | Sharing a link, quick setup | A product page or cart in your own design | A checkout you design end to end | One-press purchase of a variant | A cart or total you compute yourself |

Whatever you pick, two rules hold. Fulfill from the [`payment.succeeded` webhook](https://docs.whop.com/developer/guides/webhooks), never from the browser. And where a path takes a `returnUrl`, the buyer lands there after an off-site step **whatever its outcome**. Whop appends `payment` and `status` (`succeeded`, `failed`, or `canceled`), so the page at `returnUrl` has to read `status` before it shows a thank-you.

> [card] [Build a checkout with payment elements](https://docs.whop.com/developer/guides/payment-elements.md)
>
> Collect a confirmation token in the browser and charge it from your server.

> [card] [Embed a checkout](https://docs.whop.com/developer/guides/embed-checkout.md)
>
> Drop Whop's whole checkout into your page with no server code in the purchase path.

> [card] [Add express checkout](https://docs.whop.com/developer/guides/express-checkout.md)
>
> One-press Apple Pay and Google Pay buttons for a variant.

> [card] [Build a wallet button](https://docs.whop.com/developer/guides/payment-request.md)
>
> Open the wallet sheet from your own button and price the order yourself.

## Share a checkout link

Checkout links are the simplest way to accept payments. Create a variant to get a shareable checkout URL.

> [tab] Dashboard
>
> 1. Go to your [Dashboard](https://whop.com/dashboard/links/checkout) > **Checkout links**
>    2\. Select **+ Create checkout link**
> 2. Select a product and configure your pricing (free, one-time, or recurring)
> 3. Select **Create checkout link**
>
> The generated link can be shared directly with customers or embedded on your website.

> [tab] API
>
> Create a variant via the API to get a `purchase_url`:
>
> ```typescript TypeScript theme={null}
> import { WhopClient } from "@whop/sdk";
>
> const client = new WhopClient({
>   token: "Account API Key",
> });
>
> // account_id: find in Dashboard > Settings
> // product_id: find in Dashboard > Products
> const variant = await client.variants.create({
>   account_id: "biz_xxxxxxxxxxxxx",
>   product_id: "prod_xxxxxxxxxxxxx",
>   initial_price: 10.0,
>   plan_type: "one_time",
> });
>
> console.log(variant.purchase_url);
> ```
>
> ```python Python theme={null}
> from whop_sdk import Whop
>
> client = Whop(
>     token="Account API Key",
> )
>
> # account_id: find in Dashboard > Settings
> # product_id: find in Dashboard > Products
> variant = client.variants.create(
>     account_id="biz_xxxxxxxxxxxxx",
>     product_id="prod_xxxxxxxxxxxxx",
>     initial_price=10.0,
>     plan_type="one_time",
> )
>
> print(variant.purchase_url)
> ```
>
> ```rust Rust theme={null}
> use whop_sdk::prelude::*;
>
> let config = ClientConfig {
>     token: Some("Account API Key".to_string()),
>     ..Default::default()
> };
> let client = Whop::new(config).expect("Failed to build client");
>
> // account_id: find in Dashboard > Settings
> // product_id: find in Dashboard > Products
> let variant = client
>     .variants
>     .create(
>         &CreateVariantsRequest {
>             account_id: Some("biz_xxxxxxxxxxxxx".to_string()),
>             product_id: Some("prod_xxxxxxxxxxxxx".to_string()),
>             initial_price: Some(10.0),
>             plan_type: Some("one_time".to_string()),
>             ..Default::default()
>         },
>         None,
>     )
>     .await?;
>
> println!("{}", variant.purchase_url);
> ```
>
> ```go Go theme={null}
> import (
>     "context"
>     "fmt"
>     "log"
>
>     whopsdk "github.com/whopio/whopsdk-go/v2"
>     "github.com/whopio/whopsdk-go/v2/client"
>     "github.com/whopio/whopsdk-go/v2/option"
> )
>
> client := client.NewWhop(option.WithToken("Account API Key"))
>
> // account_id: find in Dashboard > Settings
> // product_id: find in Dashboard > Products
> variant, err := client.Variants.Create(context.TODO(), &whopsdk.CreateVariantsRequest{
>     AccountID:    whopsdk.String("biz_xxxxxxxxxxxxx"),
>     ProductID:    whopsdk.String("prod_xxxxxxxxxxxxx"),
>     InitialPrice: whopsdk.Float64(10.0),
>     PlanType:     whopsdk.String("one_time"),
> })
> if err != nil {
>     log.Fatal(err)
> }
>
> fmt.Println(variant.PurchaseURL)
> ```
>
> Redirect customers to the `purchase_url` to complete payment on a Whop-hosted checkout page.

## Test it end to end

Run a test charge in the sandbox and watch it reach your server and dashboard. The [sandbox guide](https://docs.whop.com/developer/guides/sandbox.md) covers the URL changes and lists test cards for every payment outcome. When the charge settles, your handler receives a `payment.succeeded` event with the `metadata` you attached, which is how you map the payment back to your own order. The test charge also appears in the sandbox dashboard under **Payments**.

![Sandbox dashboard Payments page showing the payments list](https://mintcdn.com/whop/jU1qiugUa1yCR1I4/images/sandbox-payments-dashboard.png?fit=max\&auto=format\&n=jU1qiugUa1yCR1I4\&q=85\&s=0dac30d4936ad5a412f1ceb5d54d056b)

## Next steps

> [card] [Webhooks](https://docs.whop.com/developer/guides/webhooks.md)
>
> Handle payment events in real time

> [card] [Save payment methods](https://docs.whop.com/developer/guides/save-payment-methods.md)
>
> Save a method now and charge it later

> [card] [iOS payments](https://docs.whop.com/developer/guides/ios/overview.md)
>
> Accept payments in your iOS app with lower fees

> [card] [Payment methods](https://docs.whop.com/payments-and-billing/local-payment-methods.md)
>
> 100+ payment methods across 195 countries


## Related topics

- [Payments Overview](https://docs.whop.com/developer/payments/overview.md)
- [Collect Payments for Subaccounts](https://docs.whop.com/developer/platforms/collect-payments-for-subaccounts.md)
- [Save Payment Methods](https://docs.whop.com/developer/guides/save-payment-methods.md)
