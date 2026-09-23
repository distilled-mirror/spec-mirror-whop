> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Retry Failed Ads Payments

> Queues one background retry of the account's failed ads payments across its campaigns, using the account's configured ads payment methods. A queued response does not mean payment succeeded. Read campaign delivery_status and issues for the outcome. Successful settlement clears the payment block without changing configured active or paused status; legacy payment_failed status becomes paused. Another request while the account retry is queued or running returns an error asking you to wait.



## OpenAPI

<!-- OpenAPI source: `post /accounts/{id}/retry_ads_payment` in specs/api-v1-native.json (inlined by docs.whop.com; stripped on download) -->