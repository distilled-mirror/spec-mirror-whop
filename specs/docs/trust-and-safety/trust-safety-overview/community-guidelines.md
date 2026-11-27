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

# Community Guidelines

> Standards for conduct and content on Whop, including prohibited conduct, review rules, earnings claims, enforcement, and how to report violations.

Whop's Community Guidelines set the standards for behavior and content on the platform. They apply to all users, sellers and buyers alike, so the platform stays safe, trusted, and built on mutual respect.

These guidelines apply to all content on Whop, including store pages, chats, forum posts, and direct messages. They protect your ability to create, share, and engage freely while keeping the community safe.

These Community Guidelines cover:

* **Prohibited conduct and content** — conduct and content that isn't permitted on Whop.
* **Reviews** — Whop's policies and guidelines for reviews.
* **Earnings claims** — requirements for content that makes income or earnings statements.
* **Enforcement** — what happens if you violate the Terms of Service or these guidelines.
* **Reporting** — how to report violations of these guidelines.

## What the guidelines require

### 1. Prohibited conduct and content

You may not engage in any of the following:

* **Adult and sexual content** — Pornographic content, sexually explicit material, or adult services. Sexually suggestive content involving minors is strictly prohibited.
* **Age requirements** — Misrepresenting your age or allowing minors to use Whop in violation of the Terms of Service.
* **Child safety** — Creating, sharing, or soliciting content that exploits, sexualizes, or endangers minors, including any child sexual abuse material.
* **Dangerous organizations** — Promoting, supporting, or representing terrorist organizations, violent extremist groups, or organized criminal enterprises.
* **Fraud and deceptive practices** — Scams, phishing, pyramid schemes, misleading product descriptions, fake reviews, false earnings claims, or any other deceptive business practice. This includes manipulating pricing or artificially inflating engagement metrics.
* **Hacking and security violations** — Tampering with security features, bypassing content restrictions, reverse engineering code, spreading malware, or disrupting servers and networks.
* **Harassment and hate speech** — Unsolicited, unwelcome, disrespectful, abusive, or offensive behavior toward any user. This includes user-targeting attempts such as coordinated bans, mass reporting, or attacks on other users' social media accounts; unwanted physical intimidation; doxxing or coordinated harassment ("raids"); and depicting individuals negatively based on protected characteristics such as race, ethnicity, religion, gender, age, sexual orientation, or disability. Content that promotes hatred, discrimination, or violence based on these characteristics is strictly prohibited.
* **Illegal activity** — Using Whop or selling products for any unlawful purpose, or violating local, state, national, or international laws.
* **Impersonation and fraud** — Pretending to be someone else, misrepresenting your identity or affiliation, or accessing another user's account without permission.
* **Intellectual property violations** — Selling, leaking, or disclosing proprietary data or information obtained through Whop; infringing third-party copyrights, trademarks, or proprietary rights; or selling counterfeit goods or unauthorized replicas.
* **Payment and transaction abuse** — Payment fraud, chargeback abuse, money laundering, or other financial misconduct.
* **Platform manipulation** — Creating fake accounts, artificially inflating engagement or sales metrics, evading bans, or coordinating inauthentic behavior.
* **Privacy violations** — Collecting or sharing personal information of other users or third parties without consent, or doxxing and publicly sharing private information to harass or intimidate.
* **Prohibited products and services** — Selling items or services listed in the [Prohibited Businesses](https://docs.whop.com/trust-and-safety/trust-safety-overview/what-is-not-allowed-on-whop.md) policy, including weapons, illegal drugs, pharmaceuticals without authorization, and other restricted items.
* **Self-harm content** — Promoting, glorifying, or providing instructions for suicide, self-injury, or eating disorders.
* **Solicitation and spam** — Sending unsolicited offers, advertisements, or promotions to other users, or posting repetitive or irrelevant content.
* **Unauthorized data access** — Using bots, scrapers, crawlers, or other automated tools to access or collect data from Whop.
* **Violent and graphic content** — Sharing excessively violent, gory, or disturbing content intended to shock or disgust.

### 2. Reviews

Reviews help prospective customers make informed purchase decisions. To keep them fair and transparent, all reviews must follow these guidelines:

* **Honest and accurate** — Reviews must reflect genuine opinions, findings, and experiences. False, misleading, or unsubstantiated information isn't allowed.
* **Full disclosure** — If you have any connection to the creator that could influence your review, such as employment, free products, or an affiliate relationship, you must clearly disclose it.
* **Incentivized reviews** — Creators who offer incentives for reviews must inform users of these guidelines, monitor the content, and report non-compliant reviews to [support@whop.com](mailto:support@whop.com). You may not offer an incentive for a fake review.

Whop doesn't approve reviews that contain:

* **Spam or repetitive content** — Excessive symbols, distracting text, or irrelevant images.
* **Private information** — Your own or others' personal details, such as phone numbers, email addresses, mailing addresses, or license plates.
* **External links** — Links aren't allowed, including those with referrer tags or affiliate codes, to prevent phishing and malware risks.
* **Prohibited content** — Anything that violates these Community Guidelines.

If you come across a review that violates these guidelines, report it with the report button next to the review.

### 3. Earnings claims

When your content contains express or implied statements about the ability to earn income ("earnings claims"), you must:

* Ensure earnings claims aren't false, deceptive, or misleading in any respect, including by implying that atypical results are typical.
* Have a reasonable basis to substantiate any earnings claim you make.
* Account for material expenses incurred in generating income when an earnings claim reflects gross income.
* Ensure earnings claims in testimonials are accurate and either representative of generally expected results or accompanied by a clear and conspicuous disclosure of relevant context, such as time and effort expended, and typical and ordinary earnings.
* Comply with all applicable legal requirements, including Federal Trade Commission (FTC) guidelines.

### 4. Enforcement

Violating these Community Guidelines may result in content removal, account suspension, a permanent ban, legal action, or a report to law enforcement. The severity of the action depends on the nature and frequency of the violation.

If you violate these guidelines, Whop may take one or more of the following actions:

* Remove content
* Limit platform access
* Suspend your account
* Withhold payouts
* Claw back withdrawals from your bank account
* Refund customers

Whop enforces these measures to maintain a fair and secure marketplace for all users. You can appeal an enforcement decision by contacting [support@whop.com](mailto:support@whop.com) within 30 days of notification.

### 5. Reporting

If you encounter content or behavior that violates these guidelines, follow [Report a bad actor](https://docs.whop.com/trust-and-safety/staying-safe/report-a-bad-actor.md) or email [support@whop.com](mailto:support@whop.com). Whop reviews all reports and takes appropriate action. Submitting false or bad-faith reports may result in action on your account.

## Next steps

> [card] [Prohibited businesses](https://docs.whop.com/trust-and-safety/trust-safety-overview/what-is-not-allowed-on-whop.md)
>
> Review the categories of products and services prohibited on Whop.

> [card] [Account suspensions](https://docs.whop.com/trust-and-safety/suspensions/account-suspensions.md)
>
> Learn what happens when an account is suspended and how appeals work.


## Related topics

- [Prohibited Businesses](https://docs.whop.com/trust-and-safety/trust-safety-overview/what-is-not-allowed-on-whop.md)
- [Paid Groups](https://docs.whop.com/supported-business-models/paid-groups.md)
- [Account Suspensions](https://docs.whop.com/trust-and-safety/suspensions/account-suspensions.md)
