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

# Create and Send Invoices on Whop

> Create a one-time or recurring invoice from your dashboard, choose payment methods, and track, remind, void, download, or export invoices afterward.

Your business might send customers an invoice upfront for a specific service. A customer might also require an invoice before making a payment. In either case, you can create an invoice on Whop in a few seconds.

## How to create and send an invoice

### 1. Navigate to 'Invoices'

* Go to your **Dashboard** and select **Invoices** in the **Payments** section.
* Select **+ Create invoice.**

### 2. Add customer details

* Select the **Find or add a customer** dropdown menu. - Select an existing
  customer in the dropdown, or select **+ Add new customer** and enter their
  email and name.

### 3. Add due date

Select the dropdown menu under **Due date** and select when the invoice will
become due for payment.

### 4. Add description

Add a clear description of what you're billing for — your customer will see
this on their invoice.

### 5. Add amount

* Choose whether it's a **One-time** or **Recurring** payment: - **One-time:**
  The customer receives a single invoice with the amount you set. -
  **Recurring:** Creates a subscription. With **Send invoice**, Whop emails the customer
  a new invoice every billing period. With **Charge automatically**,
  Whop charges the customer's saved payment method each period. If you select
  *Recurring*, you'll need to choose how often to charge the customer (e.g.
  every 7 days, monthly, quarterly, yearly, or a custom billing cycle). -
  **Enter the amount:** Type in the price and use the dropdown to choose the
  currency. - Configure advanced options: - **Ask questions before checkout:**
  Collect information from customers before they purchase. - **Redirect after checkout:** Send customers to a specific page after they complete checkout. -
  **Payment methods:** Choose how your customer can pay the invoice — options
  include card, ACH, platform balance, financing (apply for access), PayPal, or
  crypto (setup required).

### 6. Pass processing fees (optional)

You can pass payment processing fees on to the customer by checking the box
next to **Pass payment processing fees on to the customer**. When this is
selected, the total invoice amount includes the payment processing fees.

### 7. Send invoice

When you are happy with the email and checkout link previews, and you're sure all the details are correct, select **Send invoice**. Your customer will immediately receive the invoice in their email inbox.

The customer pays a 5% service fee on invoices, which is automatically added on to the invoiced amount. This service fee is to cover the cost of payment processing.

## How reminders work

When an invoice passes the due date, Whop's invoice reminders start at certain intervals. Whop reminds your customers after:

* 3 days
* 7 days
* 14 days
* 30 days
* 60 days

If the invoice is still unpaid after 120 days, it will be automatically voided.

## How to cancel / void an invoice

**1. Go to Invoices**

Go to your **Invoices** dashboard

**2. Find the invoice**

Find the invoice you want to void

**3. Void the invoice**

Select the three-dot menu on the right and select **Void invoice**

**4. Done**

After you void the invoice, the customer can no longer pay it

## How to download an invoice

**1. Go to Invoices**

Go to your **Invoices** dashboard

**2. Find the invoice**

Find the invoice you want to download

**3. Download PDF**

Select the three-dot menu on the right and select **Download PDF**

## Tracking invoices in your dashboard

Your dashboard gives you a full view of every invoice and its status across all your customers. You can see which customers paid and which invoices remain outstanding. The **Invoices** page lists, creates, and sends native invoices, including ones created through the [Invoices API](https://docs.whop.com/developer/guides/invoices.md). Legacy invoices are on its **Legacy invoices** tab.

### Statuses

* **Open:** Whop created and delivered the invoice, but the customer hasn't paid yet.
* **Paid:** The customer paid the invoice.
* **Past due:** The customer didn't pay the invoice by the due date. The customer can still pay for up to 120 days after the due date.
* **Void:** The invoice was either canceled manually or automatically after being past due for 120 days.

### Customize your invoices dashboard

* **Filters:** Use the filters at the top of the table to only see invoices by status (e.g. Open, Paid, Past due) or by collection method (e.g. Send invoice vs Charge automatically).
* **Edit the table view:** Select Edit in the top right to choose which columns to display, such as invoice number, customer name, email, created date, or due date. Drag columns to reorder them so the most important details appear first.

### Export invoices for your records

**1. Select Export**

Select **Export** in the top right of the dashboard

**2. Choose your time zone**

Select local or UTC

**3. Select a date range**

Choose today, last 7 days, last month, or a custom range

**4. Pick columns**

Pick which columns to include, or export all

**5. Download**

Select **Export** to download a CSV file you can use for accounting or
record-keeping


## Related topics

- [Invoice Customers from Your Dashboard](https://docs.whop.com/payments/send-an-invoice.md)
- [Create a Whop](https://docs.whop.com/payments/create-a-whop.md)
- [Get Started with Whop](https://docs.whop.com/get-started.md)
