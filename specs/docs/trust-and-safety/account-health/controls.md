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

# Payment Controls

> The controls Whop places on accounts with elevated dispute rates, what each one does, and how they lift

When your account carries elevated risk, most often an elevated dispute rate, Whop may place controls on it. You can see the controls set on your account on your [Payment health](https://docs.whop.com/trust-and-safety/account-health/payment-health.md) dashboard. Controls aren't a penalty. Each one either stops disputes before they land or protects the funds needed to cover incoming disputes. The controls let you keep taking payments throughout the process.

Whop reviews every account continuously and sets controls per account, based on the risk it carries. As that risk rises, more controls apply and existing ones tighten. As it falls, controls ease and lift on their own. Your dashboard always shows exactly which controls are active on your account.

## Why controls exist

Card networks hold Whop responsible for the dispute rates of every business that processes payments on the platform. If left unchecked, a high dispute rate ends in card networks shutting off card payments for your business without warning or a way to reverse the decision. Controls let Whop intervene early enough to prevent that outcome.

They also protect your money. A high dispute rate means more chargebacks are coming. Holding funds now is what keeps you from ending up with a [negative balance](https://docs.whop.com/trust-and-safety/account-health/managing-dispute-rates.md#negative-balances) you'd have to repay out of pocket.

## The controls

These are the controls your dashboard can show, and what each one does:

| Control | What it does |
| - | - |
| **Reserve** | The reserve holds a portion of payment volume to cover future disputes and refunds. See [Reserves](https://docs.whop.com/trust-and-safety/account-health/reserves.md). |
| **Auto-refund threshold** | Pre-chargeback alerts under this amount trigger automatic refunds. An alert refunded in time never becomes a dispute, so it never counts toward your rate. |
| **Resolution-center auto-refund** | Open Resolution Center cases under set amounts trigger automatic refunds. This resolves unhappy customers before they go to their bank. |
| **Dispute alert fee** | Each pre-chargeback alert intercepted on your behalf incurs a fee. |
| **High-risk processing fee** | An additional fee applies to card transactions while controls are active, covering the extra risk the account carries. |
| **Pending delay** | This control adds days on top of the processor's settlement period before funds are available for payout. The delay keeps enough in your account to cover incoming disputes. |
| **Financing at checkout** | This control can pause buy now, pay later options. Disputes occur more often for financed purchases than for standard card payments, so pausing them removes a concentrated source of disputes. |

Two more can apply in specific situations:

* **3D Secure**: when most of your disputes are fraud-coded, the checkout flow may require customers to verify card payments with their bank. 3DS blocks fraudulent charges before they happen, and the chargebacks that come with them. You can see this in your dashboard under **Settings** → **Risk**.
* **Card brand restrictions**: if your dispute rate on a specific card network far exceeds that network's limits, this control can pause that brand at checkout. Other brands keep working. See [Dispute monitoring programs](https://docs.whop.com/trust-and-safety/account-health/dispute-monitoring-programs.md) for each network's limit.

## How controls lift

Whop continuously reviews controls, just as it reviews [reserves](https://docs.whop.com/trust-and-safety/account-health/reserves.md). As your account recovers, they ease and lift automatically. You don't need to contact support.

The fastest way out is the same as the way to never get controls at all: keep your dispute rate down. [Managing dispute rates](https://docs.whop.com/trust-and-safety/account-health/managing-dispute-rates.md) covers the steps that actually work.

You can also retrieve the controls on your account programmatically via API: `GET /accounts/{id}` returns them in the `payment_controls` field.

## Next steps

> [card] [Managing dispute rates](https://docs.whop.com/trust-and-safety/account-health/managing-dispute-rates.md)
>
> Refund faster, resolve cases, and remove what's causing disputes.

> [card] [Reserves](https://docs.whop.com/trust-and-safety/account-health/reserves.md)
>
> Why a reserve can hold part of your balance and when it releases the funds.


## Related topics

- [Payment Health](https://docs.whop.com/trust-and-safety/account-health/payment-health.md)
- [Account Suspensions](https://docs.whop.com/trust-and-safety/suspensions/account-suspensions.md)
- [Reserves](https://docs.whop.com/trust-and-safety/account-health/reserves.md)
