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

# Webhook

> A webhook endpoint that receives event notifications for a company via HTTP POST.

> [!NOTE]
>
> This resource has a successor in the [Whop
> API](https://docs.whop.com/api-reference/legacy/webhooks/list-webhooks.md), the versioned API that new
> integrations should build on. This page stays fully supported.

```json Example theme={null}
{
	"api_version": "v1",
	"api_version_date": "<string>",
	"child_resource_events": true,
	"created_at": "2023-12-01T05:00:00.401Z",
	"enabled": true,
	"events": ["account.updated"],
	"id": "hook_xxxxxxxxxxxxx",
	"resource_id": "<string>",
	"testable_events": ["account.updated"],
	"url": "https://example.com/path",
	"webhook_secret": "whsec_abc123def456"
}
```

**api\_version** `ApiVersion` *required*

The API version used to format payloads sent to this webhook endpoint.

Available options: `v1`, `v2`, `v5`

**api\_version\_date** `string | null` *required*

The dated API version (Api-Version-Date) that v1 payloads for this endpoint
are pinned to: events serialize exactly like a REST read at this version (the
native serializer where the resource has one). Null when unpinned — legacy
(v2/v5) webhooks, and v1 webhooks on the legacy payload shape.

**child\_resource\_events** `boolean` *required*

Whether events are sent for child resources. For example, if the webhook is on
a company, enabling this sends events only from the company's sub-merchants
(child companies).

**created\_at** `string\<date-time\>` *required*

The datetime the webhook was created.

Example: `2023-12-01T05:00:00.401Z`

**enabled** `boolean` *required*

Whether this webhook endpoint is currently active and receiving events.

**events** `array\<WebhookEvent\>` *required*

The list of event types this webhook is subscribed to.

Available options: `account.updated`, `account.financing_approved`, `account.financing_denied`, `account.financing_disabled`, `invoice.created`, `invoice.marked_uncollectible`, `invoice.paid`, `invoice.past_due`, `invoice.voided`, `membership.activated`, `membership.deactivated`, `membership.trial_ending_soon`, `membership.updated`, `entry.created`, `entry.approved`, `entry.denied`, `entry.deleted`, `export.completed`, `export.failed`, `setup_intent.requires_action`, `setup_intent.succeeded`, `setup_intent.canceled`, `ledger_account.funds_available`, `swap.completed`, `deposit.succeeded`, `financial_activity.funds_available`, `transfer.created`, `transfer.completed`, `transfer.failed`, `withdrawal.created`, `withdrawal.updated`, `withdrawal.reversed`, `payout.created`, `payout.updated`, `payout.reversed`, `card_transaction.created`, `card_transaction.updated`, `card_transaction.completed`, `card_transaction.declined`, `card_transaction.reversed`, `card.created`, `card.updated`, `card.frozen`, `card.canceled`, `card_application.created`, `card_application.updated`, `card_application.approved`, `card_application.denied`, `course_lesson_interaction.completed`, `domain.created`, `domain.updated`, `domain.deleted`, `capital.created`, `capital.updated`, `payout_method.created`, `verification.succeeded`, `identity_profile.approved`, `identity_profile.rejected`, `identity_profile.needs_action`, `identity_profile.updated`, `payout_account.status_updated`, `payment.authorized`, `payment.canceled`, `resolution_center_case.created`, `resolution_center_case.updated`, `resolution_center_case.decided`, `product.created`, `product.updated`, `product.deleted`, `product.published`, `product.unpublished`, `plan.created`, `plan.updated`, `plan.deleted`, `shipment.created`, `shipment.updated`, `member.created`, `member.updated`, `ad_campaign.created`, `ad_campaign.payment_failed`, `ad_campaign.updated`, `ad_campaign.events`, `ad_campaign.deleted`, `ad_campaign.claim_ended`, `ad_group.created`, `ad_group.updated`, `ad_group.deleted`, `ad.created`, `ad.updated`, `ad.deleted`, `app_installation.created`, `app_installation.deleted`, `events.created`, `chat.message.created`, `chat.reaction.created`, `payment.created`, `payment.succeeded`, `payment.failed`, `payment.pending`, `payment.requires_action`, `dispute.created`, `dispute.updated`, `refund.created`, `refund.updated`, `dispute_alert.created`, `membership.cancel_at_period_end_changed`, `shipping_address.changed`

**id** `string` *required*

The unique identifier for the webhook.

Example: `hook_xxxxxxxxxxxxx`

**resource\_id** `string` *required*

The ID of the resource (company or product) this webhook is attached to.

**testable\_events** `array\<WebhookEvent\>` *required*

The subset of subscribed event types that support sending test payloads.

Available options: `account.updated`, `account.financing_approved`, `account.financing_denied`, `account.financing_disabled`, `invoice.created`, `invoice.marked_uncollectible`, `invoice.paid`, `invoice.past_due`, `invoice.voided`, `membership.activated`, `membership.deactivated`, `membership.trial_ending_soon`, `membership.updated`, `entry.created`, `entry.approved`, `entry.denied`, `entry.deleted`, `export.completed`, `export.failed`, `setup_intent.requires_action`, `setup_intent.succeeded`, `setup_intent.canceled`, `ledger_account.funds_available`, `swap.completed`, `deposit.succeeded`, `financial_activity.funds_available`, `transfer.created`, `transfer.completed`, `transfer.failed`, `withdrawal.created`, `withdrawal.updated`, `withdrawal.reversed`, `payout.created`, `payout.updated`, `payout.reversed`, `card_transaction.created`, `card_transaction.updated`, `card_transaction.completed`, `card_transaction.declined`, `card_transaction.reversed`, `card.created`, `card.updated`, `card.frozen`, `card.canceled`, `card_application.created`, `card_application.updated`, `card_application.approved`, `card_application.denied`, `course_lesson_interaction.completed`, `domain.created`, `domain.updated`, `domain.deleted`, `capital.created`, `capital.updated`, `payout_method.created`, `verification.succeeded`, `identity_profile.approved`, `identity_profile.rejected`, `identity_profile.needs_action`, `identity_profile.updated`, `payout_account.status_updated`, `payment.authorized`, `payment.canceled`, `resolution_center_case.created`, `resolution_center_case.updated`, `resolution_center_case.decided`, `product.created`, `product.updated`, `product.deleted`, `product.published`, `product.unpublished`, `plan.created`, `plan.updated`, `plan.deleted`, `shipment.created`, `shipment.updated`, `member.created`, `member.updated`, `ad_campaign.created`, `ad_campaign.payment_failed`, `ad_campaign.updated`, `ad_campaign.events`, `ad_campaign.deleted`, `ad_campaign.claim_ended`, `ad_group.created`, `ad_group.updated`, `ad_group.deleted`, `ad.created`, `ad.updated`, `ad.deleted`, `app_installation.created`, `app_installation.deleted`, `events.created`, `chat.message.created`, `chat.reaction.created`, `payment.created`, `payment.succeeded`, `payment.failed`, `payment.pending`, `payment.requires_action`, `dispute.created`, `dispute.updated`, `refund.created`, `refund.updated`, `dispute_alert.created`, `membership.cancel_at_period_end_changed`, `shipping_address.changed`

**url** `string` *required*

The destination URL where webhook payloads are delivered via HTTP POST.

Example: `https://example.com/path`

**webhook\_secret** `string` *required*

The secret key used to sign webhook payloads for verification. Include this in your HMAC validation logic. Returned on the create response and to interactive dashboard sessions; empty for API-key and OAuth callers on later reads.

Example: `whsec_abc123def456`


## Related topics

- [Webhooks](https://docs.whop.com/developer/guides/webhooks.md)
- [Webhooks API](https://docs.whop.com/api-reference/webhooks/webhook.md)
- [Delete Webhook](https://docs.whop.com/api-reference/webhooks/delete-webhook.md)
