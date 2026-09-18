> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Create a Partner Referral Request

> Creates a pending manual request for an existing business as the authenticated, enrolled, verified Whop partner. Provide exactly one of account_id or account_url. Whop business and product links resolve to their business. A business owner must accept before attribution changes. An existing pending manual request from the same partner returns 200; a new request returns 201. Use a Whop login session or an account API key with `partner:referral_request:create`. The key must have been created by the account's current owner. Account API keys submit requests as their account owner, who must be enrolled, verified, and not suspended.



## OpenAPI

<!-- OpenAPI source: `post /partner_referral_requests` in specs/api-v1-native.json (inlined by docs.whop.com; stripped on download) -->