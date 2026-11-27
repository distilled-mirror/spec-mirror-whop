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

# Invoices

> Bill a customer with a numbered invoice through the API and collect payment from a saved method or hosted checkout.

An invoice is a numbered bill for one or more of your variants, addressed to one customer. A variant is a price and billing setup you sell, usually for a product. Its ID starts with `plan_`, and invoice items take it as `plan_id`. Use an invoice when you agree a price with a customer before they pay, such as a quote, a service, or a business order. You build a draft until it's right, then issue it to lock the totals and open it for payment. Sending it does that and emails it in one call. To send one from your dashboard without code, see [Create and send invoices](https://docs.whop.com/manage-your-business/manage-payments/send-invoice.md).

> [!NOTE]
>
> Invoices on this page need `Api-Version-Date: 2026-10-24` or later on every request. Requests with an earlier version reach the legacy invoice API, which works differently. See [Before you start](#before-you-start).

## How an invoice works

A native invoice is a payment. Its ID starts with `pay_` and stays the same after the customer pays, so you can also read it with [Retrieve Payment](https://docs.whop.com/api-reference/payments/retrieve-payment.md). Each invoice also gets the next number in your account's own sequence, such as `#00000042`, or `#SHINE-00000042` when your account has an invoice prefix.

1. **You create a draft** with items, a customer, and billing details. A draft charges nothing and emails no one.
2. **You issue it, or send it.** Issuing calculates final tax and opens the invoice for payment. Sending issues it and emails it, either now or at a time you schedule.
3. **The customer pays.** Whop charges the customer's saved payment method once you issue or send the invoice, or the customer pays through Whop's hosted checkout.
4. **You fulfill the order** from the `payment.succeeded` webhook.

Invoices bill buy-now variants, the ones with `release_method: buy_now` rather than a waitlist. A one-time variant is billed once, and a recurring variant starts a subscription when the invoice is paid. See [Invoice a subscription](#invoice-a-subscription), which also covers stopping it after a set number of payments.

## Before you start

### Send the API version

`/invoices` serves two different APIs, and the `Api-Version-Date` your request resolves to decides which one answers.

| You send | `/invoices` serves |
| - | - |
| `Api-Version-Date: 2026-10-24` or later | The native invoices on this page |
| An earlier `Api-Version-Date` | The [legacy invoice API](https://docs.whop.com/api-reference/legacy/invoices/legacy-invoice.md) |
| No header | Your credential's stored version pin. With no pin, the legacy invoice API. |
| A date that isn't a released version | A `400` error |

Issue, send, cancel a scheduled send, and generate a PDF exist only in the native API, so they reach native invoices whatever version you send. The paths both APIs have, such as `GET /invoices` and `POST /invoices/{id}/void`, follow the version.

Send the header on every request rather than relying on a key's pin, so changing keys never moves you between the two. [API versions](https://docs.whop.com/developer/api/versioning.md) explains pins and the changelog.

| | Native invoices | Legacy invoices |
| - | - | - |
| **ID** | `pay_`, the same as the payment | `inv_` |
| **What you bill** | One-time and recurring variants, as `items` | One-time or recurring billing |
| **Invoice email** | Sent when you call Send Invoice, with reminders after `due_at` | Whop emails the invoice and sends reminders |
| **Webhooks** | `payment.*` | `invoice.*` |
| **Dashboard** | **Invoices** tab: listed, created, and sent | **Legacy invoices** tab |

The native API doesn't list or retrieve legacy `inv_` invoices, and the legacy API doesn't return native ones.

### Call the API

The examples on this page call the API through a small helper that sends the version header on every request.

```typescript TypeScript theme={null}
const WHOP_API = "https://api.whop.com/api/v1";

export async function whop(method: string, path: string, body?: unknown, idempotencyKey?: string) {
	const response = await fetch(`${WHOP_API}${path}`, {
		method,
		headers: {
			Authorization: `Bearer ${process.env.WHOP_API_KEY}`,
			"Api-Version-Date": "2026-10-24",
			"Content-Type": "application/json",
			...(idempotencyKey ? { "Idempotency-Key": idempotencyKey } : {}),
		},
		body: body === undefined ? undefined : JSON.stringify(body),
	});
	const data = await response.json();
	if (!response.ok) throw new Error(`${data.error.type}: ${data.error.message}`);
	return data;
}
```

```python Python theme={null}
import os
import uuid

import requests

WHOP_API = "https://api.whop.com/api/v1"


def whop(method, path, body=None, idempotency_key=None):
    headers = {
        "Authorization": f"Bearer {os.environ['WHOP_API_KEY']}",
        "Api-Version-Date": "2026-10-24",
    }
    if idempotency_key:
        headers["Idempotency-Key"] = idempotency_key
    response = requests.request(method, f"{WHOP_API}{path}", json=body, headers=headers)
    data = response.json()
    if not response.ok:
        raise RuntimeError(f"{data['error']['type']}: {data['error']['message']}")
    return data
```

Send an [`Idempotency-Key`](https://docs.whop.com/developer/api/idempotency) on every `POST` so a retry never creates a second invoice or repeats an action. Generate Invoice PDF is the exception: each call makes a new file, so it doesn't replay responses.

## Create a draft

[Create Invoice](https://docs.whop.com/api-reference/invoices/create-invoice.md) saves a draft with `POST /invoices`.

```typescript TypeScript theme={null}
const invoice = await whop(
	"POST",
	"/invoices",
	{
		account_id: "biz_xxxxxxxxxxxxx",
		email_address: "dana@shinetime.example",
		customer_name: "Dana Lee",
		items: [{ plan_id: "plan_xxxxxxxxxxxxx", quantity: 2 }],
		due_at: "2026-11-30T17:00:00Z",
		billing_address: {
			line1: "123 Main St",
			city: "New York",
			state: "NY",
			postal_code: "10001",
			country: "US",
		},
		metadata: { order_id: "ord_1042" },
	},
	crypto.randomUUID(),
);

console.log(invoice.id, invoice.number, invoice.status); // pay_xxxxxxxxxxxxx #00000042 draft
```

```python Python theme={null}
invoice = whop(
    "POST",
    "/invoices",
    {
        "account_id": "biz_xxxxxxxxxxxxx",
        "email_address": "dana@shinetime.example",
        "customer_name": "Dana Lee",
        "items": [{"plan_id": "plan_xxxxxxxxxxxxx", "quantity": 2}],
        "due_at": "2026-11-30T17:00:00Z",
        "billing_address": {
            "line1": "123 Main St",
            "city": "New York",
            "state": "NY",
            "postal_code": "10001",
            "country": "US",
        },
        "metadata": {"order_id": "ord_1042"},
    },
    idempotency_key=str(uuid.uuid4()),
)

print(invoice["id"], invoice["number"], invoice["status"])  # pay_xxxxxxxxxxxxx #00000042 draft
```

```bash cURL theme={null}
curl -X POST https://api.whop.com/api/v1/invoices \
  -H "Authorization: Bearer $WHOP_API_KEY" \
  -H "Api-Version-Date: 2026-10-24" \
  -H "Idempotency-Key: 6f1c2a0e-3b7d-4d55-9a8e-2c4b1f0d9e21" \
  -H "Content-Type: application/json" \
  -d '{
    "account_id": "biz_xxxxxxxxxxxxx",
    "email_address": "dana@shinetime.example",
    "customer_name": "Dana Lee",
    "items": [{ "plan_id": "plan_xxxxxxxxxxxxx", "quantity": 2 }],
    "due_at": "2026-11-30T17:00:00Z",
    "billing_address": { "line1": "123 Main St", "city": "New York", "state": "NY", "postal_code": "10001", "country": "US" },
    "metadata": { "order_id": "ord_1042" }
  }'
```

The response is `201 Created` with the draft. Creating a draft charges nothing, sends no email, and sends no webhook. This response shows a subset of the fields:

```json theme={null}
{
	"id": "pay_xxxxxxxxxxxxx",
	"object": "invoice",
	"number": "#00000042",
	"status": "draft",
	"totals_status": "estimated",
	"member_id": "mber_xxxxxxxxxxxxx",
	"recipient_emails": ["dana@shinetime.example"],
	"line_items": [
		{
			"id": "li_xxxxxxxxxxxxx",
			"plan_id": "plan_xxxxxxxxxxxxx",
			"product_id": "prod_xxxxxxxxxxxxx",
			"label": "Strategy workshop",
			"quantity": 2,
			"unit_amount": { "currency": "usd", "amount": "250.00", "decimals": 2, "display_decimals": 2 },
			"subtotal": { "currency": "usd", "amount": "500.00", "decimals": 2, "display_decimals": 2 }
		}
	],
	"subtotal": { "currency": "usd", "amount": "500.00", "decimals": 2, "display_decimals": 2 },
	"tax_amount": null,
	"total": { "currency": "usd", "amount": "500.00", "decimals": 2, "display_decimals": 2 },
	"amount_due": { "currency": "usd", "amount": "500.00", "decimals": 2, "display_decimals": 2 },
	"metadata": { "order_id": "ord_1042" }
}
```

`account_id` is the account issuing the invoice, prefixed `biz_`. An account API key can leave it out to use its own account. [Create Invoice](https://docs.whop.com/api-reference/invoices/create-invoice.md) lists every request and response field.

A draft uses its number when you create it, so a draft you discard leaves a gap in your sequence. To discard a draft, [delete it](#delete-an-invoice). To keep it in your list, [void it](#void-or-mark-uncollectible) instead.

### Add items

`items` takes 1 to 50 items. Each item takes either `plan_id` for an existing variant or `plan` to find or create one inline, plus an optional `quantity`.

An inline `plan` uses the same find-or-create input as [Create Payment](https://docs.whop.com/api-reference/payments/create-payment.md), for example `{ "plan": { "product_id": "prod_xxxxxxxxxxxxx", "plan_type": "one_time", "initial_price": 250, "currency": "usd" } }`. It needs `plan:create`, and an inline `product` also needs the product permission, `access_pass:create` or `access_pass:update`.

Create and update refuse items that break these rules:

* Each item is a buy-now variant of your account. A variant appears once, so use `quantity` for more units.
* No variant is archived, and issuing or sending a draft checks this again, so unarchive the variant or choose another one.
* `quantity` is between 1 and 999,999, and above 1 only for a variant that allows multiple quantities. Recurring variants don't, so a recurring item's quantity is 1.
* A recurring variant must belong to a product.
* Every item uses the same currency.
* In an invoice with several items, every item that has a product must be a regular product (the standard product type), not an app. Recurring items share one billing period, free trial length, and `split_pay_required_payments`, and all items need compatible payment, tax, and access settings. Variants with custom fields or a password can't be combined. Items without a product, such as a custom fee, can sit beside the rest.

Each line item in the response has a `label`, which is the product's title, or the variant's title for a variant with no product. The email, PDF, and checkout show it.

The draft keeps the price each variant had when you saved its items, so a later price change on the variant doesn't move the invoice. A one-time item is billed at its price. From API version `2026-11-11`, creating or replacing a recurring item includes its `initial_price` plus `renewal_price` in the first invoice. Earlier version pins use the renewal price only. Edits that omit `items` preserve the saved prices.

### Set the customer and billing details

| Field | What it does |
| - | - |
| `member_id` | The customer, an existing member of the account prefixed `mber_`. |
| `email_address`, `customer_name` | The customer by email, instead of `member_id`. Whop uses the account's member with that email, then a Whop user with it, and creates a user when neither exists. `customer_name` names a new one. |
| `billing_address` | A new billing address for the customer, with `line1`, `city`, `postal_code`, and an ISO 3166-1 alpha-2 `country`, plus optional `name`, `line2`, `state`, and `phone`. |
| `mailing_address_id` | One of the customer's saved addresses instead, prefixed `ma_`. |
| `payment_method_id` | A saved payment method of the customer, prefixed `payt_`. Whop charges it when you issue the invoice. See [Charge a saved payment method](#charge-a-saved-payment-method). |
| `due_at` | When payment is due, as an ISO 8601 timestamp. It sets `overdue`, the reminder dates, and the due date the email, PDF, and checkout show. |
| `buyer_fee_percentage` | A fee the buyer pays, as a percentage of the subtotal from 0 to 100 with at most two decimal places. The response returns the percentage as `buyer_fee_percentage` and the fee as `buyer_fee_amount`. |
| `recipient_emails` | Up to 50 addresses that receive the invoice email, separate from the billed customer. Defaults to the customer's email when you create the draft. An update that leaves it out keeps the saved list, even when the customer changes. Saving it sends no email. See [Who gets the email](#who-gets-the-email). |
| `metadata` | Up to 50 string key-value pairs of your own. Whop copies them onto the membership it creates when you issue the invoice, and onto the automatic charge. |

Send `member_id` or `email_address`, and `billing_address` or `mailing_address_id`, never both of a pair. A billing address needs a customer. A customer is optional on a draft and required to issue it.

Whop calculates tax when you issue the invoice, using the billing address to place the customer. When tax applies and Whop can't tell where the customer is, the issue is refused, so add a billing address whenever you collect tax.

### Edit a draft

[Update Invoice](https://docs.whop.com/api-reference/invoices/update-invoice.md) changes a draft with `PATCH /invoices/{id}`. Send only the fields you're changing.

```typescript TypeScript theme={null}
const updated = await whop("PATCH", `/invoices/${invoice.id}`, {
	items: [
		{ plan_id: "plan_xxxxxxxxxxxxx", quantity: 3 },
		{ plan_id: "plan_yyyyyyyyyyyyy" },
	],
	buyer_fee_percentage: 2.5,
});
```

```python Python theme={null}
updated = whop(
    "PATCH",
    f"/invoices/{invoice['id']}",
    {
        "items": [
            {"plan_id": "plan_xxxxxxxxxxxxx", "quantity": 3},
            {"plan_id": "plan_yyyyyyyyyyyyy"},
        ],
        "buyer_fee_percentage": 2.5,
    },
)
```

* `items` replaces the whole list and reads each variant's current price. Leave it out to keep the saved items and prices.
* Changing `member_id` drops the saved billing address and payment method unless you send new ones. It keeps `recipient_emails`, so send a new list too, or the previous customer still gets the invoice email.
* `email_address` and `customer_name` aren't accepted. Send a different `member_id` to bill someone else.
* A draft with a [scheduled send](#schedule-a-send) accepts only a change to `recipient_emails` until you cancel the send.
* Once the invoice is issued, the only change you can make is `recipient_emails`, sent on its own, and only while the invoice is `open` or `uncollectible`. Any other edit is refused with a `400`.

## Choose how the customer pays

There are two ways to get paid for an issued invoice. Pick one before you issue it, because the saved payment method goes on the draft.

| | Charge a saved payment method | Customer pays through checkout |
| - | - | - |
| **Customer present** | No. Whop charges the method off session. | Yes, on Whop's hosted checkout |
| **What you need** | `payment_method_id` on the draft | The invoice's `checkout_link` |
| **3D Secure** | The charge can't complete a challenge | The customer completes it |
| **Your payment rules** | `block` and `allow` apply | All four actions apply |

An issued invoice gets a `checkout_link` even when it has a saved payment method. If the automatic charge fails, the customer can still pay through checkout.

### Charge a saved payment method

Put a saved payment method on the draft with `payment_method_id`. It has to be one of the customer's saved methods. [Save payment methods](https://docs.whop.com/developer/guides/save-payment-methods.md) shows how a customer saves one, and [List payment methods](https://docs.whop.com/api-reference/legacy/payment-methods/list-payment-methods.md) finds the ones a member has.

When you issue or send the invoice and its total is above zero, Whop:

1. Runs the same checks [Create Payment](https://docs.whop.com/api-reference/payments/create-payment.md) runs on a saved payment method.
2. Reserves stock for the invoiced quantities and prepares the payment's fees.
3. Returns the invoice as `open` and charges the method off session. The charge runs at the `charge_at` you pass to Issue Invoice or Send Invoice, otherwise on `due_at` while it's still ahead, otherwise right away. `scheduled_charge_at` shows a charge that's still to come.

The response doesn't tell you whether the charge succeeded. Fulfill from the [`payment.succeeded` webhook](#webhooks) for the invoice's `pay_` ID. A paid invoice completes the membership, and your `metadata` is on the charge and the membership. [Payment rules on invoice payments](#payment-rules-on-invoice-payments) covers how your payment rules screen the charge.

### If the automatic charge is declined

A declined charge leaves the invoice `open` and returns the reserved stock. [Retrieve the invoice as a payment](https://docs.whop.com/api-reference/payments/retrieve-payment.md) and read `failure_message` for why the latest attempt failed.

When the charge is declined or needs the customer to act, Whop emails the invoice's pay link to its [recipients](#who-gets-the-email), so the customer can pay through checkout instead. It's the one email Whop sends for an invoice you issue without sending it. It needs a `checkout_link` and at least one entry in `recipient_emails`, and it doesn't count toward the [send limit](#send-limit).

> [!WARNING]
>
> An off-session charge can't show the customer a 3D Secure challenge. If their bank asks for one, the automatic charge doesn't complete, and the invoice stays unpaid until the customer authenticates or pays another way.

## Issue the invoice

[Issue Invoice](https://docs.whop.com/api-reference/invoices/issue-invoice.md) turns a draft into an open invoice. Whop checks the invoice again, calculates final tax, and sets `status` to `open` and `totals_status` to `final`. When the invoice has a saved payment method and a total above zero, Whop then charges it, as described in [Charge a saved payment method](#charge-a-saved-payment-method).

```typescript TypeScript theme={null}
const issued = await whop("POST", `/invoices/${invoice.id}/issue`, undefined, crypto.randomUUID());

console.log(issued.status, issued.totals_status); // open final
```

```python Python theme={null}
issued = whop("POST", f"/invoices/{invoice['id']}/issue", idempotency_key=str(uuid.uuid4()))

print(issued["status"], issued["totals_status"])  # open final
```

```bash cURL theme={null}
curl -X POST https://api.whop.com/api/v1/invoices/pay_xxxxxxxxxxxxx/issue \
  -H "Authorization: Bearer $WHOP_API_KEY" \
  -H "Api-Version-Date: 2026-10-24" \
  -H "Idempotency-Key: 0b6f2d3c-91a4-4c7e-8f1e-5a2d7c9b3e10"
```

* **Tax becomes final.** `tax_amount` is `null` on a draft and set once you issue it. `total` and `amount_due` include it.
* **Whop creates the customer's membership** when the invoice bills a product. It stays drafted, with no access, until the invoice is paid. An invoice whose items all have no product gets no membership.
* **`payment.created` fires once** for the invoice's `pay_` ID.
* **Issuing again is safe.** Issuing an invoice that's already `open` runs the same checks again and, if they pass, returns it unchanged: its totals stay, and nothing is charged again.
* **Issuing doesn't email the invoice,** apart from the [pay link after a declined charge](#if-the-automatic-charge-is-declined). To email the invoice, [send it](#send-schedule-and-remind) instead, or deliver the checkout link yourself.

A refused issue changes nothing and charges nothing. Fix the cause and issue the invoice again. [Issue errors](#issue-errors) lists the reasons.

### Get the checkout link

An issued, `open` invoice carries a `checkout_link`, a signed value in the form `pay_xxxxxxxxxxxxx.<signature>`. It opens the invoice on Whop's hosted checkout at its fixed price.

**1. Read the link from the issued invoice**

`checkout_link` is `null` unless the invoice is `open`, so drafts and paid, void, or uncollectible invoices have none. It's also `null` for an invoice billed to a business buyer, one whose membership was canceled, and when your credential lacks `payment:charge`. A business buyer is a customer that's another Whop account rather than a person.

**2. Build the hosted checkout URL**

Split the link at its first `.`: the invoice's `pay_` ID comes before it, and the token after it.

```typescript TypeScript theme={null}
const { id, checkout_link } = await whop("GET", `/invoices/${invoice.id}`);

if (checkout_link) {
	const token = checkout_link.slice(checkout_link.indexOf(".") + 1);
	const url = `https://whop.com/checkout/${id}/?token=${encodeURIComponent(token)}`;
	console.log(url);
}
```

```python Python theme={null}
from urllib.parse import quote

current = whop("GET", f"/invoices/{invoice['id']}")

if current["checkout_link"]:
    token = current["checkout_link"].split(".", 1)[1]
    url = f"https://whop.com/checkout/{current['id']}/?token={quote(token, safe='')}"
    print(url)
```

**3. Send it to the customer**

[Send the invoice](#send-schedule-and-remind) to have Whop email the link, or put the URL in your own email or invoice page. Only the invoice's customer can pay through it. Anyone else is refused and asked to sign in as the invoice's customer. The link stops working once the invoice is paid, voided, or marked uncollectible.

**4. Fulfill from the webhook**

Listen for `payment.succeeded` for the invoice's `pay_` ID, the same as for the automatic charge. [Webhooks](#webhooks) shows a handler.

The checkout shows the invoice's number and due date beside its items, and the due date turns red once it has passed. The customer pays the items and total on the invoice and can't add a promo code.

## Send, schedule, and remind

[Send Invoice](https://docs.whop.com/api-reference/invoices/send-invoice.md) emails an invoice with `POST /invoices/{id}/send`. Sending a draft issues it first, the same as Issue Invoice. Sending an `open` invoice emails it again.

```typescript TypeScript theme={null}
await whop("POST", `/invoices/${invoice.id}/send`, undefined, crypto.randomUUID());
```

```python Python theme={null}
whop("POST", f"/invoices/{invoice['id']}/send", idempotency_key=str(uuid.uuid4()))
```

The email shows your account's name and logo, the invoice number, the total, the due date, each item, and a button that opens the invoice's checkout. Whop only emails an invoice the customer can pay from the email. An `open` invoice needs a `checkout_link` to be sent, and a draft needs a customer who isn't a business buyer. A draft billed to a business buyer can still be sent when it has a saved payment method, because it's charged instead.

### Who gets the email

Each send emails every address once, as a separate email, so recipients never see each other's addresses. The addresses are:

* The email on the customer's Whop account, which the invoice PDF prints under **Bill to**.
* Each entry in `recipient_emails`.

Whop saves `recipient_emails` in lowercase and de-duplicates the list, so an address in both gets one email.

An empty `recipient_emails` means Whop emails nobody, the customer included. Use it to keep Whop from emailing an invoice:

* Send Invoice refuses the invoice, unless it charges a saved payment method instead.
* No [pay link](#if-the-automatic-charge-is-declined) goes out after a declined charge.
* No reminders go out. Clearing `recipient_emails` on an `open` invoice stops its remaining reminders.

### When Whop charges instead of emailing

A draft with a `payment_method_id` and a total above zero is charged instead of emailed, the same as when you issue it. [If the automatic charge is declined](#if-the-automatic-charge-is-declined) covers what happens when the charge fails. Such a draft can be sent with an empty `recipient_emails`, because nobody needs the email.

### Send limit

Each invoice can be emailed three times in 24 hours, including the send that issued it. A fourth send in that window is refused with a `429` whose `error.code` is `invoice_send_limit_reached`. Reminders, the [pay link after a declined charge](#if-the-automatic-charge-is-declined), and a send that charges instead of emailing don't count toward the limit.

### Schedule a send

To send a draft later instead, pass `send_at`. The draft stays a draft and shows the time in `scheduled_send_at`. At that time, Whop sends it as if you had called Send Invoice then.

```typescript TypeScript theme={null}
const scheduled = await whop("POST", `/invoices/${invoice.id}/send`, { send_at: "2026-12-01T15:00:00Z" }, crypto.randomUUID());

console.log(scheduled.status, scheduled.scheduled_send_at); // draft 2026-12-01T15:00:00.000Z
```

```python Python theme={null}
scheduled = whop("POST", f"/invoices/{invoice['id']}/send", {"send_at": "2026-12-01T15:00:00Z"}, idempotency_key=str(uuid.uuid4()))

print(scheduled["status"], scheduled["scheduled_send_at"])  # draft 2026-12-01T15:00:00.000Z
```

* **Only a draft can be scheduled,** and `send_at` has to be in the future. Whop checks the recipients and the pay link when you schedule, the same as for a send now.
* **Send again with a new `send_at`** to move the time.
* **Editing is locked** until the send happens or you cancel it. An update that changes only `recipient_emails` still works, but it can't remove the last recipient from a draft that won't be charged.
* **[Cancel Scheduled Send](https://docs.whop.com/api-reference/invoices/cancel-scheduled-send.md)** with `POST /invoices/{id}/unschedule`. `scheduled_send_at` returns to `null`, and the draft can be edited again. Canceling an invoice with no scheduled send returns it unchanged.
* **Voiding a scheduled draft** stops its scheduled send.

```typescript TypeScript theme={null}
await whop("POST", `/invoices/${invoice.id}/unschedule`, undefined, crypto.randomUUID());
```

```python Python theme={null}
whop("POST", f"/invoices/{invoice['id']}/unschedule", idempotency_key=str(uuid.uuid4()))
```

Whop checks for due sends every minute, so a scheduled send goes out within about a minute of `send_at`. If it's refused, for example because a variant was deleted since you scheduled it, the invoice stays a draft, `scheduled_send_at` returns to `null`, and the API doesn't return the reason. Any other failure is retried about every 10 minutes, up to 12 attempts in all. Until an attempt succeeds or Whop stops retrying, `scheduled_send_at` shows the time of the next attempt rather than `null`. Read the invoice after `send_at` to confirm it's `open`.

### Reminders

When Send Invoice emails an invoice that has a `due_at`, Whop also schedules reminders 1, 3, 7, 14, 30, and 60 days after `due_at`, skipping any whose date has already passed. Each reminder goes to the same addresses as the invoice and says how many days the invoice is past due.

Reminders go out only while the invoice is `open`, so they stop once it's paid, voided, or marked uncollectible. A reminder that comes due while a payment is in progress waits until the payment finishes. An invoice you issue without sending, or one that's charged instead of emailed, gets no reminders.

### Email history

[List Invoice Sends](https://docs.whop.com/api-reference/invoices/list-invoice-sends.md) returns recorded email batches with `GET /invoices/{id}/sends`, newest first. It includes initial and repeat emails, scheduled emails, reminders, and payment follow-ups after a failed charge to a saved payment method. Use the first entry for the most recent recorded send, and `page_info.end_cursor` as `after` to read older entries. Manual batches use `send` when the invoice has no earlier completed email history and `resend` otherwise. The kind is selected when the completed batch is recorded, so requests queued together are classified in completion order. Earlier unrecorded emails remain unknown.

Each entry contains `kind`, `sent_at`, `recipient_count`, `recipients`, and `actor`. Reading history requires `payment:basic:read`. Addresses also require `member:email:read`. Without it, `recipients` is empty. The sender also requires `company:authorized_user:read`. Otherwise, `actor` is null. Automatic sends and account API keys also have a null actor.

The worker adds an entry after submitting the whole batch for delivery. For a partial batch, it adds an entry only if a retry completes the batch. The entry keeps the original recipient list. A recorded batch doesn't confirm inbox delivery. The mailer may suppress messages if the invoice or recipients change before delivery. History excludes emails sent before this feature was introduced. Issuing an invoice without emailing adds no history entry.

## Download a PDF

[Generate Invoice PDF](https://docs.whop.com/api-reference/invoices/generate-invoice-pdf.md) builds the invoice as a PDF and returns a short-lived link to it. It needs `payment:basic:read` and works at any status.

```typescript TypeScript theme={null}
const pdf = await whop("POST", `/invoices/${invoice.id}/generate_pdf`);

console.log(pdf.url, pdf.expires_at);
```

```python Python theme={null}
pdf = whop("POST", f"/invoices/{invoice['id']}/generate_pdf")

print(pdf["url"], pdf["expires_at"])
```

The response has `url` and `expires_at`. The link works for 15 minutes, and anyone holding it can download the file. Generate a new one when it expires.

The PDF leads with your business: your account's name heads the page, and the footer says Whop Inc issues the invoice in your name and on your behalf. It shows the invoice number, its date and due date, and the **Bill to** details. Each item shows its quantity, unit price, and amount, followed by the subtotal, service fee, tax, and amount due. An invoice with a recurring item also states the renewal amount and how often it renews. A draft's PDF shows its estimated totals, with tax at zero until you issue it.

Some content depends on your credential's permissions:

| Permission | Adds to the PDF |
| - | - |
| `member:basic:read` | The customer's name under **Bill to**. Without it, the PDF shows only a name saved on the billing address. |
| `member:email:read` | The customer's email under **Bill to** |
| `payment:charge` | A **Pay now** link that opens the invoice's checkout, on an invoice that has a `checkout_link` |

## Invoice a subscription

Put a recurring variant on an invoice to start a subscription when the customer pays it. Whop then renews it once per billing period.

1. **The invoice bills the first period and any upfront fee.** A recurring item must belong to a product and has a quantity of 1. From API version `2026-11-11`, its first invoice bills `initial_price` plus `renewal_price`. Paying starts the first paid billing period immediately, unless the variant has a free trial. Then the subscription [starts with the trial](#start-with-a-free-trial). In the dashboard, a custom recurring item has an optional **Upfront fee** field. The line item's `initial_price` states that one-time part of `unit_amount`, and the invoice page, email, and PDF list it on its own **Upfront fee** line.
2. **Paying the invoice starts the subscription,** whether Whop charges the saved payment method on issue or the customer pays through the `checkout_link`. The first period starts at the payment. The checkout opened from the link states the renewal amount, including the invoice's buyer fee, and how often it renews.
3. **Each renewal is collected the way you chose when you issued or sent the invoice.** It bills only the recurring items, with the invoice's buyer fee, at each variant's renewal price when you issued the invoice. That can differ from the price saved on the draft if the variant's price changed after you saved the items. One-time items are billed once.

Pass `collection_method` to [Issue Invoice](https://docs.whop.com/api-reference/invoices/issue-invoice.md) or [Send Invoice](https://docs.whop.com/api-reference/invoices/send-invoice.md) to choose how renewals are collected. A scheduled send applies the `collection_method` you scheduled it with, which the draft shows as `scheduled_collection_method`.

| `collection_method` | Each renewal |
| - | - |
| `charge_automatically` | Charges the payment method that paid the invoice: the draft's `payment_method_id`, or the one the customer pays the invoice link with. The default. |
| `send_invoice` | Issues a new numbered invoice due on the renewal date, and emails it with its payment link and reminders to the latest invoice's `recipient_emails` |

`send_invoice` needs at least one `recipient_emails` address and isn't available for invoices billed to a business. While it applies, the invoice's last recipient can't be removed. An invoice with no recurring item refuses `collection_method`. Paying the first invoice by card keeps `send_invoice`.

Each renewal is its own payment, with its own `pay_` ID and `payment.*` webhooks. A `charge_automatically` renewal isn't a numbered invoice, so it doesn't appear in List Invoices. A `send_invoice` renewal is a numbered invoice in List Invoices, and Whop doesn't charge it automatically. The subscription stays `past_due` until the customer pays through the invoice's `checkout_link`. The customer keeps access in the meantime. [Cancel the membership](https://docs.whop.com/api-reference/memberships/cancel-membership.md) to stop renewals. Once the membership is canceled, an open renewal invoice's `checkout_link` stops working.

To find the subscription, [retrieve the invoice as a payment](https://docs.whop.com/api-reference/payments/retrieve-payment.md) and read its `membership_id`, which needs `member:basic:read`.

### Start with a free trial

An invoice honors a recurring variant's `trial_period_days`, as checkout does. The first invoice bills only the variant's `initial_price`. Paying it starts the trial, and the first renewal bills `renewal_price` when the trial ends. Invoices created before 2026-11-16 17:00 UTC bill the first period and skip the trial.

* A customer who already had a subscription to the variant gets no trial. Their first invoice bills `initial_price` plus `renewal_price`. Renewal invoices never start a trial.
* The trial comes from the variant. For a custom recurring item, set `variant.trial_period_days`. An item-level `trial_period_days` is refused.
* Every recurring item on the invoice needs the same trial length.
* When the invoice charges nothing, issue or send it with a `payment_method_id`, which starts the trial right away, or with `collection_method: "send_invoice"`.
* In the dashboard, a custom recurring item has a **Free trial days** field. A product's billing plan shows its own trial.

### Stop after a set number of payments

To bill a fixed payment plan, such as six monthly payments, set `split_pay_required_payments` on the recurring variant, from 2 to 115. The invoice is the first payment, including any upfront fee. For example, a $75 upfront fee with three $40 installments bills $115, then $40, then \$40. Whop stops renewing once that many payments are made, and the membership stays active. The customer can't cancel the subscription themselves before then, but you can. Leave it unset to renew until the subscription is canceled.

## Statuses and lifecycle

| `status` | What it means | What can happen next |
| - | - | - |
| `draft` | You can still edit it. Totals are an estimate and tax isn't calculated yet. | You issue, send, schedule, void, or delete it |
| `open` | Issued. Totals and tax are final, and the invoice is waiting for payment. | The customer pays, or you void it or mark it uncollectible |
| `paid` | The customer paid. `amount_due` is zero. | Nothing |
| `uncollectible` | You marked an open invoice as one you don't expect to collect. | You void it |
| `void` | Canceled. `amount_due` is zero. | You delete it |

`status` takes its values from payments, so an invoice can also show a payment state such as `pending` while a payment is in progress. `totals_status` tells you whether the money fields are final: `estimated` while the invoice is a draft, `final` after you issue it. `overdue` is `true` for an `open` or `uncollectible` invoice whose `due_at` has passed.

### Void or mark uncollectible

[Void Invoice](https://docs.whop.com/api-reference/invoices/void-invoice.md) cancels a `draft`, `open`, or `uncollectible` invoice. Voiding an invoice that's already void returns it unchanged, and a paid invoice can't be voided.

[Mark an invoice uncollectible](https://docs.whop.com/api-reference/invoices/mark-invoice-uncollectible.md) to record that you don't expect to collect it. Only an `open` invoice can be marked, and marking an uncollectible invoice again returns it unchanged. An uncollectible invoice has no `checkout_link` and gets no reminders.

This example marks an `open` invoice uncollectible, then voids it:

```typescript TypeScript theme={null}
await whop("POST", `/invoices/${invoice.id}/mark_uncollectible`, undefined, crypto.randomUUID());

await whop("POST", `/invoices/${invoice.id}/void`, undefined, crypto.randomUUID());
```

```python Python theme={null}
whop("POST", f"/invoices/{invoice['id']}/mark_uncollectible", idempotency_key=str(uuid.uuid4()))

whop("POST", f"/invoices/{invoice['id']}/void", idempotency_key=str(uuid.uuid4()))
```

### Delete an invoice

[Delete Invoice](https://docs.whop.com/api-reference/invoices/delete-invoice.md) removes a `draft` or `void` invoice with `DELETE /invoices/{id}`. The response is `{ id, deleted: true }`. A deleted invoice no longer appears in List Invoices or List Payments, and its ID returns a `404`. You can't undo a delete.

Void an `open` invoice before you delete it. A draft with a scheduled send or charge needs [Cancel Scheduled Send](https://docs.whop.com/api-reference/invoices/cancel-scheduled-send.md) first. A draft renewal invoice can't be deleted while it bills a subscription. A void invoice that collected money or still awaits a bank wire can't be deleted, and neither can a paid, pending, or uncollectible invoice. An invoice's `deletable` field is `true` when it can be deleted.

Deleting needs `invoice:update` or `payment:charge`. The number of a deleted draft isn't reused.

```typescript TypeScript theme={null}
await whop("DELETE", `/invoices/${invoice.id}`);
```

```python Python theme={null}
whop("DELETE", f"/invoices/{invoice['id']}")
```

## List and retrieve invoices

[List Invoices](https://docs.whop.com/api-reference/invoices/list-invoices.md) returns the invoices your credential can read, newest first. An API key sees its own account, and a user token sees every account the user can read payments for. Narrow the list with these query parameters:

| Parameter | Filters to |
| - | - |
| `account_id` | One account |
| `status` | One status, such as `open`. An unknown value is a `400`. |
| `overdue` | `true` for overdue invoices, `false` for the rest |
| `scheduled` | `true` for scheduled drafts, `false` for the rest |
| `member_id` | One customer |
| `product_id` | Invoices that bill a product, prefixed `prod_` |
| `created_after`, `created_before` | A creation window, as ISO 8601 timestamps |

A scheduled draft is one with a `scheduled_send_at`. An `open` invoice is never scheduled, and neither is a draft that sends on its due date.

Sort with `order` (`created_at`, `due_at`, or `scheduled_send_at`) and `direction` (`asc` or `desc`). Sorting by `due_at` or `scheduled_send_at` puts invoices without that date last. The response is `{ data, page_info }`. While `page_info.has_next_page` is `true`, pass `page_info.end_cursor` as `after` to read the next page.

```typescript TypeScript theme={null}
const overdue = await whop("GET", "/invoices?account_id=biz_xxxxxxxxxxxxx&overdue=true&order=due_at&direction=asc");

for (const invoice of overdue.data) {
	console.log(invoice.number, invoice.due_at, invoice.amount_due.amount);
}
```

```python Python theme={null}
overdue = whop("GET", "/invoices?account_id=biz_xxxxxxxxxxxxx&overdue=true&order=due_at&direction=asc")

for invoice in overdue["data"]:
    print(invoice["number"], invoice["due_at"], invoice["amount_due"]["amount"])
```

[Retrieve Invoice](https://docs.whop.com/api-reference/invoices/retrieve-invoice.md) reads one invoice by its `pay_` ID. [List Payments](https://docs.whop.com/api-reference/payments/list-payments.md) leaves drafts out.

To export invoices as a CSV file, call [Create Export](https://docs.whop.com/api-reference/exports/create-export.md) with `resource: "receipt_invoices"`. It needs `payment:basic:read` and `payment:basic:export`. The customer columns appear only when your credential also has `member:basic:read` or `member:email:read`.

## Webhooks

Native invoices send payment webhooks for their `pay_` ID. The `invoice.*` events belong to the legacy invoice API, and native invoices don't send them.

| Event | When |
| - | - |
| `payment.created` | You issue the invoice. Drafts send nothing. |
| `payment.succeeded` | The invoice is paid. Fulfill from this event. |

Other `payment.*` events, such as `payment.failed`, can also arrive for the invoice's `pay_` ID as its payment attempts change. Subscribe to them with the [webhooks guide](https://docs.whop.com/developer/guides/webhooks.md).

To fulfill an order, save the invoice's `id` with the order when you create the draft. The `data` of a `payment.succeeded` event is the payment, so `data.id` is the invoice's `pay_` ID. After you [verify the event](https://docs.whop.com/developer/guides/webhooks.md#verify-and-handle-events), look up the order by that ID. In this handler, `orders` stands for your own order store:

```typescript TypeScript theme={null}
export async function handleWhopEvent(event: { type: string; data: { id: string } }) {
	if (event.type !== "payment.succeeded") return;

	const order = await orders.findByInvoiceId(event.data.id);
	if (!order || order.fulfilled) return;

	await orders.fulfill(order.id);
}
```

```python Python theme={null}
def handle_whop_event(event):
    if event["type"] != "payment.succeeded":
        return

    order = orders.find_by_invoice_id(event["data"]["id"])
    if order is None or order.fulfilled:
        return

    orders.fulfill(order.id)
```

Checking `fulfilled` keeps a redelivered event from fulfilling the order twice. Subscription renewals have their own `pay_` IDs, so they don't match an invoice.

## Payment rules on invoice payments

Your [payment rules](https://docs.whop.com/developer/guides/manage-fraud.md) run on invoice payments after Whop's own fraud screening, as they do on any checkout. What they can do depends on whether the customer is present.

* **The automatic charge on issue is off session.** A `block` rule declines it, and an `allow` rule exempts it from your other rules. `review` and `enforce_3ds` rules are skipped because the charge is off session.
* **A payment through the checkout link is on session.** All four actions apply, with the eligibility limits in [Manage fraud](https://docs.whop.com/developer/guides/manage-fraud.md).

On the automatic charge, card conditions read the saved payment method and billing conditions read the invoice's billing address. `customer_email` is the email on the customer's Whop account. The payment's `payment_rule_matches` lists the rule that decided it. A skipped rule isn't listed.

## Invoices in the dashboard

The **Invoices** page lists, creates, and sends native invoices. Legacy invoices are on its **Legacy invoices** tab, which [Send an Invoice](https://docs.whop.com/payments/send-an-invoice.md) covers.

## Permissions

| Call | Permission |
| - | - |
| [Create](https://docs.whop.com/api-reference/invoices/create-invoice.md), [update](https://docs.whop.com/api-reference/invoices/update-invoice.md), [issue](https://docs.whop.com/api-reference/invoices/issue-invoice.md), [send](https://docs.whop.com/api-reference/invoices/send-invoice.md), [cancel a scheduled send](https://docs.whop.com/api-reference/invoices/cancel-scheduled-send.md), [void](https://docs.whop.com/api-reference/invoices/void-invoice.md), or [mark uncollectible](https://docs.whop.com/api-reference/invoices/mark-invoice-uncollectible.md) | `payment:charge` |
| [List](https://docs.whop.com/api-reference/invoices/list-invoices.md), [retrieve](https://docs.whop.com/api-reference/invoices/retrieve-invoice.md), or [generate a PDF](https://docs.whop.com/api-reference/invoices/generate-invoice-pdf.md) | `payment:basic:read` |

Every write needs `payment:charge`, the same permission as [Create Payment](https://docs.whop.com/api-reference/payments/create-payment.md), and every read needs `payment:basic:read`. The `invoice:*` permissions belong to the legacy invoice API, and native invoices don't use them. A create or update that sends an inline `plan` item also needs `plan:create`.

Some fields need a permission of their own. Without it, the field comes back `null`, and `recipient_emails` comes back empty.

| Permission | Fields it reveals |
| - | - |
| `member:basic:read` | `member_id`, `customer_name` |
| `member:email:read` | `email_address`, `recipient_emails` |
| `payment:basic:read` | `payment_method_id`, `mailing_address_id`, `billing_address` |
| `payment:charge` | `checkout_link` |

If your credential loses one of these read permissions after a `POST`, replaying it with the same `Idempotency-Key` is refused with a `403` whose `error.code` is `invoice_replay_permission_revoked` when the stored response shows those fields. Fetch the invoice with `GET` instead. Add permissions from the [Permissions guide](https://docs.whop.com/developer/guides/permissions.md).

## Errors

Refusals come back as `{ error: { type, message, code } }`. Every code below is a `400` unless it says otherwise. An unknown `plan_id`, `member_id`, `mailing_address_id`, or `payment_method_id` is a `404`, and a timestamp Whop can't read is a `400` with `invalid_timestamp`.

These refusals are a `400` with no `error.code`, so read `error.message` for the reason:

* An unknown top-level field.
* A `quantity` outside 1 to 999,999.
* An invalid address in `recipient_emails`, or more than 50 of them.
* A broken several-item rule, such as a billing period mismatch.

### Create and update errors

| `error.code` | Why |
| - | - |
| `invoice_item_count_invalid`, `invoice_item_invalid` | `items` doesn't have 1 to 50 entries, or an item doesn't have exactly one of `plan_id` and `plan`. |
| `invoice_plan_account_mismatch`, `invoice_plan_duplicated`, `invoice_currency_mismatch` | A variant belongs to another account, appears twice, or uses another currency than the rest. |
| `invoice_quantity_unsupported` | `quantity` is above 1 for a variant that doesn't allow multiple quantities. |
| `invoice_plan_type_unsupported`, `invoice_legacy_plan_unsupported` | The variant isn't buy-now, or belongs to a legacy invoice. |
| `invoice_installment_count_mismatch` | Recurring items on the invoice stop after different numbers of payments. |
| `invoice_recurring_plan_product_required` | A recurring variant has no product. |
| `invoice_plan_unavailable`, `invoice_plan_archived`, `invoice_plan_currency_changed` | A variant was deleted or archived, or a saved variant changed currency. Send `items` again. |
| `invoice_customer_ambiguous`, `invoice_address_ambiguous` | You sent both `member_id` and `email_address`, or both `billing_address` and `mailing_address_id`. |
| `invoice_address_properties_invalid`, `invoice_address_incomplete`, `invoice_address_country_invalid` | `billing_address` has an unknown field, misses a required one, or has a `country` that isn't a valid code. |
| `invoice_address_member_required`, `invoice_member_mismatch`, `invoice_address_mismatch`, `invoice_payment_method_mismatch` | The address has no customer, or the member, address, or payment method belongs to someone else. |
| `invoice_payment_method_unsupported` | The saved payment method can't pay invoices. |
| `invoice_buyer_fee_invalid`, `invoice_metadata_invalid`, `invoice_recipients_invalid` | `buyer_fee_percentage` is out of range, a `metadata` value isn't a string, or `recipient_emails` isn't an array. |
| `invoice_not_editable`, `invoice_recipients_not_editable` | The invoice isn't a draft, or a `recipient_emails` change reached an invoice that isn't a draft, `open`, or `uncollectible`. |
| `invoice_scheduled_not_editable`, `invoice_scheduled_recipients_required` | The draft has a scheduled send. Cancel it before editing, or before removing the last recipient. |

### Issue errors

| `error.code` | Why |
| - | - |
| `invoice_not_issuable` | The invoice isn't a draft or `open`. |
| `invoice_recipient_required` | The invoice has no customer. |
| `invoice_account_verification_required` | Your account hasn't completed verification. |
| `invoice_account_suspended` | Your account is suspended. |
| `invoice_items_required`, `invoice_items_changed`, `invoice_plan_unavailable`, `invoice_plan_archived` | The invoice has no items, or a variant was deleted, archived, or changed account, currency, or product. Update the items. |
| `invoice_tax_address_required` | Tax applies and Whop can't tell where the customer is. Add a billing address. |
| `invoice_tax_unresolved` | Tax couldn't be calculated. |
| `invoice_changed_during_issuance`, `invoice_changed_during_calculation` | The invoice changed while tax was calculated. Issue it again. |
| `invoice_payment_method_unavailable`, `invoice_payment_method_unsupported`, `payment_method_unavailable` | The saved payment method was removed from the customer, can't pay invoices, or no longer works. |
| `out_of_stock` | Only when charging a saved payment method: a variant doesn't have enough stock for the invoiced quantity. |
| `payment_fees_unavailable` | Only when charging a saved payment method: the payment's fees couldn't be prepared. |

### Send and schedule errors

Sending a draft issues it, so a send can also be refused for any reason in [Issue errors](#issue-errors).

| `error.code` | Why |
| - | - |
| `invoice_not_sendable` | The invoice isn't a draft or `open`. |
| `invoice_send_recipients_required` | The invoice has no `recipient_emails` and nothing to charge. |
| `invoice_payment_link_unavailable` | There's no pay link to email: an `open` invoice has no `checkout_link`, or a draft without a saved payment method bills a business buyer. |
| `invoice_send_limit_reached` | A `429`: the invoice was already emailed three times in the last 24 hours. |
| `invoice_not_schedulable` | Only a draft can be scheduled. |
| `invoice_scheduled_send_in_past` | `send_at` isn't in the future. |

### Other errors

| `error.code` | Why |
| - | - |
| `invoice_not_uncollectible` | Only an `open` invoice can be marked uncollectible. |
| `invoice_replay_permission_revoked` | A `403`: an idempotent replay would show fields your credential can no longer read. |

Voiding a paid invoice is a `400` without a code.

## Next steps

> [card] [Invoices API reference](https://docs.whop.com/api-reference/invoices/create-invoice.md)
>
> Every invoice endpoint, parameter, and response field.

> [card] [Save payment methods](https://docs.whop.com/developer/guides/save-payment-methods.md)
>
> Collect the saved payment method an invoice charges on issue.

> [card] [Manage fraud](https://docs.whop.com/developer/guides/manage-fraud.md)
>
> Write the payment rules that screen invoice payments.

> [card] [Listen to webhooks](https://docs.whop.com/developer/guides/webhooks.md)
>
> Track `payment.created` and `payment.succeeded` for your invoices.


## Related topics

- [Invoices API](https://docs.whop.com/api-reference/invoices/invoice.md)
- [Delete Invoice](https://docs.whop.com/api-reference/invoices/delete-invoice.md)
- [Update Invoice](https://docs.whop.com/api-reference/invoices/update-invoice.md)
