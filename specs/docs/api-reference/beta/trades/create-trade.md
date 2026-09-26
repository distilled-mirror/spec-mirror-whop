> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Create Trade

> Submits perpetual orders from a funded trading wallet. Send several limit orders for a ladder, or attach `take_profit` and `stop_loss` to a single entry order. Whop's builder fee is approved and attached automatically. The returned `trop_` ID identifies the submission, not a position, and `completed` doesn't mean filled: check each order acknowledgement, and read live orders and positions from the account's `trading` field. Requires an `Idempotency-Key`. Early beta: email support@whop.com for access.



## OpenAPI

<!-- OpenAPI source: `post /trades` in specs/api-v1-native.json (inlined by docs.whop.com; stripped on download) -->