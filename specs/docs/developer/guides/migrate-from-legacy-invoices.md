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

# Migrate from Legacy Invoices

> Move from the legacy invoice API to native invoices: switch the API version, rebuild requests around items, and map fields, statuses, and webhooks.

Native invoices use the same `/invoices` paths as legacy invoices, but have a different request format and lifecycle. This guide maps the fields and actions you need to change when migrating an integration.

> [!NOTE]
>
> The shared paths route to native invoices from `Api-Version-Date: 2026-10-24`.
> The native examples below use `Api-Version-Date: 2026-11-02-1`, with the
> current variant field names and customer-locking rules. Send that header on
> each native request. Keep legacy calls on an earlier released version, such as
> `2026-10-15-1`. [API versions](https://docs.whop.com/developer/api/versioning.md) explains how headers
> override credential pins.

## What changes

| | Legacy invoices | Native invoices |
| - | - | - |
| **ID** | `inv_` | `pay_`, the same as the invoice's payment |
| **What you bill** | One plan, with optional labels that add up to its price | 1 to 50 items, each an existing variant or one found or created inline |
| **Create** | Finalizes in one call, unless `save_as_draft` or `automatically_finalizes_at` is set | Always saves a draft. You then issue or send it. |
| **Amounts** | The plan's price | `subtotal`, `buyer_fee_amount`, `tax_amount`, `total`, `amount_due` |
| **Payment page** | `pay_online_url`, a URL | `checkout_link`, a signed token used to open checkout |
| **Past due** | `status: past_due` | `overdue: true` on an `open` or `uncollectible` invoice |
| **Webhooks** | `invoice.*` | Payment events for the invoice's `pay_` ID |

The native API doesn't list or retrieve legacy `inv_` invoices. Keep reading existing legacy invoices with the earlier API version. Creating a native invoice creates a separate bill. It doesn't convert or settle a legacy invoice.

## Before you migrate

Check these differences before moving a flow. Keep it on the legacy API if it needs a feature the native API doesn't provide.

| Legacy feature | Native behavior |
| - | - |
| `invoice.*` webhooks | Native invoices use payment events. Some legacy events have no direct counterpart. See [Map webhooks](#map-webhooks). |
| Mark an invoice paid after collecting money outside Whop | There is no native `mark_paid` endpoint. |
| Delete a draft | There is no native `DELETE /invoices/{id}`. Voiding keeps the invoice record and number. |
| `plan.payment_method_configuration` | The invoice request doesn't accept this field. Configure the existing variant or account's payment methods instead. |
| Free trials and initial fees on recurring plans | From `2026-11-11`, the invoice adds the initial fee. Earlier versions bill the renewal price, so add the fee as a one-time item. The variant's trial applies. |
| `subscription_billing_anchor_at` | Send `subscription_starts_at` on the invoice. A time that has passed when the invoice is paid starts the period at the payment. As before, it skips the free trial. |
| Arbitrary `line_items` labels, fractional quantities, or negative credits | Native lines bill variants. Quantities must be positive integers, and labels come from product or variant titles. Rebuild the charges rather than copying the breakdown. |
| Reusing the legacy invoice's `current_plan.id` | Native invoices reject variants tied to legacy invoices. Use a separate variant or create one inline. |

## Map the create request

A legacy create sends one plan and, optionally, labels that split its price. A native create sends an item for each billed variant.

These examples set a combined item price of 19.99 USD. The existing native variant represents the 15 USD setup charge. The 4.99 USD service fee is a custom one-time charge without a product. Both APIs accept prices in currency units. Use `4.99` for 4.99 USD. Sending `499` bills 499 USD.

```json Legacy theme={null}
{
	"account_id": "biz_xxxxxxxxxxxxxx",
	"product_id": "prod_xxxxxxxxxxxxx",
	"member_id": "mber_xxxxxxxxxxxxx",
	"collection_method": "send_invoice",
	"due_date": "2026-12-01T00:00:00Z",
	"plan": {
		"plan_type": "one_time",
		"initial_price": 19.99,
		"currency": "usd"
	},
	"line_items": [
		{ "label": "Setup", "quantity": 1, "unit_price": 15 },
		{ "label": "Service fee", "quantity": 1, "unit_price": 4.99 }
	]
}
```

```json Native theme={null}
{
	"account_id": "biz_xxxxxxxxxxxxxx",
	"member_id": "mber_xxxxxxxxxxxxx",
	"due_at": "2026-12-01T00:00:00Z",
	"items": [
		{ "variant_id": "plan_xxxxxxxxxxxxx", "quantity": 1 },
		{
			"variant": {
				"title": "Service fee",
				"billing_type": "one_time",
				"initial_price": 4.99,
				"currency": "usd"
			}
		}
	]
}
```

The native request creates a draft. Send it with `POST /invoices/{id}/send` to issue and email it. `member_id` identifies a customer (`mber_`), not a subscription (`mem_`).

| Legacy field | Native field | Notes |
| - | - | - |
| `account_id` | `account_id` | The account issuing the invoice. |
| `plan` | `items` | Each item takes `variant_id` or inline `variant`, plus an optional `quantity`. All items use the same currency. |
| `product`, `product_id` | `items[].variant.product`, `items[].variant.product_id` | Use one of the two. Inline `product` requires `external_identifier` and `title`. A recurring variant needs a product. A one-time custom charge can omit it. |
| `line_items` | `items` | Rebuild billed lines as variants. `quantity` defaults to 1. Values above 1 require a variant that allows multiple quantities. |
| `collection_method` | `payment_method_id`, then issue or send | Set a saved method for an automatic charge, or omit it for payment through checkout. See [Issue, send, and charge](#issue-send-and-charge). |
| `save_as_draft` | None | Create always saves a draft. |
| `automatically_finalizes_at` | `send_at` on Send Invoice | Delays issuance until the scheduled send. `charge_at` delays a saved-method charge, not issuance. |
| `due_date` | `due_at` | Send an ISO 8601 timestamp. |
| `charge_buyer_fee` | `buyer_fee_percentage` | Map `false` to `0`, and the legacy `true` fee to `5`. Other nonzero percentages must be between 5 and 100, with at most two decimal places. |
| `payment_method_id`, `payment_token_id` | `payment_method_id` | One of the customer's saved payment methods, prefixed `payt_`. Setting it requires `payment:charge`. |
| `member_id`, `email_address`, `customer_name` | Same on create | Choose `member_id` or `email_address`, not both. Native invoices also take `recipient_emails`, independently of the billed customer. |
| `mailing_address_id`, `billing_address` | Same names, narrower address input | Choose one. An inline address needs `line1`, `city`, `postal_code`, and `country`. It also accepts `name`, `line2`, `state`, and `phone`. It doesn't accept the legacy `tax_id_type` or `tax_id_value` fields. |
| `subscription_billing_anchor_at` | `subscription_starts_at` | When the first billing period starts. Must be in the future and needs a recurring item. |
| None | `memo`, `metadata` | `memo` shows on the email, PDF, and checkout. `metadata` stores string key-value pairs. |

For native versions before `2026-10-27-2`, use `plan_id`, `plan`, and `plan_type` instead of `variant_id`, `variant`, and `billing_type`. The response's line items use `plan_id` on those older versions too.

### Keep issued amounts locked

`PATCH /invoices/{id}` edits a draft's items and billing details. Sending `items` replaces the entire list using the variants' current prices. Omitting it keeps the saved items and prices.

* Cancel a scheduled send before editing items or billing details.
* From `2026-11-02-1`, you can't replace or clear a draft's assigned customer. A draft without a customer can still take one. Void the draft before creating a new invoice for someone else.
* `email_address` and `customer_name` are create-only inputs.
* Issued amounts and items are locked. An update containing only `memo` and/or `recipient_emails` is allowed on `draft`, `open`, and `uncollectible` invoices, including a scheduled draft.

## Issue, send, and charge

A legacy create chooses collection behavior with `collection_method`. Native invoices use separate calls after creating the draft:

1. **Issue it** with `POST /invoices/{id}/issue` to calculate final tax and open it for payment without sending the initial invoice email.
2. **Or send it** with `POST /invoices/{id}/send`. This issues the draft. It emails the recipients unless it charges a saved method immediately. Sending an `open` invoice emails it again. `send_at` schedules a draft's send. `POST /invoices/{id}/unschedule` cancels that schedule.
3. **Charge a saved method** by setting `payment_method_id` on the draft. For a positive total, issue or send charges at a future `charge_at`, otherwise a future `due_at`, otherwise immediately. `charge_at` must be in the future and can't precede `send_at`.

Sending a draft that charges immediately skips the initial invoice email. Sending with a later charge emails it now. A failed automatic charge can trigger a payment-link email even when you used Issue Invoice. [Charge a saved payment method](https://docs.whop.com/developer/guides/invoices.md#charge-a-saved-payment-method) explains the flow.

An `open` response doesn't confirm payment. Wait for `payment.succeeded` before fulfilling. To charge or retry an open invoice, use [Charge Invoice](https://docs.whop.com/api-reference/invoices/charge-invoice.md). Issuing or sending it again doesn't start another charge.

For a recurring variant, `collection_method` on Issue Invoice or Send Invoice controls **renewals**. `charge_automatically` collects future payments automatically. `send_invoice` creates and emails a new invoice each period. It doesn't replace `payment_method_id` for collecting the first invoice. See [Invoice a subscription](https://docs.whop.com/developer/guides/invoices.md#invoice-a-subscription).

## Map the response

| Legacy field | Native field | Notes |
| - | - | - |
| `id` (`inv_`) | `id` (`pay_`) | `object` is `invoice`. The same ID identifies the payment. |
| `current_plan` price | `subtotal`, `buyer_fee_amount`, `tax_amount`, `total`, `amount_due` | Money objects contain a decimal-string `amount`, `currency`, `decimals`, and `display_decimals`. `totals_status` is `estimated` on a draft and `final` otherwise. |
| `company.id` | `account_id` | Retrieve the account by ID. |
| `product.id`, `current_plan.id` | `line_items[].product_id`, `line_items[].variant_id` | Each billed line identifies its product and variant. A custom charge can have no product. |
| `member.id` | `member_id` | Customer ID, prefixed `mber_`. Requires `member:basic:read`. |
| `user` | No top-level user field | Retrieve the member to find its user. `membership_id` (`mem_`) identifies a membership, not the customer. |
| `mailing_address` | `billing_address`, `mailing_address_id` | The inline address remains an object. |
| `payment.id` | The invoice's own `id` | Retrieve payment details with `GET /payments/{id}` using the same `pay_` ID. |
| `number` | `number`, `invoice_prefix` | The formatted number and account prefix. |
| `due_date` | `due_at` | ISO 8601 timestamp or `null`. |
| `automatically_finalizes_at` | `scheduled_send_at` | `scheduled_charge_at` shows a planned saved-method charge. `scheduled_collection_method` shows the renewal collection choice for a scheduled send. |
| `charge_buyer_fee` | `buyer_fee_percentage`, `buyer_fee_amount` | The percentage and its calculated amount. |
| `customer_name`, `email_address` | Same | Plus `recipient_emails`. These fields are permission-gated. |
| `pay_online_url` | `checkout_link` | A signed token, not a URL. See [Open the invoice checkout](#open-the-invoice-checkout). |
| `line_items` | `line_items` | Lines have `variant_id`, `product_id`, `label`, `description`, `quantity`, `unit_amount`, `subtotal`, `discount_amount`, and `tax_amount`. |
| `status` | `status`, `overdue` | See [Map statuses](#map-statuses). |
| `payment_processing` | `status`, plus the payment details | Handle `pending` while payment settles. `paid_at` records payment completion. It isn't a processing flag. |
| `fetch_invoice_token` | No direct equivalent | Use authenticated invoice reads, `checkout_link` for checkout, and Generate Invoice PDF for the PDF. |
| None | `paid_at`, `memo`, `metadata` | Payment completion time, the customer note, and your metadata. |

To download a PDF, call `POST /invoices/{id}/generate_pdf` for a short-lived link.

### Open the invoice checkout

`checkout_link` contains the invoice ID and a signed token separated by a `.`. Pass the entire value as `link` to `POST /checkout_sessions` to open a session for this invoice. For a customer-facing URL, follow [Build the hosted checkout URL](https://docs.whop.com/developer/guides/invoices.md#get-the-checkout-link).

The field is present only while checkout is available: the invoice must be `open`. Business buyers and canceled memberships can't use this checkout. Your key needs `payment:charge`, or `invoice:create`/`invoice:update` when the invoice has no saved payment method. It's `null` on a draft, paid, void, or uncollectible invoice, and for business buyers.

## Map statuses

| Legacy `status` | Native |
| - | - |
| `draft` | `draft` |
| `open` | `open` |
| `past_due` | `open` or `uncollectible`, with `overdue: true` |
| `paid` | `paid` |
| `uncollectible` | `uncollectible` |
| `void` | `void` |

Native invoices can also return `authorized`, `pending`, or `unresolved`. Don't treat every status outside the legacy enum as paid or failed. [Statuses and lifecycle](https://docs.whop.com/developer/guides/invoices.md#statuses-and-lifecycle) explains them.

## Map endpoints

| Legacy | Native |
| - | - |
| `GET /invoices` | [List Invoices](https://docs.whop.com/api-reference/invoices/list-invoices.md), with an `overdue` filter |
| `GET /invoices/{id}` | [Retrieve Invoice](https://docs.whop.com/api-reference/invoices/retrieve-invoice.md) |
| `POST /invoices` | [Create Invoice](https://docs.whop.com/api-reference/invoices/create-invoice.md), which saves a draft |
| `PATCH /invoices/{id}` | [Update Invoice](https://docs.whop.com/api-reference/invoices/update-invoice.md), subject to the draft and issued-invoice rules above |
| `POST /invoices/{id}/resend` | [Send Invoice](https://docs.whop.com/api-reference/invoices/send-invoice.md) |
| `POST /invoices/{id}/void` | [Void Invoice](https://docs.whop.com/api-reference/invoices/void-invoice.md) |
| `POST /invoices/{id}/mark_uncollectible` | [Mark an invoice uncollectible](https://docs.whop.com/api-reference/invoices/mark-invoice-uncollectible.md) |
| `DELETE /invoices/{id}` | No equivalent. Void the draft to cancel it while keeping its record. |
| `POST /invoices/{id}/mark_paid` | No equivalent |
| None | [Issue Invoice](https://docs.whop.com/api-reference/invoices/issue-invoice.md), [Cancel Scheduled Send](https://docs.whop.com/api-reference/invoices/cancel-scheduled-send.md), [Charge Invoice](https://docs.whop.com/api-reference/invoices/charge-invoice.md), [List Invoice Sends](https://docs.whop.com/api-reference/invoices/list-invoice-sends.md), [Generate Invoice PDF](https://docs.whop.com/api-reference/invoices/generate-invoice-pdf.md) |

## Map webhooks

Native invoices send payment events for their `pay_` ID. The `invoice.*` events describe legacy invoices.

| Legacy event | Native behavior |
| - | - |
| `invoice.created` | `payment.created` when a draft is issued, not when it's created |
| `invoice.paid` | `payment.succeeded` |
| `invoice.past_due` | Read or filter on `overdue`. There is no matching past-due payment event. |
| `invoice.voided`, `invoice.marked_uncollectible` | Read the invoice's `status`. There is no direct payment-event replacement for these transitions. |

Payment events also cover payments unrelated to invoices. Match the event's payment ID to the native invoice IDs you store. [Webhooks](https://docs.whop.com/developer/guides/invoices.md#webhooks) shows how to fulfill from `payment.succeeded`.

## Permissions

| Operation | Required permission |
| - | - |
| Create a draft without a saved payment method | `invoice:create` or `payment:charge` |
| Update a draft without a saved payment method or scheduled send | `invoice:update`, `invoice:create`, or `payment:charge` |
| Send or schedule a draft without a saved payment method, or cancel its scheduled send | `invoice:update`, `invoice:create`, or `payment:charge` |
| Set a saved payment method, write an invoice that has one, issue, charge, void, or mark uncollectible | `payment:charge` |
| Update a scheduled draft, or update or resend an issued invoice | `payment:charge` |
| List, retrieve, generate a PDF, or list email sends | `payment:basic:read` |
| Supply an inline `variant` | Also `plan:create` |
| Create or update an inline `product` | Also `access_pass:create` or `access_pass:update`, respectively |

`member:basic:read` exposes customer names and member/membership IDs. `member:email:read` exposes `email_address` and `recipient_emails`. Payment method and address fields require `payment:basic:read`.

## Migration checklist

1. Check [Before you migrate](#before-you-migrate) for workflows that need to stay on the legacy API.
2. Pin native calls to a released version matching their field names, such as `2026-11-02-1` for these examples.
3. Build `items` from eligible variants, checking quantities, prices, currency, and product requirements.
4. Replace the legacy one-call create with create, then issue or send. Choose saved-method charges for the first invoice and a collection method for renewals.
5. Map due dates, scheduled sends, and buyer fees according to their behavior, not just their field names.
6. Keep native `pay_` and legacy `inv_` IDs distinct. Keep legacy reads on the earlier version.
7. Read amounts from money objects, and open checkout using the signed `checkout_link`.
8. Handle `overdue` and the additional payment statuses.
9. Fulfill from `payment.succeeded` for the invoice's payment ID.
10. Grant the permissions your flow uses, including inline variant/product and customer-read permissions.

## Next steps

> [card] [Invoices guide](https://docs.whop.com/developer/guides/invoices.md)
>
> Create, issue, send, and collect native invoices.

> [card] [Invoices API reference](https://docs.whop.com/api-reference/invoices/create-invoice.md)
>
> Every native invoice endpoint and field.

> [card] [Legacy invoice API](https://docs.whop.com/api-reference/legacy/invoices/legacy-invoice.md)
>
> The endpoints your current integration calls.

> [card] [API versions](https://docs.whop.com/developer/api/versioning.md)
>
> How `Api-Version-Date` and version pins work.


## Related topics

- [Legacy Invoice](https://docs.whop.com/api-reference/legacy/invoices/legacy-invoice.md)
- [Invoices](https://docs.whop.com/developer/guides/invoices.md)
- [Delete invoice](https://docs.whop.com/api-reference/legacy/invoices/delete-invoice.md)
