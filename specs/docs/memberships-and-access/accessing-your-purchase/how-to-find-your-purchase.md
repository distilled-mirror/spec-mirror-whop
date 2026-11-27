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

# How to Find Your Purchase

> Find a whop you bought by signing in with the purchase email, tracking down a missing receipt, and checking your orders, or contact support for help.

After you buy access to a product, Whop associates your membership and content with your account. You need the email address you used for the purchase. Users who can't find a purchase often signed in with another email address.

## Where to find your purchase

### 1. Log in at whop.com

Make sure you are using the same email address you used to make the purchase.

### 2. Find your Joined memberships on the left-hand side

You'll see all whops you've joined. Select one to open it.

> [!TIP]
>
> **Tips:** If you've ever created a business on Whop, you'll also see **Your businesses** on this sidebar. If you don't see anything, your sidebar might appear collapsed. Open it by selecting the menu in the top-left corner.

You can also use links from your order confirmation email to go straight to the whop. On your phone, see [Access content on mobile](https://docs.whop.com/memberships-and-access/access-discord-server/access-content-on-mobile.md).

## Find a missing recent purchase

### 1. Search your inbox

Look for an email with **Order Confirmation** in the subject. It comes from **[no-reply@businesses.whop.com](mailto:no-reply@businesses.whop.com)** under the name of the business you bought from, like **Acme via Whop**. Check the receipt to see which email address and Whop ID you used to log in to your account (e.g. **Whop ID: [support@whop.com](mailto:support@whop.com)**).

### 2. Log in with the correct email and Whop ID

If the Whop ID on the receipt isn't the one you usually use, sign in with that ID. Make sure you're logged into Whop with the correct credentials (email and password).

> [!NOTE]
>
> If you've logged in and still don't see your purchase, you may have two
> different Whop accounts. Some users check out with one email and log in with
> another. Searching your inbox for the purchase confirmation shows which email
> the purchase is under — check all your email addresses so you know which
> one you used to sign up. See [Duplicate Whop
> accounts](https://docs.whop.com/memberships-and-access/accessing-your-purchase/duplicate-whop-accounts.md)
> for more on this issue.

## If you don't see a receipt

### 1. Check another email address

You might have made your purchase using a different email address.

### 2. Check your bank or credit card statement

Look for a line like `WHOP.COM/PAY/ABC123`. Follow that link and find the **Customer name** on the purchase details page. Sign into Whop with that email address.

## Check your purchases on Whop

To confirm you're on the correct Whop account or to manage your memberships and purchases:

### 1. Log into whop.com

Go to [whop.com](https://whop.com) and sign in.

### 2. Select your profile icon

Open your profile menu.

### 3. Select Orders

Here you can view and manage all your purchases. If you don't see your purchase on this page, you're logged in with the wrong email address.

## How to transfer subscriptions or memberships to a different Whop account

If you have multiple Whop accounts, you can move your memberships to the one account you want to use.

> [card] [Transfer a membership](https://docs.whop.com/memberships-and-access/accessing-your-purchase/transfer-a-membership.md)
>
> Learn how to move your membership or subscription to another Whop account.

### Contact Whop support

[Contact the team](https://whop.com) or select the **Get help** icon in the app. Ask the team to transfer your membership.


## Related topics

- [Duplicate Whop Accounts](https://docs.whop.com/memberships-and-access/accessing-your-purchase/duplicate-whop-accounts.md)
- [Transfer a Membership](https://docs.whop.com/memberships-and-access/accessing-your-purchase/transfer-a-membership.md)
- [Access a Discord Server](https://docs.whop.com/memberships-and-access/access-discord-server/access-a-discord-server.md)
