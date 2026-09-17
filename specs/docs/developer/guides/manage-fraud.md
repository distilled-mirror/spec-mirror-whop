> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Manage Fraud

> Cut fraud and chargebacks with payment rules that block risky checkouts, send card payments to review, challenge them with 3D Secure, or let trusted buyers through.

A fraudulent payment costs you the money, a [\$15 dispute fee](/manage-your-business/manage-payments/manage-disputes), and a worse [dispute rate](/trust-and-safety/account-health/managing-dispute-rates). Whop screens checkouts with its own fraud controls. Payment rules add your own layer on top: conditions you write, and an action Whop takes when a checkout matches them all.

Rules run after Whop's controls and never loosen them. A payment Whop blocks stays blocked.

## Four ways a rule can act

| Action        | What happens                                                                                                                                                                                                                        | When several rules match         |
| ------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------- |
| `block`       | The payment is declined before authorization. The buyer sees a generic decline.                                                                                                                                                     | Beats `review` and `enforce_3ds` |
| `review`      | An eligible card is authorized without capture. Automatic capture is scheduled for 48 hours after authorization. You can capture or void it sooner. Unsupported methods skip review and continue through normal payment processing. | Beats `enforce_3ds`              |
| `enforce_3ds` | The buyer confirms the payment with their bank through 3D Secure.                                                                                                                                                                   | Lowest precedence                |
| `allow`       | The payment skips your other rules. It never skips Whop's controls.                                                                                                                                                                 | Beats all three                  |

A rule can read `risk_score`, `amount_in_usd`, `card_country`, `customer_email`, and `ip_address`. [List fields](/api-reference/beta/payment-rules/list-fields) returns the operators and values each one accepts.

Where you're unsure, challenge rather than block. A block also turns away real buyers who match, while a challenge lets them prove they're the cardholder.

<Note>
  Run these examples in a trusted server environment with `WHOP_API_KEY` set.
  Reuse the imports and client setup from the first example for your language.
</Note>

## Block the obvious fraud

Start with the pattern you see most in your [disputes](/developer/guides/refunds-and-disputes). This rule blocks a payment Whop scores 85 or higher on a card issued outside the US.

<CodeGroup>
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

      whopsdk "github.com/whopio/whopsdk-go"
      "github.com/whopio/whopsdk-go/client"
      "github.com/whopio/whopsdk-go/option"
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
</CodeGroup>

Always pass `account_id`. Without it the rule lands on whichever account your credential defaults to, which is easy to get wrong for a platform that manages several. Rules are published to checkout every minute, so a new or changed rule starts applying within a couple of minutes.

<Note>
  Required permission: `payment:manage` to write rules, `payment:basic:read` to
  read them. Add permissions from the [Permissions
  guide](/developer/guides/permissions).
</Note>

## Challenge large orders

A large order from a stolen card is the chargeback you least want. A large order from a real customer is the sale you least want to lose. A 3D Secure challenge separates the two, and the payment records `three_ds_verified: true` when the buyer passes.

`enforce_3ds` is skipped, but still recorded on the payment, when the checkout can't carry a challenge. That covers off-session payments such as renewals, plans that set their own 3D Secure level, non-card payments, cards the processor can't challenge, and American Express.

<CodeGroup>
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
</CodeGroup>

## Review a payment before charging it

A `review` rule requests authorization for the full amount of an eligible card payment, without capturing it. Whop grants membership access after successful authorization. Checkout Elements supports completion while the payment is held. Legacy embeds have a completion limitation described below.

Eligible means an on-session card payment through Whop Payments, including saved cards. Apple Pay, Google Pay, bank payments, balance payments, and other unsupported methods skip review. They continue through normal payment processing. Other applicable rules and fraud controls still apply. Matching skipped rules are recorded on the payment.

Automatic capture is scheduled for 48 hours after authorization. Before then, you can:

* [Capture](/api-reference/beta/payments/capture-payment) the payment to collect the authorized amount. Once capture succeeds, the payment becomes `paid` and Whop sends `payment.succeeded`.
* [Void](/api-reference/beta/payments/void-payment) the payment to release the authorization before capture. A successful void revokes membership access, schedules reserved stock to be returned, and sends `payment.canceled`.
* Leave it held. Whop attempts automatic capture once `auto_capture_at` is due.

Capture can remain in progress or fail. Confirm collection through the payment status or `payment.succeeded` before fulfilling an order by hand.

Whop sends `payment.authorized` after successful authorization, which you can use to start a review workflow. This event also covers API-requested authorizations, so it doesn't identify review-rule holds on its own. While held, the payment has `status: "authorized"` and `substatus: "requires_capture"`. [Retrieve its status](/api-reference/beta/payments/retrieve-payment-status) to read `auto_capture_at`.

This rule matches eligible card payments of at least \$250 whose card was issued outside the US. Higher-priority account rules and Whop's fraud controls still apply.

<CodeGroup>
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
</CodeGroup>

Once you have looked at the order, choose either capture or void by payment ID. To collect the authorized funds:

<CodeGroup>
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
</CodeGroup>

To release the authorization instead of collecting it:

<CodeGroup>
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
</CodeGroup>

### Where a review rule helps

* **Orders you fulfil by hand.** Review physical goods or bookings before delivery. Confirm capture succeeded before you fulfil the order.
* **A pattern that's risky but not certainly fraud.** Review delays capture while you assess the order, with membership access granted during the hold.
* **A new market or campaign.** Apply a review rule while you assess payment patterns, then adjust or deactivate it as you learn.

### Review limitations

* **Only eligible cards can be held.** Apple Pay and Google Pay, including saved wallet cards, and every other unsupported method skip review. A matching rule still appears in `payment_rule_matches`, and another matching rule can apply.
* **Off-session payments skip review.** This includes automatic subscription renewals. An on-session renewal recovery can still match a review rule.
* **Payments already created with `capture: false` keep their capture settings.** A review rule doesn't replace the API caller's capture schedule.
* **One account-rule action wins.** `review` takes precedence over an account's `enforce_3ds` rule, while `allow` and `block` take precedence over `review`. Other 3DS requirements still apply.
* **The automatic capture delay is fixed at 48 hours.** This schedules a capture attempt. It doesn't guarantee completion within 48 hours. For another schedule, create each eligible card payment through [`POST /payments`](/api-reference/beta/payments/create-payment) with `capture: false` and `auto_capture_after_minutes`. The delay must be between 5 and 5,760 minutes. Other fraud controls still apply.
* **Capture is all or nothing.** Capture collects the full authorized amount. Use the refund API after capture to return part of it.
* **Tell the buyer if you void their order.** A successful void releases the authorization before capture and revokes membership access.
* **Legacy embedded checkout has a completion limitation.** Its completion flow can remain on **Processing** until capture, even though authorization can already grant membership access. Capture may complete after the scheduled time or fail. Rules are account-wide and still apply to legacy embeds. Migrate those embeds if buyers must see checkout complete while the payment is held.
* **Membership access starts at authorization.** A review hold grants access before you decide whether to capture. Voiding revokes that access, so consider whether buyers could consume the product before your decision.

## Keep trusted buyers moving

An `allow` rule exempts buyers you trust from your block, review, and challenge rules.

<CodeGroup>
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
</CodeGroup>

## Tighten a rule

What a rule does is fixed once created, so the payments it decided keep naming the rule that decided them. To change its action or conditions, [replace](/api-reference/beta/payment-rules/replace-a-payment-rule) it: the old rule is deleted and a successor with a new ID inherits its name, `metadata`, and active state. Here the block threshold is raised from 85 to 90 after catching too many real buyers.

<CodeGroup>
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
</CodeGroup>

[Deactivate](/api-reference/beta/payment-rules/deactivate-a-payment-rule) a rule to pause it and [activate](/api-reference/beta/payment-rules/activate-a-payment-rule) it to resume. [Delete](/api-reference/beta/payment-rules/delete-a-payment-rule) it to retire it. It stays readable with `status: "deleted"`.

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

Disputes arriving on payments with no matches mean a rule is too narrow. A rule matching far more payments than you were losing is too broad. For totals over time, the [Stats API](/developer/guides/stats) metrics `payment_rule_matches` and `payment_rule_matched_volume` break down by `payment_rule_ids` or `action`.

## Next steps

<CardGroup cols={2}>
  <Card title="Payment Rules reference" icon="list-check" href="/api-reference/beta/payment-rules/list-payment-rules">
    Every endpoint, parameter, and response field.
  </Card>

  <Card title="Refunds and disputes" icon="shield" href="/developer/guides/refunds-and-disputes">
    Respond to the chargebacks that still get through.
  </Card>
</CardGroup>
