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

# Account Suspensions

> Why Whop suspends accounts and what happens to your balance, plus how to appeal within 30 days.

Account suspensions on Whop can happen for many reasons, including policy violations, fraud, and content violations. This article covers common questions about suspensions, how to appeal them, and Whop's policies on refunds and funds.

## Businesses and users

Whop can suspend two kinds of account. A **business** sells products and receives the resulting funds. A **user** is a person's account and can hold a personal balance, receive payments for services, and transfer funds to other users. Whop may suspend either kind.

## Reasons for suspensions

Every suspension carries one of the reasons below. The suspension notice names the reason, and the list below uses the same wording.

* **Fraudulent payment activity**: fraudulent payments, such as card testing, or using a balance to move funds rather than to buy or sell.
* **Scamming or deceptive practices**: scamming buyers, misrepresenting what you offer, untrue income guarantees, or abusive upsells.
* **Unverifiable business**: Whop couldn't verify the business after repeated requests for information.
* **Prohibited business category**: selling a product or service on the [prohibited list](https://docs.whop.com/trust-and-safety/trust-safety-overview/what-is-not-allowed-on-whop.md).
* **Prohibited content**: posting content the [Community Guidelines](https://whop.com/guidelines/) ban, such as adult content, gore, or illegal material. This also covers conduct such as harassment, slurs, or bullying.
* **Intellectual property infringement**: selling products that infringe another person's or company's copyright, trademark, or other intellectual property.
* **Excessive dispute activity**: a dispute rate over the last 90 days that stays higher than an acceptable level.
* **Sanctions**: a confirmed match on a sanctions list, or a jurisdiction under country-wide sanctions.
* **Unsupported jurisdiction**: the account operates from a jurisdiction Whop can't support because of its risk profile, even though the jurisdiction isn't under country-wide sanctions.
* **Tax authority order**: a tax authority, such as the IRS, ordered Whop to hold the funds or the account.
* **Platform request**: the platform the business runs under asked Whop to suspend it.

Whop handles most dispute exposure through [payment controls](https://docs.whop.com/trust-and-safety/account-health/controls.md) and [reserves](https://docs.whop.com/trust-and-safety/account-health/reserves.md). A suspension for excessive dispute activity comes only after the Trust & Safety team manually reviews the account.

Automated systems apply some suspensions. The Trust & Safety team applies others after a review. The Trust & Safety team reviews every appealed suspension.

## Appealing a suspension

You can appeal a suspension, except for the cases listed below. You have **30 days** from the suspension notice to use Whop's suspension appeal flow. The flow delivers the case directly to the Trust & Safety team. A human reviews every appeal. For an automated suspension, the appeal is the first time a person reviews the case.

When you appeal, you can submit any information you consider relevant, including:

* Prior processing statements
* Social media accounts or your website
* Proof of fulfillment for the sales in question
* Any further context on the activity that led to the suspension

The reason for the suspension determines the standard for an appeal:

* **Sanctions and unsupported jurisdiction:** Whop lifts these only when documents show that you aren't the listed person and aren't in that jurisdiction. You can't appeal a suspension because you are in the jurisdiction.
* **Fraudulent payment activity:** Whop lifts the suspension only when evidence shows that the original finding was wrong.
* **Tax authority order:** support can't lift it. Contact Whop's legal team at [legal@whop.com](mailto:legal@whop.com).
* **Platform request:** contact the platform that requested the suspension. Whop Support can't reverse its decision.

If you don't appeal within 30 days, or the Trust & Safety team denies your appeal, the suspension stands and the funds handling below runs to its conclusion.

## Handling accounts and funds during suspensions

### Access to funds

When Whop suspends an account, it holds the balance so it can cover refunds and disputes on past sales. The reason for the suspension decides what you can do with it:

| | Fraud, sanctions, or tax order | Every other reason |
| - | - | - |
| **Refund your buyers** | After Whop reviews the account. | Yes, from your available balance. |
| **Withdraw your balance** | After Whop reviews the account. | 120 days after the suspension. |

If Whop upholds a fraud, sanctions, or tax order suspension after its review, the funds aren't released. A sanctions suspension also holds the balance of every business the suspended person owns. For every other reason, your dashboard shows the date you can withdraw, and a new chargeback can move it out to 90 days after that chargeback.

* During the hold, Whop may refund buyers from the held balance for disputed, undelivered, or misrepresented transactions.
* After the date, you can withdraw what remains to a bank account linked to your account with a standard withdrawal. Instant withdrawals, crypto withdrawals, transfers, and payments to other people stay unavailable for as long as the suspension lasts.
* Money in a reserve becomes available on the reserve's own schedule. Suspension doesn't shorten it, and you can't refund buyers from it.

### Refund requests

If a suspension blocks your access to the Whop Resolution Center:

* Find the creator's store or your receipt so you can contact them directly.
* Refund requests must follow Whop's policy and eligibility criteria.

## Permanent bans

Repeated or serious violations, such as fraudulent payment activity, can result in a permanent ban.

Whop adds a permanently banned individual to its platform blocklist. They can't open a new business or user account. You can appeal a permanent ban within the same 30-day window as any suspension. Whop lifts it only when evidence shows that the original finding was wrong.

## Need more help

If you have questions or a situation that isn't covered here, contact [Whop Support](https://whop.com/support) with all relevant details. Understanding and following the platform's policies helps keep the experience fair and smooth for everyone.


## Related topics

- [Community Guidelines](https://docs.whop.com/trust-and-safety/trust-safety-overview/community-guidelines.md)
- [Prohibited Businesses](https://docs.whop.com/trust-and-safety/trust-safety-overview/what-is-not-allowed-on-whop.md)
- [Manage Disputes](https://docs.whop.com/manage-your-business/manage-payments/manage-disputes.md)
