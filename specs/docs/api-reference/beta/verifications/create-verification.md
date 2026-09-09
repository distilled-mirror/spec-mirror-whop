> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Create Verification

> Starts a hosted verification session for an account or user, or returns the active session when one already exists. Any fields you include in the request body are used to prefill the session. Send `documents` (with `document_type`) to instead verify the person from identity documents included in this request — no hosted session involved. Send `share_token` to reuse a verification another Sumsub account has already completed for this person, instead of verifying them again. Send `verification_id` to reuse a verification the signed-in user already completed on Whop. Every mode except `verification_id` is rejected once the account has an `approved` verification — unlink it first to start a new one — while `verification_id` replaces whichever verification of that kind the account currently has.



## OpenAPI

<!-- OpenAPI source: `post /verifications` in specs/api-v1-native.json (inlined by docs.whop.com; stripped on download) -->