> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Create Payment

> Charges a buyer for a plan. Pass a payment method already on file (`member_id` and `payment_method_id`), or a `confirmation_token` describing a method the buyer just supplied. Collection runs in the background: the response is the payment as created, not its outcome — poll Retrieve status for how far it has got and, for a confirmation-token payment, what the buyer must still do. `plan_id` names the plan to charge for.



## OpenAPI

<!-- OpenAPI source: `post /payments` in specs/api-v1-native.json (inlined by docs.whop.com; stripped on download) -->