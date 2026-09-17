> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Payment Rules

A Payment Rule lets an account act on its own payments before they reach the bank: block them, let them through, send them to review, or ask the buyer for 3D Secure. Each rule matches on a small set of payment attributes, and every condition must hold for it to apply.

A rule's definition is fixed once created, so the payments it decided keep naming the rule that decided them. Use [Replace](/api-reference/beta/payment-rules/replace-a-payment-rule) to change one, and [List fields](/api-reference/beta/payment-rules/list-fields) for the attributes, operators and values a condition can use.

For a walkthrough of blocking, reviewing, challenging, and allowing checkouts to cut fraud, read the [Manage Fraud](/developer/guides/manage-fraud) guide.

<Warning>
  A `review` rule can leave legacy embeds on **Processing**
  until capture. Automatic capture is scheduled for 48 hours after
  authorization, but completion can take longer or fail. Review rules are
  account-wide and still apply to legacy embeds. See [review hold eligibility
  and checkout
  limitations](/developer/guides/manage-fraud#review-a-payment-before-charging-it)
  before enabling them.
</Warning>

## Endpoints

| Endpoint                                                                                 | Request                                                                             |
| ---------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| [List payment rules](/api-reference/beta/payment-rules/list-payment-rules)               | <Badge color="blue" size="sm" stroke>GET</Badge> `/payment_rules`                   |
| [Create a payment rule](/api-reference/beta/payment-rules/create-a-payment-rule)         | <Badge color="green" size="sm" stroke>POST</Badge> `/payment_rules`                 |
| [Retrieve a payment rule](/api-reference/beta/payment-rules/retrieve-a-payment-rule)     | <Badge color="blue" size="sm" stroke>GET</Badge> `/payment_rules/{id}`              |
| [Update a payment rule](/api-reference/beta/payment-rules/update-a-payment-rule)         | <Badge color="orange" size="sm" stroke>PATCH</Badge> `/payment_rules/{id}`          |
| [Delete a payment rule](/api-reference/beta/payment-rules/delete-a-payment-rule)         | <Badge color="red" size="sm" stroke>DELETE</Badge> `/payment_rules/{id}`            |
| [Activate a payment rule](/api-reference/beta/payment-rules/activate-a-payment-rule)     | <Badge color="green" size="sm" stroke>POST</Badge> `/payment_rules/{id}/activate`   |
| [Deactivate a payment rule](/api-reference/beta/payment-rules/deactivate-a-payment-rule) | <Badge color="green" size="sm" stroke>POST</Badge> `/payment_rules/{id}/deactivate` |
| [Replace a payment rule](/api-reference/beta/payment-rules/replace-a-payment-rule)       | <Badge color="green" size="sm" stroke>POST</Badge> `/payment_rules/{id}/replace`    |
| [List fields](/api-reference/beta/payment-rules/list-fields)                             | <Badge color="blue" size="sm" stroke>GET</Badge> `/payment_rules/fields`            |

## Attributes

<Columns cols={2}>
  <Column>
    <ResponseField name="id" type="string" required>
      Payment rule ID, prefixed `prule_`.
    </ResponseField>

    <ResponseField name="account_id" type="string" required>
      Account ID, prefixed `biz_`.
    </ResponseField>

    <ResponseField name="action" type="string" required>
      What this account's rule requests when every condition matches. One applicable account-rule action wins, in this order: `allow`, `block`, `review`, `enforce_3ds`. An `allow` overrides this account's other rules, never Whop's own fraud controls. A `review` requests authorization without capture for an eligible on-session card payment through Whop Payments. Automatic capture is scheduled for 48 hours after authorization; capture or void the payment before then to decide sooner. Capture may complete later or fail. Review is skipped for unsupported methods, off-session payments, and payments already configured for manual capture. An `enforce_3ds` is skipped when the account rule cannot apply a challenge. Other 3DS requirements still apply.

      Available options: `allow`, `block`, `review`, `enforce_3ds`
    </ResponseField>

    <ResponseField name="conditions" type="object" required>
      The conditions a payment is matched against. Up to 10 conditions, and 8 KiB once serialized.

      <Accordion title="Properties" defaultOpen={true}>
        <ResponseField name="all" type="object[]" required>
          Conditions that must all match for the rule to apply. A payment attribute the rule cannot read does not match.

          <Accordion title="Properties" defaultOpen={true}>
            <ResponseField name="field" type="string" required>
              The payment attribute this condition reads.

              Available options: `risk_score`, `amount_in_usd`, `card_country`, `customer_email`, `ip_address`
            </ResponseField>

            <ResponseField name="operator" type="string" required>
              How the payment attribute is compared to the value.

              Available options: `eq`, `neq`, `gt`, `gte`, `lt`, `lte`, `in`, `not_in`, `contains`, `starts_with`, `ends_with`, `in_cidr`
            </ResponseField>

            <ResponseField name="value" type="integer or string or string[]" required>
              The value to compare against.
            </ResponseField>
          </Accordion>
        </ResponseField>
      </Accordion>
    </ResponseField>

    <ResponseField name="created_at" type="string" required>
      When the rule was created, as an ISO 8601 timestamp.
    </ResponseField>

    <ResponseField name="deleted_at" type="string | null" required>
      When the rule was deleted, as an ISO 8601 timestamp. `null` unless `status` is
      `deleted`.
    </ResponseField>

    <ResponseField name="metadata" type="object" required>
      Custom string-to-string values for your integration. Maximum 50 keys, 40
      characters per key, 500 characters per value.
    </ResponseField>

    <ResponseField name="name" type="string" required>
      A name for this rule. Up to 255 characters.
    </ResponseField>

    <ResponseField name="status" type="string" required>
      Whether the rule is applied to payments. A `deleted` rule is kept so the payments it already decided still name it.

      Available options: `active`, `inactive`, `deleted`
    </ResponseField>

    <ResponseField name="updated_at" type="string" required>
      When the rule was last changed, as an ISO 8601 timestamp.
    </ResponseField>
  </Column>

  <Column>
    <div className="api-resource-sticky-example">
      ```json PaymentRule theme={null}
      {
      	"id": "prule_xxxxxxxxxxxxx",
      	"account_id": "biz_xxxxxxxxxxxxxx",
      	"name": "Block high-risk cards from outside the US",
      	"status": "active",
      	"action": "block",
      	"conditions": {
      		"all": [
      			{
      				"field": "risk_score",
      				"operator": "gte",
      				"value": 85
      			},
      			{
      				"field": "card_country",
      				"operator": "neq",
      				"value": "US"
      			}
      		]
      	},
      	"metadata": {},
      	"created_at": "2026-09-01T12:00:00.000Z",
      	"updated_at": "2026-09-01T12:00:00.000Z",
      	"deleted_at": null
      }
      ```
    </div>
  </Column>
</Columns>
