> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Retry a Failed Ad Campaign Payment

> Queues a background payment retry for the campaign's entire account, including other campaigns with failed payments. Deprecated: API versions 2026-09-22 and later return 410 Gone. Use POST /accounts/{id}/retry_ads_payment instead. The returned campaign does not confirm payment success; read delivery_status and issues for the outcome.



## OpenAPI

<!-- OpenAPI source: `post /ad_campaigns/{id}/retry_payment` in specs/api-v1-native.json (inlined by docs.whop.com; stripped on download) -->