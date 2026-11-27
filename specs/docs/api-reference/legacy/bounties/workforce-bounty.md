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

# Workforce Bounty

> A privately accessible bounty.

> [!NOTE]
>
> This resource has a successor in the [Whop
> API](https://docs.whop.com/api-reference/legacy/bounties/list-bounties.md), the versioned API that new
> integrations should build on. This page stays fully supported.

```json Example theme={null}
{
	"bounty_type": "classic",
	"created_at": "2023-12-01T05:00:00.401Z",
	"currency": "usd",
	"description": "<string>",
	"id": "bnty_xxxxxxxxxxxxx",
	"status": "published",
	"title": "<string>",
	"total_available": 6.9,
	"total_paid": 6.9,
	"updated_at": "2023-12-01T05:00:00.401Z",
	"vote_threshold": 42
}
```

**bounty\_type** `BountyTypes` *required*

The underlying bounty implementation type.

Available options: `classic`, `user_funded`, `workforce`

**created\_at** `string\<date-time\>` *required*

The datetime the bounty was created.

Example: `2023-12-01T05:00:00.401Z`

**currency** `Currencies` *required*

The currency used for the bounty funds.

Available options: `usd`, `sgd`, `inr`, `aud`, `brl`, `cad`, `dkk`, `eur`, `nok`, `gbp`, `sek`, `chf`, `hkd`, `huf`, `jpy`, `mxn`, `myr`, `pln`, `czk`, `nzd`, `aed`, `eth`, `ape`, `cop`, `ron`, `thb`, `bgn`, `idr`, `dop`, `php`, `try`, `krw`, `twd`, `vnd`, `pkr`, `clp`, `uyu`, `ars`, `zar`, `dzd`, `tnd`, `mad`, `kes`, `kwd`, `jod`, `all`, `xcd`, `amd`, `bsd`, `bhd`, `bob`, `bam`, `khr`, `crc`, `xof`, `egp`, `etb`, `gmd`, `ghs`, `gtq`, `gyd`, `ils`, `jmd`, `mop`, `mga`, `mur`, `mdl`, `mnt`, `nad`, `ngn`, `mkd`, `omr`, `pyg`, `pen`, `qar`, `rwf`, `sar`, `rsd`, `lkr`, `tzs`, `ttd`, `uzs`, `rub`, `btc`, `cny`, `usdt`, `kzt`, `awg`, `whop_usd`, `xau`, `hnl`

**description** `string` *required*

The description of the bounty.

**id** `string` *required*

The unique identifier for the bounty.

Example: `bnty_xxxxxxxxxxxxx`

**status** `Statuses` *required*

The current lifecycle status of the bounty.

Available options: `published`, `archived`, `scheduled`

**title** `string` *required*

The title of the bounty.

**total\_available** `number` *required*

The total amount currently funded in the bounty pool for payout.

Example: `6.9`

**total\_paid** `number` *required*

The total amount paid out for this bounty.

Example: `6.9`

**updated\_at** `string\<date-time\>` *required*

The datetime the bounty was last updated.

Example: `2023-12-01T05:00:00.401Z`

**vote\_threshold** `integer` *required*

The number of watcher votes required before the submission can resolve.

Example: `42`


## Related topics

- [Create bounty](https://docs.whop.com/api-reference/legacy/bounties/create-bounty.md)
- [Retrieve bounty](https://docs.whop.com/api-reference/legacy/bounties/retrieve-bounty.md)
- [Create Bounty Submission](https://docs.whop.com/api-reference/bounty-submissions/create-bounty-submission.md)
