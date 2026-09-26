> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Trades

A Trade records an order batch, cancellation, or leverage change submitted to a trading provider from an account or user's Whop-managed wallet. Its `status` tracks the submission, not whether orders filled.

Use the Trades API to place limit or market orders with optional take-profit and stop-loss protection, cancel a submitted batch, set leverage, and list or retrieve past submissions. Read live margin, positions, and open orders by passing `include_trading=true` to Retrieve Account or Retrieve User with `id=me`. Whop's builder fee is added to each order. Hyperliquid perpetuals are currently supported; email [support@whop.com](mailto:support@whop.com) to request access.

## Endpoints

| Endpoint                                                                  | Request                                                                  |
| ------------------------------------------------------------------------- | ------------------------------------------------------------------------ |
| [List Trades](/api-reference/beta/trades/list-trades)                     | <Badge color="blue" size="sm" stroke>GET</Badge> `/trades`               |
| [Retrieve Trade](/api-reference/beta/trades/retrieve-trade)               | <Badge color="blue" size="sm" stroke>GET</Badge> `/trades/{id}`          |
| [Create Trade](/api-reference/beta/trades/create-trade)                   | <Badge color="green" size="sm" stroke>POST</Badge> `/trades`             |
| [Cancel Trade](/api-reference/beta/trades/cancel-trade)                   | <Badge color="green" size="sm" stroke>POST</Badge> `/trades/{id}/cancel` |
| [Update Trade Leverage](/api-reference/beta/trades/update-trade-leverage) | <Badge color="green" size="sm" stroke>POST</Badge> `/trades/leverage`    |

## Attributes

<Columns cols={2}>
  <Column>
    <ResponseField name="id" type="string" required>
      Trade ID, prefixed `trop_`.
    </ResponseField>

    <ResponseField name="account_id" type="string | null" required>
      The account that owns the trading wallet, prefixed `biz_`. `null` when a user
      owns it.
    </ResponseField>

    <ResponseField name="cancellations" type="object[] | null" required>
      Cancellation results for `cancel_orders` trades, or `null` for other trades or before completion.

      <Accordion title="Properties" defaultOpen={true}>
        <ResponseField name="id" type="string" required>
          Client order ID of the order the cancellation targeted, prefixed `trdcloid_`.
        </ResponseField>

        <ResponseField name="error" type="string | null" required>
          The provider's rejection reason, or `null` when the order was canceled.
        </ResponseField>

        <ResponseField name="status" type="string" required>
          `canceled` when the provider canceled the order; `rejected` when it refused, for example because the order had already filled.

          Available options: `canceled`, `rejected`
        </ResponseField>
      </Accordion>
    </ResponseField>

    <ResponseField name="completed_at" type="string | null" required>
      When the submission finished, as an ISO 8601 timestamp, or `null` while it is
      pending or its outcome is unknown.
    </ResponseField>

    <ResponseField name="created_at" type="string" required>
      When the trade was submitted, as an ISO 8601 timestamp.
    </ResponseField>

    <ResponseField name="failure_code" type="string | null" required>
      Why the trade failed or has an unknown outcome, or `null` otherwise.

      Available options: `pre_submission_error`, `provider_rejected`, `provider_outcome_unknown`
    </ResponseField>

    <ResponseField name="hyperliquid" type="object | null" required>
      Hyperliquid-specific submission details. Present when `provider` is `hyperliquid`, otherwise `null`.

      <Accordion title="Properties" defaultOpen={true}>
        <ResponseField name="builder_fee_bps" type="string | null" required>
          Builder fee Whop charged on the submitted orders, in basis points as a decimal string, or `null` for trades that place no orders.
        </ResponseField>
      </Accordion>
    </ResponseField>

    <ResponseField name="instrument_type" type="string" required>
      The kind of instrument traded.

      Available options: `perpetual`
    </ResponseField>

    <ResponseField name="leverage" type="object | null" required>
      The leverage requested by an `update_leverage` trade. `null` for other trades.

      <Accordion title="Properties" defaultOpen={true}>
        <ResponseField name="leverage" type="integer | null" required>
          Requested leverage multiplier, such as `10` for 10x, or `null` if the request
          didn't contain a whole number.
        </ResponseField>

        <ResponseField name="margin_mode" type="string | null" required>
          `cross` shares margin across positions; `isolated` limits margin to this market's position. `null` if the request didn't contain a supported mode.

          Available options: `cross`, `isolated`
        </ResponseField>

        <ResponseField name="market" type="string" required>
          Market symbol, such as `ETH`.
        </ResponseField>
      </Accordion>
    </ResponseField>

    <ResponseField name="metadata" type="object" required>
      String-to-string annotations you provided when submitting the trade.
    </ResponseField>

    <ResponseField name="object" type="string" required />

    <ResponseField name="operation_type" type="string" required>
      `create_orders` places orders, `cancel_orders` cancels a submitted batch, and `update_leverage` sets a market's leverage.

      Available options: `create_orders`, `cancel_orders`, `update_leverage`
    </ResponseField>

    <ResponseField name="orders" type="trading_order[] | null" required>
      Order acknowledgements recorded at submission for `create_orders` trades, or `null` for other trades or before completion. They don't update as orders fill; read live orders and positions from the account's `trading` field.

      <Accordion title="Properties" defaultOpen={true}>
        <ResponseField name="id" type="string" required>
          Trading order ID, prefixed `trdord_` once the provider assigns one, otherwise
          the client order ID prefixed `trdcloid_`.
        </ResponseField>

        <ResponseField name="average_price" type="object | null" required>
          Average fill price in USD for an immediate fill, or `null` when nothing filled.

          <Accordion title="Properties" defaultOpen={true}>
            <ResponseField name="amount" type="string" required>
              The amount in major units, as an exact decimal string — `"10.00"` is ten
              dollars. A string so no float rounds it in transit.
            </ResponseField>

            <ResponseField name="currency" type="string" required>
              Three-letter ISO 4217 currency code, lowercase.
            </ResponseField>

            <ResponseField name="decimals" type="integer" required>
              How many decimal places the amount CARRIES — the precision the charge itself
              runs at.
            </ResponseField>

            <ResponseField name="display_decimals" type="integer" required>
              How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
            </ResponseField>
          </Accordion>
        </ResponseField>

        <ResponseField name="client_order_id" type="string" required>
          Client order ID Whop assigned to the order, prefixed `trdcloid_`.
        </ResponseField>

        <ResponseField name="error" type="string | null" required>
          The provider's rejection reason, or `null` when the order was accepted.
        </ResponseField>

        <ResponseField name="filled_size" type="string | null" required>
          Size filled immediately at submission, as a decimal string, or `null` when
          nothing filled.
        </ResponseField>

        <ResponseField name="hyperliquid" type="object | null" required>
          Hyperliquid-specific order details. Present on Hyperliquid orders, otherwise `null`.

          <Accordion title="Properties" defaultOpen={true}>
            <ResponseField name="reduce_only" type="boolean | null" required>
              Whether the order can only reduce an existing position, or `null` when
              Hyperliquid omits it.
            </ResponseField>

            <ResponseField name="trigger_price" type="object | null" required>
              Trigger price in USD for take-profit and stop-loss orders, or `null` for orders without a trigger.

              <Accordion title="Properties" defaultOpen={true}>
                <ResponseField name="amount" type="string" required>
                  The amount in major units, as an exact decimal string — `"10.00"` is ten
                  dollars. A string so no float rounds it in transit.
                </ResponseField>

                <ResponseField name="currency" type="string" required>
                  Three-letter ISO 4217 currency code, lowercase.
                </ResponseField>

                <ResponseField name="decimals" type="integer" required>
                  How many decimal places the amount CARRIES — the precision the charge itself
                  runs at.
                </ResponseField>

                <ResponseField name="display_decimals" type="integer" required>
                  How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
                </ResponseField>
              </Accordion>
            </ResponseField>
          </Accordion>
        </ResponseField>

        <ResponseField name="market" type="string" required>
          Market symbol on the provider, such as `ETH`.
        </ResponseField>

        <ResponseField name="object" type="string" required />

        <ResponseField name="order_type" type="string" required>
          Available options: `limit`, `market`, `take_profit`, `stop_loss`
        </ResponseField>

        <ResponseField name="parent_client_order_id" type="string | null" required>
          For an attached take-profit or stop-loss, the client order ID of its entry
          order, prefixed `trdcloid_`. `null` for other orders.
        </ResponseField>

        <ResponseField name="price" type="object" required>
          Submitted limit price in USD. For market and trigger orders, the worst price allowed after slippage.

          <Accordion title="Properties" defaultOpen={true}>
            <ResponseField name="amount" type="string" required>
              The amount in major units, as an exact decimal string — `"10.00"` is ten
              dollars. A string so no float rounds it in transit.
            </ResponseField>

            <ResponseField name="currency" type="string" required>
              Three-letter ISO 4217 currency code, lowercase.
            </ResponseField>

            <ResponseField name="decimals" type="integer" required>
              How many decimal places the amount CARRIES — the precision the charge itself
              runs at.
            </ResponseField>

            <ResponseField name="display_decimals" type="integer" required>
              How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
            </ResponseField>
          </Accordion>
        </ResponseField>

        <ResponseField name="provider_order_id" type="string | null" required>
          The provider's own order ID, or `null` until the provider assigns one, such as
          for a rejected order or a take-profit or stop-loss that hasn't triggered yet.
        </ResponseField>

        <ResponseField name="side" type="string" required>
          Available options: `buy`, `sell`
        </ResponseField>

        <ResponseField name="size" type="string" required>
          Submitted size as a decimal string.
        </ResponseField>

        <ResponseField name="status" type="string" required>
          The provider's acknowledgement at submission time, not the current fill status.

          Available options: `open`, `filled`, `rejected`
        </ResponseField>
      </Accordion>
    </ResponseField>

    <ResponseField name="provider" type="string" required>
      Trading venue the trade was submitted to.

      Available options: `hyperliquid`
    </ResponseField>

    <ResponseField name="requested_orders" type="object[] | null" required>
      The orders submitted for a `create_orders` trade, including attached take-profit and stop-loss, with the client order IDs Whop assigned. Present in every status, so an unknown outcome can be reconciled by `client_order_id`. `null` for other trades.

      <Accordion title="Properties" defaultOpen={true}>
        <ResponseField name="client_order_id" type="string" required>
          Client order ID Whop assigned to the order, prefixed `trdcloid_`. Matches the
          order in `orders` and on the provider.
        </ResponseField>

        <ResponseField name="market" type="string" required>
          Market symbol as submitted, such as `ETH`.
        </ResponseField>

        <ResponseField name="order_type" type="string | null" required>
          Submitted order type, or `null` if the request didn't contain a supported type.

          Available options: `limit`, `market`, `take_profit`, `stop_loss`
        </ResponseField>

        <ResponseField name="parent_client_order_id" type="string | null" required>
          For an attached take-profit or stop-loss, the client order ID of its entry
          order, prefixed `trdcloid_`. `null` for other orders.
        </ResponseField>

        <ResponseField name="price" type="object | null" required>
          Submitted limit price in USD, or `null` for orders submitted without one.

          <Accordion title="Properties" defaultOpen={true}>
            <ResponseField name="amount" type="string" required>
              The amount in major units, as an exact decimal string — `"10.00"` is ten
              dollars. A string so no float rounds it in transit.
            </ResponseField>

            <ResponseField name="currency" type="string" required>
              Three-letter ISO 4217 currency code, lowercase.
            </ResponseField>

            <ResponseField name="decimals" type="integer" required>
              How many decimal places the amount CARRIES — the precision the charge itself
              runs at.
            </ResponseField>

            <ResponseField name="display_decimals" type="integer" required>
              How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
            </ResponseField>
          </Accordion>
        </ResponseField>

        <ResponseField name="side" type="string | null" required>
          Submitted side, or `null` if the request didn't contain a supported side.

          Available options: `buy`, `sell`
        </ResponseField>

        <ResponseField name="size" type="string" required>
          Submitted size as a decimal string.
        </ResponseField>

        <ResponseField name="trigger_price" type="object | null" required>
          Submitted trigger price in USD, or `null` for orders without a trigger.

          <Accordion title="Properties" defaultOpen={true}>
            <ResponseField name="amount" type="string" required>
              The amount in major units, as an exact decimal string — `"10.00"` is ten
              dollars. A string so no float rounds it in transit.
            </ResponseField>

            <ResponseField name="currency" type="string" required>
              Three-letter ISO 4217 currency code, lowercase.
            </ResponseField>

            <ResponseField name="decimals" type="integer" required>
              How many decimal places the amount CARRIES — the precision the charge itself
              runs at.
            </ResponseField>

            <ResponseField name="display_decimals" type="integer" required>
              How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
            </ResponseField>
          </Accordion>
        </ResponseField>
      </Accordion>
    </ResponseField>

    <ResponseField name="status" type="string" required>
      Submission status, not fill status. `completed` means the provider response was recorded, even if individual orders were rejected. Never resubmit a `submission_unknown` trade with a new idempotency key.

      Available options: `pending`, `submitted`, `completed`, `failed`, `submission_unknown`
    </ResponseField>

    <ResponseField name="trade_id" type="string | null" required>
      For a cancellation, the ID of the canceled trade, prefixed `trop_`. `null`
      otherwise.
    </ResponseField>

    <ResponseField name="updated_at" type="string" required>
      When the trade last changed, as an ISO 8601 timestamp.
    </ResponseField>

    <ResponseField name="user_id" type="string | null" required>
      The user who owns the trading wallet, prefixed `user_`. `null` when an account
      owns it.
    </ResponseField>
  </Column>

  <Column>
    <div className="api-resource-sticky-example">
      ```json Trade theme={null}
      {
      	"id": "trop_xxxxxxxxxxxxx",
      	"object": "trade",
      	"instrument_type": "perpetual",
      	"account_id": "biz_xxxxxxxxxxxxxx",
      	"user_id": null,
      	"provider": "hyperliquid",
      	"operation_type": "create_orders",
      	"status": "completed",
      	"metadata": {
      		"strategy": "eth-breakout"
      	},
      	"trade_id": null,
      	"created_at": "2026-09-23T15:04:05.000Z",
      	"updated_at": "2026-09-23T15:04:05.812Z",
      	"completed_at": "2026-09-23T15:04:05.812Z",
      	"failure_code": null,
      	"requested_orders": [
      		{
      			"client_order_id": "trdcloid_9f2c4e1a7b3d5f60a1b2c3d4e5f60718",
      			"parent_client_order_id": null,
      			"market": "ETH",
      			"side": "buy",
      			"size": "0.5",
      			"order_type": "limit",
      			"price": {
      				"currency": "usd",
      				"amount": "3000.00",
      				"decimals": 2,
      				"display_decimals": 2
      			},
      			"trigger_price": null
      		},
      		{
      			"client_order_id": "trdcloid_1a2b3c4d5e6f708192a3b4c5d6e7f801",
      			"parent_client_order_id": "trdcloid_9f2c4e1a7b3d5f60a1b2c3d4e5f60718",
      			"market": "ETH",
      			"side": "sell",
      			"size": "0.5",
      			"order_type": "take_profit",
      			"price": null,
      			"trigger_price": {
      				"currency": "usd",
      				"amount": "3300.00",
      				"decimals": 2,
      				"display_decimals": 2
      			}
      		}
      	],
      	"orders": [
      		{
      			"id": "trdord_123456789",
      			"object": "trading_order",
      			"client_order_id": "trdcloid_9f2c4e1a7b3d5f60a1b2c3d4e5f60718",
      			"provider_order_id": "123456789",
      			"parent_client_order_id": null,
      			"market": "ETH",
      			"side": "buy",
      			"size": "0.5",
      			"price": {
      				"currency": "usd",
      				"amount": "3000.0",
      				"decimals": 2,
      				"display_decimals": 2
      			},
      			"order_type": "limit",
      			"status": "open",
      			"filled_size": null,
      			"average_price": null,
      			"error": null,
      			"hyperliquid": {
      				"reduce_only": false,
      				"trigger_price": null
      			}
      		},
      		{
      			"id": "trdcloid_1a2b3c4d5e6f708192a3b4c5d6e7f801",
      			"object": "trading_order",
      			"client_order_id": "trdcloid_1a2b3c4d5e6f708192a3b4c5d6e7f801",
      			"provider_order_id": null,
      			"parent_client_order_id": "trdcloid_9f2c4e1a7b3d5f60a1b2c3d4e5f60718",
      			"market": "ETH",
      			"side": "sell",
      			"size": "0.5",
      			"price": {
      				"currency": "usd",
      				"amount": "3283.5",
      				"decimals": 2,
      				"display_decimals": 2
      			},
      			"order_type": "take_profit",
      			"status": "open",
      			"filled_size": null,
      			"average_price": null,
      			"error": null,
      			"hyperliquid": {
      				"reduce_only": true,
      				"trigger_price": {
      					"currency": "usd",
      					"amount": "3300.0",
      					"decimals": 2,
      					"display_decimals": 2
      				}
      			}
      		}
      	],
      	"cancellations": null,
      	"leverage": null,
      	"hyperliquid": {
      		"builder_fee_bps": "1"
      	}
      }
      ```
    </div>
  </Column>
</Columns>
