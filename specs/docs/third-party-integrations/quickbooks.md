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

# QuickBooks

> Record Whop sales, refunds, and payouts in QuickBooks Online, and let customers pay QuickBooks invoices through Whop, with the QuickBooks Sync app.

> [card] [Install QuickBooks Sync](https://whop.com/apps/app_ygXqVjukBrVaU3/install/)
>
> Available on the Whop App Store

The QuickBooks Sync app connects a Whop business to a QuickBooks Online company. Use it to record Whop activity in your books, and to collect payment on QuickBooks invoices through Whop. The sync mode you choose decides which Whop activity QuickBooks records.

You need admin access to your Whop business and to a QuickBooks Online company. QuickBooks Desktop isn't supported.

## What syncs

| Activity | Direction | What happens |
| - | - | - |
| **Whop sales** | Whop → QuickBooks | A Sales Receipt for each sale (All Sales mode only) |
| **Whop payouts** | Whop → QuickBooks | A Bank Deposit for each completed withdrawal, with Whop fees on their own line (All Sales and Withdrawals Only modes) |
| **Refunds** | Both | A refund in QuickBooks (Refund Receipt or Credit Memo) refunds the Whop payment. A refund in Whop creates a Refund Receipt in QuickBooks, except in Invoicing Only mode. |
| **Invoices** | QuickBooks → Whop | New QuickBooks invoices get a Whop payment link in the memo. When the customer pays, the app marks the invoice paid. |
| **Recurring transactions** | QuickBooks → Whop | A recurring invoice template creates a weekly, monthly, or yearly Whop plan |
| **Products** | QuickBooks → Whop | New and updated QuickBooks items create or update Whop products. Whop products reach QuickBooks only through a backfill. |
| **Customers** | Both | Matched by email address |

While connected, the app turns off QuickBooks Payments on new invoices so customers pay through Whop. Invoices that are already open don't change.

## Set up the app

**1. Install the app**

Select **Install QuickBooks Sync** at the top of this page, or search for "QuickBooks Sync" in the Whop App Store. The app then appears in your Whop dashboard.

**2. Connect QuickBooks**

Open the app and go to **Connection**. Select **Connect (Production)** for your live company, or **Connect (Sandbox)** for a QuickBooks sandbox company. Sign in to Intuit, choose the company, and approve the connection.

**3. Map your accounts**

Go to **Accounts** and pick the revenue account, the processing fee account, and the bank account your Whop withdrawals land in. The app can create a "Whop Processing Fees" expense account for you. If you don't pick a fee account, payout deposits leave out the fee line.

**4. Choose a sync mode**

Go to **Overview** and pick a sync mode (see below). Check this before you rely on the sync, because the mode decides whether the app records individual Whop sales.

**5. Import existing data (optional)**

Go to **Backfill** to bring over data from before you connected:

* **Users to Customers** — creates QuickBooks customers from Whop members, matched by email
* **Items / Products** — syncs items between QuickBooks and Whop
* **Unpaid Invoices** — adds Whop payment links to open invoices
* **Paid Invoice Matching** — matches paid invoices to Whop payments

The app skips records it has already mapped.

## Sync modes

Switching modes only affects activity from that point on. Earlier records don't change.

| | All Sales | Withdrawals Only | Invoicing Only |
| - | - | - | - |
| **Whop sales** | Sales Receipt for each sale | Not recorded | Not recorded |
| **Whop payouts** | Bank Deposit | Bank Deposit | Not recorded |
| **Whop refunds** | Refund Receipt | Refund Receipt | Not recorded |
| **QuickBooks invoices, recurring templates, items, refunds** | Synced | Synced | Synced |
| **Best for** | Selling mainly on Whop | Tracking only payouts | Using Whop only to collect QuickBooks invoices |

In **All Sales** mode, the app creates a "Whop Clearing" account. Sales Receipts deposit into it, and each payout moves the balance out to your bank account.

In Withdrawals Only mode, the app records payouts as revenue. If you also invoice customers in QuickBooks, you count that revenue twice: once on the invoice and again on the deposit.

## Taxes

For QuickBooks invoices, QuickBooks calculates tax and the customer pays the invoice total through Whop. Remove tax settings from the matching Whop products so buyers aren't taxed twice. Sales Receipts for direct Whop sales record the full amount on one line, without a separate tax line.

## Monitor the sync

**Sync History** lists every event the app processed, filtered by status. **Errors** lists failed events. The app retries a failed event up to 5 times, with increasing delays between attempts.

## Disconnect

Go to **Connection** and select **Disconnect QuickBooks**. The app keeps your sync history and mappings if you reconnect later. Records already in QuickBooks stay there.

## Next steps

> [card] [Xero](https://docs.whop.com/third-party-integrations/xero.md)
>
> Send Whop activity to Xero as a bank feed.

> [card] [Pricing and fees](https://docs.whop.com/fees.md)
>
> See the fees that appear on your payouts.


## Related topics

- [Xero](https://docs.whop.com/third-party-integrations/xero.md)
- [Connect a Social Account](https://docs.whop.com/api-reference/social-accounts/connect-a-social-account.md)
- [Connect an External Account](https://docs.whop.com/api-reference/external-accounts/connect-an-external-account.md)
