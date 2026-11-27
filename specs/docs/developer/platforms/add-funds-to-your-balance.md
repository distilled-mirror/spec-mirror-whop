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

# Add Funds to Your Balance

> Create a first top up in the dashboard to save a payment method, then list payment methods, create top ups through the API, and handle payment webhooks.

**1. Save a payment method on your account**

Create a top up from the [Dashboard](https://whop.com/dashboard/balance/), or save a card through the API with [Create setup intent](https://docs.whop.com/api-reference/setup-intents/create-setup-intent.md) and `owner_id` set to your account ID. Either saves a payment method on your account that you can reuse programmatically.

**2. Get payment methods**

List saved payment methods for your account:

```typescript TypeScript theme={null}
const paymentMethods = await client.paymentMethods.list({
  account_id: "biz_xxxxxxxxxxxxx",
});

const paymentMethod = paymentMethods.data[0];
```

```python Python theme={null}
payment_methods = client.payment_methods.list(
    account_id="biz_xxxxxxxxxxxxx",
)

payment_method = payment_methods.items[0]
```

```rust Rust theme={null}
let payment_methods = client
    .payment_methods
    .list(
        &PaymentMethodsListQueryRequest {
            account_id: Some("biz_xxxxxxxxxxxxx".to_string()),
            ..Default::default()
        },
        None,
    )
    .await?;

let payment_method = &payment_methods.data[0];
println!("{payment_method:?}");
```

```go Go theme={null}
page, err := client.PaymentMethods.List(context.TODO(), &whopsdk.ListPaymentMethodsRequest{
    AccountID: whopsdk.String("biz_xxxxxxxxxxxxx"),
})
if err != nil {
    log.Fatal(err)
}

paymentMethod := page.Results[0]
fmt.Println(paymentMethod)
```

**3. Create a top up**

Create a deposit to your account and pass the saved payment method to charge:

```typescript TypeScript theme={null}
const deposit = await client.deposits.create({
  destination: "biz_xxxxxxxxxxxxx",
  amount: 1000.0,
  payment_method_id: paymentMethod.id,
});

console.log(deposit.methods.card?.payment_id);
```

```python Python theme={null}
deposit = client.deposits.create(
    destination="biz_xxxxxxxxxxxxx",
    amount=1000.0,
    payment_method_id=payment_method.id,
)

print(deposit.methods.card.payment_id)
```

In this example:

* `destination` is your platform's account ID
* `amount` is the amount in USD to add to your balance
* `payment_method_id` is the saved payment method to charge

The response's `methods.card.payment_id` is the payment the charge created. The request needs an API key with the `payment:charge` scope.

**4. Handle payment events**

Listen for payment webhooks to confirm the top up was successful:

```typescript theme={null}
if (webhookData.type === "payment.succeeded") {
  const payment = webhookData.data;
  console.log("Top up successful:", payment.id);
}

if (webhookData.type === "payment.failed") {
  const payment = webhookData.data;
  console.log("Top up failed:", payment.failure_message);
}
```

## Related resources

> [card] [Collect payments for subaccounts](https://docs.whop.com/developer/platforms/collect-payments-for-subaccounts.md)
>
> Transfer funds to subaccounts

> [card] [Render payout portal](https://docs.whop.com/developer/platforms/render-payout-portal.md)
>
> Let users withdraw their funds


## Related topics

- [Collect Payments for Subaccounts](https://docs.whop.com/developer/platforms/collect-payments-for-subaccounts.md)
- [Balances](https://docs.whop.com/elements/latest/wallet/balances.md)
- [Payouts & Money Movement](https://docs.whop.com/developer/payouts/overview.md)
