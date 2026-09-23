> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Create Cashback Rule

> Creates a future-dated card cashback rule funded by the authenticated platform account. Requires payout:transfer_funds. Merchant name and MCC are optional. Every supplied merchant filter must match. When both are omitted or null, scoped_account_id is required and all eligible transactions for that account match. Optionally limit the rule to one direct connected account. The funding account is derived from the credential and cannot be supplied. Creation does not transfer funds. Supports Idempotency-Key for safe retries.



## OpenAPI

<!-- OpenAPI source: `post /cashback_rule` in specs/api-v1-native.json (inlined by docs.whop.com; stripped on download) -->