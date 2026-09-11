> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Update Cashback Rule

> Updates a cashback rule funded by the authenticated platform account. Requires payout:transfer_funds. Only merchant_name, merchant_category_code, description, and expires_at can change; starts_at, rate_bps, funding_account_id, and scoped_account_id are immutable. Omitted fields stay unchanged. Scheduled, active, and expired rules can be updated; discarded rules cannot. Updating a rule does not transfer funds.



## OpenAPI

<!-- OpenAPI source: `patch /cashback_rules/{id}` in specs/api-v1-native.json (inlined by docs.whop.com; stripped on download) -->