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

# Payment Rules API

> A Payment Rule lets an account block, allow, review, or challenge its own payments, including payments that fail a CVV or billing address check.

A challenge asks the buyer for 3D Secure. Most rules run before the payment reaches the bank. A rule on the bank's CVV check or address verification (AVS) check runs after the bank authorizes the card. Each rule matches on a small set of payment attributes, and every condition must hold for it to apply.

A rule's definition is fixed once created, so the payments it decided keep naming the rule that decided them. Use [Replace](https://docs.whop.com/api-reference/payment-rules/replace-a-payment-rule.md) to change one, and [List fields](https://docs.whop.com/api-reference/payment-rules/list-fields.md) for the attributes, operators and values a condition can use.

For a walkthrough of blocking, reviewing, challenging, and allowing checkouts to cut fraud, read the [Manage Fraud](https://docs.whop.com/developer/guides/manage-fraud.md) guide.

Automatic renewals skip your account's rules by default. Other off-session charges use only `block` and `allow` rules. Set `off_session_payment_rules` to `true` with [Update account preferences](https://docs.whop.com/api-reference/accounts/update-account-preferences.md) to apply all active rules to future off-session attempts. Set it to `false` to opt out.

> [!WARNING]
>
> A `review` rule can leave legacy embeds on **Processing**
> until capture. Automatic capture is scheduled for 48 hours after
> authorization, but completion can take longer or fail. Review rules are
> account-wide and still apply to legacy embeds. See [review hold eligibility
> and checkout
> limitations](https://docs.whop.com/developer/guides/manage-fraud.md#review-a-payment-before-charging-it)
> before enabling them.

## Endpoints

| Endpoint | Request |
| - | - |
| [List payment rules](https://docs.whop.com/api-reference/payment-rules/list-payment-rules.md) | GET `/payment_rules` |
| [Retrieve a payment rule](https://docs.whop.com/api-reference/payment-rules/retrieve-a-payment-rule.md) | GET `/payment_rules/{id}` |
| [List fields](https://docs.whop.com/api-reference/payment-rules/list-fields.md) | GET `/payment_rules/fields` |
| [Create a payment rule](https://docs.whop.com/api-reference/payment-rules/create-a-payment-rule.md) | POST `/payment_rules` |
| [Activate a payment rule](https://docs.whop.com/api-reference/payment-rules/activate-a-payment-rule.md) | POST `/payment_rules/{id}/activate` |
| [Deactivate a payment rule](https://docs.whop.com/api-reference/payment-rules/deactivate-a-payment-rule.md) | POST `/payment_rules/{id}/deactivate` |
| [Replace a payment rule](https://docs.whop.com/api-reference/payment-rules/replace-a-payment-rule.md) | POST `/payment_rules/{id}/replace` |
| [Update a payment rule](https://docs.whop.com/api-reference/payment-rules/update-a-payment-rule.md) | PATCH `/payment_rules/{id}` |
| [Delete a payment rule](https://docs.whop.com/api-reference/payment-rules/delete-a-payment-rule.md) | DELETE `/payment_rules/{id}` |

## Attributes

**id** `string` *required*

Payment rule ID, prefixed `prule_`.

**account\_id** `string` *required*

Account ID, prefixed `biz_`.

**action** `string` *required*

What this account's rule requests when every condition matches. One applicable account-rule action wins, in this order: `allow`, `block`, `review`, `enforce_3ds`. An `allow` overrides this account's other rules, never Whop's own fraud controls. A `review` requests authorization without capture for an eligible card payment through Whop Payments. Off-session renewals require `off_session_payment_rules` to be enabled. Automatic capture is scheduled for 48 hours after authorization. Capture or void the payment before then to decide sooner. Capture may complete later or fail. Review is skipped for unsupported methods, numbered invoice payments, and payments already configured for manual capture. An `enforce_3ds` is skipped when the account rule cannot apply a challenge. Other 3DS requirements still apply. A rule on `avs_check` or `cvv_check` accepts `block` or `review`. Whop authorizes a freshly entered card first. When the rule matches the bank's result, `block` voids and declines the payment. A `review` holds it for you to capture or void, with the same automatic capture.

Available options: `allow`, `block`, `review`, `enforce_3ds`

**conditions** `object` *required*

The conditions a payment is matched against. Up to 10 conditions, and 8 KiB once serialized.

> [accordion] Properties
>
> **all** `object[]` *required*
>
> Conditions that must all match for the rule to apply. A payment attribute the rule cannot read does not match.
>
> > [accordion] Properties
> >
> > **field** `string` *required*
> >
> > The payment attribute this condition reads.
> >
> > Available options: `risk_score`, `amount_in_usd`, `card_country`, `card_brand`, `card_funding`, `card_bin`, `billing_country`, `billing_postal_code`, `billing_city`, `customer_email`, `email_domain`, `ip_address`, `ip_country`, `avs_check`, `cvv_check`
> >
> > **operator** `string` *required*
> >
> > How the payment attribute is compared to the value.
> >
> > Available options: `eq`, `neq`, `gt`, `gte`, `lt`, `lte`, `in`, `not_in`, `starts_with`, `contains`, `ends_with`, `in_cidr`
> >
> > **value** `number or string or string[]` *required*
> >
> > The value to compare against.

**created\_at** `string` *required*

When the rule was created, as an ISO 8601 timestamp.

**deleted\_at** `string | null` *required*

When the rule was deleted, as an ISO 8601 timestamp. `null` unless `status` is
`deleted`.

**metadata** `object` *required*

Custom string-to-string values for your integration. Maximum 50 keys, 40
characters per key, 500 characters per value.

**name** `string` *required*

A name for this rule. Up to 255 characters.

**status** `string` *required*

Whether the rule is applied to payments. A `deleted` rule is kept so the payments it already decided still name it.

Available options: `active`, `inactive`, `deleted`

**updated\_at** `string` *required*

When the rule was last changed, as an ISO 8601 timestamp.

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


## Related topics

- [Delete a Payment Rule](https://docs.whop.com/api-reference/payment-rules/delete-a-payment-rule.md)
- [Activate a Payment Rule](https://docs.whop.com/api-reference/payment-rules/activate-a-payment-rule.md)
- [Deactivate a Payment Rule](https://docs.whop.com/api-reference/payment-rules/deactivate-a-payment-rule.md)
