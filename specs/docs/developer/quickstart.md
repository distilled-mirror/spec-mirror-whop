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

# Whop API Quickstart

> Make your first Whop API call, create a checkout, pay it with a test card, and receive the webhook, all in the sandbox. Then switch to production.

This quickstart runs in the sandbox, a separate Whop environment where money never moves. You'll get a sandbox API key, make your first call, create a checkout, pay it with a test card, and receive the `payment.succeeded` webhook on your server. Going live is the last step, and it's mostly swapping keys.

> [!TIP]
>
> If you use a coding agent, give it this page as Markdown, `https://docs.whop.com/developer/quickstart.md`, or paste the prompt from the [Overview](https://docs.whop.com/developer/start.md#build-with-your-coding-agent).

## Sandbox and production

Whop runs two environments that share nothing: not accounts, not keys, not products, not webhooks. You build in the sandbox and launch in production. Every value below changes between them.

| | Sandbox: build and test | Production: launch |
| - | - | - |
| Money | Never moves | Real |
| Dashboard | [sandbox.whop.com](https://sandbox.whop.com) | [whop.com](https://whop.com) |
| API keys and webhooks | [sandbox.whop.com/dashboard/developer](https://sandbox.whop.com/dashboard/developer) | [whop.com/dashboard/developer](https://whop.com/dashboard/developer) |
| API base URL | `https://sandbox-api.whop.com/api/v1` | `https://api.whop.com/api/v1` |
| SDK environment | `Sandbox` | `Production`, the SDK default |
| Checkout pages | `sandbox.whop.com/checkout/...` | `whop.com/checkout/...` |
| `WHOP_ENVIRONMENT` | `sandbox` | `production` |

> [!WARNING]
>
> Sandbox and production API keys look identical, so nothing in a key tells you which environment it belongs to. The snippets on this page pick the environment from a `WHOP_ENVIRONMENT` variable rather than hardcoding it, so the same code runs in both. The [Whop MCP server](https://docs.whop.com/developer/ai/overview.md#mcp-server) only acts on production data.

## Get a sandbox API key

**1. Sign up for the sandbox**

Go to [sandbox.whop.com](https://sandbox.whop.com) and create an account. It's separate from any account you have on whop.com. The sign-up flow creates your first business, which is the Account that owns API keys, products, webhooks, and payments.

**2. Create an Account API key**

Open [sandbox.whop.com/dashboard/developer](https://sandbox.whop.com/dashboard/developer). In **Account API Keys**, select **Create**, and name the key something like `Local development`.

For this quickstart, use the Admin role. Before production, switch to a custom permission set that covers only the endpoints you call.

**3. Store the key on your server**

Copy the key when Whop shows it, and put it in your local `.env` file with the environment it belongs to:

```bash theme={null}
WHOP_ENVIRONMENT=sandbox
WHOP_API_KEY=your_sandbox_key
```

![Whop dashboard Account API Keys section showing a key name, masked value, and Create button](https://mintcdn.com/whop/xuW6TKFlagvnbmV0/images/developer-api-key-field.png?fit=max\&auto=format\&n=xuW6TKFlagvnbmV0\&q=85\&s=699bf36af18f7a12dfd855eb0684b38e)

> [!NOTE]
>
> Keep `WHOP_API_KEY` on your server. Don't put API keys in browser code, mobile apps, or public repositories. Sandbox and production keys look identical, so name them so you can tell them apart.

An Account API key is right when your server acts for your own business. To build an app other businesses install, or to let users sign in with Whop, see [Auth & API keys](https://docs.whop.com/developer/guides/auth-scoping.md).

## Install the SDK

```bash TypeScript theme={null}
pnpm add @whop/sdk
```

```bash Python theme={null}
pip install whop-sdk
```

```bash Ruby theme={null}
gem install whop_sdk
```

> [!NOTE]
>
> This page needs SDK 2.7.0 or later. Earlier versions don't accept the inline `variant` on checkout configurations.

## Make your first call

Point the client at the environment named in `WHOP_ENVIRONMENT` and retrieve the account behind your key. This confirms the key works, and prints your account ID, which starts with `biz_`.

```typescript TypeScript theme={null}
import { WhopClient, WhopEnvironment } from "@whop/sdk";

const apiKey = process.env.WHOP_API_KEY;

if (!apiKey) {
  throw new Error("Set WHOP_API_KEY");
}

const client = new WhopClient({
  token: apiKey,
  environment: process.env.WHOP_ENVIRONMENT === "production" ? WhopEnvironment.Production : WhopEnvironment.Sandbox,
});

const account = await client.accounts.me();

console.log(account.id);
```

```python Python theme={null}
import os
from whop_sdk import Whop, WhopEnvironment

client = Whop(
    token=os.environ["WHOP_API_KEY"],
    environment=WhopEnvironment.PRODUCTION if os.environ.get("WHOP_ENVIRONMENT") == "production" else WhopEnvironment.SANDBOX,
)

account = client.accounts.me()

print(account.id)
```

```ruby Ruby theme={null}
require "whop_sdk"

whop = Whop_sdk::Client.new(
  token: ENV.fetch("WHOP_API_KEY"),
  environment: ENV["WHOP_ENVIRONMENT"] == "production" ? Whop_sdk::Environment::PRODUCTION : Whop_sdk::Environment::SANDBOX,
)

account = whop.accounts.me

puts account.id
```

```bash curl (sandbox) theme={null}
curl https://sandbox-api.whop.com/api/v1/accounts/me \
  -H "Authorization: Bearer $WHOP_API_KEY" \
  -H "Api-Version-Date: 2026-11-04-1"
```

```json Response (trimmed) theme={null}
{
	"id": "biz_XXXXXXXX",
	"title": "Shine Time Auto Detailing",
	"route": "shine-time-auto-detailing",
	"status": "active"
}
```

> [!TIP]
>
> That's your sandbox account. The key works.

## Create a checkout

One call creates a variant and returns a `purchase_url`, a checkout page hosted on `sandbox.whop.com`.

```typescript TypeScript theme={null}
const checkout = await client.checkoutConfigurations.create({
	variant: {
		title: "Starter",
		billing_type: "one_time",
		initial_price: 10.0,
		currency: "usd",
	},
});

console.log(checkout.purchase_url);
```

```python Python theme={null}
checkout = client.checkout_configurations.create(
    variant={
        "title": "Starter",
        "billing_type": "one_time",
        "initial_price": 10.0,
        "currency": "usd",
    },
)

print(checkout.purchase_url)
```

```ruby Ruby theme={null}
checkout = whop.checkout_configurations.create(
  variant: {
    title: "Starter",
    billing_type: "one_time",
    initial_price: 10.0,
    currency: "usd",
  },
)

puts checkout.purchase_url
```

You'll open this URL in the payment step. If you already have a variant, pass its ID as `variant_id` instead of the inline `variant`. You can't send both. [Create Checkout Configuration](https://docs.whop.com/api-reference/checkout-configurations/create-a-checkout-configuration.md) lists every option, including recurring pricing, trials, and redirect URLs.

## Listen for the webhook

Set up your endpoint before you pay, so the `payment.succeeded` event has somewhere to land.

**1. Add an endpoint**

Add a `POST` route that verifies the signature and logs the event. Verification needs the raw request body, so read it before any JSON parsing.

```typescript TypeScript + Next.js theme={null}
import { unwrapWebhook } from "@whop/sdk/helpers";
import type { NextRequest } from "next/server";

export async function POST(request: NextRequest): Promise<Response> {
  const event = unwrapWebhook<{ type: string; data: { id: string } }>(await request.text(), {
    headers: Object.fromEntries(request.headers),
    key: process.env.WHOP_WEBHOOK_SECRET!,
  });

  console.log(event.type, event.data.id);

  return new Response("OK", { status: 200 });
}
```

```python Python + FastAPI theme={null}
import os

from fastapi import FastAPI, Request, Response
from whop_sdk.lib.verify_webhook import unwrap

app = FastAPI()

@app.post("/api/webhooks/whop")
async def whop_webhook(request: Request):
    event = unwrap(await request.body(), headers=dict(request.headers), key=os.environ["WHOP_WEBHOOK_SECRET"])

    print(event["type"], event["data"]["id"])

    return Response(status_code=200)
```

```ruby Ruby on Rails theme={null}
class Api::WebhooksController < ApplicationController
  skip_before_action :verify_authenticity_token

  def whop
    event = WhopSDK::Helpers::VerifyWebhook.unwrap(
      request.raw_post,
      headers: {
        "webhook-id" => request.headers["webhook-id"],
        "webhook-timestamp" => request.headers["webhook-timestamp"],
        "webhook-signature" => request.headers["webhook-signature"],
      },
      key: ENV.fetch("WHOP_WEBHOOK_SECRET"),
    )

    Rails.logger.info("#{event[:type]} #{event[:data][:id]}")

    head :ok
  end
end
```

While you develop, expose the route over HTTPS with ngrok, Cloudflare Tunnel, or another tunnel.

**2. Create the webhook in the sandbox dashboard**

Open **Developer > Webhooks** in the sandbox dashboard, then select **Create webhook**. Paste the HTTPS address of your endpoint, keep the API version on `v1`, and select `payment.succeeded`.

![Create webhook form showing the payment\_succeeded event selected in the events list](https://mintcdn.com/whop/RLIxcN4GVU-CLB5B/images/create-webhook-select-events.png?fit=max\&auto=format\&n=RLIxcN4GVU-CLB5B\&q=85\&s=079887392e21e4680a82415244aeaea4)

**3. Store the signing secret**

Copy the secret from the webhook's **Secret** column and add it to `.env` exactly as shown, `ws_` prefix included:

```bash theme={null}
WHOP_WEBHOOK_SECRET=ws_your_secret
```

Restart your server so it picks up the new value.

## Pay with a test card

Open the `purchase_url` from the checkout step and pay with:

| Field | Value |
| - | - |
| Card number | `4242 4242 4242 4242` |
| Expiry | Any future date, such as `12/34` |
| CVC | Any 3 digits, such as `123` |
| Billing address | Any address |

Within a few seconds, your server logs `payment.succeeded` and the payment ID, which starts with `pay_`.

> [!TIP]
>
> You've taken a payment and handled its webhook, end to end. Nothing was charged.

To test a declined card or 3D Secure, use the other numbers in [Test cards](https://docs.whop.com/developer/guides/sandbox.md#test-cards). If nothing arrives, check the delivery log on the webhook in the dashboard and [Webhook delivery](https://docs.whop.com/developer/troubleshooting.md#webhook-delivery).

## Go live

Sandbox and production share nothing, so going live means repeating the setup on whop.com with production credentials. Your code doesn't change. Only the values in your environment do.

**1. Create a production key**

Sign in at [whop.com](https://whop.com/start), open [whop.com/dashboard/developer](https://whop.com/dashboard/developer), and create an Account API key with only the permissions your integration uses.

**2. Swap every value for its production twin**

In your production environment, set all three variables to production values. Never reuse a sandbox key or secret in production, or the reverse.

```bash Sandbox theme={null}
WHOP_ENVIRONMENT=sandbox
WHOP_API_KEY=your_sandbox_key
WHOP_WEBHOOK_SECRET=ws_your_sandbox_secret
```

```bash Production theme={null}
WHOP_ENVIRONMENT=production
WHOP_API_KEY=your_production_key
WHOP_WEBHOOK_SECRET=ws_your_production_secret
```

If you call the API directly, switch the base URL to `https://api.whop.com/api/v1`.

**3. Recreate what you made in the sandbox**

Products, variants, checkout configurations, and webhooks don't carry over. Create the production webhook against your production URL and store its new signing secret.

**4. Pin your API version**

The SDK sends the version it was built against. If you call the API directly, send `Api-Version-Date` on every request so later changes don't alter your responses. See [API versioning](https://docs.whop.com/developer/api/versioning.md).

> [!WARNING]
>
> Production checkout links take real money. Check which key you're holding before running anything that creates a charge or moves a balance.

## Now build what you came for

> [card] [You sell a subscription or a one-time product](https://docs.whop.com/developer/guides/accept-payments.md)
>
> Checkout links, the Checkout element, variants, and trials.

> [card] [You owe money to people who aren't you](https://docs.whop.com/developer/platforms/manual-payouts.md)
>
> Payouts, transfers, balances, and the payout portal.

> [card] [You take a cut of what other people sell](https://docs.whop.com/developer/platforms/quickstart.md)
>
> Subaccounts, collecting on their behalf, and paying them out.

> [card] [You want distribution, not just infrastructure](https://docs.whop.com/developer/guides/app-views.md)
>
> Apps that run inside Whop, with auth, views, and the CLI.

> [card] [Your product needs people talking to each other](https://docs.whop.com/developer/guides/chat/quickstart.md)
>
> Chat, DMs, and forums you can embed.

> [card] [You just want to read the reference](https://docs.whop.com/api-reference/overview.md)
>
> Every resource in the Current API, with a playground on each endpoint.


## Related topics

- [Whop API Overview](https://docs.whop.com/api-reference/overview.md)
- [Whop API Core Concepts](https://docs.whop.com/developer/concepts.md)
- [Whop Partners API](https://docs.whop.com/developer/partners/overview.md)
