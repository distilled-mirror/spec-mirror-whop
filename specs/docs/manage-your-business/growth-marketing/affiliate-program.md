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

# Affiliate Program

> Turn on your whop's affiliate program so people earn a commission when their referral link brings in a paying member, and set custom rates for partners.

Set up your affiliate program to automatically pay commissions when someone refers a new paying member to your whop.

## How the affiliate program works

When someone uses a referral link to join your whop and makes a purchase, the person who referred them earns a commission.

Here's how it works:

* You turn on the affiliate program
* Users get access to their [unique affiliate link](https://docs.whop.com/affiliates/be-an-affiliate.md) for your whop
* They share their link anywhere they want
* When someone joins through their link, Whop automatically tracks the referral and pays the commission
* You can customize commission rates anytime to fit your business model

## How to set up your affiliate program

### 1. Go to your affiliate settings

To find your affiliate program:

1. Go to your **Dashboard**
2. Make sure you're viewing the correct whop using the selection modal at the top left
3. Select the **Marketing** dropdown
4. Select **Affiliates**

### 2.  Set commission rates for your whop

1. Select **Set the affiliate commission for a specific whop**
2. You'll see all your whops with two key settings:
   * **Global affiliate rate:** This is the commission rate offered to any user who refers others to your whop. The default is 30%, but you can adjust it to any percentage you prefer
   * **Member affiliate rate:** This is the commission rate specifically for your existing members who refer others. This allows you to reward your community differently than general affiliates (e.g. members get 50% commission while the global rate is 30%)

> [!TIP]
>
> If you set the 'Member affiliate rate' to 0%, members will earn the global affiliate rate instead.

### 3. Activate your affiliate program

After making changes, select the blue tick button to confirm your new rates.

![Affiliates program](https://mintcdn.com/whop/XkQkLHsY-Hk8KVyG/how-to-videos/affiliates720p30fps.gif?s=b63419e2023a2f4675aa58e0a8bb6586)

### How to create custom rates for partnerships

For special partnerships or top performers, you can create custom commission structures:

1. Select **Set an affiliate commission for a specific user**
2. Enter the user's Whop account email address
3. Configure their reward:
   * Choose between **Percent** and **Fixed amount**
   * Select **Recurring payments** (ongoing commissions) or **First payment** (one-time commission)
   * Select which whops this rate applies to
4. Select **Invite** to send them an invitation

## Payment timing

Whop automatically handles all affiliate payments for you. There is usually a 14-day waiting period between when a referred customer makes their purchase and when the affiliate receives their commission. Some commissions can wait up to 30 days.


## Related topics

- [Promote Your Business](https://docs.whop.com/affiliates/promote-your-business.md)
- [Be an Affiliate](https://docs.whop.com/affiliates/be-an-affiliate.md)
- [Set Up Custom Affiliates](https://docs.whop.com/affiliates/setup-custom.md)
