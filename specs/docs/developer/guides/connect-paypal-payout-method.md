> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

> ## Agent Instructions
> Treat these docs and the Whop MCP server as the source of truth for Whop API behavior. Don't infer endpoint behavior from marketing pages.
> MCP server: connect https://mcp.whop.com/mcp (streamable HTTP, browser sign-in, no API key in the client config) to search these docs and read or change live Whop data. Setup: https://docs.whop.com/developer/guides/ai_and_mcp.md
> Whop CLI: install with `curl -fsSL https://whop.com/install.sh | sh` to scaffold, run, and deploy Whop apps and websites. It acts on production, not the sandbox. Overview: https://docs.whop.com/cli/overview.md Agent mode: https://docs.whop.com/cli/agent-mode.md
> Hosting: Whop serves a site or app from the build `whop apps deploy` uploads, with rollbacks, secrets, and server logs. Guide: https://docs.whop.com/developer/websites/hosting.md
> Plugins: in Claude Code, run `/plugin marketplace add whopio/plugins`, then `/plugin install whop@whop`. Guide: https://docs.whop.com/developer/guides/plugins/claude-code.md Every agent tool: https://docs.whop.com/developer/ai/overview.md
> Environments: build and test in the sandbox (API `https://sandbox-api.whop.com/api/v1`, keys at https://sandbox.whop.com/dashboard/developer, SDK environment `Sandbox`; no real money). Production (API `https://api.whop.com/api/v1`, keys at https://whop.com/dashboard/developer, SDK environment `Production`, the SDK default) moves real money, so use it only at launch. The two share no accounts, keys, products, or webhooks, and their keys look identical: never mix them. Sandbox guide: https://docs.whop.com/developer/guides/sandbox.md Comparison: https://docs.whop.com/developer/quickstart.md
> API: send `Authorization: Bearer <API_KEY>` from your server only, and pin a dated version with the `Api-Version-Date` header. OpenAPI spec: https://api.whop.com/api/v1/openapi.json
> Which API: build on the Current API, whose pages live under https://docs.whop.com/api-reference/ (start at https://docs.whop.com/api-reference/overview.md). Pages under /api-reference/legacy/ are the Legacy API. Use one only when https://docs.whop.com/api-reference/stability.md lists the resource as Legacy-only.

# Connect a PayPal Payout Method

> Add PayPal as a payout method by having the account holder log in to PayPal, so payouts only go to a verified account in their name.

Make sure payouts only reach a PayPal account the holder owns. The holder logs in to PayPal instead of typing an email, and Whop saves PayPal's email once its name matches their payout setup.

| | Log in with PayPal (this guide) | Typed email |
| - | - | - |
| **Payout email** | From PayPal | Typed by the holder |
| **Proves the holder owns it** | Yes | No |
| **Integration** | A redirect and a return page | One API call |

> [!NOTE]
>
> Before you start, you need:
>
> * An API key with the `payout:destination:read` and `payout:create_destination` scopes.
> * PayPal login enabled on the account. Contact Whop to turn it on. Until then, `supports_paypal_login` is `false` and step 2 returns `403`.
> * A completed [payout setup](https://docs.whop.com/developer/verification/overview.md). Whop matches the name on it to the PayPal account.
> * An `https` page for PayPal to send the holder back to, on a domain added to the account or its platform. Add it with [Create Domain](https://docs.whop.com/api-reference/domains/create-domain.md) and `verification: {}`. It doesn't need to finish verifying. `https://localhost` works for testing.
> * Whop SDK 2.5.0 or later, if you use an SDK.

## How the redirect works

It all happens in the holder's browser, in one tab. You never call PayPal directly.

1. Your server starts a session and gets an `authorize_url` on PayPal.
2. The holder's browser goes there, and they log in and approve.
3. PayPal returns the browser to Whop, which reads the account's email, name, and verification status.
4. Whop sends the browser to your `return_url` with `?paypal_link_token=…` or `?error=…`.
5. Your server passes the token to Whop, which checks the name and saves the method.

## 1. Find the payout method

[List supported payout methods](https://docs.whop.com/api-reference/payouts/list-supported-payout-methods.md) and keep the `id` of the one with `supports_paypal_login: true`. The SDKs page through every result for you.

```typescript TypeScript theme={null}
import { WhopClient } from "@whop/sdk";

const client = new WhopClient({ token: process.env.WHOP_API_KEY });

const supportedMethods = await client.payouts.supportedMethods.list({
	account_id: "biz_xxxxxxxxxxxxx",
	destination_currency: "usd",
});

let paypalMethodId: string | undefined;
for await (const method of supportedMethods) {
	if (method.supports_paypal_login) {
		paypalMethodId = method.id;
		break;
	}
}
if (!paypalMethodId) throw new Error("PayPal login is not enabled for this account");
```

```python Python theme={null}
import os

from whop_sdk import Whop

client = Whop(token=os.environ["WHOP_API_KEY"])

supported_methods = client.payouts.supported_methods.list(
    account_id="biz_xxxxxxxxxxxxx",
    destination_currency="usd",
)

paypal_method_id = next(
    (method.id for method in supported_methods if method.supports_paypal_login),
    None,
)
if paypal_method_id is None:
    raise RuntimeError("PayPal login is not enabled for this account")
```

```ruby Ruby theme={null}
require "whop_sdk"

client = Whop_sdk::Client.new(token: ENV.fetch("WHOP_API_KEY"))

paypal_method = client.payouts.supported_methods
  .list(account_id: "biz_xxxxxxxxxxxxx", destination_currency: "usd")
  .find(&:supports_paypal_login)
raise "PayPal login is not enabled for this account" if paypal_method.nil?

paypal_method_id = paypal_method.id
```

```go Go theme={null}
import (
	"context"
	"fmt"
	"log"
	"os"

	whopsdk "github.com/whopio/whopsdk-go/v2"
	"github.com/whopio/whopsdk-go/v2/client"
	"github.com/whopio/whopsdk-go/v2/option"
	"github.com/whopio/whopsdk-go/v2/payouts"
)

ctx := context.TODO()
client := client.NewWhop(option.WithToken(os.Getenv("WHOP_API_KEY")))

page, err := client.Payouts.SupportedMethods.List(ctx, &payouts.ListSupportedMethodsRequest{
	AccountID:           whopsdk.String("biz_xxxxxxxxxxxxx"),
	DestinationCurrency: whopsdk.String("usd"),
})
if err != nil {
	log.Fatal(err)
}

var paypalMethodID string
methods := page.Iterator()
for methods.Next(ctx) {
	method := methods.Current()
	if method.SupportsPaypalLogin != nil && *method.SupportsPaypalLogin {
		paypalMethodID = method.ID
		break
	}
}
if err := methods.Err(); err != nil {
	log.Fatal(err)
}
if paypalMethodID == "" {
	log.Fatal("PayPal login is not enabled for this account")
}
```

```bash cURL theme={null}
curl "https://api.whop.com/api/v1/payouts/supported_methods?account_id=biz_xxxxxxxxxxxxx&destination_currency=usd" \
  -H "Authorization: Bearer $WHOP_API_KEY"
```

## 2. Start the login

[Create a PayPal link session](https://docs.whop.com/api-reference/payouts/create-paypal-link-session.md) and redirect the holder to its `authorize_url`. The session expires in 15 minutes, at `expires_at`.

```typescript TypeScript theme={null}
const session = await client.payouts.paypalLinkSessions.create({
	account_id: "biz_xxxxxxxxxxxxx",
	supported_payout_method_id: paypalMethodId,
	destination_currency: "usd",
	return_url: "https://yourapp.example/settings/payouts",
});

// Redirect the holder's browser to this URL
console.log(session.authorize_url);
```

```python Python theme={null}
session = client.payouts.paypal_link_sessions.create(
    account_id="biz_xxxxxxxxxxxxx",
    supported_payout_method_id=paypal_method_id,
    destination_currency="usd",
    return_url="https://yourapp.example/settings/payouts",
)

# Redirect the holder's browser to this URL
print(session.authorize_url)
```

```ruby Ruby theme={null}
session = client.payouts.paypal_link_sessions.create(
  account_id: "biz_xxxxxxxxxxxxx",
  supported_payout_method_id: paypal_method_id,
  destination_currency: "usd",
  return_url: "https://yourapp.example/settings/payouts",
)

# Redirect the holder's browser to this URL
puts session.authorize_url
```

```go Go theme={null}
session, err := client.Payouts.PaypalLinkSessions.Create(ctx, &payouts.CreatePaypalLinkSessionsRequest{
	AccountID:               whopsdk.String("biz_xxxxxxxxxxxxx"),
	SupportedPayoutMethodID: paypalMethodID,
	DestinationCurrency:     whopsdk.String("usd"),
	ReturnURL:               "https://yourapp.example/settings/payouts",
})
if err != nil {
	log.Fatal(err)
}

// Redirect the holder's browser to this URL
fmt.Println(session.AuthorizeURL)
```

```bash cURL theme={null}
curl "https://api.whop.com/api/v1/payouts/paypal_link_sessions" \
  -H "Authorization: Bearer $WHOP_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "account_id": "biz_xxxxxxxxxxxxx",
    "supported_payout_method_id": "podst_xxxxxxxxxxxxx",
    "destination_currency": "usd",
    "return_url": "https://yourapp.example/settings/payouts"
  }'
```

The browser comes back to your `return_url` with one of these:

| Query string | Meaning | Next |
| - | - | - |
| `?paypal_link_token=…` | The holder logged in | Save the method within 15 minutes |
| `?error=paypal_login_canceled` | The holder backed out | Offer to start again |
| `?error=paypal_login_failed` | PayPal refused, the link was reused, or the exchange failed | Start a new session |

If the holder finishes after `expires_at`, or after the `return_url` domain is removed from the account, Whop can't send them back. They see an "Invalid or expired link" page.

## 3. Save the payout method

On your server, [create the saved payout method](https://docs.whop.com/api-reference/payouts/create-saved-payout-method.md) with `paypal_link_token` in place of `fields`. Use the same `account_id`, `supported_payout_method_id`, and `destination_currency` as the session, since the token only works with those. Whop saves PayPal's email and names the method after it.

```typescript TypeScript theme={null}
// paypalLinkToken is the paypal_link_token from your return_url's query string
const method = await client.payouts.methods.create({
	account_id: "biz_xxxxxxxxxxxxx",
	supported_payout_method_id: paypalMethodId,
	destination_currency: "usd",
	paypal_link_token: paypalLinkToken,
});

console.log(method.id);
```

```python Python theme={null}
# paypal_link_token is the value from your return_url's query string
method = client.payouts.methods.create(
    account_id="biz_xxxxxxxxxxxxx",
    supported_payout_method_id=paypal_method_id,
    destination_currency="usd",
    paypal_link_token=paypal_link_token,
)

print(method.id)
```

```ruby Ruby theme={null}
# paypal_link_token is the value from your return_url's query string
method = client.payouts.methods.create(
  account_id: "biz_xxxxxxxxxxxxx",
  supported_payout_method_id: paypal_method_id,
  destination_currency: "usd",
  paypal_link_token: paypal_link_token,
)

puts method.id
```

```go Go theme={null}
// paypalLinkToken is the paypal_link_token from your return_url's query string
method, err := client.Payouts.Methods.Create(ctx, &payouts.CreateMethodsRequest{
	AccountID:               whopsdk.String("biz_xxxxxxxxxxxxx"),
	SupportedPayoutMethodID: paypalMethodID,
	DestinationCurrency:     whopsdk.String("usd"),
	PaypalLinkToken:         whopsdk.String(paypalLinkToken),
})
if err != nil {
	log.Fatal(err)
}

fmt.Println(method.ID)
```

```bash cURL theme={null}
curl "https://api.whop.com/api/v1/payouts/methods" \
  -H "Authorization: Bearer $WHOP_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "account_id": "biz_xxxxxxxxxxxxx",
    "supported_payout_method_id": "podst_xxxxxxxxxxxxx",
    "destination_currency": "usd",
    "paypal_link_token": "'"$PAYPAL_LINK_TOKEN"'"
  }'
```

The response is the new payout method, abridged:

```json theme={null}
{
	"object": "payout_method",
	"id": "potk_xxxxxxxxxxxxx",
	"nickname": "PayPal j•••@example.com",
	"institution_name": "PayPal",
	"destination_currency": "usd"
}
```

You can now [send a payout](https://docs.whop.com/developer/platforms/manual-payouts.md) to it.

### If Whop refuses the account

Whop answers `400` with a `message` for the holder, such as a name mismatch. Show it as is.

* **The token survives a refusal**, so retry a temporary error with the same token.
* **If the message says to connect PayPal again**, start a new session once the holder fixes the problem.
* **An unverified PayPal account or unconfirmed email still saves.**

## What the holder sees

PayPal's consent screen names Whop as the app asking for their email, name, and verified status. Every redirect stays in the tab that opened PayPal, so if you use a pop-up, your return page must notify its opener and close.

## Next steps

> [card] [Send a payout](https://docs.whop.com/developer/platforms/manual-payouts.md)
>
> Pay out to the PayPal method you just saved

> [card] [Payouts API reference](https://docs.whop.com/api-reference/payouts/payout)
>
> Every payout and payout method endpoint


## Related topics

- [Manual Payouts to Subaccounts](https://docs.whop.com/developer/platforms/manual-payouts.md)
- [Create PayPal Link Session](https://docs.whop.com/api-reference/payouts/create-paypal-link-session.md)
- [Payouts API](https://docs.whop.com/api-reference/payouts/payout.md)
