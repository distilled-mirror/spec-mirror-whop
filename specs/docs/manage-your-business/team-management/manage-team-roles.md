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

# Team Management

> Invite team members to your whop and choose a role that sets what each person can access.

Your Team tab is where you decide who gets access to your whop and what permissions they have.

## How to add team members to your whop

**1. Go to Team**

Go to **Dashboard** > **All tools** > **More** > **[Team](https://whop.com/dashboard/team/)**

**2. Enable 2FA (recommended)**

Toggle on **Require 2FA** to make sure all team members use secure login. Each member turns it on for their own account by following [Enable 2FA](https://docs.whop.com/account-settings/account-security/enable-2fa.md).

**3. Invite a team member**

Select **Invite team member** in the top right corner

**4. Enter their email**

Enter the person's email address in the **Email** field

**5. Assign a role**

Select their role from the **Role** dropdown menu

**6. Send the invite**

Select **Invite**

Whop automatically sends your team member an invitation email. When they accept, they'll show up in your team table with their new permissions.

You can see all your pending invites on the **Invites** tab next to **Members**.

> [!WARNING]
>
> Start small. For security, grant someone the
> lowest-permissioned role that lets them do their job. You can always expand
> their access with a higher role later if needed.

## Roles and permissions overview

There are five pre-set roles you can assign to your team members, plus the ability to create your own custom roles:

| Role | Permissions |
| :- | :- |
| **Owner** | Full access to everything |
| **Operations** | Manage products, members, payments, invoices, team, chat, forums, support tickets, courses, affiliates, promo codes, tracking links, checkout, and more — everything except payouts, developer tools, analytics, and account profile settings |
| **Sales** | View members, plans, payments, and promo codes. Create plans. Read and send in chat and forums. View tracking links and waitlists |
| **Support** | Moderate chat and livestreams. Manage support tickets, courses, and experiences. Mute and ban members. Read and post in chat and forums |
| **Advertiser** | Create and manage ad campaigns. View analytics and account balance |

### **Owner**

Full access to every feature, setting, and tool on the platform.

* Manage payouts, withdrawals, and subaccounts
* View and manage API keys, OAuth, and Webhooks
* Edit account profile and legal settings
* View analytics and stats
* Invite and manage all team members, including other Owners
* Transfer ownership
* Manage crypto wallets

> [!TIP]
>
> The Owner role is best for founders, co-founders, technical leaders, or finance/legal leads
> who need complete control.

### **Operations**

Everything an Owner can do, except:

* **No access to Payouts** — can't create or manage payout accounts, initiate withdrawals, or transfer funds
* **No access to Developer tools** — can't create apps, manage API keys, OAuth, or webhooks
* **No access to Analytics** — can't view stats on the dashboard home page
* **Can't edit account profile** — can't update account name, branding, or general settings (but can view them and manage legal info)
* **No access to crypto wallets**

> [!TIP]
>
> The Operations role is best for operations managers, team leads, or anyone who
> needs broad dashboard access without control over payouts or developer
> settings.

### **Sales**

A focused role with read and create access to sales-related features:

* View the Members table and member emails
* View member and plan analytics
* View Payments (read only — can't charge, refund, or manage disputes)
* View and create Plans
* View Promo codes and Tracking links
* View `Waitlists`
* Read and send messages in Chat and Forums
* View basic account info

> [!TIP]
>
> The Sales role is best for marketing team members, affiliate managers, or
> sales reps who need to see members, payments, and checkout links.

### **Support**

A focused role for community management and customer support:

* Moderate Chat — delete messages, mute and ban members, and manage chat webhooks
* Moderate Livestreams — moderate livestream chat
* Manage Support tickets — view, create, and reply to support tickets
* Manage Courses — view and edit course content
* Manage Experiences — create, edit, and delete experiences
* Moderate [Content Rewards](https://docs.whop.com/memberships-and-access/third-party-apps/content-rewards.md) submissions
* Read and post in Chat and Forums
* View members, member emails, plans, products, tracking links, and waitlists

> [!TIP]
>
> The Support role is best for community managers, support agents, or anyone
> helping manage your community and answer customer questions.

### **Advertiser**

A narrow role specifically for running ad campaigns:

* Create, view, and manage ad campaigns
* Log conversions and add credits
* View analytics and stats
* View account balance (required for ad spend)

> [!TIP]
>
> The Advertiser role is best for marketing specialists or agencies who only
> need to manage your ad campaigns.

### **Custom roles**

You can also create fully custom roles with any combination of permissions. Go to **[Team](https://whop.com/dashboard/team/)** and select **Manage roles** to create a new custom role and toggle individual permission groups on or off.

### **Complete permissions breakdown**

| Permission group | Support | Sales | Advertiser | Operations | Owner |
| :- | :- | :- | :- | :- | :- |
| **Products** — manage products, apps, and experiences | View only | View only | ❌ | ✔️ | ✔️ |
| **Members** — view, manage, moderate, and export members | Moderate + view | View + stats | ❌ | ✔️ | ✔️ |
| **Payments** — charge, refund, disputes, Resolution Center | ❌ | View only | ❌ | ✔️ | ✔️ |
| **Invoices** — view, create, edit, and export invoices | ❌ | ❌ | ❌ | ✔️ | ✔️ |
| **Payouts** — withdraw funds, manage payout accounts | ❌ | ❌ | ❌ | ❌ | ✔️ |
| **Chat** — read, send, delete messages, manage DMs | Full + moderate | Read + send | ❌ | ✔️ | ✔️ |
| **Forums** — read, create, and moderate forum posts | Read + post | Read + post | ❌ | ✔️ | ✔️ |
| **Support tickets** — view, create, and reply | ✔️ | ❌ | ❌ | ✔️ | ✔️ |
| **Courses** — view and edit course content | ✔️ | ❌ | ❌ | ✔️ | ✔️ |
| **Content rewards** — manage campaigns and submissions | Moderate only | ❌ | ❌ | ✔️ | ✔️ |
| **Affiliates** — view and manage affiliates | ❌ | ❌ | ❌ | ✔️ | ✔️ |
| **Promo codes** — create, edit, and delete promo codes | ❌ | View only | ❌ | ✔️ | ✔️ |
| **Tracking links** — create and manage tracking links | View only | View only | ❌ | ✔️ | ✔️ |
| **Ads** — create and manage ad campaigns | ❌ | ❌ | ✔️ | ✔️ | ✔️ |
| **Livestreaming** — create and moderate livestreams | Moderate only | ❌ | ❌ | ✔️ | ✔️ |
| **Notifications** — send email and push notifications | ❌ | ❌ | ❌ | ✔️ | ✔️ |
| **Team** — invite/remove members, manage roles | ❌ | ❌ | ❌ | ✔️ | ✔️ |
| **Account settings** — edit profile, legal, billing | ❌ | ❌ | ❌ | View + legal | ✔️ |
| **Developer tools** — API keys, OAuth, webhooks, apps | ❌ | ❌ | ❌ | ❌ | ✔️ |
| **Checkout** — checkout configurations and settings | ❌ | ❌ | ❌ | ✔️ | ✔️ |
| **Analytics** — view dashboard stats | ❌ | ❌ | ✔️ | ❌ | ✔️ |

> [!WARNING]
>
> Some actions your team can perform (like deleting messages or removing
> checkout links) can't be undone. Start with the lowest-permissioned role that
> lets someone do their job.

## How to change team member roles

You can update anyone's role at any time to give them more or less access.

**1. Go to Team**

Go to **Dashboard** > **All tools** > **More** > **[Team](https://whop.com/dashboard/team/)**

**2. Find the team member**

Find the team member in the members table

**3. Change their role**

Select the dropdown menu in their **Role** column

**4. Select the new role**

Select their new role

**5. Save**

Select **Save**

The change happens right away, and they'll have their new permissions immediately.

## How to remove team members

**1. Go to Team**

Go to **Dashboard** > **All tools** > **More** > **[Team](https://whop.com/dashboard/team/)**

**2. Find the team member**

Find the team member you want to remove

**3. Open the menu**

Select the three dots (︙) on the right side of their row

**4. Remove them**

Select **Remove user**

**5. Confirm**

Select the red **Remove user** button in the confirmation pop-up

They'll immediately lose access to your whop. To add them back to the team, send a new invitation.


## Related topics

- [Pay Your Team](https://docs.whop.com/manage-your-business/manage-payouts/pay-your-team.md)
- [Business Analytics Dashboard](https://docs.whop.com/manage-your-business/manage-business/analytics.md)
- [Agencies](https://docs.whop.com/supported-business-models/agency-services.md)
