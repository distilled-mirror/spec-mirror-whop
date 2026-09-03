> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# List Payments

> Lists payments, newest first. Without filters this is every payment the caller can read: a company credential's own account, or for a user every account they can read payments for. Filters narrow by account, buyer, product, plan, membership, status, billing reason, currency, and creation window. Filtering by `billing_reason=subscription_cycle` also matches renewals recorded as `subscription_update`. `settlement_time_at` is null on list rows — retrieve the payment for it.



## OpenAPI

<!-- OpenAPI source: `get /payments` in specs/api-v1-native.json (inlined by docs.whop.com; stripped on download) -->