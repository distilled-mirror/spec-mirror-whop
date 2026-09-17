> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Payment requires action

> Sent when the bank asks the buyer to verify an off-session charge, such as a subscription renewal or a saved-card payment your server made. `data.recovery_url` is the Whop link where the buyer signs in and completes 3D Secure; the event is only sent while that link exists, so it is null only when you lack `member:basic:read`. Then handle `payment.succeeded` or `payment.failed` for the outcome.

Required permissions:
 - `payment:basic:read`
 - `plan:basic:read`
 - `access_pass:basic:read`
 - `member:email:read`
 - `member:basic:read`
 - `member:phone:read`
 - `promo_code:basic:read`
 - `shipment:basic:read`
 - `payment:dispute:read`
 - `payment:resolution_center_case:read`
 - `webhook_receive:payments`



## OpenAPI

<!-- OpenAPI source: `webhook payment.requires_action` in specs/api-v1-stable.json (inlined by docs.whop.com; stripped on download) -->