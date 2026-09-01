> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# API Stability

> Which state every Current API resource is in: current, available on both surfaces, or Legacy-only, and where to build.

Whop serves two API surfaces. The **Current API** is the versioned surface that new integrations should build on. The **Legacy API** predates it and stays fully supported, but new capability lands on the Current API first. Every resource is in one of three states.

## Current (Current API only)

Build on these directly. They have no Legacy counterpart.

API Keys · Ad Campaigns · Ad Groups · Ads · Audiences · Bounty Submissions · Deposits · Events · Exports · Media · Partners · Payment Method Domains · People · Permissions · Plans · Recommended Actions · Social Accounts · Swaps · Team Members · Transfers · Users

## Available on both surfaces

These exist on the Legacy API and the Current API. Build new integrations against the Current API column; existing Legacy integrations keep working.

| Legacy resource                                                                          | Current API resource                                                                          |
| ---------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| [App builds](/api-reference/app-builds/app-build)                                        | [App builds](/api-reference/beta/app-builds/app-build)                                        |
| [Apps](/api-reference/apps/app)                                                          | [Apps](/api-reference/beta/apps/app)                                                          |
| Bounties                                                                                 | [Bounties](/api-reference/beta/bounties/bounty)                                               |
| [Card transactions](/api-reference/card-transactions/card-transaction)                   | [Cards](/api-reference/beta/cards/card)                                                       |
| [Checkout configurations](/api-reference/checkout-configurations/checkout-configuration) | [Checkout configurations](/api-reference/beta/checkout-configurations/checkout-configuration) |
| [Companies](/api-reference/companies/company)                                            | [Accounts](/api-reference/beta/accounts/account), same object, renamed                        |
| [Dispute alerts](/api-reference/dispute-alerts/dispute-alert)                            | [Dispute alerts](/api-reference/beta/dispute-alerts/dispute-alert)                            |
| [Files](/api-reference/files/file)                                                       | Files                                                                                         |
| [Disputes](/api-reference/disputes/dispute)                                              | [Disputes](/api-reference/beta/disputes/dispute)                                              |
| [Ledger accounts](/api-reference/ledger-accounts/ledger-account)                         | Ledgers                                                                                       |
| [Members](/api-reference/members/member)                                                 | [Members](/api-reference/beta/members/member)                                                 |
| [Memberships](/api-reference/memberships/membership)                                     | [Memberships](/api-reference/beta/memberships/membership)                                     |
| Notifications                                                                            | Notifications                                                                                 |
| [Payments](/api-reference/payments/payment)                                              | Payments (status and return URL operations)                                                   |
| [Payout methods](/api-reference/payout-methods/payout-method)                            | [Payouts](/api-reference/beta/payouts/payout)                                                 |
| [Products](/api-reference/products/product)                                              | [Products](/api-reference/beta/products/product)                                              |
| [Promo codes](/api-reference/promo-codes/promo-code)                                     | Promo codes                                                                                   |
| [Resolution center cases](/api-reference/resolution-center-cases/resolution-center-case) | Resolution center cases                                                                       |
| [Setup intents](/api-reference/setup-intents/setup-intent)                               | Setup intents (status and return URL operations)                                              |
| [Shipments](/api-reference/shipments/shipment)                                           | Shipments                                                                                     |
| Stats                                                                                    | [Stats](/api-reference/beta/stats/stats)                                                      |
| [Verifications](/api-reference/verifications/verification)                               | [Verifications](/api-reference/beta/verifications/verification)                               |
| [Webhooks](/api-reference/webhooks/webhook)                                              | Webhooks                                                                                      |

## Legacy only

These have no Current API successor. Using them is fine, and they stay supported.

Access tokens · Account links · Ad reports · Affiliates · AI chats · Authorized users · Chat channels · Company token transactions · Course chapters · Course lesson interactions · Course lessons · Course students · Courses · DM channels · DM members · Entries · Experiences · Fee markups · Forum posts · Forums · Identity profiles · Invoices · Leads · Messages · Payment methods · Payout accounts · Reactions · Refunds · Reviews · Support channels · Topups
