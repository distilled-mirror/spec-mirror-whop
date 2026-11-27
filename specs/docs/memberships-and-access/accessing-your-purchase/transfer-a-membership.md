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

# Transfer a Membership

> Move a membership or purchase from one Whop account to another with a transfer link.

If you have multiple Whop accounts and want to move a membership or purchase from one account to the other, you can do so with transfer links.

> [!NOTE]
>
> If you made a purchase on Whop and don't see your membership on your account,
> you may have created two Whop accounts with two different emails.
> See [How to find your
> purchase](https://docs.whop.com/memberships-and-access/accessing-your-purchase/how-to-find-your-purchase.md)
> for help locating it.

## How to transfer your membership

### 1. Log in to Whop

Go to [whop.com](https://whop.com) with the email and password (or security code) for the account that has the membership.

### 2. Open your profile

Select your avatar to open your
[profile](https://whop.com/@me/settings/memberships).

### 3. Open the dropdown and select Orders

Open the dropdown menu and select **Orders**.

### 4. Open the menu for the membership you want to transfer

Select the **⋯** (three dots) on the membership you want to transfer.

### 5. Copy the transfer link

Select **Transfer membership** to copy the transfer link to your clipboard. You
will see a message in the bottom right of your screen.

### 6. Switch accounts

Log out and sign back into Whop on the account where you want to send the
membership.

### 7. Complete the transfer

Paste the transfer link into your browser's address bar and open it. Follow the prompts to complete the transfer.

## How to transfer a Whop membership linked to a Discord account

When you link your Whop and Discord accounts, you must take some additional steps to transfer memberships or resolve access issues. **You can link each Whop membership to only one Discord account at a time.**

### Resolving a Discord–Whop account linking issue

If you linked your Discord account to the wrong Whop account, you may need to disconnect and reconnect it:

### 1. Go to your profile

Go to your [Profile](https://whop.com/@me/).

### 2. Open Settings > External accounts

Select **Edit profile** to open **Settings** > **[External accounts](https://whop.com/@me/settings/external-accounts/)**.

### 3. Unlink the Discord account

Unlink the Discord account linked to the incorrect Whop account.

### 4. Re-link your Discord account

Follow the steps in [Access a Discord server](https://docs.whop.com/memberships-and-access/access-discord-server/access-a-discord-server.md) to re-link your Discord account to your Whop account.

You can't use one Discord account with multiple Whop accounts. Duplicate Whop accounts can therefore prevent you from linking Discord. Your Discord account may already connect to another Whop account.

First, use the preceding steps to consolidate your memberships on one Whop account. Then connect your Discord account to that Whop account.

### Changing the Discord account connected to a membership

To switch the Discord account associated with your Whop purchase:

### 1. Use a claim or VIP access link

Use a **claim** or **VIP access** link from your membership account settings (e.g. Premium Membership Access). You can find the link in your account.

### 2. Sign out of Discord, then sign back in

Sign out of Discord, then sign back in with the Discord account you want to
use.

### 3. Link that Discord account to Whop

Link that Discord account to Whop following the preceding steps.

To switch: go to the **[External accounts](https://whop.com/@me/settings/external-accounts/)** page on Whop, disconnect the current Discord account, then connect the new one.


## Related topics

- [How to Find Your Purchase](https://docs.whop.com/memberships-and-access/accessing-your-purchase/how-to-find-your-purchase.md)
- [Duplicate Whop Accounts](https://docs.whop.com/memberships-and-access/accessing-your-purchase/duplicate-whop-accounts.md)
- [Billing Portal](https://docs.whop.com/payments-and-billing/manage-billing/billing-portal.md)
