> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Collect Payments for Connected Accounts

> Direct charges and transfers for connected accounts

There are two ways to collect payments for connected accounts:

1. **Direct charges**: Create a checkout for the connected account and collect an application fee.
2. **Transfers**: Collect payment to your platform account and transfer funds to connected accounts later.

## Direct charges

Create a checkout configuration with a connected account's ID to charge customers directly on the connected account. The connected account is responsible for Whop fees, refunds, and disputes.

### How it works

1. Create a checkout configuration for your connected account with an `application_fee_amount`
2. When a customer purchases, Whop creates the charge directly on the connected account
3. Your platform collects the application fee, and the remaining amount goes to the connected account
4. The connected account handles any disputes or refunds for the transaction

### Example

<CodeGroup>
  ```typescript TypeScript theme={null}
  import { WhopClient } from "@whop/sdk";

  const client = new WhopClient({
  	token: "Account API Key",
  });

  const checkoutConfig = await client.checkoutConfigurations.create({
  	account_id: "biz_xxxxxxxxxxxxx", // Connected account's ID
  	plan: {
  		initial_price: 10.0,
  		plan_type: "one_time",
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
      account_id="biz_xxxxxxxxxxxxx",  # Connected account's ID
      plan={
          "initial_price": 10.0,
          "plan_type": "one_time",
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
              account_id: Some("biz_xxxxxxxxxxxxx".to_string()), // Connected account's ID
              plan: Some(CreateCheckoutConfigurationsRequestPlan {
                  initial_price: Some(10.0),
                  plan_type: Some(CreateCheckoutConfigurationsRequestPlanPlanType::OneTime),
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

      whopsdk "github.com/whopio/whopsdk-go"
      "github.com/whopio/whopsdk-go/client"
      "github.com/whopio/whopsdk-go/option"
  )

  client := client.NewWhop(option.WithToken("Account API Key"))

  checkoutConfig, err := client.CheckoutConfigurations.Create(context.TODO(), &whopsdk.CreateCheckoutConfigurationsRequest{
      AccountID: whopsdk.String("biz_xxxxxxxxxxxxx"), // Connected account's ID
      Plan: &whopsdk.CreateCheckoutConfigurationsRequestPlan{
          InitialPrice: whopsdk.Float64(10.0),
          PlanType:     whopsdk.CreateCheckoutConfigurationsRequestPlanPlanTypeOneTime.Ptr(),
      },
  })
  if err != nil {
      log.Fatal(err)
  }

  fmt.Println(*checkoutConfig.PurchaseURL)
  ```
</CodeGroup>

In this example:

* `account_id` is the connected account's ID where Whop will create the charge
* `plan.initial_price` is the total payment amount (10.00 United States dollars)
* `plan.application_fee_amount` is the fee your platform collects (1.23 United States dollars)
* The connected account receives 8.77 United States dollars (10.00 - 1.23)

### Limitations

* The `application_fee_amount` must be positive and less than the total payment amount
* The application fee can't exceed the captured payment amount

## Transfers

Use transfers when your platform collects the payment first, or when your backend decides how much and when to pay each connected account.

### Before you transfer

* **Origin account**: the account sending funds must complete Know Your Customer (KYC) verification and have enough balance for the transfer. You can top up the balance from the dashboard or via [Add funds to your balance](/developer/platforms/add-funds-to-your-balance).
* **Destination account**: the recipient must already have a Whop account. If they don't, onboard them first with [Enroll connected accounts](/developer/platforms/enroll-connected-accounts).
* **Settlement ownership**: with transfers, your platform is the merchant of record for the original payment. Your platform handles fees, disputes, and refunds on that payment.

### Example

Collect payment to your platform account, then transfer the connected account's share:

<CodeGroup>
  ```typescript TypeScript theme={null}
  import { WhopClient } from "@whop/sdk";

  const client = new WhopClient({
  	token: "Account API Key",
  });

  const transfer = await client.transfers.create({
  	amount: 90.0,
  	currency: "usd",
  	origin_id: "biz_yyyyyyyyyyyyy", // Platform's account ID
  	destination_id: "biz_xxxxxxxxxxxxx", // Connected account's ID
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
      amount=90.0,
      currency="usd",
      origin_id="biz_yyyyyyyyyyyyy",  # Platform's account ID
      destination_id="biz_xxxxxxxxxxxxx",  # Connected account's ID
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
              destination_id: Some("biz_xxxxxxxxxxxxx".to_string()), // Connected account's ID
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

      whopsdk "github.com/whopio/whopsdk-go"
      "github.com/whopio/whopsdk-go/client"
      "github.com/whopio/whopsdk-go/option"
  )

  client := client.NewWhop(option.WithToken("Account API Key"))

  transfer, err := client.Transfers.Create(context.TODO(), &whopsdk.CreateTransfersRequest{
      Amount:        90.0,
      Currency:      whopsdk.String("usd"),
      OriginID:      "biz_yyyyyyyyyyyyy",                 // Platform's account ID
      DestinationID: whopsdk.String("biz_xxxxxxxxxxxxx"), // Connected account's ID
      Metadata:      map[string]any{"order_id": "order_12345"},
  })
  if err != nil {
      log.Fatal(err)
  }

  if transfer.Object == "transfer" {
      fmt.Println(transfer.Transfer.ID)
  }
  ```
</CodeGroup>

In this example:

* `origin_id` is your platform's account ID (where Whop deducts funds)
* `destination_id` is the connected account's ID (where Whop credits funds)
* `amount` is the amount to transfer
* `metadata` stores custom data for your reference

### Handle transfer failures

Transfers fail if the origin account isn't verified, the origin balance is too low, or the destination account can't receive funds yet. Fix the underlying cause before retrying. Don't retry failed transfers without reviewing the cause.

### Common uses

* Scheduled creator payouts
* Affiliate payouts
* Marketplace splits after an order settles

## Choose a payment flow

|              | Direct charges                     | Transfers                 |
| ------------ | ---------------------------------- | ------------------------- |
| **Fees**     | Connected account pays Whop fees   | Platform pays Whop fees   |
| **Disputes** | Connected account handles disputes | Platform handles disputes |
| **Refunds**  | Connected account handles refunds  | Platform handles refunds  |

Use transfers when your backend controls payout timing and amount. Use direct charges when the connected account should own fees, disputes, and refunds from the original purchase.

## API reference

<CardGroup cols={2}>
  <Card title="Create Checkout Configuration" icon="code" href="/api-reference/checkout-configurations/create-checkout-configuration">
    Create checkout configurations with application fees
  </Card>

  <Card title="Create Transfer" icon="code" href="/api-reference/beta/transfers/create-transfer">
    Transfer funds between accounts
  </Card>
</CardGroup>

## Related resources

<CardGroup cols={2}>
  <Card title="Accept payments" icon="credit-card" href="/developer/guides/accept-payments">
    Learn about checkout links and embedded checkout
  </Card>

  <Card title="Enroll connected accounts" icon="users" href="/developer/platforms/enroll-connected-accounts">
    Create recipient accounts and finish onboarding before paying them
  </Card>
</CardGroup>
