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

# Contact a Merchant

> Contact a merchant about a refund, and see your options on Whop if they don't respond.

If you are seeking a refund or dealing with an unresponsive merchant (creator) on Whop, there are several methods and escalation steps available. This guide walks you through the recommended actions and explains potential consequences for escalations like bank disputes.

## Contacting a merchant directly

To reach a merchant directly, Whop provides several tools:

* **Whop's messaging feature** — Use the messaging function on Whop to communicate with the merchant. You can find the link to message them in the membership or purchase details section of your account. This is the primary and most direct method for refund requests.
* **Other Whop contact links** — Many merchants have a direct contact link provided by Whop. Check the purchase confirmation or merchant page for details.
* **Evidence submission to Whop** — If you contact the merchant but still need Whop's involvement, submit evidence of your purchase and refund requests through the [Resolution Center](https://docs.whop.com/memberships-and-access/cancellations-and-refunds/status-of-resolution-center-case.md).

## Options if the merchant is unresponsive

If the merchant doesn't respond to your requests for refunds or account reinstatements, follow these steps:

1. **Notify Whop** — Report the merchant's unresponsiveness. Whop can contact the creator on your behalf, though the merchant decides whether to issue a refund.
2. **Submit a case to the Resolution Center** — Start your refund process in Whop's Resolution Center. Whop first forwards your case to the merchant. If the merchant misses the deadline, the case escalates to Whop's Trust & Safety team for review and a final decision.
3. **File a formal report** — On the merchant's store page, select **Report this company**. This prompts a Trust & Safety review. See [Report a bad actor](https://docs.whop.com/trust-and-safety/staying-safe/report-a-bad-actor.md).
4. **Final resort — bank dispute** — If all direct and Whop-based options fail, you may dispute the charge with your bank. Whop temporarily suspends your account until the bank reaches a final decision.

## Important considerations

* Refund success often depends on merchants' response times. Communicate clearly and provide evidence of your refund eligibility.
* Repeated cases of unresponsiveness or unresolved disputes may impact your relationship with Whop and the creator.
* Always attempt to resolve the matter through Whop's internal systems before resorting to external disputes.


## Related topics

- [Blocked Categories & Merchants](https://docs.whop.com/whop-finance/blocked-categories-and-merchants.md)
- [Failed Subscription Payments](https://docs.whop.com/payments-and-billing/payment-issues/troubleshoot-failed-payments.md)
- [SeQura Guide](https://docs.whop.com/payments-and-billing/financing/sequra-guide.md)
