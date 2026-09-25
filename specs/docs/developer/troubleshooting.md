> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Troubleshooting

> Fix common Whop API, OAuth, checkout, webhook, and Whop Elements errors.

When something breaks, figure out what's failing, check you're hitting the right environment, and look at the error response. Most issues come down to: wrong credential, sandbox/production mismatch, missing permission, or a webhook handler that isn't responding correctly.

## Quick triage

| Symptom                                                 | First check                                                                                                                              | Go deeper                                         |
| ------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------- |
| API calls return `4xx` or `5xx`                         | Check the status code, response body, endpoint, and environment.                                                                         | [API request errors](#api-request-errors)         |
| API calls return `401`                                  | Confirm the API key is present on the server and belongs to the same environment as the API base URL.                                    | [API authentication](#api-authentication)         |
| API calls return `403`                                  | Check that the app, user, or account has the required permission for the resource.                                                       | [Permissions and scopes](#permissions-and-scopes) |
| OAuth redirects back with `error`                       | Read `error` and `error_description` from the callback URL.                                                                              | [OAuth errors](#oauth-errors)                     |
| The Checkout element fails to load or the payment fails | Attach `onError`, inspect the plan or checkout configuration you passed in, and let the buyer retry inside the element.                  | [Checkout errors](#checkout-errors)               |
| Webhooks don't arrive or keep retrying                  | Verify the endpoint is public, returns `2xx` quickly before long-running work, and uses the raw request body for signature verification. | [Webhook delivery](#webhook-delivery)             |
| A Whop Element reports `onError`                        | Log `message` and `code`, then confirm the access token and the options you passed.                                                      | [Element errors](#element-errors)                 |

## API request errors

Whop returns standard HTTP status codes. When a request fails, the response body tells you why.

Most errors include a code and a message:

```json theme={null}
{
	"error": {
		"code": "invalid_request",
		"message": "Missing required parameter: company_id"
	}
}
```

Some OAuth endpoints return the OAuth-standard `error` and `error_description` fields instead. See [OAuth errors](#oauth-errors) for those cases.

| Status | Meaning                                                     | What to do                                                                                                                                                                                |
| ------ | ----------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `400`  | The request has an invalid shape or lacks a required field. | Check required parameters, enum values, IDs, and nested object shape.                                                                                                                     |
| `401`  | The request is missing valid authentication.                | Confirm the API key or access token is present, valid, and sent from your server.                                                                                                         |
| `403`  | The credential is valid but can't perform the action.       | Check app permissions, OAuth scopes, account ownership, and connected-account context.                                                                                                    |
| `404`  | The resource or endpoint wasn't found.                      | Confirm the route, API version, and Whop tag ID such as `biz_`, `plan_`, `mem_`, or `user_`.                                                                                              |
| `409`  | The request conflicts with the current resource state.      | Fetch the latest resource state and retry only if the action still applies.                                                                                                               |
| `422`  | The request is valid JSON but fails validation.             | Show the validation message to the user so they can fix their input.                                                                                                                      |
| `429`  | The client sent too many requests in a short window.        | For `/api/v1` requests, Whop limits authenticated API calls to 600 requests per minute per operation and API credential. Back off until the retry window in the error message has passed. |
| `5xx`  | Whop couldn't complete the request.                         | Retry with backoff for idempotent operations, then contact support if it persists.                                                                                                        |

### Rate limits

For `/api/v1` requests, Whop tracks request volume per API operation and API credential. The default limit is 600 requests per minute.

When you exceed the limit, the API returns `429` with:

```json theme={null}
{
	"error": {
		"type": "rate_limit_exceeded",
		"message": "Try again in 12 seconds."
	}
}
```

Wait for the delay in the message before retrying. For idempotent requests, retry with exponential backoff. Don't retry validation, authentication, or permission errors without changing the request.

## API authentication

Use the credential that matches where the code runs.

| Credential         | Use it when                                                | Common mistake                                                                |
| ------------------ | ---------------------------------------------------------- | ----------------------------------------------------------------------------- |
| Account API key    | Your server acts for your own account.                     | Putting the key in browser, mobile, or public client code.                    |
| App API key        | Your app server acts for accounts that installed your app. | Calling endpoints for an account that hasn't installed or authorized the app. |
| OAuth access token | A signed-in user grants your app access to their account.  | Treating a user-scoped token like an account-wide API key.                    |
| iframe user token  | A Whop app verifies the user inside an iframe request.     | Skipping `verifyUserToken` or forwarding the token to unrelated clients.      |

If an API request fails:

1. Confirm the environment matches the key: the SDK's `Production` environment (the default) for production keys and `Sandbox` for sandbox keys, or `https://api.whop.com/api/v1` and `https://sandbox-api.whop.com/api/v1` when you call the API directly.
2. Confirm IDs use Whop tag prefixes like `biz_`, `user_`, `mem_`, `plan_`, `prod_`, `app_`, `pay_`, or `ch_`, not internal numeric IDs.
3. Log the HTTP status, response body, endpoint, and request ID if Whop returns one.
4. Retry only when the error is transient or rate-limited. Don't retry `401`, `403`, or validation errors without changing the request.

<Warning>
  Never expose Account API keys or App API keys in client-side code. Browser, mobile, and iframe code should call your server, and your server should call Whop.
</Warning>

## Permissions and scopes

`403` and `insufficient_scope` errors usually mean the credential is valid but can't perform that action.

* For Whop apps, confirm the app requests the required permissions in [Permissions](/developer/guides/permissions), then save the app settings and reinstall or reauthorize when needed.
* For OAuth, request only the scopes your feature needs, then make sure the user completed the OAuth flow after you added those scopes.
* For dashboard-created API keys, confirm the key belongs to the account whose ID you pass (`company_id` or `account_id`, depending on the endpoint).
* For connected accounts, confirm whether the parent or child account should own the action before choosing the account you pass.

## OAuth errors

OAuth errors return a standard payload or redirect query string with `error` and `error_description`.

```json theme={null}
{
	"error": "invalid_grant",
	"error_description": "Authorization code has expired"
}
```

| Error                       | Meaning                                                                                                   | Fix                                                                                                |
| --------------------------- | --------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| `invalid_request`           | A required parameter is missing or malformed.                                                             | Check `client_id`, `redirect_uri`, `response_type`, `code_challenge`, and `code_challenge_method`. |
| `invalid_grant`             | The authorization code, refresh token, or access token has expired, was already used, or Whop revoked it. | Restart the OAuth flow or ask the user to sign in again. Authorization codes are single-use.       |
| `invalid_client`            | Whop doesn't recognize the app client.                                                                    | Confirm you are using the correct app and environment.                                             |
| `invalid_scope`             | The requested scope isn't valid or recognized.                                                            | Check available scopes in your app settings and remove any typos.                                  |
| `insufficient_scope`        | The token doesn't include the scope required by the endpoint.                                             | Add the scope in your app settings and re-run OAuth consent.                                       |
| `access_denied`             | The user or account denied the authorization request.                                                     | The user chose not to grant access, or the account restricts this app.                             |
| `unsupported_response_type` | The `response_type` parameter isn't supported.                                                            | Use `response_type=code` for the authorization code flow.                                          |
| `unsupported_grant_type`    | The `grant_type` parameter isn't supported.                                                               | Use `authorization_code` or `refresh_token`.                                                       |
| `rate_limit_exceeded`       | OAuth authorize, token, or introspection requests hit a short-window limit.                               | Back off and respect the `Retry-After` header if present.                                          |

<Tip>
  Redirect URIs must match exactly. If you configured `https://example.com/oauth/callback`, `https://example.com/oauth/callback/` is a different URI.
</Tip>

## Checkout errors

The Checkout element and iOS checkout can fail for different reasons. Debug them the same way: check the plan or checkout configuration, inspect the error callback, and confirm fulfillment on your server.

### Checkout element

* Attach `onError`. It runs when the element fails to load or crashes, with a `message` and a `code` for programmatic handling.
* If you pass `plan`, confirm the plan exists, is active, and belongs to the account you expect.
* If you pass `checkoutConfiguration`, confirm your server created it and that you passed its `ch_` ID.
* Every option is set at creation. To change the order, mount a new checkout instead of calling `update()`.
* A failed payment reopens the same checkout with the reason shown, so the buyer can pay again without a remount.
* Provide a `returnUrl` on a route that can render success and retry states. An off-site step such as 3DS or a bank page returns the buyer there.
* Use webhooks for fulfillment. The redirect is useful for UI, but your server should rely on `payment.succeeded`.

### `iOS` checkout

Handle expected and unexpected failures with different paths:

```swift theme={null}
do {
    let result = try await checkout.purchase(planId)
    print("Receipt ID: \(result.receiptId)")
} catch WhopCheckoutError.notConfigured {
    // SDK was not set up. Call WhopCheckout.configure() first.
} catch WhopCheckoutError.cancelled {
    // The user dismissed checkout. This is not a payment failure.
} catch WhopCheckoutError.paymentFailed(let message) {
    showError(message)
} catch {
    showError("Something went wrong. Please try again.")
}
```

## Webhook delivery

<Warning>
  `webhooks.unwrap` and `verifyUserToken` are not available in the current `@whop/sdk`. Guidance on this page that names them is kept for reference.
</Warning>

Common webhook issues:

* **No event received**: confirm the webhook URL is publicly reachable, uses `https`, and belongs to the right account and environment.
* **Signature verification fails**: pass the raw request body to `whopsdk.webhooks.unwrap`. Don't parse JSON before verification.
* **Retries keep happening**: return a `2xx` response quickly after verification, before starting long-running work in a background job.
* **Duplicate fulfillment**: Whop delivers webhooks at least once. Store `webhook-id` or another event identifier and make the handler idempotent.
* **Unexpected order**: don't assume events arrive in chronological order. Fetch the current resource state when ordering matters.

For local development, forward a public URL to your machine with a tunnel and use that URL in the dashboard webhook settings.

## Element errors

Every [Whop Element](/elements/latest/getting-started) accepts an `onError` callback. It runs when the element fails to load or crashes, and the value carries a `message`, an optional `code` for programmatic handling, and a `sourceKey` when a host-state source failed.

Always attach it while developing:

```tsx theme={null}
<CheckoutElement
	onError={(error) => {
		console.error("CheckoutElement failed", error.code, error.message);
	}}
/>
```

If an element fails to load:

1. Confirm the access token was minted for the account you expect and carries the scopes the element's reference page lists.
2. On your own domain, always pass an `accessToken`. The viewer's session only works on whop.com.
3. Confirm the container exists before calling `mount`. React components mount themselves.
4. Listen for `onReady` so you know whether the element initialized.
5. If the element enters an unrecoverable state, call `destroy()` and create a new one.

The [Elements troubleshooting table](/elements/latest/getting-started#troubleshooting) lists the common symptoms and their causes.

## Sandbox and production

Sandbox data and production data are separate.

| Environment | Dashboard                  | API                                   |
| ----------- | -------------------------- | ------------------------------------- |
| Sandbox     | `https://sandbox.whop.com` | `https://sandbox-api.whop.com/api/v1` |
| Production  | `https://whop.com`         | `https://api.whop.com/api/v1`         |

When switching from sandbox to production:

* Create new production API keys.
* Switch the SDK environment from `Sandbox` to `Production`, or remove a 1.x `baseUrl` / `base_url` / `WithBaseURL` sandbox override.
* Recreate sandbox-only products, checkout links, webhooks, and connected-account records in production.
* Confirm webhook URLs point to production infrastructure, not a local tunnel.

## What to include when asking for help

Include as much of the following as you can:

* The endpoint, SDK method, or element name that failed.
* The environment: sandbox or production.
* The status code, error body, OAuth `error_description`, or element `onError` value.
* The Whop IDs involved, with secrets removed.
* The webhook event ID or `webhook-id` header for webhook issues.
* A short code snippet showing how you create the client, checkout, OAuth URL, webhook handler, or element.

<CardGroup cols={2}>
  <Card title="OAuth" icon="key" href="/developer/guides/oauth">
    Build the OAuth 2.1 + PKCE flow and handle token refresh.
  </Card>

  <Card title="Webhooks" icon="webhook" href="/developer/guides/webhooks">
    Verify signatures before processing events, and handle retries.
  </Card>

  <Card title="Permissions" icon="shield" href="/developer/guides/permissions">
    Choose app permissions and explain them to creators.
  </Card>

  <Card title="Test in sandbox" icon="flask" href="/developer/guides/sandbox">
    Test API calls and checkout flows without touching production.
  </Card>
</CardGroup>
