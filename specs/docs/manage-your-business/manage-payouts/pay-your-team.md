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

# Pay Your Team

> Pay team members instantly from your Whop balance, either from the Team page of your dashboard or their profile, so they can cash out to their bank.

Pay your team directly from your Whop balance. Payments are instant and your team can cash out to their bank.

> [!WARNING]
>
> There's a 3% fee for all team payments.

## Pay your team through your Dashboard

1. Go to **Dashboard** → **Team**.
2. Select the **Pay** button next to the person you want to pay.
3. Enter the amount you want to send.
4. Select the **Balance** you'll send the payment from.
5. Add a note (optional).
6. Select **Send** to send it.

> [!NOTE]
>
> The total amount you send includes the 3% fee.

When you send it, your team member gets an email and a DM notification. They can then go to **Profile** → **Balance** to cash out.

![Pay team dashboard](https://mintcdn.com/whop/gkU9N_oxv5Gb4qnv/images/payteam1.png?fit=max\&auto=format\&n=gkU9N_oxv5Gb4qnv\&q=85\&s=7c969858034e7308aa7bfa482d50e324)

## Pay your team through their profile

You can also send money to any Whop user through their profile — even if they're not on your team.

1. Select the user's profile picture.
   * If you know their username, go to their profile by entering **whop.com/@\[username]** in your browser (e.g. **whop.com/@George**).

![Pay team profile](https://mintcdn.com/whop/gkU9N_oxv5Gb4qnv/images/payteamprofile.png?fit=max\&auto=format\&n=gkU9N_oxv5Gb4qnv\&q=85\&s=34a365848c4c762a59f52ed09e205f5b)

2. On their profile, select the **Pay** button next to **Message**.
3. Fill out the payment details:
   * **Enter amount:** Type how much you want to send.
   * Select the **Balance** you'll send the payment from.
   * **Write a note:** Add a note (optional).
4. Select **Send** to send the money.

They'll receive an email and message notification, and can cash out through **Profile** → **Balance**.


## Related topics

- [Agencies](https://docs.whop.com/supported-business-models/agency-services.md)
- [Payout Methods](https://docs.whop.com/manage-your-business/manage-payouts/payout-methods.md)
- [Team Management](https://docs.whop.com/manage-your-business/team-management/manage-team-roles.md)
