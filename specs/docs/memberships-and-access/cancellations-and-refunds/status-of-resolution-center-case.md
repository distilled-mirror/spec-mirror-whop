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

# Status of a Resolution Center Case

> Check the status of your refund or dispute case in the Resolution Center, reply to requests for more information, and cancel or appeal a case.

Use the Resolution Center to track refund requests and disputes.

## How to check the status of your Resolution Center case

1. Log in at [whop.com](https://whop.com).
2. Go to **Account** or **Settings** and open **Resolution Center**, or use the link from your case confirmation email.
3. Open your case to see its **status** (e.g. Under review, Approved, Denied, More info needed).
4. If the creator or Whop asked for more information, reply in the case thread or via the link they sent.

This is what each status means on your Resolution Center case:

* **Under review** — Whop or the creator is reviewing your case. You may get a reply or request for more details.
* **Approved** — Whop or the creator approved a refund or resolution. Funds may take a few days to return.
* **Denied** — Whop or the creator didn't approve the request. You may be able to [appeal](#appeal-a-resolution-center-case).
* **Closed** — Whop or the creator finished reviewing the case.

Respond to any messages or deadlines so the reviewer can process your case.

## Cancel a Resolution Center case

If you no longer want to pursue a refund or dispute, you can cancel the case.

1. Log in at [whop.com](https://whop.com).
2. Open **Resolution Center** (from Account, Settings, or the link in your case email).
3. Open the case you want to cancel.
4. Look for **Cancel case**, **Withdraw request**, or **Close case** and confirm.

After you cancel, Whop closes the case and takes no further action on that request. You can often open a new case later if something changes, subject to Whop's and the creator's policies.

## Appeal a Resolution Center case

If Whop or the creator denied your Resolution Center case and you believe the decision was wrong, you may be able to appeal within 7 days.

1. Log in at [whop.com](https://whop.com) and open **Resolution Center**.
2. Open the **denied** case.
3. Look for **Appeal** or **Request review** and follow the steps, or reply in the case with new information or reasons.
4. Submit any extra evidence (screenshots, emails, receipts) that support your appeal.

Whop or the creator reviews each appeal again. Not all cases qualify for appeal, and approval isn't guaranteed. Response times can vary.

If you don't see an appeal option, contact [Whop support](https://whop.com/support) and reference your case.


## Related topics

- [Respond to Refund Requests in the Resolution Center](https://docs.whop.com/manage-your-business/manage-payments/resolution-center.md)
- [Review Refund Claims in the Resolution Center](https://docs.whop.com/payments/resolution-center.md)
- [Managing Dispute Rates](https://docs.whop.com/trust-and-safety/account-health/managing-dispute-rates.md)
