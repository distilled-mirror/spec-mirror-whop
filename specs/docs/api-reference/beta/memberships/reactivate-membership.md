> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Reactivate Membership

> Restores access to a `canceled` or `expired` membership that contains only one-time purchases and sets its `status` to `completed`. Lifetime memberships regain lifetime access. For memberships with an expiration, `days` sets `current_period_end` that many days from now; without it the original `current_period_end` is kept, so `days` is required once that has passed. Active and recurring memberships cannot be reactivated.



## OpenAPI

<!-- OpenAPI source: `post /memberships/{id}/reactivate` in specs/api-v1-native.json (inlined by docs.whop.com; stripped on download) -->