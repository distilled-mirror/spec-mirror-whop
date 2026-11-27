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

# Trades API

> A Trade is one request on an account's or user's Whop-managed wallet.

An `order` on a perpetual trades from the trading balance, bridging the USDT0 it needs from the wallet first when the balance is short; an `order` on spot swaps between USDT0 and the asset in the wallet. A `close` closes the whole position in one market; when the account is then flat, a `withdraw` trade sends the trading balance back to the wallet. A `deposit` or `withdraw` moves money between the wallet and the trading balance. Perpetuals trade on Hyperliquid.

Use the Trades API to list the markets you can trade, place orders, close positions and move funds, and follow each trade until it finishes. Trades run in the background; an order that bridged money in and does not fill sends that money back to the wallet, and `funds_location` says where the money is.

The trading API, including `include_trading` on accounts and users, is in beta. It can change without a new API version date.

## Endpoints

| Endpoint | Request |
| - | - |
| [List Trades](https://docs.whop.com/api-reference/trades/list-trades.md) | GET `/trades` |
| [Retrieve Trade](https://docs.whop.com/api-reference/trades/retrieve-trade.md) | GET `/trades/{id}` |
| [List Candles](https://docs.whop.com/api-reference/trades/list-candles.md) | GET `/trades/candles` |
| [List Markets](https://docs.whop.com/api-reference/trades/list-markets.md) | GET `/trades/markets` |
| [Create Trade](https://docs.whop.com/api-reference/trades/create-trade.md) | POST `/trades` |
| [Cancel Trade](https://docs.whop.com/api-reference/trades/cancel-trade.md) | POST `/trades/{id}/cancel` |

## Markets

[List Markets](https://docs.whop.com/api-reference/trades/list-markets.md) returns every market you can trade on Whop.

* **Spot:** listed by symbol: `BTC`, `SOL` and `XAUT` (gold).
* **Perpetuals:** such as `BTC`, and stock and commodity perpetuals from Hyperliquid's `xyz` exchange, such as `xyz:TSLA`. Send the `id` as `market` when you create a trade.

The same symbol can appear twice, once as a perpetual and once as spot, so use `id` together with `instrument_type` to tell them apart. Results are sorted by `id`, then `instrument_type`.

This endpoint is public: no API key is needed, and each IP address can make up to 60 requests a minute.

A perpetual order's `size` can have at most `size_decimals` decimals, and its `leverage` can be at most `max_leverage`, which is `null` for a spot market.

```json theme={null}
[
	{
		"id": "BTC",
		"instrument_type": "perpetual",
		"max_leverage": 40,
		"size_decimals": 5,
		"price": {
			"currency": "usd",
			"amount": "80155.500000",
			"decimals": 6,
			"display_decimals": 6
		}
	},
	{
		"id": "BTC",
		"instrument_type": "spot",
		"max_leverage": null,
		"size_decimals": 5,
		"price": {
			"currency": "usd",
			"amount": "80065.50000000",
			"decimals": 8,
			"display_decimals": 8
		}
	}
]
```

The market list is in beta like the rest of the trading API, and can change without a new API version date.

## Attributes

**id** `string` *required*

Trade ID, prefixed `tint_`.

**account\_id** `string | null` *required*

The account that owns the trading wallet, prefixed `biz_`. `null` when a user
owns it.

**amount** `string | null` *required*

The USDT0 that a deposit, a spot buy, or a perpetual order short of trading
balance, sends from the wallet, or the USDC that a withdraw sends back, in
native units, as a decimal string. A withdraw of `all` shows what it sent once
it finishes. `null` otherwise.

**average\_price** `string | null` *required*

The average fill price of the order, in USD, as a decimal string. For spot,
the price the swap was quoted at. `null` when it is not known.

**client\_order\_id** `string | null` *required*

The caller's own ID for the trade, unique for the account. `null` when none
was sent.

**completed\_at** `string | null` *required*

When the trade ended as completed or failed, as an ISO 8601 timestamp. `null`
before that.

**created\_at** `string` *required*

When the trade was created, as an ISO 8601 timestamp.

**external\_order\_id** `string | null` *required*

ID Whop sends to the exchange with this trade's order, so the exchange's order
updates can be matched to the trade. On Hyperliquid, the order's `cloid`
reported in `orderUpdates`, a `0x`-prefixed 32-character hex string. Not the
caller's `client_order_id`. `null` for a deposit or withdraw.

**failure\_code** `string | null` *required*

Why the trade failed, or `null`.

Available options: `market_unavailable`, `leverage_too_high`, `amount_too_small`, `isolated_position_open`, `trading_paused`, `funding_failed`, `margin_unavailable`, `builder_fee_unapproved`, `leverage_rejected`, `leverage_unconfirmed`, `order_rejected`, `close_rejected`, `return_failed`, `withdraw_failed`

**filled\_size** `string | null` *required*

The size the order filled, in units of the market, as a decimal string. For a
spot buy, the amount the swap was quoted to deliver. `null` until it fills.

**funds\_location** `string | null` *required*

Where the money is when the trade did not complete as asked: `wallet`, `trading_account`, or `unknown` while a person checks it. `null` otherwise.

Available options: `wallet`, `trading_account`, `unknown`

**instrument\_type** `string | null` *required*

`perpetual` or `spot` for an order, `perpetual` for a close. `null` for a deposit or withdraw.

Available options: `perpetual`, `spot`

**leverage** `integer | null` *required*

The cross leverage set before an opening perpetual order. `null` for a spot
order, close, deposit, withdraw or reduce-only order.

**market** `string | null` *required*

The market, for example `BTC` for a perpetual or `XAUT` for spot. `null` for a
deposit or withdraw.

**metadata** `object` *required*

Free-form string-to-string annotations stored on the trade.

**object** `string` *required*

**order\_type** `string | null` *required*

`market`, `limit`, `take_profit` or `stop_loss` for an order. `null` otherwise.

Available options: `market`, `limit`, `take_profit`, `stop_loss`

**parent\_id** `string | null` *required*

For a take-profit or stop-loss sent with an order, that order's trade,
prefixed `tint_`. `null` otherwise.

**price** `object | null` *required*

The limit price of the order in USD. A market order's price is the mid price moved by `slippage_bps`; a take-profit or stop-loss's is the worst price its market order may fill at once triggered. `null` for other types.

> [accordion] Properties
>
> **amount** `string` *required*
>
> The amount in major units, as an exact decimal string. For example, `"10.00"`
> is ten dollars. It is a string so that no float rounds it in transit.
>
> **currency** `string` *required*
>
> Three-letter ISO 4217 currency code, lowercase.
>
> **decimals** `integer` *required*
>
> How many decimal places the amount carries. This is the precision the charge
> itself runs at.
>
> **display\_decimals** `integer` *required*
>
> How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.

**reduce\_only** `boolean | null` *required*

Whether the order can only reduce a position. `null` for other types.

**side** `string | null` *required*

`buy` or `sell` for an order. `null` otherwise.

Available options: `buy`, `sell`

**size** `string | null` *required*

The order size in units of the market, as a decimal string. `null` for other
types.

**status** `string` *required*

`pending` until the trade starts. `processing` while it runs, also while an order that did not fill sends the money it bridged in back to the wallet. `completed` when it did what was asked. `failed` when it did not; `funds_location` says where the money is. `in_review` when the outcome is unknown and a person checks it.

Available options: `pending`, `processing`, `in_review`, `completed`, `failed`

**status\_detail** `string | null` *required*

`partial_fill` when the order filled only part of its size. `null` otherwise.

Available options: `partial_fill`

**step** `string | null` *required*

What a live trade is doing now: `depositing` while the wallet's USDT0 is being sent to the bridge, `landing` once sent and until it reaches the trading balance, `placing` while the order or close is with the venue, `withdrawing` while USDC heads back to the wallet, `returning` while money an unfilled order bridged in goes back. A spot order is `quoting` while the swap is priced, `swapping` while it runs onchain, and `crediting` until the new balance shows in the wallet. `null` once the trade is no longer pending or processing.

Available options: `depositing`, `landing`, `placing`, `withdrawing`, `returning`, `quoting`, `swapping`, `crediting`

**trigger\_price** `object | null` *required*

The price in USD at which a take-profit or stop-loss sends its market order. `null` for other orders and types.

> [accordion] Properties
>
> **amount** `string` *required*
>
> The amount in major units, as an exact decimal string. For example, `"10.00"`
> is ten dollars. It is a string so that no float rounds it in transit.
>
> **currency** `string` *required*
>
> Three-letter ISO 4217 currency code, lowercase.
>
> **decimals** `integer` *required*
>
> How many decimal places the amount carries. This is the precision the charge
> itself runs at.
>
> **display\_decimals** `integer` *required*
>
> How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.

**type** `string` *required*

`order` places one order: a perpetual order trades from the trading balance, a spot order swaps between USDT0 and the asset in the wallet. `deposit` bridges USDT0 from the wallet to the trading balance. `withdraw` sends USDC from the trading balance back to the wallet. `close` closes the whole position in one market with a market reduce-only order; when the account is then flat, a `withdraw` trade sends the trading balance back to the wallet.

Available options: `close`, `order`, `deposit`, `withdraw`

**updated\_at** `string` *required*

When the trade last changed, as an ISO 8601 timestamp.

**user\_id** `string | null` *required*

The user who owns the trading wallet, prefixed `user_`. `null` when an account
owns it.

```json Trade theme={null}
{
	"id": "tint_xxxxxxxxxxxxx",
	"object": "trade",
	"type": "order",
	"status": "completed",
	"status_detail": null,
	"step": null,
	"account_id": "biz_xxxxxxxxxxxxxx",
	"user_id": null,
	"instrument_type": "perpetual",
	"market": "BTC",
	"side": "buy",
	"order_type": "market",
	"size": "0.00166",
	"price": {
		"currency": "usd",
		"amount": "60600.000000",
		"decimals": 6,
		"display_decimals": 6
	},
	"trigger_price": null,
	"reduce_only": false,
	"leverage": 5,
	"amount": null,
	"parent_id": null,
	"filled_size": "0.00166",
	"average_price": "60012.0",
	"funds_location": null,
	"failure_code": null,
	"client_order_id": "my-btc-entry-1",
	"external_order_id": "0x63cabad10f072c65ad84526e0c33081b",
	"metadata": {},
	"created_at": "2026-09-23T15:04:05.000Z",
	"updated_at": "2026-09-23T15:04:09.812Z",
	"completed_at": "2026-09-23T15:04:09.812Z"
}
```


## Related topics

- [Cancel Trade](https://docs.whop.com/api-reference/trades/cancel-trade.md)
- [Create Trade](https://docs.whop.com/api-reference/trades/create-trade.md)
- [List Trades](https://docs.whop.com/api-reference/trades/list-trades.md)
