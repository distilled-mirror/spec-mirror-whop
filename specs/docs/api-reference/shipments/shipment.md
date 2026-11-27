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

# Shipments API

> A Shipment attaches a carrier tracking number to a payment and follows the package from label creation to delivery, exposing the current delivery status and a customer-facing tracking URL.

Use the Shipments API to list an account's shipments, retrieve one by its ID or the payment it fulfills, attach a tracking number to a payment, and update the tracking number on an existing shipment.

## Endpoints

| Endpoint | Request |
| - | - |
| [List Shipments](https://docs.whop.com/api-reference/shipments/list-shipments.md) | GET `/shipments` |
| [Retrieve Shipment](https://docs.whop.com/api-reference/shipments/retrieve-shipment.md) | GET `/shipments/{id}` |
| [Create Shipment](https://docs.whop.com/api-reference/shipments/create-shipment.md) | POST `/shipments` |
| [Look Up Shipment](https://docs.whop.com/api-reference/shipments/look-up-shipment.md) | POST `/shipments/lookup` |
| [Update Shipment](https://docs.whop.com/api-reference/shipments/update-shipment.md) | PATCH `/shipments/{id}` |

## Attributes

**id** `string` *required*

Shipment ID, prefixed `ship_`.

**account\_id** `string` *required*

The account that owns this shipment, prefixed `biz_`.

**carrier** `string | null` *required*

The shipping carrier detected for this shipment. Null until a tracking update
identifies it.

**checkpoints** `object[]` *required*

Carrier scan history for this shipment, oldest scan first. Empty until the carrier reports its first scan.

> [accordion] Properties
>
> **location** `string | null` *required*
>
> Where the carrier recorded the scan, such as `PHILADELPHIA, PA`. Null when the
> carrier sent none.
>
> **message** `string | null` *required*
>
> Carrier's description of the scan, such as `Departed USPS Regional Facility`.
> Null when the carrier sent none.
>
> **status** `string` *required*
>
> Delivery status this carrier scan maps to.
>
> Available options: `unknown`, `pre_transit`, `in_transit`, `out_for_delivery`, `delivered`, `available_for_pickup`, `return_to_sender`, `failure`, `cancelled`, `error`
>
> **timestamp** `string | null` *required*
>
> When the carrier recorded the scan, as an ISO 8601 timestamp. Null when the carrier sent no scan time.

**created\_at** `string` *required*

The datetime the shipment was created (ISO 8601).

**payment\_id** `string` *required*

The payment this shipment fulfills, prefixed `pay_`.

**status** `string` *required*

The current delivery status of this shipment.

Available options: `unknown`, `pre_transit`, `in_transit`, `out_for_delivery`, `delivered`, `available_for_pickup`, `return_to_sender`, `failure`, `cancelled`, `error`

**tracking\_number** `string` *required*

The carrier-assigned tracking number used to look up shipment progress.

**tracking\_url** `string` *required*

A customer-facing URL to track this shipment's progress.

**updated\_at** `string` *required*

The datetime the shipment was last updated (ISO 8601).

```json Shipment theme={null}
{
	"id": "ship_xxxxxxxxxxxxxx",
	"account_id": "biz_xxxxxxxxxxxxxx",
	"payment_id": "pay_xxxxxxxxxxxxxx",
	"carrier": "ups",
	"tracking_number": "1Z999AA10123456784",
	"tracking_url": "https://track.aftership.com/xxxxxxxxxxxxxxxxxx",
	"status": "in_transit",
	"checkpoints": [
		{
			"status": "in_transit",
			"message": "Picked up",
			"location": "PHILADELPHIA, PA",
			"timestamp": "2026-09-15T16:42:00.000Z"
		}
	],
	"created_at": "2026-09-15T14:30:00.000Z",
	"updated_at": "2026-09-15T16:45:10.000Z"
}
```


## Related topics

- [Shipment](https://docs.whop.com/api-reference/legacy/shipments/shipment.md)
- [API Versions](https://docs.whop.com/developer/api/versioning.md)
- [Create shipment](https://docs.whop.com/api-reference/legacy/shipments/create-shipment.md)
