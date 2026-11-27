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

# Webhooks

> Receive signed notifications on your server when events like payments and membership changes happen on Whop.

A webhook is a `POST` request that Whop sends to your server when an event occurs. Examples of events: a payment succeeds, a membership activates, a dispute opens. You register a public URL. Whop sends a signed JSON event to this URL each time one of your subscribed events occurs.

> [!TIP]
>
> Whop webhooks use the [Standard Webhooks](https://github.com/standard-webhooks/standard-webhooks) specification. The Whop SDKs verify the signature and parse the event in one call. For Go, use **Verify without an SDK** at the bottom of this page.

## What a webhook looks like

Each event is one `POST` request:

```http theme={null}
POST /webhooks/whop HTTP/1.1
content-type: application/json
webhook-id: msg_bQPHmO2eBnHYtWWuxAN9K3Xd
webhook-timestamp: 1786381404
webhook-signature: v1,K5oZfzN95Z9UVu1EsfQmfVNQhnkZ2pCPljWFR61G0P0=
```

```json theme={null}
{
  "id": "msg_bQPHmO2eBnHYtWWuxAN9K3Xd",
  "type": "payment.succeeded",
  "api_version": "v1",
  "api_version_date": "2026-08-14",
  "timestamp": "2026-08-10T17:03:24.291Z",
  "account_id": "biz_XXXXXXXX",
  "data": {
    "id": "pay_XXXXXXXX",
    "...": "the full payment object"
  }
}
```

The envelope follows your webhook's `api_version_date` pin, the same way `data` does. Webhooks pinned before `2026-08-14` — and webhooks without a pin — receive the account field as `company_id` instead of `account_id`.

The HTTP request headers version the same way: any future header change ships as a dated API version, and your pin keeps the headers you integrated against. Four headers are contractually frozen and never change on any version: `webhook-id`, `webhook-signature`, `webhook-timestamp`, and `content-type` — signature verification works identically on every pin.

One optional envelope field describes what changed:

* **`previous_attributes`** — present on `.updated` events that capture changes (`account.updated`, `product.updated`, `plan.updated`, and `shipment.updated`): an object with the old values of the `data` fields that changed, keyed by field name. Renaming the account to "Shine Time Auto Detailing" delivers `{"title": "Webb's Mobile Detailing"}` — the value each changed field had before the write. When the field is absent, no change capture was available for the event. Values follow your webhook's `api_version_date` pin, the same way `data` does.

## Recover a subscription payment

When the bank asks the buyer to verify an off-session charge, Whop emits `payment.requires_action`. Send its `data.recovery_url` to the buyer so they can sign in to Whop and complete 3D Secure for that payment. On-session checkouts never emit it, because the buyer is already completing the challenge. A failed subscription renewal also emits `payment.failed`. Inspect `data.decline_code` and `data.failure_message` for the reason when available, and use `data.recovery_url` there too when it's present.

The same `recovery_url` field is available when you retrieve the [Payment resource](https://docs.whop.com/api-reference/payments/payment). Payment lists don't include recovery links. It's `null` when authentication recovery is unavailable or you lack `member:basic:read`. Opening the link doesn't start a charge: the buyer must confirm, and Whop checks whether the payment can still be recovered. After recovery, continue handling `payment.succeeded` or `payment.failed` for the outcome.

## Create a webhook

> [tab] API
>
> ```bash theme={null}
> curl -X POST "https://api.whop.com/api/v1/webhooks" \
>   -H "Authorization: Bearer $WHOP_API_KEY" \
>   -H "Content-Type: application/json" \
>   -d '{
>     "url": "https://shinetime.example/webhooks/whop",
>     "events": ["payment.succeeded"]
>   }'
> ```
>
> The response contains `webhook_secret`. **The API shows this value only one time.** Store it immediately. See the [Webhooks API reference](https://docs.whop.com/api-reference/webhooks/webhook) for all fields, including `resource_id` (the account or app that owns the webhook) and `child_resource_events`. A webhook that belongs to an app also receives that app's install and uninstall events. See [App Installations](https://docs.whop.com/developer/guides/app-installations.md).

> [tab] Dashboard
>
> 1. Open the [Developer tab of your dashboard](https://whop.com/dashboard/developer) and select **Create webhook**.
> 2. Enter your endpoint URL and select the events.
> 3. After you create the webhook, copy the signing secret from the **Secret** column of the table. Store it as `WHOP_WEBHOOK_SECRET`.

## Verify and handle events

Always verify the signature before you use a payload.

**1. Store your signing secret**

Verification needs the secret, not an SDK client. Store the value the API returned as `WHOP_WEBHOOK_SECRET`, and pass it to the helper exactly as Whop gave it to you — a `ws_` string. Don't strip the prefix, and don't base64-encode it. The helper derives the key.

```bash theme={null}
WHOP_WEBHOOK_SECRET=ws_0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef
```

**2. Handle events on your server**

One call verifies the signature and returns the parsed event. If the signature is wrong, the call raises and your handler never sees the payload.

```typescript Typescript + NextJS theme={null}
import { waitUntil } from "@vercel/functions";
import { unwrapWebhook } from "@whop/sdk/helpers";
import type { Whop } from "@whop/sdk";
import type { NextRequest } from "next/server";

export async function POST(request: NextRequest): Promise<Response> {
   	// Give the raw body. Parsing it first changes the bytes and the signature check fails.
   	const payload = await request.text();
   	const headers = Object.fromEntries(request.headers);

   	const event = unwrapWebhook<{ type: string; data: Whop.Payment }>(payload, {
  		headers,
  		key: process.env.WHOP_WEBHOOK_SECRET!,
   	});

   	if (event.type === "payment.succeeded") {
  		waitUntil(handlePaymentSucceeded(event.data));
   	}

   	// Respond in less than 5 seconds, or Whop retries.
   	return new Response("OK", { status: 200 });
}

async function handlePaymentSucceeded(payment: Whop.Payment) {
   	// Do the fulfillment work here: update your database, give access, send email.
   	console.log("[PAYMENT SUCCEEDED]", payment);
}
```

```python Python + FastAPI theme={null}
import os

from fastapi import BackgroundTasks, FastAPI, Request, Response
from whop_sdk.lib.verify_webhook import unwrap

app = FastAPI()

@app.post("/api/webhooks/whop")
async def whop_webhook(request: Request, background: BackgroundTasks):
    # Give the raw body. Parsing it first changes the bytes and the signature check fails.
    payload = await request.body()

    event = unwrap(payload, headers=dict(request.headers), key=os.environ["WHOP_WEBHOOK_SECRET"])

    if event["type"] == "payment.succeeded":
        background.add_task(handle_payment_succeeded, event["data"])

    # Respond in less than 5 seconds, or Whop retries.
    return Response(status_code=200)


def handle_payment_succeeded(payment):
    # Do the fulfillment work here: update your database, give access, send email.
    print(f"[PAYMENT SUCCEEDED] {payment['id']}")
```

```ruby Ruby on Rails theme={null}
require "whop_sdk"

class Api::WebhooksController < ApplicationController
  skip_before_action :verify_authenticity_token

  def whop
    # Give the raw body. Parsing it first changes the bytes and the signature check fails.
    event = WhopSDK::Helpers::VerifyWebhook.unwrap(
      request.raw_post,
      headers: {
        "webhook-id" => request.headers["webhook-id"],
        "webhook-timestamp" => request.headers["webhook-timestamp"],
        "webhook-signature" => request.headers["webhook-signature"],
      },
      key: ENV.fetch("WHOP_WEBHOOK_SECRET"),
    )

    if event[:type] == "payment.succeeded"
      HandlePaymentSucceededJob.perform_later(event[:data][:id])
    end

    # Respond in less than 5 seconds, or Whop retries.
    head :ok
  end
end
```

The helper returns the parsed body, not a typed event object: a plain object in TypeScript, a `dict` in Python, and a `Hash` with symbol keys in Ruby. Branch on `type` and read `data` as shown.

> [!TIP]
>
> Do only the necessary work before you respond: verify the event, put the work in a queue, and return `200`. The TypeScript example uses `waitUntil` from `@vercel/functions`. On other runtimes, use the background-task function of your framework or a job queue.

> [accordion] Verify without an SDK
>
> Whop signs the string `{webhook-id}.{webhook-timestamp}.{raw body}` with HMAC-SHA256. The key is your `ws_...` secret. The `webhook-signature` header contains the result in base64: `v1,<signature>`.
>
> To verify a request: compute the HMAC over the raw request body, encode the result in base64, and compare it to the header value with a constant-time comparison. Reject a request if its `webhook-timestamp` is more than 5 minutes from the current time. This prevents replay attacks. The SDK verifiers apply this limit automatically.

## Test your endpoint

* **Send a test event.** In the dashboard, open the menu of the webhook and select a test event. Or call [`POST /api/v1/webhooks/{id}/test`](https://docs.whop.com/api-reference/webhooks/webhook). Whop sends a sample payload to your URL and returns the response status and body from your server.
* **Examine deliveries.** Whop keeps each delivery for 30 days: the request, the response code, the response body, and the timing. See the deliveries in the dashboard or with [`GET /api/v1/webhooks/{id}/deliveries`](https://docs.whop.com/api-reference/webhooks/webhook).

> [!NOTE]
>
> A webhook URL must be reachable from the public internet. Whop rejects `localhost` and private-network addresses. For local development, use [ngrok](https://ngrok.com) or [Cloudflare Tunnel](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/) to forward a public URL to your machine. Register the tunnel URL.

## Delivery

**Respond with a 2xx status in less than 5 seconds.** All other results are failed attempts: a timeout, an error status, or a redirect. Whop doesn't follow redirects.

**Whop delivers each event at least one time.** The same event can arrive more than one time. Make your handler idempotent. Each retry of a delivery has the same `webhook-id`. Store the `webhook-id` and ignore duplicates.

**Whop retries failed deliveries for approximately 3 days.** After the first attempt, Whop retries 12 times with increased delays: 30 seconds, 2 minutes, 8 minutes, 30 minutes, 1 hour, 3 hours, 6 hours, and then each 12 hours. The full schedule is approximately 71 hours.

**The sequence of deliveries isn't guaranteed.** A newer event can arrive before an older event. Process each event independently. If the sequence is important, read the current state from the API.

**Whop disables endpoints that continue to fail.** If all deliveries to your endpoint fail for 24 hours, Whop sends a warning email to your account. If the failures continue for 72 hours and 10 or more deliveries failed, Whop disables the webhook and sends a second email. To enable the webhook again, use the dashboard or send `PATCH /api/v1/webhooks/{id}` with `"enabled": true`. This resets the failure history. Whop doesn't send the events that occurred while the webhook was turned off. Read the API to find the data that you missed.

> [card] [Troubleshoot webhook delivery](https://docs.whop.com/developer/troubleshooting.md#webhook-delivery)
>
> Fix signature failures, retries, duplicate events, and local tunnel issues.

## Events and versions

Two version fields control the data that your endpoint receives:

* **`api_version`** — the envelope format. Use `v1`. This page describes only `v1`. The legacy formats `v2` and `v5` exist for old integrations and don't use Standard Webhooks signatures. Don't use them for new integrations. They stop receiving events at the end of December 6, 2026, Pacific time.
* **`api_version_date`** — the dated API version that sets the shape of `data` and of the envelope itself (for example, pins from `2026-08-14` carry `account_id` where older pins carry `company_id`). It operates the same as the `Api-Version-Date` header on REST reads. Set it when you create a webhook with the API. Then the payload shape stays stable when the API changes. Webhooks without a pin keep their initial payload and envelope shape. Exception: resources that exist only on the current API (cards, variants, transfers, swaps, deposits, exports) always receive the latest `data` shape — the envelope still follows the pin.

Event names have the format `resource.action`. Each event in the table links to a reference page with the payload schema:

| Resource | Events |
| - | - |
| Accounts | [`account.updated`](https://docs.whop.com/api-reference/accounts/account-updated) |
| Ad Campaigns | [`ad_campaign.payment_failed`](https://docs.whop.com/api-reference/ad-campaigns/ad-campaign-payment-failed), [`ad_campaign.updated`](https://docs.whop.com/api-reference/ad-campaigns/ad-campaign-updated) |
| Ad Groups | [`ad_group.updated`](https://docs.whop.com/api-reference/ad-groups/ad-group-updated) |
| Ads | [`ad.updated`](https://docs.whop.com/api-reference/ads/ad-updated) |
| App Installations | [`app_installation.created`](https://docs.whop.com/api-reference/app-installations/app-installation-created), [`app_installation.deleted`](https://docs.whop.com/api-reference/app-installations/app-installation-deleted) |
| Card transactions | [`card_transaction.completed`](https://docs.whop.com/api-reference/cards/card-transaction-completed), [`card_transaction.created`](https://docs.whop.com/api-reference/cards/card-transaction-created), [`card_transaction.declined`](https://docs.whop.com/api-reference/cards/card-transaction-declined), [`card_transaction.reversed`](https://docs.whop.com/api-reference/cards/card-transaction-reversed), [`card_transaction.updated`](https://docs.whop.com/api-reference/cards/card-transaction-updated) |
| Cards | [`card.canceled`](https://docs.whop.com/api-reference/cards/card-canceled), [`card.created`](https://docs.whop.com/api-reference/cards/card-created), [`card.frozen`](https://docs.whop.com/api-reference/cards/card-frozen), [`card.updated`](https://docs.whop.com/api-reference/cards/card-updated), [`card_application.approved`](https://docs.whop.com/api-reference/cards/card-application-approved), [`card_application.created`](https://docs.whop.com/api-reference/cards/card-application-created), [`card_application.denied`](https://docs.whop.com/api-reference/cards/card-application-denied), [`card_application.updated`](https://docs.whop.com/api-reference/cards/card-application-updated) |
| Course lesson interactions | [`course_lesson_interaction.completed`](https://docs.whop.com/api-reference/legacy/course-lesson-interactions/course-lesson-interaction-completed) |
| Deposits | [`deposit.succeeded`](https://docs.whop.com/api-reference/deposits/deposit-succeeded) |
| Dispute alerts | [`dispute_alert.created`](https://docs.whop.com/api-reference/dispute-alerts/dispute-alert-created) |
| Disputes | [`dispute.created`](https://docs.whop.com/api-reference/disputes/dispute-created), [`dispute.updated`](https://docs.whop.com/api-reference/disputes/dispute-updated) |
| Domains | [`domain.created`](https://docs.whop.com/api-reference/domains/domain-created), [`domain.deleted`](https://docs.whop.com/api-reference/domains/domain-deleted), [`domain.updated`](https://docs.whop.com/api-reference/domains/domain-updated) |
| Entries | [`entry.approved`](https://docs.whop.com/api-reference/legacy/entries/entry-approved), [`entry.created`](https://docs.whop.com/api-reference/legacy/entries/entry-created), [`entry.deleted`](https://docs.whop.com/api-reference/legacy/entries/entry-deleted), [`entry.denied`](https://docs.whop.com/api-reference/legacy/entries/entry-denied) |
| Events | [`events.created`](https://docs.whop.com/api-reference/events/events-created) |
| Exports | [`export.completed`](https://docs.whop.com/api-reference/exports/export-completed), [`export.failed`](https://docs.whop.com/api-reference/exports/export-failed) |
| Financing Applications | [`account.financing_approved`](https://docs.whop.com/api-reference/financing-applications/account-financing-approved), [`account.financing_denied`](https://docs.whop.com/api-reference/financing-applications/account-financing-denied), [`account.financing_disabled`](https://docs.whop.com/api-reference/financing-applications/account-financing-disabled) |
| Identity profiles | [`identity_profile.updated`](https://docs.whop.com/api-reference/verifications/identity-profile-updated) |
| Invoices | [`invoice.created`](https://docs.whop.com/api-reference/legacy/invoices/invoice-created), [`invoice.marked_uncollectible`](https://docs.whop.com/api-reference/legacy/invoices/invoice-marked-uncollectible), [`invoice.paid`](https://docs.whop.com/api-reference/legacy/invoices/invoice-paid), [`invoice.past_due`](https://docs.whop.com/api-reference/legacy/invoices/invoice-past-due), [`invoice.voided`](https://docs.whop.com/api-reference/legacy/invoices/invoice-voided) |
| Ledger accounts | [`ledger_account.funds_available`](https://docs.whop.com/api-reference/legacy/ledger-accounts/ledger-account-funds-available) |
| Ledgers | [`financial_activity.funds_available`](https://docs.whop.com/api-reference/ledgers/financial-activity-funds-available) |
| Members | [`member.created`](https://docs.whop.com/api-reference/members/member-created), [`member.updated`](https://docs.whop.com/api-reference/members/member-updated) |
| Memberships | [`membership.activated`](https://docs.whop.com/api-reference/memberships/membership-activated), [`membership.trial_ending_soon`](https://docs.whop.com/api-reference/memberships/membership-trial-ending-soon), [`membership.updated`](https://docs.whop.com/api-reference/memberships/membership-updated) |
| Messages | [`chat.message.created`](https://docs.whop.com/api-reference/legacy/messages/chat-message-created) |
| Payments | [`payment.authorized`](https://docs.whop.com/api-reference/payments/payment-authorized), [`payment.canceled`](https://docs.whop.com/api-reference/payments/payment-canceled), [`payment.created`](https://docs.whop.com/api-reference/payments/payment-created), [`payment.failed`](https://docs.whop.com/api-reference/payments/payment-failed), [`payment.pending`](https://docs.whop.com/api-reference/payments/payment-pending), [`payment.requires_action`](https://docs.whop.com/api-reference/payments/payment-requires-action), [`payment.succeeded`](https://docs.whop.com/api-reference/payments/payment-succeeded), [`shipping_address.changed`](https://docs.whop.com/api-reference/payments/shipping-address-changed) |
| Payout accounts | [`payout_account.status_updated`](https://docs.whop.com/api-reference/legacy/payout-accounts/payout-account-status-updated) |
| Payout methods | [`payout_method.created`](https://docs.whop.com/api-reference/payouts/payout-method-created) |
| Payouts | [`payout.created`](https://docs.whop.com/api-reference/payouts/payout-created), [`payout.reversed`](https://docs.whop.com/api-reference/payouts/payout-reversed), [`payout.updated`](https://docs.whop.com/api-reference/payouts/payout-updated) |
| Products | [`product.created`](https://docs.whop.com/api-reference/products/product-created), [`product.deleted`](https://docs.whop.com/api-reference/products/product-deleted), [`product.published`](https://docs.whop.com/api-reference/products/product-published), [`product.unpublished`](https://docs.whop.com/api-reference/products/product-unpublished), [`product.updated`](https://docs.whop.com/api-reference/products/product-updated) |
| Reactions | [`chat.reaction.created`](https://docs.whop.com/api-reference/legacy/reactions/chat-reaction-created) |
| Refunds | [`refund.created`](https://docs.whop.com/api-reference/legacy/refunds/refund-created), [`refund.updated`](https://docs.whop.com/api-reference/legacy/refunds/refund-updated) |
| Resolution center cases | [`resolution_center_case.created`](https://docs.whop.com/api-reference/resolution-center-cases/resolution-center-case-created), [`resolution_center_case.decided`](https://docs.whop.com/api-reference/resolution-center-cases/resolution-center-case-decided), [`resolution_center_case.updated`](https://docs.whop.com/api-reference/resolution-center-cases/resolution-center-case-updated) |
| Setup intents | [`setup_intent.canceled`](https://docs.whop.com/api-reference/setup-intents/setup-intent-canceled), [`setup_intent.requires_action`](https://docs.whop.com/api-reference/setup-intents/setup-intent-requires-action), [`setup_intent.succeeded`](https://docs.whop.com/api-reference/setup-intents/setup-intent-succeeded) |
| Shipments | [`shipment.created`](https://docs.whop.com/api-reference/shipments/shipment-created), [`shipment.updated`](https://docs.whop.com/api-reference/shipments/shipment-updated) |
| Swaps | [`swap.completed`](https://docs.whop.com/api-reference/swaps/swap-completed) |
| Transfers | [`transfer.completed`](https://docs.whop.com/api-reference/transfers/transfer-completed), [`transfer.created`](https://docs.whop.com/api-reference/transfers/transfer-created), [`transfer.failed`](https://docs.whop.com/api-reference/transfers/transfer-failed) |
| Variants | [`plan.created`](https://docs.whop.com/api-reference/variants/variant-created), [`plan.deleted`](https://docs.whop.com/api-reference/variants/variant-deleted), [`plan.updated`](https://docs.whop.com/api-reference/variants/variant-updated) |
| Verifications | [`verification.succeeded`](https://docs.whop.com/api-reference/legacy/verifications/verification-succeeded) |

## Next steps

> [card] [Accept payments](https://docs.whop.com/developer/guides/accept-payments.md)
>
> Use `payment.succeeded` webhooks with checkout to fulfill orders.

> [card] [Save payment methods](https://docs.whop.com/developer/guides/save-payment-methods.md)
>
> Use setup intents and webhooks to charge customers later.

> [card] [Quickstart](https://docs.whop.com/developer/quickstart.md)
>
> See how webhooks operate with checkout, transfers, and KYC.

> [card] [Webhooks API reference](https://docs.whop.com/api-reference/webhooks/webhook)
>
> All endpoints and fields: create, update, test, deliveries.


## Related topics

- [Webhook](https://docs.whop.com/api-reference/legacy/webhooks/webhook.md)
- [Webhooks API](https://docs.whop.com/api-reference/webhooks/webhook.md)
- [Delete Webhook](https://docs.whop.com/api-reference/webhooks/delete-webhook.md)
