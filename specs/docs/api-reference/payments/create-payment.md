> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Create payment

> Charge a buyer on-session with a `confirmation_token` for the method they selected, or charge an existing member off-session using a stored payment method. You can provide an existing plan or create one inline. The endpoint returns a payment immediately, but processing continues asynchronously. Use webhooks to learn whether it succeeds or fails, and poll the payment's status endpoint for any step the buyer must complete.

Required permissions:
 - `payment:charge`
 - `plan:create`
 - `access_pass:create`
 - `access_pass:update`
 - `plan:basic:read`
 - `access_pass:basic:read`
 - `member:email:read`
 - `member:basic:read`
 - `member:phone:read`
 - `promo_code:basic:read`
 - `shipment:basic:read`
 - `payment:dispute:read`
 - `payment:resolution_center_case:read`



## OpenAPI

<!-- OpenAPI source: `post /payments` in specs/api-v1-stable.json (inlined by docs.whop.com; stripped on download) -->