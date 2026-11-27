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

# Promote Your Business

> Set up your affiliate program to automatically pay commissions when someone refers a new paying member to your whop

Set up your affiliate program to automatically pay commissions when someone refers a new paying member to your whop.

## How the affiliate program works

Here's how it works:

* You turn on the affiliate program
* Users get access to their [unique affiliate link](https://docs.whop.com/affiliates/be-an-affiliate.md) for your whop
* They share their link anywhere they want
* When someone joins through their link, Whop automatically tracks the referral and pays the commission
* You can customize commission rates anytime to fit your business model

### Types of affiliates

> [card] [Global Affiliates](https://docs.whop.com/affiliates/setup-global.md)
>
> Promote your product through Whop's affiliate network.

> [card] [Custom Affiliates](https://docs.whop.com/affiliates/setup-custom.md)
>
> Create and manage personalized affiliate deals with custom commission rates
> for partners and creators.

> [card] [Revenue Share](https://docs.whop.com/affiliates/setup-rev-share.md)
>
> Share revenue directly with partners per transaction using automatic Whop
> Payments payouts.

## How to set up your affiliate program

### 1. Go to your affiliate settings

To find your affiliate program:

1. Go to your **Dashboard**
2. Make sure you're viewing the correct whop using the selection modal at the top left
3. Select the **Marketing** dropdown
4. Select **Affiliates**

### 2. Set commission rates for your whop

Select **Set the affiliate commission for a specific whop**

You'll see all your whops with two key settings:

* **Global affiliate rate:** This is the commission rate offered to any user who refers others to your whop. The default is 30%, but you can adjust it to any percentage you prefer.
* **Member affiliate rate:** This is the commission rate specifically for your existing members who refer others. This allows you to reward your community differently than general affiliates (e.g. members get 50% commission while the global rate is 30%).

> [!TIP]
>
> If you set the 'Member affiliate rate' to 0%, members will earn the global affiliate rate instead.

### 3. Activate your affiliate program

After making changes, select the blue tick button to confirm your new rates.

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

## How to edit an existing affiliate offer

Once you've set up at least one affiliate reward, managing details is a piece of cake. Just follow these steps:

1. Head to your **Dashboard**
2. Select the **Affiliates** tab under **Marketing**
3. Here, you will see individually invited (custom) affiliates at the top and member (global) affiliates at the bottom. Anyone can access the global affiliate rewards.

**To edit individual affiliates:**

1. Select the three dots next to the individual affiliate whose rewards you want to edit.
2. Select **Manage**.
3. Scroll down on the user profile to see the plans with custom rewards. Select the three dots next to the plan reward you'd like to change.
4. Select **Edit** and make your changes.

**To edit member (global) affiliates:**

1. Select the three dots next to the member affiliate plan you would like to change, and then select **Edit**.
2. Adjust your percentage and save.

You can also edit this public affiliate reward from the whop's store page. Scroll to the bottom and select the blue **Edit affiliates** button.

## Manage the affiliates on your business

### Remove an affiliate

If you want to remove an affiliate, you can archive the affiliate and their link, so they will no longer be able to promote your business.

After you archive the link, the affiliate no longer earns commission on new sales. The affiliate continues to earn recurring commission from sales they closed before you archived the link.

To remove an affiliate:

* Go to the **Affiliates** section in your seller dashboard
* Find the affiliate you want to remove
* Archive the affiliate (owners and admins have permission to do this)

Whop doesn't support sub-affiliates, so you can't track registrations made by a sub-affiliate.

### If you suspect fraudulent affiliate activity

Contact [Whop Support](https://whop.com/support) if you suspect fraudulent affiliate activity. For example, you may see a sudden spike in expensive purchases with affiliate payouts on your whop.


## Related topics

- [Set Up Global Affiliates](https://docs.whop.com/affiliates/setup-global.md)
- [Set Up Custom Affiliates](https://docs.whop.com/affiliates/setup-custom.md)
- [Launch Your Business](https://docs.whop.com/launch-your-business.md)
