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

# Send Automated Messages to Members

> Send automatic DMs and emails when someone joins or leaves your whop, with personalized variables, media, and a chosen team member as the sender.

You can set up automated messages on Whop to send personalized DMs and emails when:

1. Someone joins your whop.
2. Someone leaves your whop.

## How to find the Support chats settings

1. **Open your Dashboard**: From your Whop account, select **Dashboard** on the left side of your screen to access your whop's advanced settings.
2. **Find Support chats**: Under the **Marketing** section in your sidebar, select **Support chats**.

Here, you'll see two automated message options: User joined and User left.

## How to set up 'User joined' messages

Send welcome messages automatically when new members join your whop. Use this feature to greet new members, share their next steps, or upsell higher-priced offers with a discount code.

### 1. Enable the automation

Select **Enable automation** on the **User joined** card to activate this automated message.

### 2. Set up your automated message

After enabling, you'll see several customization options:

* **Toggle button**: Turn this on or off anytime (top right)
* **Message field**: Write the message that's sent to new members
* **Variables**: Add these variables to personalize the message:
  * `recipient_name` - Shows their actual name
  * `whop_name` - Shows your whop's name
* **Add media**: Upload images or videos to your message
* **Send email to user when triggered**: Check this box to send users both an email notification and a DM on Whop
* **Send message as**: Select which team member the message should appear to come from

### 3. Save your settings

Select **Save** to activate your User joined automation.

## How to set up 'User left' messages

Send a message when someone leaves your whop and decides they don't want to be a member anymore. Use this feature to gather feedback (to learn why they left), or even try entice them back with a discount.

### 1. Enable the automation

Select **Enable automation** on the **User left** card to activate this automated message.

### 2. Set up your automated message

After enabling, you'll see several customization options:

* **Toggle button**: Turn this on or off anytime (top right)
* **Message field**: Write the message to send to churned members
* **Variables**: Add these variables to personalize the message:
  * `recipient_name` - Shows their actual name
  * `whop_name` - Shows your whop's name
* **Add media**: Upload images or videos to your message
* **Send email to user when triggered**: Check this box to send users both an email notification and a DM on Whop
* **Send message as**: Select which team member the message should appear to come from

### 3. Save your settings

Select **Save** to activate your User left automation.

You can return to these settings anytime to change your messages or turn automated messages on and off.


## Related topics

- [Checkout Branding](https://docs.whop.com/manage-your-business/payment-processing/checkout-branding.md)
- [Team Management](https://docs.whop.com/manage-your-business/team-management/manage-team-roles.md)
- [Transfer a Membership](https://docs.whop.com/memberships-and-access/accessing-your-purchase/transfer-a-membership.md)
