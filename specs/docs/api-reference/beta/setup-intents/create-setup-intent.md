> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Create Setup Intent

> Saves a buyer's payment method for later without charging it. Pass a `confirmation_token` for a method the buyer just supplied through the payment elements in setup mode, or a `payment_method_id` already on file to re-verify it. The response is the setup intent as created, not its outcome: while it is `requires_action` the buyer still has a step, so hand `client_secret` to the elements' `handleNextAction` or poll Retrieve setup status. A buyer's own token holding `member:payment_methods:use` may create a setup intent for itself from a confirmation token.



## OpenAPI

<!-- OpenAPI source: `post /setup_intents` in specs/api-v1-native.json (inlined by docs.whop.com; stripped on download) -->