> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Resume a Parked Renewal

> Starts a fresh on-session attempt with the saved card for a subscription renewal that is waiting on the customer to authenticate; the bank's step then arrives in `next_action` on the following status reads. Only the payment's own customer may call it — with the payment's `client_secret` or their own session — and it is a no-op for any payment that is not a parked renewal.



## OpenAPI

<!-- OpenAPI source: `post /payments/{payment_id}/resume` in specs/api-v1-native.json (inlined by docs.whop.com; stripped on download) -->