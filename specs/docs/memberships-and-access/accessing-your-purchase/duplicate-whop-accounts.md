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

# Duplicate Whop Accounts

> Why a purchase can seem missing when you have more than one Whop account, the common causes, and how to find the account that holds your membership.

Having more than one Whop account can make a purchase look missing. The membership might belong to a different account than the one you're using.

## Why you might have multiple Whop accounts

* You signed up or made a purchase using different emails, such as personal and work addresses
* You used "Continue with Google" on one device and email/password on another
* You signed up or logged in with iCloud or Discord credentials, and it created you a new account
* You created a new account instead of logging into an existing one
* You made a purchase with Apple Pay, and you don't know the email used to complete that purchase

## What to do

### 1. Find the email address you used to create your Whop accounts

Read [How to find your purchase](https://docs.whop.com/memberships-and-access/accessing-your-purchase/how-to-find-your-purchase.md) to track down the different email addresses you used to make your different accounts.

### 2. Choose which account you would like to use moving forward

Pick the single Whop account you want to use for all your memberships.

### 3. Follow the membership transfer guide to consolidate your accounts

See [Transfer a membership](https://docs.whop.com/memberships-and-access/accessing-your-purchase/transfer-a-membership.md) for the full steps.

Avoid creating new accounts for future purchases. Always log in to the account you use for your memberships. If that account uses an email you no longer check, [update its email](https://docs.whop.com/account-settings/personal-details/update-email.md).


## Related topics

- [Update Email](https://docs.whop.com/account-settings/personal-details/update-email.md)
- [Transfer a Membership](https://docs.whop.com/memberships-and-access/accessing-your-purchase/transfer-a-membership.md)
- [How to Find Your Purchase](https://docs.whop.com/memberships-and-access/accessing-your-purchase/how-to-find-your-purchase.md)
