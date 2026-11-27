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

# Dispute Monitoring Programs

> Understand the dispute and fraud monitoring programs the card networks run, the limit each one sets, and how each network counts your disputes

Every card network sets a limit on how often the payments it carries can end in a dispute or a fraud claim, and runs a monitoring program to enforce it. Cross a network's limit and it enrolls your business in its program. From there the network bills a fine for each month you stay over the limit, and if you stay long enough it can stop accepting your business's payments on its cards altogether.

The networks hold Whop, as your payment processor, responsible for keeping every business it processes for under these limits. Any fine a network assesses for your business's dispute or fraud rate is your responsibility.

> [!NOTE]
>
> The networks own these programs and revise them without notice. Use this page for orientation; the rules each network publishes decide every case.

## Network limits at a glance

| Payment method | Program | What counts | Limit |
| - | - | - | - |
| Visa | [Visa Acquirer Monitoring Program](#vamp-visa-acquirer-monitoring-program) | Chargebacks and fraud reports | 1.5% (2.2% in Central Europe, the Middle East, and Africa) |
| Visa | [Visa Secure Excessive Fraud Program](#visa-secure-excessive-fraud-program-us-only) | Fraud on 3D Secure payments, by amount, US only | 0.9% |
| Mastercard | [Excessive Chargeback Program](#ecp-mastercard-excessive-chargeback-program) | Chargebacks | 1.5% |
| Mastercard | [Excessive Fraud Merchant program](#efm-mastercard-excessive-fraud-merchant-program) | Fraud chargebacks | 0.5% |
| American Express | [Fraud Full Recourse](#american-express-fraud-full-recourse) | Fraud disputes, by amount | 0.9%; full recourse at 1.8% |
| PayPal | [PayPal claim rate](#paypal-claim-rate) | Item not received and not as described claims, by amount | 1.5% |

## What counts as a dispute

A dispute, which the networks call a chargeback, counts whatever its reason and whatever its outcome. A dispute you win still counts, and so does a dispute on a payment you had already refunded. Refunding early only helps when it stops the customer from disputing at all.

Each network sets its own measurement month and its own formula, given in the sections below. To view your dispute rate on each payment method, open [Payment health](https://docs.whop.com/trust-and-safety/account-health/payment-health.md).

## Visa monitoring programs

### VAMP: Visa Acquirer Monitoring Program

The Visa Acquirer Monitoring Program (VAMP) replaced the Visa Dispute Monitoring Program and the Visa Fraud Monitoring Program in April 2025.

Visa counts two kinds of event. The first is a chargeback. The second is a fraud report (Visa calls it a TC40): a notice from the customer's bank that a charge was fraudulent, filed whether or not a chargeback follows. A fraud report and a chargeback on the same payment are two events, not one.

Your **VAMP ratio** is the month's chargebacks plus fraud reports, divided by the Visa payments you settled in that same month. Visa leaves out disputes that Rapid Dispute Resolution (an automatic refund you've opted into) or Compelling Evidence 3.0 (proof of earlier undisputed purchases by the same customer) settles before they become chargebacks.

Visa identifies a business as **Excessive** at a VAMP ratio of 1.5% or more in North America, Europe, Asia Pacific, and Latin America and the Caribbean, and 2.2% or more in Central Europe, the Middle East, and Africa. North America, Europe, and Asia Pacific moved from 2.2% to 1.5% on April 1, 2026. Visa assesses fees on a business identified as Excessive.

Visa also holds Whop as a whole to a ratio across every business on the platform: **Above Standard** at 0.5% and **Excessive** at 0.7%. Every Visa payment on Whop counts toward it.

### Visa Secure Excessive Fraud Program (US only)

This program covers fraud reported on US domestic Visa payments that went through 3D Secure. Each calendar month, Visa compares the amount of reported fraud on those payments with the amount of sales on them.

| Level | Fraud-to-sales ratio | Consequence |
| - | - | - |
| Early warning | 0.65% | None |
| Standard | 0.9% | No fines. Instead you become liable for fraud disputes on these payments, which 3D Secure otherwise shifts to the customer's bank, until you exit the program. |

You exit after one calendar month under 0.9%.

## Mastercard monitoring programs

Mastercard runs two programs. The Excessive Chargeback Program (ECP) has two levels: Excessive Chargeback Merchant (ECM) and High Excessive Chargeback Merchant (HECM). The Excessive Fraud Merchant (EFM) program monitors fraud. Cross both in one month and you pay EFM fines only, but Mastercard counts the month toward both programs.

You exit a Mastercard program after three consecutive months under its limit.

### ECP: Mastercard Excessive Chargeback Program

Mastercard places you in ECP based on your chargeback rate: the month's chargebacks divided by the previous month's Mastercard payments. A slow January therefore raises February's rate even when February's chargebacks are normal.

| Level | Chargeback rate |
| - | - |
| ECM | 1.5%–2.99% |
| HECM | 3% or more |

The first month carries no fine. From the second month the fine rises with each month you stay in the program:

| Months in the program | ECM fine | HECM fine |
| - | - | - |
| 1 | 0 USD | 0 USD |
| 2 | 1,000 USD | 1,000 USD |
| 3 | 1,000 USD | 2,000 USD |
| 4–6 | 5,000 USD | 10,000 USD |
| 7–11 | 25,000 USD | 50,000 USD |
| 12–18 | 50,000 USD | 100,000 USD |
| 19 or more | 100,000 USD | 200,000 USD |

From the fourth month, Mastercard also charges 5 USD for every chargeback beyond the first 300 in a month.

### EFM: Mastercard Excessive Fraud Merchant program

Mastercard places you in EFM when, in a month, you meet both of these:

* A fraud chargeback rate of 0.5% or more: the month's fraud chargebacks divided by the previous month's e-commerce Mastercard payments.
* Less than 10% of your Mastercard payments authenticated with 3D Secure (50% in countries that require strong customer authentication).

The first month carries no fine. From the second month the fine rises with each month you stay in the program:

| Months in the program | Fine |
| - | - |
| 1 | 0 USD |
| 2 | 500 USD |
| 3 | 1,000 USD |
| 4–6 | 5,000 USD |
| 7–11 | 25,000 USD |
| 12–18 | 50,000 USD |
| 19 or more | 100,000 USD |

## American Express Fraud Full Recourse

American Express issues its own cards, so it codes disputes itself. Its Fraud Full Recourse program compares the amount of your fraud-coded disputes with the amount of your American Express sales. American Express identifies your business at 0.9%. At 1.8% your business is placed on full recourse and bears every fraud chargeback in full.

* **By amount.** One 2,000 USD fraud dispute on 50,000 USD of American Express sales is 4%. Two 20 USD fraud disputes on the same sales are 0.08%.
* **Fraud only.** Disputes coded as product not received, not as described, or canceled don't count.

## PayPal claim rate

PayPal doesn't run a card network program. It holds your business to a claim rate of 1.5%, measured by amount over the previous three calendar months: the amount of your item not received and significantly not as described claims, divided by your net PayPal sales (sales minus refunds).

* **Claims, not only chargebacks.** A claim the customer opens with PayPal counts, and so does a chargeback filed with the customer's bank.
* **Unauthorized-payment claims don't count.**

If your claim rate is 1.5% or higher, Whop may increase the fees on your PayPal payments. The PayPal tab in [Payment health](https://docs.whop.com/trust-and-safety/account-health/payment-health.md) shows your claim rate calculated the way PayPal calculates it.

## Next steps

> [card] [Managing dispute rates](https://docs.whop.com/trust-and-safety/account-health/managing-dispute-rates.md)
>
> Refund faster, resolve cases, and remove what's causing disputes.

> [card] [Payment health](https://docs.whop.com/trust-and-safety/account-health/payment-health.md)
>
> Your dispute rate per payment method.


## Related topics

- [Payment Health](https://docs.whop.com/trust-and-safety/account-health/payment-health.md)
- [Payment Controls](https://docs.whop.com/trust-and-safety/account-health/controls.md)
- [Managing Dispute Rates](https://docs.whop.com/trust-and-safety/account-health/managing-dispute-rates.md)
