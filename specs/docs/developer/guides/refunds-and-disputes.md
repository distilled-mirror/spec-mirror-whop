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

# Refunds and Disputes

> Issue refunds, respond to disputes with evidence, and listen for chargeback events.

Two things can happen after a payment goes through. **Refunds** are a reversal you (or your support team) issue on purpose. **Disputes** are chargebacks the buyer files with their bank. You don't always control how a dispute lands, but you can respond with evidence.

> [!TIP]
>
> Subscribe to [webhooks](https://docs.whop.com/developer/guides/webhooks.md) for `refund.created`, `refund.updated`, `dispute.created`, `dispute.updated`, and `dispute_alert.created` so you can reconcile status changes without polling.

## Refunds

Refunds reverse a successful payment. Whop processes them through the original payment processor. The matching membership status updates automatically.

### Issue a refund

Create refunds for a payment, not directly on the refunds resource. Call `payments.refund({ id })`. Omit `partial_amount` for a full refund, or pass it for a partial.

```typescript TypeScript theme={null}
import { WhopClient } from "@whop/sdk";

const client = new WhopClient({ token: process.env.WHOP_API_KEY });

// Full refund
const fullRefund = await client.payments.refund({ id: "pay_xxxxxxxxxxxxx" });

// Partial refund
const partialRefund = await client.payments.refund({
  id: "pay_xxxxxxxxxxxxx",
  partial_amount: 5.0, // in payment currency, e.g. 5 USD
});
```

```python Python theme={null}
import os
from whop_sdk import Whop

client = Whop(token=os.environ["WHOP_API_KEY"])

# Full refund
full_refund = client.payments.refund("pay_xxxxxxxxxxxxx")

# Partial refund
partial_refund = client.payments.refund(
    "pay_xxxxxxxxxxxxx",
    partial_amount=5.0,
)
```

```ruby Ruby theme={null}
require "whop_sdk"

client = Whop_sdk::Client.new(token: ENV.fetch("WHOP_API_KEY"))

# Full refund
full_refund = client.payments.refund(id: "pay_xxxxxxxxxxxxx")

# Partial refund
partial_refund = client.payments.refund(
  id: "pay_xxxxxxxxxxxxx",
  partial_amount: 5.0,
)
```

```rust Rust theme={null}
use whop_sdk::prelude::*;

let config = ClientConfig {
    token: Some(std::env::var("WHOP_API_KEY").unwrap()),
    ..Default::default()
};
let client = Whop::new(config).expect("Failed to build client");

// Full refund
let full_refund = client
    .payments
    .refund(&"pay_xxxxxxxxxxxxx".to_string(), &RefundPaymentsRequest::default(), None)
    .await?;

// Partial refund
let partial_refund = client
    .payments
    .refund(
        &"pay_xxxxxxxxxxxxx".to_string(),
        &RefundPaymentsRequest {
            partial_amount: Some(5.0), // in payment currency, e.g. 5 USD
        },
        None,
    )
    .await?;
```

```go Go theme={null}
import (
    "context"
    "log"
    "os"

    whopsdk "github.com/whopio/whopsdk-go/v2"
    "github.com/whopio/whopsdk-go/v2/client"
    "github.com/whopio/whopsdk-go/v2/option"
)

client := client.NewWhop(option.WithToken(os.Getenv("WHOP_API_KEY")))

// Full refund
_, err := client.Payments.Refund(context.TODO(), &whopsdk.RefundPaymentsRequest{
    ID: "pay_xxxxxxxxxxxxx",
})
if err != nil {
    log.Fatal(err)
}

// Partial refund
_, err = client.Payments.Refund(context.TODO(), &whopsdk.RefundPaymentsRequest{
    ID:            "pay_xxxxxxxxxxxxx",
    PartialAmount: whopsdk.Float64(5.0), // in payment currency, e.g. 5 USD
})
if err != nil {
    log.Fatal(err)
}
```

The response is the updated `Payment` object. The `refunded_amount` field reflects the cumulative refunded total. You can call `refund` multiple times for a payment up to its full amount.

> [!NOTE]
>
> Required permission: `payment:manage`. Add it from the [Permissions guide](https://docs.whop.com/developer/guides/permissions.md) before publishing your app.

### Inspect refunds

Refunds have their own resource if you need to look them up after the fact.

```typescript TypeScript theme={null}
const refund = await client.refunds.retrieve({ id: "ref_xxxxxxxxxxxxx" });

const refunds = await client.refunds.list({
  account_id: "biz_xxxxxxxxxxxxx",
});

for await (const item of refunds) {
  console.log(item);
}
```

```python Python theme={null}
refund = client.refunds.retrieve("ref_xxxxxxxxxxxxx")

for item in client.refunds.list(account_id="biz_xxxxxxxxxxxxx"):
    print(item)
```

```ruby Ruby theme={null}
refund = client.refunds.retrieve(id: "ref_xxxxxxxxxxxxx")

client.refunds.list(account_id: "biz_xxxxxxxxxxxxx").each do |item|
  puts item
end
```

```rust Rust theme={null}
let refund = client
    .refunds
    .retrieve(&"ref_xxxxxxxxxxxxx".to_string(), None)
    .await?;

let refunds = client
    .refunds
    .list(
        &RefundsListQueryRequest {
            account_id: Some("biz_xxxxxxxxxxxxx".to_string()),
            ..Default::default()
        },
        None,
    )
    .await?;

for item in refunds.data {
    println!("{item:?}");
}
```

```go Go theme={null}
refund, err := client.Refunds.Retrieve(context.TODO(), &whopsdk.RetrieveRefundsRequest{
    ID: "ref_xxxxxxxxxxxxx",
})
if err != nil {
    log.Fatal(err)
}
fmt.Println(refund)

page, err := client.Refunds.List(context.TODO(), &whopsdk.ListRefundsRequest{
    AccountID: whopsdk.String("biz_xxxxxxxxxxxxx"),
})
if err != nil {
    log.Fatal(err)
}

iter := page.Iterator()
for iter.Next(context.TODO()) {
    fmt.Println(iter.Current())
}
if err := iter.Err(); err != nil {
    log.Fatal(err)
}
```

## Disputes

A dispute (chargeback) opens when the buyer's bank challenges a payment. You have a window to respond with evidence before the bank decides. Whop forwards the dispute to you, you submit evidence, the bank rules, and the dispute closes.

### Watch for incoming disputes

Three events to subscribe to:

| Event | Meaning |
| - | - |
| [`dispute_alert.created`](https://docs.whop.com/api-reference/dispute-alerts/dispute-alert-created) | Dispute alert before a formal dispute. Some processors give a heads-up. Not every alert turns into a dispute. |
| [`dispute.created`](https://docs.whop.com/api-reference/disputes/dispute-created) | Formal dispute opened. Response window starts. |
| [`dispute.updated`](https://docs.whop.com/api-reference/disputes/dispute-updated) | Status changes (e.g. from `needs_response` to `under_review`, then `won` or `lost`). |

### List and retrieve

```typescript TypeScript theme={null}
const disputes = await client.disputes.list({
  account_id: "biz_xxxxxxxxxxxxx",
});

for await (const item of disputes) {
  console.log(item);
}

const dispute = await client.disputes.retrieve({ id: "dspt_xxxxxxxxxxxxx" });
```

```python Python theme={null}
for item in client.disputes.list(account_id="biz_xxxxxxxxxxxxx"):
    print(item)

dispute = client.disputes.retrieve("dspt_xxxxxxxxxxxxx")
```

```ruby Ruby theme={null}
client.disputes.list(account_id: "biz_xxxxxxxxxxxxx").each do |item|
  puts item
end

dispute = client.disputes.retrieve(id: "dspt_xxxxxxxxxxxxx")
```

```rust Rust theme={null}
let disputes = client
    .disputes
    .list(
        &DisputesListQueryRequest {
            account_id: Some("biz_xxxxxxxxxxxxx".to_string()),
            ..Default::default()
        },
        None,
    )
    .await?;

for item in disputes.data {
    println!("{item:?}");
}

let dispute = client
    .disputes
    .retrieve(&"dspt_xxxxxxxxxxxxx".to_string(), None)
    .await?;
```

```go Go theme={null}
page, err := client.Disputes.List(context.TODO(), &whopsdk.ListDisputesRequest{
    AccountID: whopsdk.String("biz_xxxxxxxxxxxxx"),
})
if err != nil {
    log.Fatal(err)
}

iter := page.Iterator()
for iter.Next(context.TODO()) {
    fmt.Println(iter.Current())
}
if err := iter.Err(); err != nil {
    log.Fatal(err)
}

dispute, err := client.Disputes.Retrieve(context.TODO(), &whopsdk.RetrieveDisputesRequest{
    ID: "dspt_xxxxxxxxxxxxx",
})
if err != nil {
    log.Fatal(err)
}
fmt.Println(dispute)
```

### Respond with evidence

Disputes have a two-step response: stage your evidence with `disputes.update`, then submit with `disputes.submit`. Update is idempotent. You can edit fields up until you submit.

```typescript TypeScript theme={null}
// Stage evidence (call as many times as needed)
await client.disputes.update({
  id: "dspt_xxxxxxxxxxxxx",
  evidence: {
    customer_name: "Dana Whitfield",
    customer_email_address: "dana@shinetime.example",
    product_description: "Ceramic Coating Package, booked 2026-01-15.",
    service_date: "2026-01-15",
    refund_policy_disclosure: "Customer agreed to no-refunds policy at checkout.",
    notes: "Customer used the product 47 times across 8 sessions before disputing.",
  },
});

// Submit evidence to the processor. After this, you can no longer edit.
await client.disputes.submit({ id: "dspt_xxxxxxxxxxxxx" });
```

```python Python theme={null}
client.disputes.update(
    "dspt_xxxxxxxxxxxxx",
    evidence={
        "customer_name": "Dana Whitfield",
        "customer_email_address": "dana@shinetime.example",
        "product_description": "Ceramic Coating Package, booked 2026-01-15.",
        "service_date": "2026-01-15",
        "refund_policy_disclosure": "Customer agreed to no-refunds policy at checkout.",
        "notes": "Customer used the product 47 times across 8 sessions before disputing.",
    },
)

client.disputes.submit("dspt_xxxxxxxxxxxxx")
```

```ruby Ruby theme={null}
# Stage evidence (call as many times as needed)
client.disputes.update(
  id: "dspt_xxxxxxxxxxxxx",
  evidence: {
    customer_name: "Dana Whitfield",
    customer_email_address: "dana@shinetime.example",
    product_description: "Ceramic Coating Package, booked 2026-01-15.",
    service_date: "2026-01-15",
    refund_policy_disclosure: "Customer agreed to no-refunds policy at checkout.",
    notes: "Customer used the product 47 times across 8 sessions before disputing.",
  },
)

# Submit evidence to the processor. After this, you can no longer edit.
client.disputes.submit(id: "dspt_xxxxxxxxxxxxx")
```

```rust Rust theme={null}
// Stage evidence (call as many times as needed)
client
    .disputes
    .update(
        &"dspt_xxxxxxxxxxxxx".to_string(),
        &UpdateDisputesRequest {
            evidence: Some(DisputesUpdateDisputesRequestEvidence {
                customer_name: Some("Dana Whitfield".to_string()),
                customer_email_address: Some("dana@shinetime.example".to_string()),
                product_description: Some(
                    "Ceramic Coating Package, booked 2026-01-15.".to_string(),
                ),
                service_date: Some("2026-01-15".to_string()),
                refund_policy_disclosure: Some(
                    "Customer agreed to no-refunds policy at checkout.".to_string(),
                ),
                notes: Some(
                    "Customer used the product 47 times across 8 sessions before disputing.".to_string(),
                ),
                ..Default::default()
            }),
        },
        None,
    )
    .await?;

// Submit evidence to the processor. After this, you can no longer edit.
client
    .disputes
    .submit(&"dspt_xxxxxxxxxxxxx".to_string(), None)
    .await?;
```

```go Go theme={null}
// Stage evidence (call as many times as needed)
_, err := client.Disputes.Update(context.TODO(), &whopsdk.UpdateDisputesRequest{
    ID: "dspt_xxxxxxxxxxxxx",
    Evidence: &whopsdk.UpdateDisputesRequestEvidence{
        CustomerName:           whopsdk.String("Dana Whitfield"),
        CustomerEmailAddress:   whopsdk.String("dana@shinetime.example"),
        ProductDescription:     whopsdk.String("Ceramic Coating Package, booked 2026-01-15."),
        ServiceDate:            whopsdk.String("2026-01-15"),
        RefundPolicyDisclosure: whopsdk.String("Customer agreed to no-refunds policy at checkout."),
        Notes:                  whopsdk.String("Customer used the product 47 times across 8 sessions before disputing."),
    },
})
if err != nil {
    log.Fatal(err)
}

// Submit evidence to the processor. After this, you can no longer edit.
if _, err := client.Disputes.Submit(context.TODO(), &whopsdk.SubmitDisputesRequest{
    ID: "dspt_xxxxxxxxxxxxx",
}); err != nil {
    log.Fatal(err)
}
```

### Evidence fields

Every field is optional, but the more you provide, the stronger the response. Common ones:

| Field | What to put in it |
| - | - |
| `customer_name` / `customer_email_address` | Identifying info you have on file |
| `product_description` | What the customer bought, in plain language |
| `service_date` | When you rendered the service (International Organization for Standardization (ISO) 8601) |
| `billing_address` | The address used at checkout |
| `customer_communication_attachment` | Email or chat threads showing the customer engaged |
| `cancellation_policy_attachment` / `cancellation_policy_disclosure` | Your policy and proof the customer accepted it |
| `refund_policy_attachment` / `refund_policy_disclosure` | Same for refunds |
| `refund_refusal_explanation` | Why you didn't refund (e.g. usage past return window) |
| `access_activity_log` | Login or usage logs proving the customer used the product |
| `uncategorized_attachment` | Anything else relevant |

Attachment fields take an uploaded file as `{ id: file.id }`. Upload first via the [Files API](https://docs.whop.com/developer/guides/upload-files.md).

### Dispute statuses

The dispute moves through a state machine. There are two phases: the early warning phase (statuses prefixed `warning_`, surfaced through `dispute_alert.created`) and the formal dispute phase. Every status the SDK can return:

| Status | Phase | Meaning |
| - | - | - |
| `warning_needs_response` | Alert | A dispute alert is open. You can preempt it with a refund or response before it escalates. |
| `warning_under_review` | Alert | You responded to the alert, and the processor is deciding whether to escalate. |
| `warning_closed` | Alert | The alert resolved without becoming a formal dispute. |
| `needs_response` | Dispute | Formal dispute open. Submit evidence before the response window closes. |
| `under_review` | Dispute | You submitted evidence, and the processor is deciding. |
| `won` | Dispute | Funds stay with you. |
| `lost` | Dispute | Funds go back to the buyer. Whop reverses the original payment. |
| `prevented` | Dispute | The customer was refunded before any ruling. Settles like `lost`. |

> [!NOTE]
>
> When a dispute is **lost**, Whop doesn't automatically create a Refund record, and you can't create one yourself. `payments.refund` rejects calls against a disputed payment with `This payment has been disputed. Therefore, it cannot be refunded.` Track the lost dispute as the source of truth, and don't expect a `refund.created` event.

## Permissions

| Action | Required permission |
| - | - |
| Create a refund | `payment:manage` |
| Retrieve / list refunds | `payment:basic:read` |
| Retrieve / list disputes | `payment:dispute:read` |
| Update or submit evidence | `payment:dispute` |

Add these from the [Permissions guide](https://docs.whop.com/developer/guides/permissions.md).

## Next steps

> [card] [Accept payments](https://docs.whop.com/developer/guides/accept-payments.md)
>
> The flow that creates the payments you'll later refund or defend.

> [card] [Listen to webhooks](https://docs.whop.com/developer/guides/webhooks.md)
>
> React to refund and dispute events server-side.

> [card] [Upload files](https://docs.whop.com/developer/guides/upload-files.md)
>
> Upload evidence attachments for dispute responses.

> [card] [Disputes API reference](https://docs.whop.com/api-reference/disputes/dispute)
>
> Full resource: fields, statuses, and every endpoint.


## Related topics

- [Preferred Settlement Currency](https://docs.whop.com/developer/payouts/preferred-settlement-currency.md)
- [Collect Payments for Subaccounts](https://docs.whop.com/developer/platforms/collect-payments-for-subaccounts.md)
- [Payments Overview](https://docs.whop.com/developer/payments/overview.md)
