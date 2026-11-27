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

# Fee Markups & Subsidies

> Configure platform fee markups and payout subsidies for subaccounts, with examples, supported limits, and available balance requirements.

Fee markups let your platform adjust what subaccounts pay on top of Whop's fees. Positive markups earn your platform revenue. Negative payout markups subsidize a subaccount's withdrawal fee from your platform's balance. Whop still receives its full fee.

## Configure markups

Use your platform's API key with `company:update_child_fees` permission. [Retrieve account fees](https://docs.whop.com/api-reference/accounts/retrieve-account-fees.md) to see effective rates, defaults, and whether each markup is adjustable.

[Update account fees](https://docs.whop.com/api-reference/accounts/update-account-fees.md) with `PATCH /api/v1/accounts/{account_id}/fees`:

| Applies to | Account ID in the URL | Request section |
| - | - | - |
| Subaccounts without a custom rate | Your platform | `child_markups` |
| One subaccount | That subaccount | `markups` |

Each markup has a `percentage` and a `fixed` amount. `percentage: 1` means 1%. Set `fixed` in US dollars. Payout markups are converted to the withdrawal currency when charged.

This example sets a bank-wire payout markup of 1% minus \$0.60 for one subaccount:

```bash theme={null}
curl --request PATCH \
  --url https://api.whop.com/api/v1/accounts/biz_SUBACCOUNT_ID/fees \
  --header "Authorization: Bearer $WHOP_API_KEY" \
  --header "Content-Type: application/json" \
  --data '{
    "markups": {
      "payouts": {
        "bank_wire": { "percentage": 1, "fixed": -0.60 }
      }
    }
  }'
```

For a platform default, send the same `payouts` object under `child_markups` to your platform's account ID. Set a rate for each payout method using the keys returned by the fees endpoint.

## Choose which rates subaccounts pay

Your platform's `subaccount_fee_mode` sets the Whop rates for a subaccount without rates of its own. [Retrieve account fees](https://docs.whop.com/api-reference/accounts/retrieve-account-fees.md) for your platform's account ID to read its `mode`.

| Mode | A subaccount without its own rates pays |
| - | - |
| `inherit` | Your platform's rates. This is the default. |
| `subaccount_rates` | Whop's standard rates. |
| `subaccount_rates_collect_difference` | Whop's standard rates. On each payment, your platform receives the amount by which the subaccount's fee is above your rate. |

You can change `inherit` to either other mode. You can change `subaccount_rates_collect_difference` to `subaccount_rates`. Contact Whop for other changes. `available_modes` lists the modes you can change to.

```bash theme={null}
curl --request PATCH \
  --url https://api.whop.com/api/v1/accounts/biz_PLATFORM_ID/fees \
  --header "Authorization: Bearer $WHOP_API_KEY" \
  --header "Content-Type: application/json" \
  --data '{ "subaccount_fee_mode": { "mode": "subaccount_rates" } }'
```

## Payment markups by payment method

`payments` is the markup on every payment a subaccount receives without a checkout application fee. To charge a payment method differently, set it under `payment_methods`:

| Key | Payment methods |
| - | - |
| `card` | Cards, including Apple Pay and Google Pay |
| `ach` | US bank account payments and US bank transfers |
| `wire` | Bank wires, domestic and international |

Every other payment method uses the `payments` markup.

```json theme={null}
{
  "child_markups": {
    "payments": { "percentage": 2, "fixed": 0.30 },
    "payment_methods": {
      "card": { "percentage": 3, "fixed": 0.30 },
      "ach": { "percentage": 1, "fixed": 0 }
    }
  }
}
```

### Card markups by region

Whop's own card rate depends on the region of the acquirer that processes the payment. To price cards differently in each region, set `card_regions`. A card payment from a region without its own markup uses `payment_methods.card`.

| Key | Card payments processed by an acquirer in |
| - | - |
| `usa` | The United States |
| `eu` | Europe |
| `uk` | The United Kingdom |
| `ca` | Canada |
| `au` | Australia |

```json theme={null}
{
  "child_markups": {
    "card_regions": {
      "usa": { "percentage": 0.55, "fixed": 0.30 },
      "uk": { "percentage": 0.5, "fixed": 0.20 }
    }
  }
}
```

A payment uses the first markup it finds, in this order:

1. The subaccount's markup for the card payment's region.
2. The subaccount's markup for its payment method.
3. The subaccount's `payments` markup.
4. The platform default for the card payment's region.
5. The platform default for its payment method.
6. The platform's `payments` default.

Steps 1 and 4 apply only to card payments. A rate set for one subaccount always wins over a platform default.

## Change or clear a markup

* Omitted fields keep their effective values. For a percentage-only markup, explicitly set `fixed: 0`.
* Set both `percentage: 0` and `fixed: 0` to disable a markup for that account. The subaccount still pays Whop's fee.
* Send `null` for the entire rate, such as `markups: { payouts: { bank_wire: null } }`, to remove a subaccount override and restore its platform default, or zero if none exists.

## How payout subsidies work

The markup combines the percentage of the withdrawal amount with the fixed amount. If the result is negative, your platform pays that amount toward Whop's fee. The subsidy stops when the subaccount's total fee reaches zero. It never increases the payout above the withdrawal amount.

These examples use a $10 withdrawal and a $0.60 Whop fee. The \$0.60 is an example rate, not a standard price for every payout method.

| Percentage | Fixed | Subaccount fee | Net payout | Whop receives | Platform balance change |
| - | - | - | - | - | - |
| 0.4% | \$2.00 | \$2.64 | \$7.36 | \$0.60 | +\$2.04 |
| 1% | −\$0.60 | \$0.10 | \$9.90 | \$0.60 | −\$0.50 |
| −5% | \$0.00 | \$0.10 | \$9.90 | \$0.60 | −\$0.50 |
| 0% | −\$100.00 | \$0.00 | \$10.00 | \$0.60 | −\$0.60 |

## Limits and funding

| Rule | Behavior |
| - | - |
| Payout markup limits | Each component can be negative: −100% to 100%, and −$100 to $100 fixed. The actual subsidy can't exceed Whop's fee. |
| Other markup limits | 0% to 100%, and $0 to $100 fixed. Negative values are rejected for non-payout markups. |
| Platform balance | Keep enough available balance in the withdrawal currency to cover the subsidy. The balance check and debit happen together. |
| Insufficient balance | The payout stays pending while Whop retries. Fund the platform before the creation deadline, about 10 minutes. Otherwise, the payout fails without moving funds. |
| Full fee coverage | When `covered_payout_fees` covers the payout method, the platform pays Whop's entire fee and the markup is ignored. |

For a withdrawal from a EUR balance, your platform needs EUR to fund the subsidy. A USD balance doesn't cover it automatically.

Review a [payout quote](https://docs.whop.com/api-reference/payouts/create-payout-quote.md) before creating the payout. A quote shows fees but doesn't reserve platform funds. The platform's balance is checked when the payout is debited. Actual fees depend on the payout method and your account's pricing.


## Related topics

- [Accounts API](https://docs.whop.com/api-reference/accounts/account.md)
- [Fee Markup](https://docs.whop.com/api-reference/legacy/fee-markups/fee-markup.md)
- [Create fee markup](https://docs.whop.com/api-reference/legacy/fee-markups/create-fee-markup.md)
