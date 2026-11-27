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

# Send Invoice

> Issues a draft and emails it to its recipients, with reminders after the due date. A draft with no `payment_method_id` needs `invoice:update`, `invoice:create`, or `payment:charge`, and is never charged. A draft with a `payment_method_id` needs `payment:charge`: with a `charge_at`, or when its `due_at` is still ahead, it is emailed now and charged to that method at that time (see `scheduled_charge_at`); otherwise it is charged right away instead of emailed. Voiding the invoice, marking it uncollectible, or paying it first cancels the scheduled charge, and its recipients get the payment link if a charge fails. Sending an open invoice needs `payment:charge` and emails it again, up to three emails a day including the first send. With `send_at`, the draft is sent at that time instead, and cannot be edited, except its recipients, until it is sent or unscheduled. Each email, reminders included, is sent separately to the customer email shown on the invoice and to each address in `recipient_emails`, so recipients never see each other's addresses. With `send_as` set to `quote`, the draft is emailed as a quote with a link to view it, and it stays a draft. It can still be edited, it cannot be paid, and nothing is charged. A draft sent as a quote is not issued automatically on its `due_at`. The link stops working when the invoice is paid, void, or deleted. Send the quote again after an edit. A draft can be sent as a quote up to five times a day. A quote cannot use `send_at`, `charge_at`, or `collection_method`.



## OpenAPI

<!-- OpenAPI source: `post /invoices/{id}/send`, docs.whop.com/openapi/api-v1-native.json (inlined by docs.whop.com; stripped on download; see specs/openapi.json) -->

## Related topics

- [Invoices](https://docs.whop.com/developer/guides/invoices.md)
- [Invoices API](https://docs.whop.com/api-reference/invoices/invoice.md)
- [Migrate from Legacy Invoices](https://docs.whop.com/developer/guides/migrate-from-legacy-invoices.md)
