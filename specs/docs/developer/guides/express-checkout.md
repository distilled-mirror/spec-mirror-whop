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

# Add Express Checkout

> Put one-press Apple Pay and Google Pay buttons on your page with the Express Checkout element, straight from the browser, and fulfill from a webhook

Put one-press Apple Pay and Google Pay buttons on a product page with the Express Checkout element. Whop renders the wallets the device can pay with and confirms the payment for you. Your server only listens for the webhook.

The files under **Code** carry `Step X.Y` comments that point back to the steps below. Pick one server implementation and one client implementation. The list includes the files for every option.

## 1. Add the wallet buttons

### 1.1 Install the packages (React)

Install the Whop Elements packages for React.

```bash npm theme={null}
npm install @whop/elements-react @whop/elements
```

```bash pnpm theme={null}
pnpm add @whop/elements-react @whop/elements
```

### 1.1 Add the script tag (JavaScript)

Load the Whop Elements script on the page that shows the buttons.

### 1.2 Mount the buttons

Mount `ExpressCheckoutElement` inside a `Checkout` handle and pass the variant through the existing `plan` property. The element renders only the wallets the buyer's device can pay with, best-native-first for the browser, and renders nothing where no wallet is available. Keep a regular checkout path on the page for those buyers.

Pass `metadata`, such as your own order ID, on the handle. Whop copies it to the payment and to every webhook about it. Set `returnUrl` to a page you host over `https`. A finished purchase redirects the tab there, and so does a payment that needed a full-page step.

Wallets open only on pages whose domain has Apple Pay ready. Follow [Enable Apple Pay and Google Pay](https://docs.whop.com/payments/apple-pay.md) once per domain, serve the page over `https`, and test on a real device with a wallet set up.

### 1.3 Pick the wallets and layout

`wallets` limits which wallets may render, for example `["apple_pay"]`. The element never shows a wallet the device or the checkout's payment method configuration can't back, so the list is a ceiling, not a promise.

`layout` arranges the buttons. `auto` follows the container width, and `horizontal` or `vertical` forces one arrangement. `appearance` on the handle themes the element's outcome and error lines.

### 1.4 Record the completion

A press opens the wallet sheet. When the buyer authorizes, Whop confirms the payment on the checkout session, and the sheet's checkmark follows that confirm. `onComplete` fires once the result stands, before any navigation to `returnUrl`. Use it for analytics and ad pixels.

Don't fulfill from it. A checkout restored on a later page load fires it again for the same result. A payment that left for a full-page step completes on the return page instead.

## 2. Handle the return page

### 2.1 Read the outcome from the URL

Most wallet payments settle inline, and a payment that still needs a step runs it in a dialog when the step fits in one. A step that can only run full page, such as a bank redirect, leaves the page and returns the buyer to `returnUrl` **whatever happened**. Whop appends two query parameters. `payment` is the `pay_` ID. `status` is `succeeded` when the buyer paid, and `failed` or `canceled` when they didn't.

Your own parameters, such as `order`, stay on the URL. The element reports inline declines itself, so the return page is the one place where your site sees a failed off-site step.

### 2.2 Show the right face

Branch on `status`. `succeeded` gets the thank-you page. `failed` and `canceled` get a message that the buyer wasn't charged and a link back to the product. Anything else, such as `processing`, means the charge is still pending. Tell the buyer you'll confirm by email.

Treat the page as a display of the outcome, not as proof of payment. Anyone can type `status=succeeded` into a URL. Access, downloads, and order fulfillment come from the `payment.succeeded` webhook on your server.

## 3. Fulfill from your server

### 3.1 Install the Whop SDK (Next.js)

The webhook helper ships with the Whop SDK. Install it on the server that receives the webhook.

```bash npm theme={null}
npm install @whop/sdk @vercel/functions
```

```bash pnpm theme={null}
pnpm add @whop/sdk @vercel/functions
```

### 3.1 Install the Whop SDK (Express)

The webhook helper ships with the Whop SDK. Install it on the server that receives the webhook.

```bash npm theme={null}
npm install @whop/sdk express
```

```bash pnpm theme={null}
pnpm add @whop/sdk express
```

### 3.1 Install the Whop SDK (Python)

The webhook helper ships with the Whop SDK. Install it on the server that receives the webhook.

```bash pip theme={null}
pip install whop-sdk fastapi
```

```bash poetry theme={null}
poetry add whop-sdk fastapi
```

### 3.2 Handle the payment webhook

Whop sends `payment.succeeded` to your webhook endpoint once the buyer has paid. Verify the signature with the SDK helper, then do the fulfillment work: mark the order paid, grant access, and send the email. A wallet payment reports `apple_pay` or `google_pay` in `payment_method_type`, and the payment carries the `metadata` you set on the Checkout handle.

Fulfillment belongs here and nowhere else. The buyer can close the browser before your return page loads, a redirect can fail, and anyone can type a query parameter by hand. Follow the [Webhooks guide](https://docs.whop.com/developer/guides/webhooks.md) to create the webhook and store its `ws_` secret as `WHOP_WEBHOOK_SECRET`.

## Code

### Client: React — `BuyButton.tsx`

```tsx BuyButton.tsx theme={null}
// Step 1.1: Install the packages
import { Checkout, ExpressCheckoutElement, WhopElements } from "@whop/elements-react";
import { loadWhop } from "@whop/elements";

export function BuyButton({ orderId }: { orderId: string }) {
	return (
		<WhopElements elements={loadWhop()}>
			{/* Step 1.2: Mount the buttons */}
			<Checkout
				plan="plan_xxxxxxxxxxxxx"
				metadata={{ order_id: orderId }}
				returnUrl={`https://yoursite.com/checkout/return?order=${orderId}`}
				// Step 1.4: Record the completion
				onComplete={(completion) => {
					if (completion.result === "payment") {
						// Fire analytics or ad pixels here. Fulfill from the webhook, not from this callback.
						console.log("Express checkout completed", completion.paymentId);
					}
				}}
			>
				{/* Step 1.3: Pick the wallets and layout */}
				<ExpressCheckoutElement layout="horizontal" wallets={["apple_pay", "google_pay"]} />
			</Checkout>
		</WhopElements>
	);
}
```

### Client: React — `CheckoutReturn.tsx`

```tsx CheckoutReturn.tsx theme={null}
export function CheckoutReturn() {
	// Step 2.1: Read the outcome from the URL
	// Whop appends `payment` and `status` to your returnUrl. Your own parameters (here `order`) survive.
	const params = new URLSearchParams(window.location.search);
	const paymentId = params.get("payment");
	const status = params.get("status");
	const orderId = params.get("order");

	// Step 2.2: Show the right face
	if (status === "succeeded") {
		return (
			<section>
				<h1>Thanks for your order</h1>
				<p>Payment {paymentId} is complete. A receipt is on its way to your email.</p>
			</section>
		);
	}

	if (status === "failed" || status === "canceled") {
		return (
			<section>
				<h1>Your payment did not go through</h1>
				<p>No money was taken. You can try again with another payment method.</p>
				<a href={`/checkout?order=${orderId}`}>Back to checkout</a>
			</section>
		);
	}

	return (
		<section>
			<h1>Confirming your payment</h1>
			<p>Your payment is still being processed. We will email you as soon as it is confirmed.</p>
		</section>
	);
}
```

### Client: JavaScript — `buy.html`

```html buy.html theme={null}
<!DOCTYPE html>
<html>
  <head>
    <!-- Step 1.1: Add the script tag -->
    <script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>
  </head>
  <body>
    <div id="express-checkout"></div>

    <script type="module">
      const orderId = new URLSearchParams(window.location.search).get("order");

      // Step 1.2: Mount the buttons
      const checkout = window.WhopElements().checkout.create({
        plan: "plan_xxxxxxxxxxxxx",
        metadata: { order_id: orderId },
        returnUrl: `https://yoursite.com/checkout/return?order=${orderId}`,
        // Step 1.4: Record the completion
        onComplete: (completion) => {
          if (completion.result === "payment") {
            // Fire analytics or ad pixels here. Fulfill from the webhook, not from this callback.
            console.log("Express checkout completed", completion.paymentId);
          }
        },
      });
      // Step 1.3: Pick the wallets and layout
      checkout
        .create("expressCheckout", { layout: "horizontal", wallets: ["apple_pay", "google_pay"] })
        .mount("#express-checkout");
    </script>
  </body>
</html>
```

### Client: JavaScript — `return.html`

```html return.html theme={null}
<!DOCTYPE html>
<html>
  <body>
    <section id="succeeded" hidden>
      <h1>Thanks for your order</h1>
      <p>Payment <span id="payment-id"></span> is complete. A receipt is on its way to your email.</p>
    </section>
    <section id="failed" hidden>
      <h1>Your payment did not go through</h1>
      <p>No money was taken. You can try again with another payment method.</p>
      <a id="retry" href="/checkout">Back to checkout</a>
    </section>
    <section id="pending" hidden>
      <h1>Confirming your payment</h1>
      <p>Your payment is still being processed. We will email you as soon as it is confirmed.</p>
    </section>

    <script type="module">
      // Step 2.1: Read the outcome from the URL
      // Whop appends `payment` and `status` to your returnUrl. Your own parameters (here `order`) survive.
      const params = new URLSearchParams(window.location.search);
      const paymentId = params.get("payment");
      const status = params.get("status");
      const orderId = params.get("order");

      // Step 2.2: Show the right face
      if (status === "succeeded") {
        document.querySelector("#payment-id").textContent = paymentId;
        document.querySelector("#succeeded").hidden = false;
      } else if (status === "failed" || status === "canceled") {
        document.querySelector("#retry").href = `/checkout?order=${orderId}`;
        document.querySelector("#failed").hidden = false;
      } else {
        document.querySelector("#pending").hidden = false;
      }
    </script>
  </body>
</html>
```

### Server: Next.js — `app/api/webhooks/whop/route.ts`

```typescript app/api/webhooks/whop/route.ts theme={null}
// Step 3.1: Install the Whop SDK
import { waitUntil } from "@vercel/functions";
import { unwrapWebhook } from "@whop/sdk/helpers";
import type { NextRequest } from "next/server";

export async function POST(request: NextRequest) {
	// Step 3.2: Handle the payment webhook
	// Give the raw body. Parsing it first changes the bytes and the signature check fails.
	const payload = await request.text();
	const headers = Object.fromEntries(request.headers);

	const event = unwrapWebhook(payload, {
		headers,
		key: process.env.WHOP_WEBHOOK_SECRET!,
	});

	if (event.type === "payment.succeeded") {
		waitUntil(fulfillOrder(event.data));
	}

	// Respond in less than 5 seconds, or Whop retries.
	return new Response("OK", { status: 200 });
}

async function fulfillOrder(payment: Record<string, any>) {
	// Step 3.2: Handle the payment webhook
	// metadata.order_id is the value you set on the Checkout handle.
	const orderId = payment.metadata?.order_id;
	console.log(`[PAYMENT SUCCEEDED] ${payment.id} for order ${orderId}`);
	// Mark the order paid in your database and grant access here.
}
```

### Server: Express — `server.ts`

```typescript server.ts theme={null}
// Step 3.1: Install the Whop SDK
import { unwrapWebhook } from "@whop/sdk/helpers";
import express from "express";

const app = express();

// Step 3.2: Handle the payment webhook
// Read the raw body on this route: parsing it first changes the bytes and the signature check fails.
app.post(
	"/api/webhooks/whop",
	express.raw({ type: "application/json" }),
	(req, res) => {
		const event = unwrapWebhook(req.body.toString("utf8"), {
			headers: req.headers,
			key: process.env.WHOP_WEBHOOK_SECRET!,
		});

		if (event.type === "payment.succeeded") {
			// metadata.order_id is the value you set on the Checkout handle.
			const orderId = event.data.metadata?.order_id;
			console.log(`[PAYMENT SUCCEEDED] ${event.data.id} for order ${orderId}`);
			// Mark the order paid in your database and grant access here.
		}

		// Respond in less than 5 seconds, or Whop retries.
		res.sendStatus(200);
	},
);

app.listen(3000);
```

### Server: Python — `server.py`

```python server.py theme={null}
# Step 3.1: Install the Whop SDK
import os

from fastapi import BackgroundTasks, FastAPI, Request, Response
from whop_sdk.lib.verify_webhook import unwrap

app = FastAPI()


# Step 3.2: Handle the payment webhook
@app.post("/api/webhooks/whop")
async def whop_webhook(request: Request, background: BackgroundTasks):
    # Give the raw body. Parsing it first changes the bytes and the signature check fails.
    payload = await request.body()

    event = unwrap(payload, dict(request.headers), os.environ["WHOP_WEBHOOK_SECRET"])

    if event["type"] == "payment.succeeded":
        background.add_task(fulfill_order, event["data"])

    # Respond in less than 5 seconds, or Whop retries.
    return Response(status_code=200)


def fulfill_order(payment):
    # metadata.order_id is the value you set on the Checkout handle.
    order_id = (payment.get("metadata") or {}).get("order_id")
    print(f"[PAYMENT SUCCEEDED] {payment['id']} for order {order_id}")
    # Mark the order paid in your database and grant access here.
```

## Next steps

* [Express Checkout element reference](https://docs.whop.com/elements/latest/checkout/expressCheckout.md): every option, event, and styling hook on the element.
* [Open a wallet sheet from your own button](https://docs.whop.com/developer/guides/payment-request.md): render the button yourself and price the order yourself with the payment request resource.
* [Enable Apple Pay and Google Pay](https://docs.whop.com/payments/apple-pay.md): add Apple Pay to your domain.
* [Embed a checkout](https://docs.whop.com/developer/guides/embed-checkout.md): the full checkout form, which already shows the wallet buttons above it.


## Related topics

- [Open a Wallet Sheet from Your Own Button](https://docs.whop.com/developer/guides/payment-request.md)
- [Accept Payments](https://docs.whop.com/developer/guides/accept-payments.md)
- [ExpressCheckoutElement](https://docs.whop.com/elements/latest/checkout/expressCheckout.md)
