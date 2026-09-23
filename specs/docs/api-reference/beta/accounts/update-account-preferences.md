> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Update Account Preferences

> Updates the account's preferences. Each top-level key present in the body is replaced as a whole; omitted keys are left untouched.

Required scopes depend on the preferences being updated:

| Preferences | Required scope |
| --- | --- |
| `ads_payment_methods`, `ads_reporting_currency`, `ads_scheduling_timezone`, `ads_triple_whale_integration`, `ads_certifications` | `ad_campaign:create` |
| `cards_auto_top_up`, `cards_notifications` | `payout:account:update` |
| `dispute_fighter_enabled` | `payment:dispute` |
| `economic_intelligence` | `company:update` |

When updating preferences from multiple rows, all corresponding scopes are required for the account.




## OpenAPI

<!-- OpenAPI source: `patch /accounts/{account_id}/preferences` in specs/api-v1-native.json (inlined by docs.whop.com; stripped on download) -->