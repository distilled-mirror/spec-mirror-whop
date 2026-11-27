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

# Webhooks API

> A Webhook is an endpoint on your server that Whop sends signed HTTP POST requests to when events happen, such as a payment succeeding or a membership going active.

A webhook is attached to an account or an app and subscribes to the events you choose.

Use the Webhooks API to create and update endpoints, send a test event, inspect the delivery log, and replay deliveries your server missed. See [Webhooks & Events](https://docs.whop.com/developer/guides/webhooks.md) for verifying signatures and handling retries.

## Endpoints

| Endpoint | Request |
| - | - |
| [List Webhooks](https://docs.whop.com/api-reference/webhooks/list-webhooks.md) | GET `/webhooks` |
| [Retrieve Webhook](https://docs.whop.com/api-reference/webhooks/retrieve-webhook.md) | GET `/webhooks/{id}` |
| [List Deliveries](https://docs.whop.com/api-reference/webhooks/list-deliveries.md) | GET `/webhooks/{id}/deliveries` |
| [Create Webhook](https://docs.whop.com/api-reference/webhooks/create-webhook.md) | POST `/webhooks` |
| [Replay Delivery](https://docs.whop.com/api-reference/webhooks/replay-delivery.md) | POST `/webhooks/{id}/deliveries/{delivery_id}/replay` |
| [Replay Deliveries in a Range](https://docs.whop.com/api-reference/webhooks/replay-deliveries-in-a-range.md) | POST `/webhooks/{id}/replay` |
| [Send Test Event](https://docs.whop.com/api-reference/webhooks/send-test-event.md) | POST `/webhooks/{id}/test` |
| [Update Webhook](https://docs.whop.com/api-reference/webhooks/update-webhook.md) | PATCH `/webhooks/{id}` |
| [Delete Webhook](https://docs.whop.com/api-reference/webhooks/delete-webhook.md) | DELETE `/webhooks/{id}` |

## Attributes

**id** `string` *required*

Webhook ID, prefixed `hook_`.

**api\_version** `string` *required*

The API version used to format payloads sent to this webhook endpoint.

Available options: `v1`, `v2`, `v5`

**api\_version\_date** `string | null` *required*

The dated API version (Api-Version-Date) that v1 payloads for this endpoint
are pinned to: events serialize exactly like a REST read at this version (the
native serializer where the resource has one). Null when unpinned — legacy
(v2/v5) webhooks, and v1 webhooks on the legacy payload shape.

**child\_resource\_events** `boolean` *required*

Whether events are sent for child resources. For example, if the webhook is on
an account, enabling this sends events only from its subaccounts.

**consecutive\_failures** `integer` *required*

Number of consecutive deliveries whose first attempt to this endpoint failed
since it last accepted one. Later retries of the same delivery do not
increment it. Resets to `0` when a delivery succeeds or the webhook is
re-enabled.

**created\_at** `string` *required*

When the webhook was created, as an ISO 8601 timestamp.

**disabled\_at** `string | null` *required*

When Whop automatically disabled this webhook, as an ISO 8601 timestamp.
`null` unless the webhook was disabled by Whop; a webhook you disabled
yourself has `enabled: false` and a `null` `disabled_at`.

**disabled\_reason** `string | null` *required*

Why Whop disabled this webhook. `delivery_failures` means every delivery failed for 3 days straight. `null` when `disabled_at` is `null`.

Available options: `delivery_failures`

**enabled** `boolean` *required*

Whether this webhook endpoint is currently active and receiving events.

**events** `string[]` *required*

Event types this webhook is subscribed to, in dot form (for example `payment.succeeded`).

Available options: `account.updated`, `account.financing_approved`, `account.financing_denied`, `account.financing_disabled`, `invoice.created`, `invoice.marked_uncollectible`, `invoice.paid`, `invoice.past_due`, `invoice.voided`, `membership.activated`, `membership.deactivated`, `membership.trial_ending_soon`, `membership.updated`, `entry.created`, `entry.approved`, `entry.denied`, `entry.deleted`, `export.completed`, `export.failed`, `setup_intent.requires_action`, `setup_intent.succeeded`, `setup_intent.canceled`, `ledger_account.funds_available`, `swap.completed`, `deposit.succeeded`, `financial_activity.funds_available`, `transfer.created`, `transfer.completed`, `transfer.failed`, `withdrawal.created`, `withdrawal.updated`, `withdrawal.reversed`, `payout.created`, `payout.updated`, `payout.reversed`, `card_transaction.created`, `card_transaction.updated`, `card_transaction.completed`, `card_transaction.declined`, `card_transaction.reversed`, `card.created`, `card.updated`, `card.frozen`, `card.canceled`, `card_application.created`, `card_application.updated`, `card_application.approved`, `card_application.denied`, `course_lesson_interaction.completed`, `domain.created`, `domain.updated`, `domain.deleted`, `capital.created`, `capital.updated`, `payout_method.created`, `verification.succeeded`, `identity_profile.approved`, `identity_profile.rejected`, `identity_profile.needs_action`, `identity_profile.updated`, `payout_account.status_updated`, `payment.authorized`, `payment.canceled`, `resolution_center_case.created`, `resolution_center_case.updated`, `resolution_center_case.decided`, `product.created`, `product.updated`, `product.deleted`, `product.published`, `product.unpublished`, `plan.created`, `plan.updated`, `plan.deleted`, `shipment.created`, `shipment.updated`, `member.created`, `member.updated`, `ad_campaign.created`, `ad_campaign.payment_failed`, `ad_campaign.updated`, `ad_campaign.events`, `ad_campaign.deleted`, `ad_campaign.claim_ended`, `ad_group.created`, `ad_group.updated`, `ad_group.deleted`, `ad.created`, `ad.updated`, `ad.deleted`, `app_installation.created`, `app_installation.deleted`, `events.created`, `chat.message.created`, `chat.reaction.created`, `payment.created`, `payment.succeeded`, `payment.failed`, `payment.pending`, `payment.requires_action`, `dispute.created`, `dispute.updated`, `refund.created`, `refund.updated`, `dispute_alert.created`, `membership.cancel_at_period_end_changed`, `membership.went_valid`, `membership.went_invalid`, `membership.metadata_updated`, `resolution.created`, `resolution.updated`, `resolution.decided`, `payment.affiliate_reward_created`, `membership.experience_claimed`, `app_membership.went_valid`, `app_membership.went_invalid`, `app_payment.created`, `app_payment.succeeded`, `app_payment.failed`, `app_payment.pending`, `app_payment.requires_action`, `app_membership.cancel_at_period_end_changed`, `shipping_address.changed`

**failing\_since** `string | null` *required*

When the current failure streak began, as an ISO 8601 timestamp. Unlike
`last_failure_at`, this is set on the streak's first failed attempt, so it
shows an endpoint that is failing right now. `null` when the endpoint is
healthy.

**last\_failure\_at** `string | null` *required*

When a delivery to this endpoint most recently failed after exhausting
retries, as an ISO 8601 timestamp. `null` if no delivery has ever failed.

**resource\_id** `string` *required*

ID of the resource (account or app) this webhook is attached to.

**testable\_events** `string[]` *required*

The subset of subscribed event types that support sending test payloads, in dot form.

Available options: `account.updated`, `account.financing_approved`, `account.financing_denied`, `account.financing_disabled`, `invoice.created`, `invoice.marked_uncollectible`, `invoice.paid`, `invoice.past_due`, `invoice.voided`, `membership.activated`, `membership.deactivated`, `membership.trial_ending_soon`, `membership.updated`, `entry.created`, `entry.approved`, `entry.denied`, `entry.deleted`, `export.completed`, `export.failed`, `setup_intent.requires_action`, `setup_intent.succeeded`, `setup_intent.canceled`, `ledger_account.funds_available`, `swap.completed`, `deposit.succeeded`, `financial_activity.funds_available`, `transfer.created`, `transfer.completed`, `transfer.failed`, `withdrawal.created`, `withdrawal.updated`, `withdrawal.reversed`, `payout.created`, `payout.updated`, `payout.reversed`, `card_transaction.created`, `card_transaction.updated`, `card_transaction.completed`, `card_transaction.declined`, `card_transaction.reversed`, `card.created`, `card.updated`, `card.frozen`, `card.canceled`, `card_application.created`, `card_application.updated`, `card_application.approved`, `card_application.denied`, `course_lesson_interaction.completed`, `domain.created`, `domain.updated`, `domain.deleted`, `capital.created`, `capital.updated`, `payout_method.created`, `verification.succeeded`, `identity_profile.approved`, `identity_profile.rejected`, `identity_profile.needs_action`, `identity_profile.updated`, `payout_account.status_updated`, `payment.authorized`, `payment.canceled`, `resolution_center_case.created`, `resolution_center_case.updated`, `resolution_center_case.decided`, `product.created`, `product.updated`, `product.deleted`, `product.published`, `product.unpublished`, `plan.created`, `plan.updated`, `plan.deleted`, `shipment.created`, `shipment.updated`, `member.created`, `member.updated`, `ad_campaign.created`, `ad_campaign.payment_failed`, `ad_campaign.updated`, `ad_campaign.events`, `ad_campaign.deleted`, `ad_campaign.claim_ended`, `ad_group.created`, `ad_group.updated`, `ad_group.deleted`, `ad.created`, `ad.updated`, `ad.deleted`, `app_installation.created`, `app_installation.deleted`, `events.created`, `chat.message.created`, `chat.reaction.created`, `payment.created`, `payment.succeeded`, `payment.failed`, `payment.pending`, `payment.requires_action`, `dispute.created`, `dispute.updated`, `refund.created`, `refund.updated`, `dispute_alert.created`, `membership.cancel_at_period_end_changed`, `membership.went_valid`, `membership.went_invalid`, `membership.metadata_updated`, `resolution.created`, `resolution.updated`, `resolution.decided`, `payment.affiliate_reward_created`, `membership.experience_claimed`, `app_membership.went_valid`, `app_membership.went_invalid`, `app_payment.created`, `app_payment.succeeded`, `app_payment.failed`, `app_payment.pending`, `app_payment.requires_action`, `app_membership.cancel_at_period_end_changed`, `shipping_address.changed`

**title** `string | null` *required*

A label for this webhook endpoint, such as `Production`, shown in the
dashboard. `null` if no title was set.

**url** `string` *required*

Destination URL where webhook payloads are delivered via HTTP POST.

**webhook\_secret** `string | null` *required*

Secret key used to sign webhook payloads. Use it to verify the HMAC signature
of each delivery. Returned in the create response. `null` on later reads by
API keys and OAuth apps.

```json Webhook theme={null}
{
	"id": "hook_xxxxxxxxxxxxxx",
	"resource_id": "biz_xxxxxxxxxxxxxx",
	"title": "Production",
	"url": "https://shinetime.example/webhooks/whop",
	"enabled": true,
	"api_version": "v1",
	"api_version_date": "2026-09-01",
	"events": ["payment.succeeded", "membership.activated"],
	"child_resource_events": false,
	"testable_events": ["payment.succeeded", "membership.activated"],
	"consecutive_failures": 0,
	"failing_since": null,
	"last_failure_at": "2026-08-28T03:12:45.000Z",
	"disabled_at": null,
	"disabled_reason": null,
	"created_at": "2026-08-02T18:20:00.000Z",
	"webhook_secret": null
}
```


## Related topics

- [Webhooks](https://docs.whop.com/developer/guides/webhooks.md)
- [API Versions](https://docs.whop.com/developer/api/versioning.md)
- [Identity profile updated](https://docs.whop.com/api-reference/verifications/identity-profile-updated.md)
