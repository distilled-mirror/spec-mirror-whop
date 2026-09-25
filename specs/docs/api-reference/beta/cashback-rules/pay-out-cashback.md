> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Pay Out Cashback

> Distributes cashback on demand from the authenticated platform's available USD balance to its direct connected accounts. Requires payout:transfer_funds. Optional filters combine; an empty body includes all eligible transactions. Only completed, unpaid transactions created before this request are considered. The latest matching rule wins; its funding account must be the authenticated platform. Amounts are calculated when processed. Returns status `processing` and echoes supplied filters when background processing is queued. Status `failed` with HTTP 200 means the queue rejected the request. This is not a payment confirmation. Failed transaction jobs retry automatically; insufficient funds requires adding USD to the funding wallet. Supports Idempotency-Key, and overlapping requests cannot pay the same card transaction twice.



## OpenAPI

<!-- OpenAPI source: `post /cashback_rules/payout` in specs/api-v1-native.json (inlined by docs.whop.com; stripped on download) -->