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

# Manage Fraud

> Add your own payment rules on top of Whop's fraud controls to block or review risky checkouts, including ones that fail the bank's CVV or address check.

A fraudulent payment costs you the money, a [\$15 dispute fee](https://docs.whop.com/manage-your-business/manage-payments/manage-disputes.md), and a worse [dispute rate](https://docs.whop.com/trust-and-safety/account-health/managing-dispute-rates.md). Whop screens checkouts with its own fraud controls. Payment rules add your own layer on top: conditions you write, and an action Whop takes when a checkout matches them all.

Rules run after Whop's controls and never loosen them. A payment Whop blocks stays blocked.

## Four ways a rule can act

| Action | What happens | When several rules match |
| - | - | - |
| `block` | The payment is declined before authorization. The buyer sees a generic decline. | Beats `review` and `enforce_3ds` |
| `review` | An eligible card is authorized without capture. Automatic capture is scheduled for 48 hours after authorization. You can capture or void it sooner. Unsupported methods skip review and continue through normal payment processing. | Beats `enforce_3ds` |
| `enforce_3ds` | The buyer confirms the payment with their bank through 3D Secure. | Lowest precedence |
| `allow` | The payment skips your other rules. It never skips Whop's controls. | Beats all three |

A rule can read `risk_score`, `amount_in_usd`, `card_country`, `card_bin`, `customer_email`, and `ip_address` before the bank is asked, or the bank's own `cvv_check` and `avs_check` results once it has answered. `card_bin` is the Bank Identification Number of the card used to make the payment: the first six digits of the card number. [List fields](https://docs.whop.com/api-reference/payment-rules/list-fields.md) returns the operators and values each one accepts.

By default, automatic renewals skip your account's rules, and other off-session charges, such as a saved card charged through the API, use only your `block` and `allow` rules. Turn on **Apply rules to off-session payments** in your payment rules settings to apply every active rule to future off-session attempts, renewals included. You can also set `off_session_payment_rules` to `true` with [Update account preferences](https://docs.whop.com/api-reference/accounts/update-account-preferences.md). Set it back to `false` to opt out. On-session payments continue to use your rules in either case.

Where you're unsure, challenge rather than block. A block also turns away real buyers who match, while a challenge lets them prove they're the cardholder.

> [!NOTE]
>
> Run these examples in a trusted server environment with `WHOP_API_KEY` set.
> Reuse the imports and client setup from the first example for your language.

## Block the obvious fraud

Start with the pattern you see most in your [disputes](https://docs.whop.com/developer/guides/refunds-and-disputes.md). This rule blocks a payment Whop scores 85 or higher on a card issued outside the US.

```typescript TypeScript theme={null}
import { WhopClient } from "@whop/sdk";

const client = new WhopClient({ token: process.env.WHOP_API_KEY });

const rule = await client.paymentRules.create({
	account_id: "biz_xxxxxxxxxxxxx",
	name: "Block high-risk cards from outside the US",
	action: "block",
	conditions: {
		all: [
			{ field: "risk_score", operator: "gte", value: 85 },
			{ field: "card_country", operator: "neq", value: "US" },
		],
	},
});

console.log(rule.id); // prule_xxxxxxxxxxxxx
```

```python Python theme={null}
import os
from whop_sdk import Whop
from whop_sdk.payment_rules import (
    CreatePaymentRulesRequestConditions,
    CreatePaymentRulesRequestConditionsAllItem,
)

client = Whop(token=os.environ["WHOP_API_KEY"])

rule = client.payment_rules.create(
    account_id="biz_xxxxxxxxxxxxx",
    name="Block high-risk cards from outside the US",
    action="block",
    conditions=CreatePaymentRulesRequestConditions(
        all_=[
            CreatePaymentRulesRequestConditionsAllItem(field="risk_score", operator="gte", value=85),
            CreatePaymentRulesRequestConditionsAllItem(field="card_country", operator="neq", value="US"),
        ],
    ),
)

print(rule.id)  # prule_xxxxxxxxxxxxx
```

```ruby Ruby theme={null}
require "whop_sdk"

client = Whop_sdk::Client.new(token: ENV.fetch("WHOP_API_KEY"))

rule = client.payment_rules.create(
  account_id: "biz_xxxxxxxxxxxxx",
  name: "Block high-risk cards from outside the US",
  action: "block",
  conditions: {
    all: [
      { field: "risk_score", operator: "gte", value: 85 },
      { field: "card_country", operator: "neq", value: "US" }
    ]
  }
)

puts rule.id # prule_xxxxxxxxxxxxx
```

```rust Rust theme={null}
use whop_sdk::prelude::*;

let config = ClientConfig {
    token: Some(std::env::var("WHOP_API_KEY").unwrap()),
    ..Default::default()
};
let client = Whop::new(config).expect("Failed to build client");

let rule = client
    .payment_rules
    .create(
        &CreatePaymentRulesRequest {
            name: "Block high-risk cards from outside the US".to_string(),
            action: CreatePaymentRulesRequestAction::Block,
            conditions: CreatePaymentRulesRequestConditions {
                all: vec![
                    CreatePaymentRulesRequestConditionsAllItem {
                        field: CreatePaymentRulesRequestConditionsAllItemField::RiskScore,
                        operator: CreatePaymentRulesRequestConditionsAllItemOperator::Gte,
                        value: PaymentRuleConditionValue::Integer(85),
                    },
                    CreatePaymentRulesRequestConditionsAllItem {
                        field: CreatePaymentRulesRequestConditionsAllItemField::CardCountry,
                        operator: CreatePaymentRulesRequestConditionsAllItemOperator::Neq,
                        value: PaymentRuleConditionValue::String("US".to_string()),
                    },
                ],
                ..Default::default()
            },
            account_id: Some("biz_xxxxxxxxxxxxx".to_string()),
            metadata: None,
        },
        None,
    )
    .await?;

println!("{}", rule.id); // prule_xxxxxxxxxxxxx
```

```go Go theme={null}
import (
    "context"
    "fmt"
    "log"
    "os"

    whopsdk "github.com/whopio/whopsdk-go/v2"
    "github.com/whopio/whopsdk-go/v2/client"
    "github.com/whopio/whopsdk-go/v2/option"
)

client := client.NewWhop(option.WithToken(os.Getenv("WHOP_API_KEY")))

rule, err := client.PaymentRules.Create(context.TODO(), &whopsdk.CreatePaymentRulesRequest{
    AccountID: whopsdk.String("biz_xxxxxxxxxxxxx"),
    Name:      "Block high-risk cards from outside the US",
    Action:    whopsdk.CreatePaymentRulesRequestActionBlock,
    Conditions: &whopsdk.CreatePaymentRulesRequestConditions{
        All: []*whopsdk.CreatePaymentRulesRequestConditionsAllItem{
            {
                Field:    whopsdk.CreatePaymentRulesRequestConditionsAllItemFieldRiskScore,
                Operator: whopsdk.CreatePaymentRulesRequestConditionsAllItemOperatorGte,
                Value:    &whopsdk.PaymentRuleConditionValue{Integer: 85},
            },
            {
                Field:    whopsdk.CreatePaymentRulesRequestConditionsAllItemFieldCardCountry,
                Operator: whopsdk.CreatePaymentRulesRequestConditionsAllItemOperatorNeq,
                Value:    &whopsdk.PaymentRuleConditionValue{String: "US"},
            },
        },
    },
})
if err != nil {
    log.Fatal(err)
}
fmt.Println(rule.ID) // prule_xxxxxxxxxxxxx
```

```java Java theme={null}
import com.whop.api.WhopApiClient;
import com.whop.api.resources.paymentrules.requests.CreatePaymentRulesRequest;
import com.whop.api.resources.paymentrules.types.*;
import com.whop.api.types.PaymentRuleConditionValue;
import java.util.List;

var client = WhopApiClient.builder()
    .token(System.getenv("WHOP_API_KEY"))
    .build();

var rule = client.paymentRules().create(
    CreatePaymentRulesRequest.builder()
        .accountId("biz_xxxxxxxxxxxxx")
        .name("Block high-risk cards from outside the US")
        .action(CreatePaymentRulesRequestAction.BLOCK)
        .conditions(CreatePaymentRulesRequestConditions.builder()
            .all(List.of(
                CreatePaymentRulesRequestConditionsAllItem.builder()
                    .field(CreatePaymentRulesRequestConditionsAllItemField.RISK_SCORE)
                    .operator(CreatePaymentRulesRequestConditionsAllItemOperator.GTE)
                    .value(PaymentRuleConditionValue.of(85))
                    .build(),
                CreatePaymentRulesRequestConditionsAllItem.builder()
                    .field(CreatePaymentRulesRequestConditionsAllItemField.CARD_COUNTRY)
                    .operator(CreatePaymentRulesRequestConditionsAllItemOperator.NEQ)
                    .value(PaymentRuleConditionValue.of("US"))
                    .build()
            ))
            .build())
        .build()
);
System.out.println(rule.getId());
```

```swift Swift theme={null}
import Foundation
import WhopSDK

let client = Whop(token: ProcessInfo.processInfo.environment["WHOP_API_KEY"]!)

let rule = try await client.paymentRules.create(
    request: .init(
        accountId: "biz_xxxxxxxxxxxxx",
        action: .block,
        conditions: .init(all: [
            .init(
                field: .riskScore,
                operator: .gte,
                value: .int(85)
            ),
            .init(
                field: .cardCountry,
                operator: .neq,
                value: .string("US")
            )
        ]),
        name: "Block high-risk cards from outside the US"
    )
)
print(rule.id)
```

```bash cURL theme={null}
curl "https://api.whop.com/api/v1/payment_rules" \
  -H "Authorization: Bearer $WHOP_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "account_id": "biz_xxxxxxxxxxxxx",
    "name": "Block high-risk cards from outside the US",
    "action": "block",
    "conditions": {
      "all": [
        { "field": "risk_score", "operator": "gte", "value": 85 },
        { "field": "card_country", "operator": "neq", "value": "US" }
      ]
    }
  }'
```

Always pass `account_id`. Without it the rule lands on whichever account your credential defaults to, which is easy to get wrong for a platform that manages several. Rules are published to checkout every minute, so a new or changed rule starts applying within a couple of minutes.

> [!NOTE]
>
> Required permission: `payment:manage` to write rules, `payment:basic:read` to
> read them. Add permissions from the [Permissions
> guide](https://docs.whop.com/developer/guides/permissions.md).

## Challenge large orders

A large order from a stolen card is the chargeback you least want. A large order from a real customer is the sale you least want to lose. A 3D Secure challenge separates the two, and the payment records `three_ds_verified: true` when the buyer passes.

`enforce_3ds` is skipped, but still recorded on the payment, when the checkout can't carry a challenge. That covers off-session payments such as renewals, variants that set their own 3D Secure level, non-card payments, and cards the processor can't challenge. American Express is only challenged on USD payments.

```typescript TypeScript theme={null}
await client.paymentRules.create({
	account_id: "biz_xxxxxxxxxxxxx",
	name: "Challenge orders over $500",
	action: "enforce_3ds",
	conditions: {
		all: [{ field: "amount_in_usd", operator: "gte", value: 500 }],
	},
});
```

```python Python theme={null}
client.payment_rules.create(
    account_id="biz_xxxxxxxxxxxxx",
    name="Challenge orders over $500",
    action="enforce_3ds",
    conditions=CreatePaymentRulesRequestConditions(
        all_=[CreatePaymentRulesRequestConditionsAllItem(field="amount_in_usd", operator="gte", value=500)],
    ),
)
```

```ruby Ruby theme={null}
client.payment_rules.create(
  account_id: "biz_xxxxxxxxxxxxx",
  name: "Challenge orders over $500",
  action: "enforce_3ds",
  conditions: {
    all: [{ field: "amount_in_usd", operator: "gte", value: 500 }]
  }
)
```

```rust Rust theme={null}
client
    .payment_rules
    .create(
        &CreatePaymentRulesRequest {
            name: "Challenge orders over $500".to_string(),
            action: CreatePaymentRulesRequestAction::Enforce3Ds,
            conditions: CreatePaymentRulesRequestConditions {
                all: vec![CreatePaymentRulesRequestConditionsAllItem {
                    field: CreatePaymentRulesRequestConditionsAllItemField::AmountInUsd,
                    operator: CreatePaymentRulesRequestConditionsAllItemOperator::Gte,
                    value: PaymentRuleConditionValue::Integer(500),
                }],
                ..Default::default()
            },
            account_id: Some("biz_xxxxxxxxxxxxx".to_string()),
            metadata: None,
        },
        None,
    )
    .await?;
```

```go Go theme={null}
_, err = client.PaymentRules.Create(context.TODO(), &whopsdk.CreatePaymentRulesRequest{
    AccountID: whopsdk.String("biz_xxxxxxxxxxxxx"),
    Name:      "Challenge orders over $500",
    Action:    whopsdk.CreatePaymentRulesRequestActionEnforce3Ds,
    Conditions: &whopsdk.CreatePaymentRulesRequestConditions{
        All: []*whopsdk.CreatePaymentRulesRequestConditionsAllItem{
            {
                Field:    whopsdk.CreatePaymentRulesRequestConditionsAllItemFieldAmountInUsd,
                Operator: whopsdk.CreatePaymentRulesRequestConditionsAllItemOperatorGte,
                Value:    &whopsdk.PaymentRuleConditionValue{Integer: 500},
            },
        },
    },
})
if err != nil {
    log.Fatal(err)
}
```

```java Java theme={null}
var rule = client.paymentRules().create(
    CreatePaymentRulesRequest.builder()
        .accountId("biz_xxxxxxxxxxxxx")
        .name("Challenge orders over $500")
        .action(CreatePaymentRulesRequestAction.valueOf("enforce_3ds"))
        .conditions(CreatePaymentRulesRequestConditions.builder()
            .all(List.of(
                CreatePaymentRulesRequestConditionsAllItem.builder()
                    .field(CreatePaymentRulesRequestConditionsAllItemField.AMOUNT_IN_USD)
                    .operator(CreatePaymentRulesRequestConditionsAllItemOperator.GTE)
                    .value(PaymentRuleConditionValue.of(500))
                    .build()
            ))
            .build())
        .build()
);
System.out.println(rule.getId());
```

```swift Swift theme={null}
let rule = try await client.paymentRules.create(
    request: .init(
        accountId: "biz_xxxxxxxxxxxxx",
        action: .enforce3ds,
        conditions: .init(all: [
            .init(
                field: .amountInUsd,
                operator: .gte,
                value: .int(500)
            )
        ]),
        name: "Challenge orders over $500"
    )
)
print(rule.id)
```

```bash cURL theme={null}
curl "https://api.whop.com/api/v1/payment_rules" \
  -H "Authorization: Bearer $WHOP_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "account_id": "biz_xxxxxxxxxxxxx",
    "name": "Challenge orders over $500",
    "action": "enforce_3ds",
    "conditions": {
      "all": [
        { "field": "amount_in_usd", "operator": "gte", "value": 500 }
      ]
    }
  }'
```

## Review a payment before charging it

A `review` rule authorizes an eligible card payment without capturing it. The buyer completes checkout and gets membership access at authorization. Use it for orders you fulfil by hand, or for a pattern that's risky but not certainly fraud. Whop captures automatically 48 hours later unless you act first:

* [Capture](https://docs.whop.com/api-reference/payments/capture-payment.md) to collect the money. The payment becomes `paid` and Whop sends `payment.succeeded`, your signal to fulfil the order.
* [Void](https://docs.whop.com/api-reference/payments/void-payment.md) to release the hold. Whop revokes access, returns reserved stock, and sends `payment.canceled`.

Whop sends `payment.authorized` when the hold starts, and for API-requested authorizations too. A held payment reads `status: "authorized"` with `substatus: "requires_capture"`. [Retrieve the payment status](https://docs.whop.com/api-reference/payments/retrieve-payment-status.md) for `auto_capture_at`.

This rule holds card payments of at least \$250 whose card was issued outside the US.

```typescript TypeScript theme={null}
await client.paymentRules.create({
	account_id: "biz_xxxxxxxxxxxxx",
	name: "Review large orders from abroad",
	action: "review",
	conditions: {
		all: [
			{ field: "amount_in_usd", operator: "gte", value: 250 },
			{ field: "card_country", operator: "neq", value: "US" },
		],
	},
});
```

```python Python theme={null}
client.payment_rules.create(
    account_id="biz_xxxxxxxxxxxxx",
    name="Review large orders from abroad",
    action="review",
    conditions=CreatePaymentRulesRequestConditions(
        all_=[
            CreatePaymentRulesRequestConditionsAllItem(field="amount_in_usd", operator="gte", value=250),
            CreatePaymentRulesRequestConditionsAllItem(field="card_country", operator="neq", value="US"),
        ],
    ),
)
```

```ruby Ruby theme={null}
client.payment_rules.create(
  account_id: "biz_xxxxxxxxxxxxx",
  name: "Review large orders from abroad",
  action: "review",
  conditions: {
    all: [
      { field: "amount_in_usd", operator: "gte", value: 250 },
      { field: "card_country", operator: "neq", value: "US" }
    ]
  }
)
```

```rust Rust theme={null}
client
    .payment_rules
    .create(
        &CreatePaymentRulesRequest {
            name: "Review large orders from abroad".to_string(),
            action: CreatePaymentRulesRequestAction::Review,
            conditions: CreatePaymentRulesRequestConditions {
                all: vec![
                    CreatePaymentRulesRequestConditionsAllItem {
                        field: CreatePaymentRulesRequestConditionsAllItemField::AmountInUsd,
                        operator: CreatePaymentRulesRequestConditionsAllItemOperator::Gte,
                        value: PaymentRuleConditionValue::Integer(250),
                    },
                    CreatePaymentRulesRequestConditionsAllItem {
                        field: CreatePaymentRulesRequestConditionsAllItemField::CardCountry,
                        operator: CreatePaymentRulesRequestConditionsAllItemOperator::Neq,
                        value: PaymentRuleConditionValue::String("US".to_string()),
                    },
                ],
                ..Default::default()
            },
            account_id: Some("biz_xxxxxxxxxxxxx".to_string()),
            metadata: None,
        },
        None,
    )
    .await?;
```

```go Go theme={null}
_, err = client.PaymentRules.Create(context.TODO(), &whopsdk.CreatePaymentRulesRequest{
    AccountID: whopsdk.String("biz_xxxxxxxxxxxxx"),
    Name:      "Review large orders from abroad",
    Action:    whopsdk.CreatePaymentRulesRequestActionReview,
    Conditions: &whopsdk.CreatePaymentRulesRequestConditions{
        All: []*whopsdk.CreatePaymentRulesRequestConditionsAllItem{
            {
                Field:    whopsdk.CreatePaymentRulesRequestConditionsAllItemFieldAmountInUsd,
                Operator: whopsdk.CreatePaymentRulesRequestConditionsAllItemOperatorGte,
                Value:    &whopsdk.PaymentRuleConditionValue{Integer: 250},
            },
            {
                Field:    whopsdk.CreatePaymentRulesRequestConditionsAllItemFieldCardCountry,
                Operator: whopsdk.CreatePaymentRulesRequestConditionsAllItemOperatorNeq,
                Value:    &whopsdk.PaymentRuleConditionValue{String: "US"},
            },
        },
    },
})
if err != nil {
    log.Fatal(err)
}
```

```java Java theme={null}
var rule = client.paymentRules().create(
    CreatePaymentRulesRequest.builder()
        .accountId("biz_xxxxxxxxxxxxx")
        .name("Review large orders from abroad")
        .action(CreatePaymentRulesRequestAction.REVIEW)
        .conditions(CreatePaymentRulesRequestConditions.builder()
            .all(List.of(
                CreatePaymentRulesRequestConditionsAllItem.builder()
                    .field(CreatePaymentRulesRequestConditionsAllItemField.AMOUNT_IN_USD)
                    .operator(CreatePaymentRulesRequestConditionsAllItemOperator.GTE)
                    .value(PaymentRuleConditionValue.of(250))
                    .build(),
                CreatePaymentRulesRequestConditionsAllItem.builder()
                    .field(CreatePaymentRulesRequestConditionsAllItemField.CARD_COUNTRY)
                    .operator(CreatePaymentRulesRequestConditionsAllItemOperator.NEQ)
                    .value(PaymentRuleConditionValue.of("US"))
                    .build()
            ))
            .build())
        .build()
);
System.out.println(rule.getId());
```

```swift Swift theme={null}
let rule = try await client.paymentRules.create(
    request: .init(
        accountId: "biz_xxxxxxxxxxxxx",
        action: .review,
        conditions: .init(all: [
            .init(
                field: .amountInUsd,
                operator: .gte,
                value: .int(250)
            ),
            .init(
                field: .cardCountry,
                operator: .neq,
                value: .string("US")
            )
        ]),
        name: "Review large orders from abroad"
    )
)
print(rule.id)
```

```bash cURL theme={null}
curl "https://api.whop.com/api/v1/payment_rules" \
  -H "Authorization: Bearer $WHOP_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "account_id": "biz_xxxxxxxxxxxxx",
    "name": "Review large orders from abroad",
    "action": "review",
    "conditions": {
      "all": [
        { "field": "amount_in_usd", "operator": "gte", "value": 250 },
        { "field": "card_country", "operator": "neq", "value": "US" }
      ]
    }
  }'
```

Once you have reviewed the order, capture it by payment ID:

```typescript TypeScript theme={null}
await client.payments.capture({ id: "pay_xxxxxxxxxxxxx" });
```

```python Python theme={null}
client.payments.capture(id="pay_xxxxxxxxxxxxx")
```

```ruby Ruby theme={null}
client.payments.capture(id: "pay_xxxxxxxxxxxxx")
```

```rust Rust theme={null}
client.payments.capture("pay_xxxxxxxxxxxxx", None).await?;
```

```go Go theme={null}
_, err = client.Payments.Capture(context.TODO(), &whopsdk.CapturePaymentsRequest{
    ID: "pay_xxxxxxxxxxxxx",
})
if err != nil {
    log.Fatal(err)
}
```

```java Java theme={null}
client.payments().capture("pay_xxxxxxxxxxxxx");
```

```swift Swift theme={null}
_ = try await client.payments.capture(id: "pay_xxxxxxxxxxxxx")
```

```bash cURL theme={null}
curl -X POST "https://api.whop.com/api/v1/payments/pay_xxxxxxxxxxxxx/capture" \
  -H "Authorization: Bearer $WHOP_API_KEY"
```

Or void it:

```typescript TypeScript theme={null}
await client.payments.void({ id: "pay_xxxxxxxxxxxxx" });
```

```python Python theme={null}
client.payments.void(id="pay_xxxxxxxxxxxxx")
```

```ruby Ruby theme={null}
client.payments.void(id: "pay_xxxxxxxxxxxxx")
```

```rust Rust theme={null}
client.payments.void("pay_xxxxxxxxxxxxx", None).await?;
```

```go Go theme={null}
_, err = client.Payments.Void(context.TODO(), &whopsdk.VoidPaymentsRequest{
    ID: "pay_xxxxxxxxxxxxx",
})
if err != nil {
    log.Fatal(err)
}
```

```java Java theme={null}
client.payments().void_("pay_xxxxxxxxxxxxx");
```

```swift Swift theme={null}
_ = try await client.payments.void(id: "pay_xxxxxxxxxxxxx")
```

```bash cURL theme={null}
curl -X POST "https://api.whop.com/api/v1/payments/pay_xxxxxxxxxxxxx/void" \
  -H "Authorization: Bearer $WHOP_API_KEY"
```

### Review limitations

* **Eligible cards can be held on or off session.** Off-session charges, including subscription renewals, require the account to enable `off_session_payment_rules`. Apple Pay, Google Pay, bank, balance, and numbered invoice payments skip review. The match is still recorded in `payment_rule_matches`.
* **Payments created with `capture: false` keep their own schedule.** A review rule never overrides it.
* **Capture is all or nothing.** Refund afterwards to return part of it.
* **Buyers have the product before you decide.** Tell them if you void.

## Act on a failed address or CVV check

When a buyer types a card in, the bank checks the CVV and the billing address. Those results arrive with the authorization, so Whop authorizes the card first, then runs your rules on `cvv_check` and `avs_check`. A matching `block` rule voids and declines the payment. A matching `review` rule holds it for you to capture or void, with the same 48-hour automatic capture as any review. Whop captures the payment when no rule matches. These rules only `block` or `review`, and can't share a rule with other fields.

`avs_check` is the address verification (AVS) result. It merges the bank's postal code and street address checks into one value.

| Result | `cvv_check` | `avs_check` |
| - | - | - |
| `PASS` | The CVV matched. | Every part of the address the bank checked matched. |
| `PARTIAL` | Not used. | The postal code or the street address matched, and the other didn't. |
| `FAIL` | The CVV didn't match. | No part of the address the bank checked matched. |
| `UNAVAILABLE` | The bank couldn't run the check. | The bank couldn't run the check. |
| `UNCHECKED` | The bank didn't run the check. | The bank didn't run the check. |

A rule on `FAIL` matches only `FAIL`. To act on more than one result, use `in` with a list: `{ "field": "avs_check", "operator": "in", "value": ["FAIL", "PARTIAL"] }`. A partial match is common on real cards, so review it before you block it.

To decline when either check fails, write one rule per check. To decline only when both fail, put both conditions in one rule. Apple Pay, Google Pay, saved cards, and renewals carry no checks and never match. A payment you hold yourself, with a `review` rule or `capture: false`, is left for you to capture or void.

```bash cURL theme={null}
curl "https://api.whop.com/api/v1/payment_rules" \
  -H "Authorization: Bearer $WHOP_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "account_id": "biz_xxxxxxxxxxxxx",
    "name": "Decline when CVV and address both fail",
    "action": "block",
    "conditions": {
      "all": [
        { "field": "cvv_check", "operator": "eq", "value": "FAIL" },
        { "field": "avs_check", "operator": "eq", "value": "FAIL" }
      ]
    }
  }'
```

The matched rule shows on the payment under `payment_rule_matches`, next to the bank's results in `verification_checks`.

Whop doesn't send `payment.authorized` while it checks the card. A payment the rules pass sends `payment.succeeded` as usual. A payment they decline is voided: it reads `status: "void"`, and Whop sends `payment.canceled`. A payment they hold for review sends `payment.authorized` once the hold is yours.

## Keep trusted buyers moving

An `allow` rule exempts buyers you trust from your block, review, and challenge rules.

```typescript TypeScript theme={null}
await client.paymentRules.create({
	account_id: "biz_xxxxxxxxxxxxx",
	name: "Trusted customers",
	action: "allow",
	conditions: {
		all: [
			{
				field: "customer_email",
				operator: "in",
				value: ["ava@example.com", "noah@example.com"],
			},
		],
	},
});
```

```python Python theme={null}
client.payment_rules.create(
    account_id="biz_xxxxxxxxxxxxx",
    name="Trusted customers",
    action="allow",
    conditions=CreatePaymentRulesRequestConditions(
        all_=[
            CreatePaymentRulesRequestConditionsAllItem(
                field="customer_email",
                operator="in",
                value=["ava@example.com", "noah@example.com"],
            )
        ],
    ),
)
```

```ruby Ruby theme={null}
client.payment_rules.create(
  account_id: "biz_xxxxxxxxxxxxx",
  name: "Trusted customers",
  action: "allow",
  conditions: {
    all: [
      { field: "customer_email", operator: "in", value: ["ava@example.com", "noah@example.com"] }
    ]
  }
)
```

```rust Rust theme={null}
client
    .payment_rules
    .create(
        &CreatePaymentRulesRequest {
            name: "Trusted customers".to_string(),
            action: CreatePaymentRulesRequestAction::Allow,
            conditions: CreatePaymentRulesRequestConditions {
                all: vec![CreatePaymentRulesRequestConditionsAllItem {
                    field: CreatePaymentRulesRequestConditionsAllItemField::CustomerEmail,
                    operator: CreatePaymentRulesRequestConditionsAllItemOperator::In,
                    value: PaymentRuleConditionValue::StringList(vec![
                        "ava@example.com".to_string(),
                        "noah@example.com".to_string(),
                    ]),
                }],
                ..Default::default()
            },
            account_id: Some("biz_xxxxxxxxxxxxx".to_string()),
            metadata: None,
        },
        None,
    )
    .await?;
```

```go Go theme={null}
_, err = client.PaymentRules.Create(context.TODO(), &whopsdk.CreatePaymentRulesRequest{
    AccountID: whopsdk.String("biz_xxxxxxxxxxxxx"),
    Name:      "Trusted customers",
    Action:    whopsdk.CreatePaymentRulesRequestActionAllow,
    Conditions: &whopsdk.CreatePaymentRulesRequestConditions{
        All: []*whopsdk.CreatePaymentRulesRequestConditionsAllItem{
            {
                Field:    whopsdk.CreatePaymentRulesRequestConditionsAllItemFieldCustomerEmail,
                Operator: whopsdk.CreatePaymentRulesRequestConditionsAllItemOperatorIn,
                Value: &whopsdk.PaymentRuleConditionValue{
                    StringList: []string{"ava@example.com", "noah@example.com"},
                },
            },
        },
    },
})
if err != nil {
    log.Fatal(err)
}
```

```java Java theme={null}
var rule = client.paymentRules().create(
    CreatePaymentRulesRequest.builder()
        .accountId("biz_xxxxxxxxxxxxx")
        .name("Trusted customers")
        .action(CreatePaymentRulesRequestAction.ALLOW)
        .conditions(CreatePaymentRulesRequestConditions.builder()
            .all(List.of(
                CreatePaymentRulesRequestConditionsAllItem.builder()
                    .field(CreatePaymentRulesRequestConditionsAllItemField.CUSTOMER_EMAIL)
                    .operator(CreatePaymentRulesRequestConditionsAllItemOperator.IN)
                    .value(PaymentRuleConditionValue.of(List.of("ava@example.com", "noah@example.com")))
                    .build()
            ))
            .build())
        .build()
);
System.out.println(rule.getId());
```

```swift Swift theme={null}
let rule = try await client.paymentRules.create(
    request: .init(
        accountId: "biz_xxxxxxxxxxxxx",
        action: .allow,
        conditions: .init(all: [
            .init(
                field: .customerEmail,
                operator: .`in`,
                value: .stringArray(["ava@example.com", "noah@example.com"])
            )
        ]),
        name: "Trusted customers"
    )
)
print(rule.id)
```

```bash cURL theme={null}
curl "https://api.whop.com/api/v1/payment_rules" \
  -H "Authorization: Bearer $WHOP_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "account_id": "biz_xxxxxxxxxxxxx",
    "name": "Trusted customers",
    "action": "allow",
    "conditions": {
      "all": [
        { "field": "customer_email", "operator": "in", "value": ["ava@example.com", "noah@example.com"] }
      ]
    }
  }'
```

## Tighten a rule

What a rule does is fixed once created, so the payments it decided keep naming the rule that decided them. To change its action or conditions, [replace](https://docs.whop.com/api-reference/payment-rules/replace-a-payment-rule.md) it: the old rule is deleted and a successor with a new ID inherits its name, `metadata`, and active state. Here the block threshold is raised from 85 to 90 after catching too many real buyers.

```typescript TypeScript theme={null}
const successor = await client.paymentRules.replace({
	id: "prule_xxxxxxxxxxxxx",
	action: "block",
	conditions: {
		all: [
			{ field: "risk_score", operator: "gte", value: 90 },
			{ field: "card_country", operator: "neq", value: "US" },
		],
	},
});

console.log(successor.id); // a new prule_ ID
```

```python Python theme={null}
from whop_sdk.payment_rules import (
    ReplacePaymentRulesRequestConditions,
    ReplacePaymentRulesRequestConditionsAllItem,
)

successor = client.payment_rules.replace(
    id="prule_xxxxxxxxxxxxx",
    action="block",
    conditions=ReplacePaymentRulesRequestConditions(
        all_=[
            ReplacePaymentRulesRequestConditionsAllItem(field="risk_score", operator="gte", value=90),
            ReplacePaymentRulesRequestConditionsAllItem(field="card_country", operator="neq", value="US"),
        ],
    ),
)

print(successor.id)  # a new prule_ ID
```

```ruby Ruby theme={null}
successor = client.payment_rules.replace(
  id: "prule_xxxxxxxxxxxxx",
  action: "block",
  conditions: {
    all: [
      { field: "risk_score", operator: "gte", value: 90 },
      { field: "card_country", operator: "neq", value: "US" }
    ]
  }
)

puts successor.id # a new prule_ ID
```

```rust Rust theme={null}
let successor = client
    .payment_rules
    .replace(
        &"prule_xxxxxxxxxxxxx".to_string(),
        &ReplacePaymentRulesRequest {
            action: ReplacePaymentRulesRequestAction::Block,
            conditions: ReplacePaymentRulesRequestConditions {
                all: vec![
                    ReplacePaymentRulesRequestConditionsAllItem {
                        field: ReplacePaymentRulesRequestConditionsAllItemField::RiskScore,
                        operator: ReplacePaymentRulesRequestConditionsAllItemOperator::Gte,
                        value: PaymentRuleConditionValue::Integer(90),
                    },
                    ReplacePaymentRulesRequestConditionsAllItem {
                        field: ReplacePaymentRulesRequestConditionsAllItemField::CardCountry,
                        operator: ReplacePaymentRulesRequestConditionsAllItemOperator::Neq,
                        value: PaymentRuleConditionValue::String("US".to_string()),
                    },
                ],
                ..Default::default()
            },
        },
        None,
    )
    .await?;

println!("{}", successor.id); // a new prule_ ID
```

```go Go theme={null}
successor, err := client.PaymentRules.Replace(context.TODO(), &whopsdk.ReplacePaymentRulesRequest{
    ID:     "prule_xxxxxxxxxxxxx",
    Action: whopsdk.ReplacePaymentRulesRequestActionBlock,
    Conditions: &whopsdk.ReplacePaymentRulesRequestConditions{
        All: []*whopsdk.ReplacePaymentRulesRequestConditionsAllItem{
            {
                Field:    whopsdk.ReplacePaymentRulesRequestConditionsAllItemFieldRiskScore,
                Operator: whopsdk.ReplacePaymentRulesRequestConditionsAllItemOperatorGte,
                Value:    &whopsdk.PaymentRuleConditionValue{Integer: 90},
            },
            {
                Field:    whopsdk.ReplacePaymentRulesRequestConditionsAllItemFieldCardCountry,
                Operator: whopsdk.ReplacePaymentRulesRequestConditionsAllItemOperatorNeq,
                Value:    &whopsdk.PaymentRuleConditionValue{String: "US"},
            },
        },
    },
})
if err != nil {
    log.Fatal(err)
}
fmt.Println(successor.ID) // a new prule_ ID
```

```java Java theme={null}
import com.whop.api.resources.paymentrules.requests.ReplacePaymentRulesRequest;

var successor = client.paymentRules().replace(
    "prule_xxxxxxxxxxxxx",
    ReplacePaymentRulesRequest.builder()
        .action(ReplacePaymentRulesRequestAction.BLOCK)
        .conditions(ReplacePaymentRulesRequestConditions.builder()
            .all(List.of(
                ReplacePaymentRulesRequestConditionsAllItem.builder()
                    .field(ReplacePaymentRulesRequestConditionsAllItemField.RISK_SCORE)
                    .operator(ReplacePaymentRulesRequestConditionsAllItemOperator.GTE)
                    .value(PaymentRuleConditionValue.of(90))
                    .build(),
                ReplacePaymentRulesRequestConditionsAllItem.builder()
                    .field(ReplacePaymentRulesRequestConditionsAllItemField.CARD_COUNTRY)
                    .operator(ReplacePaymentRulesRequestConditionsAllItemOperator.NEQ)
                    .value(PaymentRuleConditionValue.of("US"))
                    .build()
            ))
            .build())
        .build()
);
System.out.println(successor.getId());
```

```swift Swift theme={null}
let successor = try await client.paymentRules.replace(
    id: "prule_xxxxxxxxxxxxx",
    request: .init(
        action: .block,
        conditions: .init(all: [
            .init(
                field: .riskScore,
                operator: .gte,
                value: .int(90)
            ),
            .init(
                field: .cardCountry,
                operator: .neq,
                value: .string("US")
            )
        ])
    )
)
print(successor.id)
```

```bash cURL theme={null}
curl "https://api.whop.com/api/v1/payment_rules/prule_xxxxxxxxxxxxx/replace" \
  -H "Authorization: Bearer $WHOP_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "action": "block",
    "conditions": {
      "all": [
        { "field": "risk_score", "operator": "gte", "value": 90 },
        { "field": "card_country", "operator": "neq", "value": "US" }
      ]
    }
  }'
```

[Deactivate](https://docs.whop.com/api-reference/payment-rules/deactivate-a-payment-rule.md) a rule to pause it and [activate](https://docs.whop.com/api-reference/payment-rules/activate-a-payment-rule.md) it to resume. [Delete](https://docs.whop.com/api-reference/payment-rules/delete-a-payment-rule.md) it to retire it. It stays readable with `status: "deleted"`.

## Measure the effect

Each payment lists the rules that matched it in `payment_rule_matches`, with the name the rule had at the time.

```json theme={null}
{
	"id": "pay_xxxxxxxxxxxxx",
	"substatus": "blocked",
	"risk_score": 91,
	"payment_rule_matches": [
		{
			"id": "prule_xxxxxxxxxxxxx",
			"name": "Block high-risk cards from outside the US",
			"action": "block"
		}
	]
}
```

Disputes arriving on payments with no matches mean a rule is too narrow. A rule matching far more payments than you were losing is too broad. For totals over time, the [Stats API](https://docs.whop.com/developer/guides/stats.md) metrics `payment_rule_matches` and `payment_rule_matched_volume` break down by `payment_rule_ids` or `action`.

## Next steps

> [card] [Payment Rules reference](https://docs.whop.com/api-reference/payment-rules/list-payment-rules.md)
>
> Every endpoint, parameter, and response field.

> [card] [Refunds and disputes](https://docs.whop.com/developer/guides/refunds-and-disputes.md)
>
> Respond to the chargebacks that still get through.


## Related topics

- [Payments Overview](https://docs.whop.com/developer/payments/overview.md)
- [Invoices](https://docs.whop.com/developer/guides/invoices.md)
- [Payment Rules API](https://docs.whop.com/api-reference/payment-rules/payment-rule.md)
