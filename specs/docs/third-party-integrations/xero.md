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

# Xero

> Send Whop sales, fees, refunds, and payouts to Xero as bank statement lines with the Xero Sync app, then match them to your invoices.

> [card] [Install Xero Sync](https://whop.com/apps/app_k7BroST9XRQ0W6/install/)
>
> Connect Whop to Xero as a bank feed

The Xero Sync app connects your Whop balance to a Xero organisation as a bank feed. Your Whop sales, fees, refunds, and payouts arrive in Xero as bank statement lines. You then match them to the invoices you already raise in Xero.

This is a bank feed, not an accounting sync. It never creates invoices, contacts, or products in Xero.

You need admin access to your Whop business and a Xero organisation on a paid plan that supports bank feeds. Demo and ledger organisations can't use bank feeds. To run a feed for more than one currency, your Xero plan needs multi-currency.

## What arrives in Xero

| Whop activity | Statement line |
| - | - |
| **Sale** | Money in, named after the customer so Xero can suggest the matching invoice |
| **Whop fee** | Money out, on its own line |
| **Refund** | Money out |
| **Lost chargeback** | Money out |
| **Payout** | Money out for the amount that left your balance, after the withdrawal fee. With payout mapping on, it's also recorded as a transfer to your bank account. |

New activity reaches Xero within about 15 minutes. Nothing reconciles until you confirm the match in Xero.

## Set up the app

**1. Install and connect**

Install the app with the button at the top of this page and open it from your Whop dashboard. On **Overview**, select **Connect to Xero**. Sign in to Xero, choose your organisation, and approve the permissions.

**2. Create your bank feed**

Open **Bank Feeds** and choose the currencies you sell in. The app creates one feed per currency, and Xero creates a matching bank account. You can point a feed at an existing Xero bank account instead.

**3. Map payout destinations (optional)**

Open **Payouts** and pick the Xero bank account for each Whop payout destination. It must be in the same currency as the payout. The app then records each payout as a transfer that matches the deposit your bank feed brings in. Unmapped destinations still appear as statement lines.

## Track what the app sends

**Activity** lists every Whop transaction the app has recorded:

* **Ready to send** — goes out with the next statement, within about 15 minutes
* **In Xero** — delivered and ready to reconcile

## Reconnect for new features

Xero grants new permissions only when you reconnect. If you connected before a feature existed, such as payout transfers, the dashboard shows a prompt to reconnect. Your existing feeds keep running and nothing already in Xero changes.

## Disconnect

Go to **Connection** and disconnect. The feed stops, and lines already sent stay in Xero.

## Next steps

> [card] [QuickBooks](https://docs.whop.com/third-party-integrations/quickbooks.md)
>
> Record Whop activity in QuickBooks Online.

> [card] [Pricing and fees](https://docs.whop.com/fees.md)
>
> See the fees that appear in your bank feed.


## Related topics

- [QuickBooks](https://docs.whop.com/third-party-integrations/quickbooks.md)
