> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Cancel Trade

> Cancels every order in an order trade, including attached take-profit and stop-loss. This doesn't close filled positions. Returns a new cancellation trade whose `trade_id` points to the original, which is left unchanged. Cancellation works even while opening new positions is disabled. Requires an `Idempotency-Key`.



## OpenAPI

<!-- OpenAPI source: `post /trades/{id}/cancel` in specs/api-v1-native.json (inlined by docs.whop.com; stripped on download) -->