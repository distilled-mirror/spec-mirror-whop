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

# Update Email

> Change the email address connected to your Whop account from account settings.

You can update the email connected to your Whop account at any time from your account settings.

If you lose access to your registered email address, you will need to complete extra verification steps to keep your account secure.

## How to update your email

> [!WARNING]
>
> If your registered email is no longer accessible, you must [contact Whop
> Support](https://whop.com/support). Whop Support must verify your identity
> before making changes.

### 1. Open your profile settings

1. Log in to Whop.
2. Select your profile picture to open your [Account profile](https://whop.com/@me/).
3. Select **Edit profile**.

### 2. Open your security settings

In the left-hand menu, select **Account security**.

### 3. Edit your email

1. Find your email at the top of the page.
2. Select **Change**.
3. Enter your new email address and select **Send code**.

> [!TIP]
>
> If you see an "email already taken" error, check whether another account uses
> that email. If so, you can [transfer
> memberships](https://docs.whop.com/memberships-and-access/accessing-your-purchase/duplicate-whop-accounts.md)
> or [contact Whop Support](https://whop.com/support) for help.

### 4. Verify your new email

Enter the six-digit code sent to your new email address, then select **Verify**.

## Troubleshooting email change errors

[Contact Whop Support](https://whop.com/support) if:

* Your current registered email is inaccessible, or
* You aren't receiving the verification code

Whop uses strict security measures to protect your account. Some account change requests require identity verification by the Trust and Safety team to confirm that the account owner made the request.

### "No user found" at login

This means the email you entered isn't linked to a Whop account.

* Search your inbox for Whop confirmation emails, or check billing statements to find the email you used to sign up. You may have misspelled it.
* Confirm whether you signed up through Discord. You may also have used Google with another email.

### Duplicate Whop accounts with different emails

If you have more than one Whop account with different email addresses, see [Duplicate Whop accounts](https://docs.whop.com/memberships-and-access/accessing-your-purchase/duplicate-whop-accounts.md). You can transfer memberships from one account to another.


## Related topics

- [Duplicate Whop Accounts](https://docs.whop.com/memberships-and-access/accessing-your-purchase/duplicate-whop-accounts.md)
- [Splitit Guide](https://docs.whop.com/payments-and-billing/financing/splitit-guide.md)
- [Newsletters](https://docs.whop.com/supported-business-models/newsletters.md)
