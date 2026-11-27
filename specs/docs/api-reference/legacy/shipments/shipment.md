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

# Shipment

> A physical shipment associated with a payment, including carrier details and tracking information.

> [!NOTE]
>
> This resource has a successor in the [Whop
> API](https://docs.whop.com/api-reference/legacy/shipments/list-shipments.md), the versioned API that
> new integrations should build on. This page stays fully supported.

```json Example theme={null}
{
	"created_at": "2023-12-01T05:00:00.401Z",
	"delivery_estimate": "2023-12-01T05:00:00.401Z",
	"id": "ship_xxxxxxxxxxxxx",
	"payment": {
		"id": "pay_xxxxxxxxxxxxxx"
	},
	"service": "Priority",
	"status": "unknown",
	"substatus": "address_correction",
	"tracking_code": "9400111899223456789012",
	"updated_at": "2023-12-01T05:00:00.401Z"
}
```

**created\_at** `string\<date-time\>` *required*

The datetime the shipment was created.

Example: `2023-12-01T05:00:00.401Z`

**delivery\_estimate** `string\<date-time\> | null` *required*

The estimated delivery date for this shipment. Null if the carrier has not provided an estimate.

Example: `2023-12-01T05:00:00.401Z`

**id** `string` *required*

The unique identifier for the shipment.

Example: `ship_xxxxxxxxxxxxx`

**payment** `object | null` *required*

The payment associated with this shipment. Null if the payment has been deleted or is inaccessible.

**child attributes**

**id** `string` *required*

The unique identifier for the payment.

Example: `pay_xxxxxxxxxxxxxx`

**service** `string | null` *required*

The shipping service level used for this shipment. Null if the carrier does not specify a service tier.

Example: `Priority`

**status** `ShipmentStatuses` *required*

The current delivery status of this shipment.

Available options: `unknown`, `pre_transit`, `in_transit`, `out_for_delivery`, `delivered`, `available_for_pickup`, `return_to_sender`, `failure`, `cancelled`, `error`

**substatus** `ShipmentSubstatuses | null` *required*

A more granular status providing additional detail about the shipment's current state. Null if no substatus applies.

Available options: `address_correction`, `arrived_at_destination`, `arrived_at_facility`, `arrived_at_pickup_location`, `awaiting_information`, `substatus_cancelled`, `damaged`, `delayed`, `delivery_exception`, `departed_facility`, `departed_origin_facility`, `expired`, `substatus_failure`, `held`, `substatus_in_transit`, `label_created`, `lost`, `missorted`, `substatus_out_for_delivery`, `received_at_destination_facility`, `received_at_origin_facility`, `refused`, `return`, `status_update`, `transferred_to_destination_carrier`, `transit_exception`, `substatus_unknown`, `weather_delay`

**tracking\_code** `string` *required*

The carrier-assigned tracking number used to look up shipment progress.

Example: `9400111899223456789012`

**updated\_at** `string\<date-time\>` *required*

The datetime the shipment was last updated.

Example: `2023-12-01T05:00:00.401Z`


## Related topics

- [Shipments API](https://docs.whop.com/api-reference/shipments/shipment.md)
- [Shipment created](https://docs.whop.com/api-reference/legacy/shipments/shipment-created.md)
- [Shipment updated](https://docs.whop.com/api-reference/legacy/shipments/shipment-updated.md)
