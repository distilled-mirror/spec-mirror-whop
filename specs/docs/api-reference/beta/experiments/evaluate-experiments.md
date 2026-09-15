> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Evaluate Experiments

> Evaluates and records an exposure without requiring authentication. When credentials resolve, their authentication method, API key ID, and signed-in user ID are recorded on the exposure event. Pass subject for bucketing identity and account_id for experiment ownership.

Pass `flag_key` to check a single flag, or omit it to fetch active flags in the account and related resource scope. Internal anonymous callers may use the `x-whop-anonymous-id` header or `ajs_anonymous_id` cookie; explicit `subject[anonymous_id]` takes precedence.

Assignments use exactly the configured `bucket_by`: `subject[user_id]`, `subject[account_id]`, or `subject[anonymous_id]`. Internal user experiments derive identity from the signed-in session. Missing the required identity fails single evaluation and omits the experiment from batch evaluation. Subjects outside all treatment ranges receive control.

Pass `subject[account_id]` to enable account-level targeting rules. Pass `properties` as a JSON object to supply the values that `property` targeting conditions match against.

Pass `log_exposure=false` to read an assignment without recording an exposure, for a client that caches assignments up front and records the exposure when the arm is actually rendered. Omitted records the exposure, so pinned callers are unchanged.




## OpenAPI

<!-- OpenAPI source: `get /experiments/exposures` in specs/api-v1-native.json (inlined by docs.whop.com; stripped on download) -->