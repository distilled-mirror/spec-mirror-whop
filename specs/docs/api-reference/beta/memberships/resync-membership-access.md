> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Resync Membership Access

> Re-runs access fulfillment for a membership: recomputes the member's content access on Whop, re-validates their Discord link (re-adding them to the server and re-assigning roles if needed), and re-fulfills TradingView indicator access. Telegram access is invite-based and is not resynced. The work runs in the background and the outcome is written to the membership's logs.



## OpenAPI

<!-- OpenAPI source: `post /memberships/{id}/resync_access` in specs/api-v1-native.json (inlined by docs.whop.com; stripped on download) -->