> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Financial Activity Funds Available

> Sent when a settlement moves one release date's pending funds into the account's available balance — one event per release date that cleared, carrying that settlement's ledger line. Its posted_at is the release date at midnight UTC, the same value as settlement_time_at on every payment it clears

Required permissions:
 - `webhook_receive:financial_activity`



## OpenAPI

<!-- OpenAPI source: `webhook financial_activity.funds_available` in specs/api-v1-native.json (inlined by docs.whop.com; stripped on download) -->