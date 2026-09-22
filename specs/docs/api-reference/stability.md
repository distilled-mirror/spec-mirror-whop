> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# API Stability

> Which state every Current API resource is in: current, available on both surfaces, or Legacy-only, and where to build.

Whop serves two API surfaces. The **Current API** is the versioned surface that new integrations should build on. The **Legacy API** predates it and stays fully supported, but new capability lands on the Current API first. Every resource is in one of three states.

## Current (Current API only)

Build on these directly. They have no Legacy counterpart.

API Keys · Ad Campaigns · Ad Groups · Ads · Audiences · Bounty Submissions · Events · Exports · Media · Partners · Payment Method Domains · People · Permissions · Plans · Recommended Actions · Social Accounts · Swaps · Transfers · Users

## Available on both surfaces

These exist on the Legacy API and the Current API. Build new integrations against the Current API column; existing Legacy integrations keep working.

| Legacy resource                                                                          | Current API resource                                                                                        |
| ---------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| [Access tokens](/api-reference/access-tokens/access-token)                               | [Access tokens](/api-reference/access-tokens/access-token)                                                  |
| Ad reports                                                                               | [Stats](/api-reference/beta/stats/stats)                                                                    |
| [App builds](/api-reference/app-builds/app-build)                                        | [App builds](/api-reference/beta/app-builds/app-build)                                                      |
| [Apps](/api-reference/apps/app)                                                          | [Apps](/api-reference/beta/apps/app)                                                                        |
| [Authorized users](/api-reference/authorized-users/authorized-user)                      | [Team members](/api-reference/beta/team-members/team-member)                                                |
| Bounties                                                                                 | [Bounties](/api-reference/beta/bounties/bounty)                                                             |
| [Card transactions](/api-reference/card-transactions/card-transaction)                   | [Cards](/api-reference/beta/cards/card)                                                                     |
| [Checkout configurations](/api-reference/checkout-configurations/checkout-configuration) | [Checkout configurations](/api-reference/beta/checkout-configurations/checkout-configuration)               |
| Companies                                                                                | [Accounts](/api-reference/beta/accounts/account), same object, renamed                                      |
| [Dispute alerts](/api-reference/dispute-alerts/dispute-alert)                            | [Dispute alerts](/api-reference/beta/dispute-alerts/dispute-alert)                                          |
| [Disputes](/api-reference/disputes/dispute)                                              | [Disputes](/api-reference/beta/disputes/dispute)                                                            |
| Fee markups                                                                              | [Account fees](/api-reference/beta/accounts/account#fee-markups)                                            |
| [Files](/api-reference/files/file)                                                       | [Files](/api-reference/beta/files/list-files)                                                               |
| [Identity profiles](/api-reference/identity-profiles/identity-profile)                   | [Verifications](/api-reference/beta/verifications/verification)                                             |
| [Ledger accounts](/api-reference/ledger-accounts/ledger-account)                         | [Account balances](/api-reference/beta/accounts/account#attributes)                                         |
| [Members](/api-reference/members/member)                                                 | [Members](/api-reference/beta/members/member)                                                               |
| [Memberships](/api-reference/memberships/membership)                                     | [Memberships](/api-reference/beta/memberships/membership)                                                   |
| Notifications                                                                            | [Notifications](/api-reference/beta/notifications/list-notifications)                                       |
| [Payment methods](/api-reference/payment-methods/payment-method)                         | [Payment methods](/api-reference/payment-methods/payment-method)                                            |
| [Payments](/api-reference/payments/payment)                                              | [Payments](/api-reference/beta/payments/list-payments)                                                      |
| [Payout accounts](/api-reference/payout-accounts/payout-account)                         | [Saved payout methods](/api-reference/beta/payouts/list-saved-payout-methods)                               |
| [Payout methods](/api-reference/payout-methods/payout-method)                            | [Payouts](/api-reference/beta/payouts/payout)                                                               |
| [Products](/api-reference/products/product)                                              | [Products](/api-reference/beta/products/product)                                                            |
| [Promo codes](/api-reference/promo-codes/promo-code)                                     | [Promo codes](/api-reference/beta/promo-codes/list-promo-codes)                                             |
| [Refunds](/api-reference/refunds/refund)                                                 | [Refunds](/api-reference/beta/refunds/list-refunds)                                                         |
| [Resolution center cases](/api-reference/resolution-center-cases/resolution-center-case) | [Resolution center cases](/api-reference/beta/resolution-center-cases/list-resolution-center-cases)         |
| [Setup intents](/api-reference/setup-intents/setup-intent)                               | [Setup intents](/api-reference/beta/setup-intents/retrieve-setup-status) (status and return URL operations) |
| [Shipments](/api-reference/shipments/shipment)                                           | [Shipments](/api-reference/beta/shipments/list-shipments)                                                   |
| Stats                                                                                    | [Stats](/api-reference/beta/stats/stats)                                                                    |
| [Topups](/api-reference/topups/topup)                                                    | [Deposits](/api-reference/beta/deposits/deposit)                                                            |
| [Verifications](/api-reference/verifications/verification)                               | [Verifications](/api-reference/beta/verifications/verification)                                             |
| [Webhooks](/api-reference/webhooks/webhook)                                              | [Webhooks](/api-reference/beta/webhooks/list-webhooks)                                                      |

## Legacy only

These have no Current API successor. Using them is fine, and they stay supported.

Account links · Affiliates · AI chats · Chat channels · Company token transactions · Course chapters · Course lesson interactions · Course lessons · Course students · Courses · DM channels · DM members · Entries · Experiences · Forum posts · Forums · Invoices · Leads · Messages · Reactions · Reviews · Support channels
