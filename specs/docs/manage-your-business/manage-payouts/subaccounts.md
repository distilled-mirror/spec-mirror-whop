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

# Subaccounts

> Use subaccounts to pay affiliates, partners, and team members, or onboard sellers and accept payments on their behalf with Whop for Platforms.

Subaccounts let you pay out to partners, affiliates, or team members directly from Whop.

## Subaccount uses

* **Affiliates and partners**: Pay commissions or revenue share to their linked accounts
* **Team**: Pay team members or contractors through subaccounts
* **Verification**: Each subaccount may need to complete identity verification before receiving payouts

Manage subaccounts from your **Dashboard** > **Balances** or **Affiliates**, depending on your setup.

## Whop platform features

Whop for Platforms enables you to build marketplaces, platforms, and multi-tenant applications. With Whop for Platforms, you can onboard subaccounts and facilitate payments on their behalf.

### About platform payments

Whop for Platforms lets you act as a platform operator. Other businesses or individuals, called subaccounts, can accept payments through your platform. This is ideal if you're building:

* **Marketplaces**: Where multiple sellers offer products or services. For example, a watch marketplace where you can buy luxury watches from different sellers.
* **Platforms**: Where independent creators or businesses operate. For example, a content creation platform that pays content creators directly for sponsored content.
* **Multi-tenant applications**: Where each entity needs to process its own payments

> [!TIP]
>
> Whop for Platforms handles the complexity of payment processing, compliance,
> and payouts so you can focus on building your platform's core features.

### How it works

As a platform, you can:

1. **Onboard subaccounts**: Invite businesses or individuals to join your platform
2. **Facilitate payments**: Process payments on behalf of your subaccounts
3. **Manage payouts**: Automatically route funds to your subaccounts
4. **Transfer funds**: Move money between accounts on your platform instantly

### Types of subaccounts

A subaccount can be either:

* **A business**: Companies, organizations, or registered entities that want to sell products or services through your platform
* **An individual**: Sole proprietors, freelancers, creators, or individual sellers who want to monetize through your platform

Each subaccount has their own account on your platform and can:

* Accept payments from customers
* Receive payouts to their bank account
* Manage their own products and pricing
* View their transaction history and analytics

### Benefits

* **Simplified onboarding**: Easy subaccount enrollment process
* **Automatic payouts**: Funds are automatically routed to subaccounts
* **Compliance handled**: Whop manages regulatory requirements
* **Flexible transfers**: Move money between accounts instantly via API
* **Unified dashboard**: Manage all subaccounts from one place

### Next steps for platform developers

> [card] [Enroll subaccounts](https://docs.whop.com/developer/platforms/enroll-subaccounts.md)
>
> Learn how to onboard businesses and individuals to your platform

> [card] [Pay subaccounts](https://docs.whop.com/developer/platforms/collect-payments-for-subaccounts.md)
>
> Make a payment to a subaccount

> [card] [Render payout portal](https://docs.whop.com/developer/platforms/render-payout-portal.md)
>
> Configure and manage payouts to your subaccounts


## Related topics

- [Team Management](https://docs.whop.com/manage-your-business/team-management/manage-team-roles.md)
- [Checkout Branding](https://docs.whop.com/manage-your-business/payment-processing/checkout-branding.md)
