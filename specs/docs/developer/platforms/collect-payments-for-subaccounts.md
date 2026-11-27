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

# Collect Payments for Subaccounts

> Collect payments for subaccounts with direct charges and an application fee, or transfers from your platform account, plus limitations and failures.

There are two ways to collect payments for subaccounts:

1. **Direct charges**: Create a checkout for the subaccount and collect an application fee.
2. **Transfers**: Collect payment to your platform account and transfer funds to subaccounts later.

## Direct charges

Create a checkout configuration with a subaccount's ID to charge customers directly on the subaccount. The subaccount is responsible for Whop fees, refunds, and disputes.

### How it works

1. Create a checkout configuration for your subaccount with an `application_fee_amount`
2. When a customer purchases, Whop creates the charge directly on the subaccount
3. Your platform collects the application fee, and the remaining amount goes to the subaccount
4. The subaccount handles any disputes or refunds for the transaction

### Example

Create an inline variant using the compatibility input named `plan`.

```typescript TypeScript theme={null}
import { WhopClient } from "@whop/sdk";

const client = new WhopClient({
	token: "Account API Key",
});

const checkoutConfig = await client.checkoutConfigurations.create({
	account_id: "biz_xxxxxxxxxxxxx", // Subaccount's ID
	variant: {
		initial_price: 10.0,
		billing_type: "one_time",
		application_fee_amount: 1.23,
	},
});

console.log(checkoutConfig.purchase_url);
```

```python Python theme={null}
from whop_sdk import Whop

client = Whop(
    token="Account API Key",
)

checkout_config = client.checkout_configurations.create(
    account_id="biz_xxxxxxxxxxxxx",  # Subaccount's ID
    variant={
        "initial_price": 10.0,
        "billing_type": "one_time",
        "application_fee_amount": 1.23,
    },
)

print(checkout_config.purchase_url)
```

```rust Rust theme={null}
use whop_sdk::prelude::*;

let config = ClientConfig {
    token: Some("Account API Key".to_string()),
    ..Default::default()
};
let client = Whop::new(config).expect("Failed to build client");

let checkout_config = client
    .checkout_configurations
    .create(
        &CreateCheckoutConfigurationsRequest {
            account_id: Some("biz_xxxxxxxxxxxxx".to_string()), // Subaccount's ID
            variant: Some(CreateCheckoutConfigurationsRequestVariant {
                initial_price: Some(10.0),
                billing_type: Some(CreateCheckoutConfigurationsRequestVariantBillingType::OneTime),
                application_fee_amount: Some(1.23),
                ..Default::default()
            }),
            ..Default::default()
        },
        None,
    )
    .await?;

println!("{}", checkout_config.purchase_url.unwrap());
```

```go Go theme={null}
import (
    "context"
    "fmt"
    "log"

    whopsdk "github.com/whopio/whopsdk-go/v2"
    "github.com/whopio/whopsdk-go/v2/client"
    "github.com/whopio/whopsdk-go/v2/option"
)

client := client.NewWhop(option.WithToken("Account API Key"))

checkoutConfig, err := client.CheckoutConfigurations.Create(context.TODO(), &whopsdk.CreateCheckoutConfigurationsRequest{
    AccountID: whopsdk.String("biz_xxxxxxxxxxxxx"), // Subaccount's ID
    Variant: &whopsdk.CreateCheckoutConfigurationsRequestVariant{
        InitialPrice:         whopsdk.Float64(10.0),
        BillingType:          whopsdk.CreateCheckoutConfigurationsRequestVariantBillingTypeOneTime.Ptr(),
        ApplicationFeeAmount: whopsdk.Float64(1.23),
    },
})
if err != nil {
    log.Fatal(err)
}

fmt.Println(*checkoutConfig.PurchaseURL)
```

In this example:

* `account_id` is the subaccount's ID where Whop will create the charge
* `variant.initial_price` is the variant's total payment amount (10.00 United States dollars)
* `variant.application_fee_amount` is the fee your platform collects (1.23 United States dollars)
* The subaccount receives 8.77 United States dollars (10.00 - 1.23)

### Limitations

* The `application_fee_amount` must be positive and less than the total payment amount
* The application fee can't exceed the captured payment amount

## Transfers

Use transfers when your platform collects the payment first, or when your backend decides how much and when to pay each subaccount.

### Before you transfer

* **Origin account**: the account sending funds must complete Know Your Customer (KYC) verification and have enough balance for the transfer. You can top up the balance from the dashboard or via [Add funds to your balance](https://docs.whop.com/developer/platforms/add-funds-to-your-balance.md).
* **Destination account**: the recipient must already have a Whop account. If they don't, onboard them first with [Enroll subaccounts](https://docs.whop.com/developer/platforms/enroll-subaccounts.md).
* **Settlement ownership**: with transfers, your platform is the merchant of record for the original payment. Your platform handles fees, disputes, and refunds on that payment.

### Example

Collect payment to your platform account, then transfer the subaccount's share:

```typescript TypeScript theme={null}
import { WhopClient } from "@whop/sdk";

const client = new WhopClient({
	token: "Account API Key",
});

const transfer = await client.transfers.create({
	type: "balance",
	amount: 90.0,
	currency: "usd",
	origin_id: "biz_yyyyyyyyyyyyy", // Platform's account ID
	destination_id: "biz_xxxxxxxxxxxxx", // Subaccount's ID
	metadata: {
		order_id: "order_12345",
	},
});

if (transfer.object === "transfer") {
	console.log(transfer.id);
}
```

```python Python theme={null}
from whop_sdk import Whop

client = Whop(
    token="Account API Key",
)

transfer = client.transfers.create(
    type="balance",
    amount=90.0,
    currency="usd",
    origin_id="biz_yyyyyyyyyyyyy",  # Platform's account ID
    destination_id="biz_xxxxxxxxxxxxx",  # Subaccount's ID
    metadata={
        "order_id": "order_12345",
    },
)

if transfer.object == "transfer":
    print(transfer.id)
```

```rust Rust theme={null}
use whop_sdk::prelude::*;

let config = ClientConfig {
    token: Some("Account API Key".to_string()),
    ..Default::default()
};
let client = Whop::new(config).expect("Failed to build client");

let transfer = client
    .transfers
    .create(
        &CreateTransfersRequest {
            amount: 90.0,
            currency: Some("usd".to_string()),
            origin_id: "biz_yyyyyyyyyyyyy".to_string(), // Platform's account ID
            destination_id: Some("biz_xxxxxxxxxxxxx".to_string()), // Subaccount's ID
            metadata: Some(HashMap::from([("order_id".to_string(), json!("order_12345"))])),
            ..Default::default()
        },
        None,
    )
    .await?;

if let CreateTransfersResponse::Transfer { id, .. } = &transfer {
    println!("{id}");
}
```

```go Go theme={null}
import (
    "context"
    "fmt"
    "log"

    whopsdk "github.com/whopio/whopsdk-go/v2"
    "github.com/whopio/whopsdk-go/v2/client"
    "github.com/whopio/whopsdk-go/v2/option"
)

client := client.NewWhop(option.WithToken("Account API Key"))

transfer, err := client.Transfers.Create(context.TODO(), &whopsdk.CreateTransfersRequest{
    Amount:        90.0,
    Currency:      whopsdk.String("usd"),
    OriginID:      "biz_yyyyyyyyyyyyy",                 // Platform's account ID
    DestinationID: whopsdk.String("biz_xxxxxxxxxxxxx"), // Subaccount's ID
    Metadata:      map[string]any{"order_id": "order_12345"},
})
if err != nil {
    log.Fatal(err)
}

if transfer.Object == "transfer" {
    fmt.Println(transfer.Transfer.ID)
}
```

In this example:

* `origin_id` is your platform's account ID (where Whop deducts funds)
* `destination_id` is the subaccount's ID (where Whop credits funds)
* `amount` is the amount to transfer
* `metadata` stores custom data for your reference

### Handle transfer failures

Transfers fail if the origin account isn't verified, the origin balance is too low, or the destination account can't receive funds yet. Fix the underlying cause before retrying. Don't retry failed transfers without reviewing the cause.

### Common uses

* Scheduled creator payouts
* Affiliate payouts
* Marketplace splits after an order settles

## Choose a payment flow

| | Direct charges | Transfers |
| - | - | - |
| **Fees** | Subaccount pays Whop fees | Platform pays Whop fees |
| **Disputes** | Subaccount handles disputes | Platform handles disputes |
| **Refunds** | Subaccount handles refunds | Platform handles refunds |

Use transfers when your backend controls payout timing and amount. Use direct charges when the subaccount should own fees, disputes, and refunds from the original purchase.

## API reference

> [card] [Create Checkout Configuration](https://docs.whop.com/api-reference/legacy/checkout-configurations/create-checkout-configuration.md)
>
> Create checkout configurations with application fees

> [card] [Create Transfer](https://docs.whop.com/api-reference/transfers/create-transfer.md)
>
> Transfer funds between accounts

## Related resources

> [card] [Accept payments](https://docs.whop.com/developer/guides/accept-payments.md)
>
> Learn about checkout links and the Checkout element

> [card] [Enroll subaccounts](https://docs.whop.com/developer/platforms/enroll-subaccounts.md)
>
> Create recipient accounts and finish onboarding before paying them


## Related topics

- [Add Funds to Your Balance](https://docs.whop.com/developer/platforms/add-funds-to-your-balance.md)
- [Enroll Subaccounts](https://docs.whop.com/developer/platforms/enroll-subaccounts.md)
- [Payments](https://docs.whop.com/elements/latest/payments/overview.md)
