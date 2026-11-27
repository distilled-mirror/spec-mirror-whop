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

# Deposits API

> Deposits describe the ways to add funds to an account or user balance: a hosted deposit page, bank transfer instructions, and crypto wallet addresses.

Use the Deposits API to get deposit instructions for an account or user, such as a crypto address to send funds to or bank details for a transfer.

## Endpoints

| Endpoint | Request |
| - | - |
| [Create Deposit](https://docs.whop.com/api-reference/deposits/create-deposit.md) | POST `/deposits` |
| [Reverse Deposit](https://docs.whop.com/api-reference/deposits/reverse-deposit.md) | POST `/deposits/{id}/reverse` |

## Attributes

**account\_id** `string | null` *required*

Account ID of the destination owner.

**amount** `string`

Requested deposit amount.

**hosted\_url** `string | null` *required*

URL of the hosted deposit page. Only present for business destinations.

**methods** `object` *required*

Available deposit methods for destination.

> [accordion] Properties
>
> **bank** `object | null` *required*
>
> Bank deposit details. Only present when bank deposits are active for the destination account.
>
> > [accordion] Properties
> >
> > **currencies** `object[]` *required*
> >
> > Bank transfer currencies available for this deposit.
> >
> > > [accordion] Properties
> > >
> > > **account\_number** `string | null` *required*
> > >
> > > Bank account number for deposits in this currency.
> > >
> > > **currency** `string` *required*
> > >
> > > Currency supported by these bank instructions.
> > >
> > > **deposit\_bank\_address** `string | null` *required*
> > >
> > > Receiving bank address.
> > >
> > > **deposit\_bank\_name** `string | null` *required*
> > >
> > > Receiving bank name.
> > >
> > > **deposit\_beneficiary\_name** `string | null` *required*
> > >
> > > Beneficiary name to use for transfer.
> > >
> > > **deposit\_reference** `string | null` *required*
> > >
> > > Reference to include with bank transfer.
> > >
> > > **rails** `string[]` *required*
> > >
> > > Active deposit rails for this currency.
> > >
> > > Available options: `ach`, `wire`, `sepa`, `fps`, `chaps`, `swift`
> > >
> > > **routing\_number** `string | null` *required*
> > >
> > > Bank routing number for deposits in this currency.
> > >
> > > **swift\_bic** `string | null` *required*
> > >
> > > SWIFT/BIC of the intermediary bank the sending bank routes an international wire through. Null unless `origin_bic` is provided and an intermediary is published for that bank and currency.
>
> **card** `object | null` *required*
>
> The charge to a saved payment method. Null unless `payment_method_id` was passed.
>
> > [accordion] Properties
> >
> > **metadata** `object | null` *required*
> >
> > The custom metadata attached to the payment the charge created. Null if none
> > was passed.
> >
> > **payment\_id** `string` *required*
> >
> > The payment the charge created, prefixed `pay_`. Retrieve it, or listen for
> > `payment.succeeded`, to confirm the funds landed.
> >
> > **payment\_method\_id** `string` *required*
> >
> > The saved payment method that was charged, prefixed `payt_`.
>
> **crypto** `object[]` *required*
>
> Crypto networks available for this deposit, each with its on-chain deposit address and the tokens accepted on that network. Crypto deposits require a \$10 minimum.
>
> > [accordion] Properties
> >
> > **deposit\_address** `string | null` *required*
> >
> > Address to send funds to on this network. Null when the provider has not
> > issued one yet.
> >
> > **icon\_url** `string | null` *required*
> >
> > Network icon URL.
> >
> > **name** `string` *required*
> >
> > Network display name.
> >
> > Available options: `Ethereum`, `Solana`, `Bitcoin`, `Tron`, `Base`, `BNB Smart Chain`, `Hyperliquid`, `Hypercore`, `MegaETH`, `Polygon`, `Plasma`, `Arbitrum`
> >
> > **supported\_currencies** `object[]` *required*
> >
> > Tokens accepted for deposit on this network.
> >
> > > [accordion] Properties
> > >
> > > **icon\_url** `string | null` *required*
> > >
> > > Token icon URL. Null when no icon is available.
> > >
> > > **name** `string` *required*
> > >
> > > Token symbol.
> > >
> > > Available options: `ARB`, `BNB`, `BTC`, `ETH`, `EURC`, `HYPE`, `PYUSD`, `SOL`, `TRX`, `USD1`, `USDC`, `USDC.e`, `USDG`, `USDT`, `USDT0`, `USDe`, `USDm`, `XO`, `XPL`, `pUSD`, `wETH`

**object** `string` *required*

```json Deposit theme={null}
{
	"account_id": "biz_xxxxxxxxxxxxxx",
	"amount": "250.00",
	"hosted_url": "https://whop.com/deposit/example/?amount=250.00",
	"methods": {
		"bank": {
			"currencies": [
				{
					"account_number": "123456789012",
					"currency": "usd",
					"deposit_bank_address": "885 Example Road, Teaneck, NJ 07666",
					"deposit_bank_name": "Example Bank",
					"deposit_beneficiary_name": "Shine Time Auto Detailing LLC",
					"deposit_reference": "WHOPDEP123",
					"rails": ["ach", "wire"],
					"routing_number": "021000021",
					"swift_bic": "EXAMUS33XXX"
				}
			]
		},
		"card": null,
		"crypto": [
			{
				"deposit_address": "0x1234abcd5678ef901234abcd5678ef901234abcd",
				"icon_url": "https://whop.com/_static/images/crypto/ethereum.svg",
				"name": "Ethereum",
				"supported_currencies": [
					{
						"icon_url": "https://whop.com/_static/images/crypto/usdc.svg",
						"name": "USDC"
					}
				]
			}
		]
	},
	"object": "deposit"
}
```


## Related topics

- [Deposit succeeded](https://docs.whop.com/api-reference/deposits/deposit-succeeded.md)
- [Create Deposit](https://docs.whop.com/api-reference/deposits/create-deposit.md)
- [Reverse Deposit](https://docs.whop.com/api-reference/deposits/reverse-deposit.md)
