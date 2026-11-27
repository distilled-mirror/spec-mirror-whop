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

# Build a Checkout with Payment Elements

> Design your own checkout around the payment elements: collect a confirmation token in the browser, charge it from your server, and fulfill from a webhook

Design your own checkout around Whop's payment elements. The elements collect the payment method in PCI-isolated fields and hand you a confirmation token. Your server passes that token to the Payments API to charge, or to the Setup Intents API to save the method. The browser then finishes any step the buyer still owes.

The files under **Code** carry `Step X.Y` comments that point back to the steps below. Pick one server implementation and one client implementation. The list includes the files for every option.

## 1. Build the payment form

### 1.1 Install the packages (React)

Install the Whop Elements packages for React.

```bash npm theme={null}
npm install @whop/elements-react @whop/elements
```

```bash pnpm theme={null}
pnpm add @whop/elements-react @whop/elements
```

### 1.1 Add the script tag (JavaScript)

Load the Whop Elements script on the page that hosts your checkout.

### 1.2 Mount the elements

Create a `Payments` handle for the variant through the existing `plan` property and mount three elements under it. `EmailElement` collects the buyer's email and offers a recognized Whop buyer a sign-in that unlocks their saved payment methods. `PaymentElement` shows the methods available for the variant and the buyer's country, and collects each method's fields in hosted frames, so card numbers never reach your page. `BrandingElement` shows Whop's merchant-of-record notice, which every payment form must carry.

The handle resolves the variant's currency, amount, and payment methods on its own. Set `returnUrl` to a page you host over `https`. Bank redirects, financing applications, and some 3D Secure flows bring the buyer back there. Add your own query parameters to it, such as the order ID. They survive the round trip.

To save a payment method without charging it, mount the same elements with `mode: "setup"` and a `currency` instead of a `plan`. The [Save payment methods](https://docs.whop.com/developer/guides/save-payment-methods.md) guide covers that form.

### 1.3 Enable the pay button

`PaymentElement` reports `complete: true` through `onChange` once the buyer has picked a method and filled in what it needs. Enable your pay button from it. You own the button: its label, its position, and what happens on press.

### 1.4 Create the confirmation token

On press, call `createConfirmationToken`. It turns what the elements collected into a single-use `ctok_` token that stands in for the payment method. The token carries the email and billing details the elements gathered. A wallet pick opens Apple Pay or Google Pay during this call, and the call refuses while no `BrandingElement` is on the page.

Post the token to your server together with your own order reference. The token is the only thing about the payment method that leaves the browser.

## 2. Charge from your server

### 2.1 Install the Whop SDK (Next.js)

Install the Whop SDK and create a client with your Account API key. The key stays on the server. The browser never sees it.

```bash npm theme={null}
npm install @whop/sdk @vercel/functions
```

```bash pnpm theme={null}
pnpm add @whop/sdk @vercel/functions
```

### 2.1 Install the Whop SDK (Express)

Install the Whop SDK and create a client with your Account API key. The key stays on the server. The browser never sees it.

```bash npm theme={null}
npm install @whop/sdk express
```

```bash pnpm theme={null}
pnpm add @whop/sdk express
```

### 2.1 Install the Whop SDK (Python)

Install the Whop SDK and create a client with your Account API key. The key stays on the server. The browser never sees it.

```bash pip theme={null}
pip install whop-sdk fastapi
```

```bash poetry theme={null}
poetry add whop-sdk fastapi
```

### 2.2 Create the payment from the confirmation token

Pass the token to the Payments API. Whop resolves the buyer from the token's email, charges the variant, and returns the payment with a `client_secret`. Return `id`, `status`, and `client_secret` to the browser. The `client_secret` only unlocks this one payment, so it's safe to expose.

Set `return_url` to the same page the elements return to. Put your own order ID in `metadata` so the payment and its webhooks carry it. For a setup-mode form, pass the token to the [Setup Intents API](https://docs.whop.com/api-reference/setup-intents/create-setup-intent.md) instead. It returns a setup intent with the same `client_secret` shape.

* `variant_id` is the variant the buyer is purchasing. Pass an inline `variant` instead to charge an amount that isn't a saved variant yet
* `confirmation_token` stands in for the payment method the elements collected
* `metadata` ties the payment back to your own order

### 2.3 Fulfill from the webhook

Whop sends `payment.succeeded` to your webhook endpoint once the charge succeeds, whether the buyer paid inline or came back from an off-site step. Verify the signature with the SDK helper, then do the fulfillment work: mark the order paid, grant access, and send the email.

Fulfillment belongs here and nowhere else. The buyer can close the browser before your return page loads, a redirect can fail, and anyone can type a query parameter by hand. Follow the [Webhooks guide](https://docs.whop.com/developer/guides/webhooks.md) to create the webhook and store its `ws_` secret as `WHOP_WEBHOOK_SECRET`.

## 3. Finish the payment

### 3.1 Handle the next action

A payment that comes back `paid` is complete. Anything else means the buyer still has a step, such as 3D Secure or a bank redirect, or the charge is still pending. Pass the `client_secret` to `handleNextAction`. It runs an inline step in a dialog and resolves with `redirected: false`, or sends the buyer to your `returnUrl` and resolves with `redirected: true`.

Branch on the `status` it returns. `succeeded` means the buyer paid, `processing` means the charge is still pending, and anything else needs another try. `lastPaymentError` carries the reason when there is one. A dismissed dialog leaves the payment at `requires_action` with no error, so a missing error never means success.

### 3.2 Read the outcome from the URL

A buyer who left for an off-site step arrives at `returnUrl` **whatever happened** there. Whop appends three query parameters. `payment` is the `pay_` ID. `status` is `succeeded` when the buyer paid, `failed` or `canceled` when they didn't, and a pending status such as `processing` while the charge is still undecided. `client_secret` lets the page poll the payment's status if it wants to wait for a decision.

Your own parameters, such as `order`, stay on the URL. Card declines never reach this page, because `handleNextAction` reports them inline. The return page is the one place where your site sees a failed off-site payment.

### 3.3 Show the right face

Branch on `status`. `succeeded` gets the thank-you page. `failed` and `canceled` get a message that the buyer wasn't charged and a link back to the checkout. Anything else means the charge is still pending. Tell the buyer you'll confirm by email. The inline paths in the form send the buyer to the same page with the same parameters, so one page handles every ending.

Treat the page as a display of the outcome, not as proof of payment. Anyone can type `status=succeeded` into a URL. Access, downloads, and order fulfillment come from the `payment.succeeded` webhook on your server.

## Code

### Client: React — `PaymentForm.tsx`

```tsx PaymentForm.tsx theme={null}
// Step 1.1: Install the packages
import { useState } from "react";
import {
	BrandingElement,
	EmailElement,
	PaymentElement,
	Payments,
	WhopElements,
	usePayments,
	useWhop,
} from "@whop/elements-react";
import { loadWhop } from "@whop/elements";

export function CheckoutPage({ orderId }: { orderId: string }) {
	return (
		<WhopElements elements={loadWhop()}>
			{/* Step 1.2: Mount the elements */}
			<Payments
				accountId="biz_xxxxxxxxxxxxx"
				plan="plan_xxxxxxxxxxxxx"
				returnUrl={`https://yoursite.com/checkout/return?order=${orderId}`}
			>
				<PaymentForm orderId={orderId} />
			</Payments>
		</WhopElements>
	);
}

function PaymentForm({ orderId }: { orderId: string }) {
	const payments = usePayments();
	const whop = useWhop();
	const [ready, setReady] = useState(false);
	const [error, setError] = useState<string | null>(null);

	async function pay() {
		if (!payments || !whop) return;
		setError(null);

		// Step 1.4: Create the confirmation token
		// A single-use ctok_ token that stands in for the payment method. Card data never touches your page.
		const { confirmationToken } = await payments.createConfirmationToken({});

		const response = await fetch("/api/pay", {
			method: "POST",
			headers: { "Content-Type": "application/json" },
			body: JSON.stringify({ confirmationToken, orderId }),
		});
		const payment = await response.json();

		// Step 3.1: Handle the next action
		if (payment.status === "paid") {
			window.location.assign(`/checkout/return?order=${orderId}&status=succeeded&payment=${payment.id}`);
			return;
		}

		// Anything but paid means the buyer still has a step, or the charge is still being decided.
		const result = await whop.payments.handleNextAction({
			clientSecret: payment.client_secret,
		});
		// The buyer left for an off-site step; they come back on the return page.
		if (result.redirected) return;

		if (result.status === "succeeded") {
			window.location.assign(`/checkout/return?order=${orderId}&status=succeeded&payment=${payment.id}`);
			return;
		}
		if (result.status === "processing") {
			window.location.assign(`/checkout/return?order=${orderId}&status=processing&payment=${payment.id}`);
			return;
		}
		// A dismissed dialog leaves the payment at requires_action with no error, so a missing error never means success.
		setError(result.lastPaymentError?.message ?? "The payment step wasn't completed. Try again.");
	}

	return (
		<>
			{/* Step 1.2: Mount the elements */}
			<EmailElement />
			{/* Step 1.3: Enable the pay button */}
			<PaymentElement onChange={(event) => setReady(event.complete)} />
			{/* Step 1.2: Mount the elements */}
			<BrandingElement />
			{/* Step 1.3: Enable the pay button */}
			<button disabled={!ready} onClick={pay}>
				Pay
			</button>
			{error && <p role="alert">{error}</p>}
		</>
	);
}
```

### Client: React — `CheckoutReturn.tsx`

```tsx CheckoutReturn.tsx theme={null}
export function CheckoutReturn() {
	// Step 3.2: Read the outcome from the URL
	// Whop appends `payment`, `status`, and `client_secret` to your returnUrl. Your own parameters (here `order`) survive.
	const params = new URLSearchParams(window.location.search);
	const paymentId = params.get("payment");
	const status = params.get("status");
	const orderId = params.get("order");

	// Step 3.3: Show the right face
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

### Client: JavaScript — `checkout.html`

```html checkout.html theme={null}
<!DOCTYPE html>
<html>
  <head>
    <!-- Step 1.1: Add the script tag -->
    <script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>
  </head>
  <body>
    <div id="email"></div>
    <div id="payment"></div>
    <div id="branding"></div>
    <button id="pay" disabled>Pay</button>
    <p id="error" role="alert"></p>

    <script type="module">
      const orderId = new URLSearchParams(window.location.search).get("order");

      // Step 1.2: Mount the elements
      const whop = window.WhopElements();
      const payments = whop.payments.create({
        accountId: "biz_xxxxxxxxxxxxx",
        plan: "plan_xxxxxxxxxxxxx",
        returnUrl: `https://yoursite.com/checkout/return?order=${orderId}`,
      });

      payments.create("email").mount("#email");
      const payButton = document.querySelector("#pay");
      const errorLine = document.querySelector("#error");

      // Step 1.3: Enable the pay button
      payments
        .create("payment", { onChange: (event) => (payButton.disabled = !event.complete) })
        .mount("#payment");
      // Step 1.2: Mount the elements
      payments.create("branding").mount("#branding");

      payButton.addEventListener("click", async () => {
        errorLine.textContent = "";

        // Step 1.4: Create the confirmation token
        // A single-use ctok_ token that stands in for the payment method. Card data never touches your page.
        const { confirmationToken } = await payments.createConfirmationToken({});

        const response = await fetch("/api/pay", {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify({ confirmationToken, orderId }),
        });
        const payment = await response.json();

        // Step 3.1: Handle the next action
        if (payment.status === "paid") {
          window.location.assign(`/checkout/return?order=${orderId}&status=succeeded&payment=${payment.id}`);
          return;
        }

        // Anything but paid means the buyer still has a step, or the charge is still being decided.
        const result = await whop.payments.handleNextAction({
          clientSecret: payment.client_secret,
        });
        // The buyer left for an off-site step; they come back on the return page.
        if (result.redirected) return;

        if (result.status === "succeeded") {
          window.location.assign(`/checkout/return?order=${orderId}&status=succeeded&payment=${payment.id}`);
          return;
        }
        if (result.status === "processing") {
          window.location.assign(`/checkout/return?order=${orderId}&status=processing&payment=${payment.id}`);
          return;
        }
        // A dismissed dialog leaves the payment at requires_action with no error, so a missing error never means success.
        errorLine.textContent = result.lastPaymentError?.message ?? "The payment step wasn't completed. Try again.";
      });
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
      // Step 3.2: Read the outcome from the URL
      // Whop appends `payment`, `status`, and `client_secret` to your returnUrl. Your own parameters (here `order`) survive.
      const params = new URLSearchParams(window.location.search);
      const paymentId = params.get("payment");
      const status = params.get("status");
      const orderId = params.get("order");

      // Step 3.3: Show the right face
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

### Server: Next.js — `app/api/pay/route.ts`

```typescript app/api/pay/route.ts theme={null}
// Step 2.1: Install the Whop SDK
import { WhopClient } from "@whop/sdk";

const whop = new WhopClient({
	token: process.env.WHOP_API_KEY,
});

export async function POST(request: Request) {
	const { confirmationToken, orderId } = await request.json();

	// Step 2.2: Create the payment from the confirmation token
	const payment = await whop.payments.create({
		account_id: process.env.WHOP_ACCOUNT_ID,
		variant_id: "plan_xxxxxxxxxxxxx",
		confirmation_token: confirmationToken,
		return_url: `https://yoursite.com/checkout/return?order=${orderId}`,
		metadata: { order_id: orderId },
	});

	// client_secret is safe to send to the browser. Your API key never is.
	return Response.json({
		id: payment.id,
		status: payment.status,
		client_secret: payment.client_secret,
	});
}
```

### Server: Next.js — `app/api/webhooks/whop/route.ts`

```typescript app/api/webhooks/whop/route.ts theme={null}
import { waitUntil } from "@vercel/functions";
import { unwrapWebhook } from "@whop/sdk/helpers";
import type { NextRequest } from "next/server";

export async function POST(request: NextRequest) {
	// Step 2.3: Fulfill from the webhook
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
	// Step 2.3: Fulfill from the webhook
	// metadata.order_id is the value you set when you created the payment.
	const orderId = payment.metadata?.order_id;
	console.log(`[PAYMENT SUCCEEDED] ${payment.id} for order ${orderId}`);
	// Mark the order paid in your database and grant access here.
}
```

### Server: Express — `server.ts`

```typescript server.ts theme={null}
// Step 2.1: Install the Whop SDK
import { WhopClient } from "@whop/sdk";
import { unwrapWebhook } from "@whop/sdk/helpers";
import express from "express";

const app = express();

const whop = new WhopClient({
	token: process.env.WHOP_API_KEY,
});

app.post("/api/pay", express.json(), async (req, res) => {
	const { confirmationToken, orderId } = req.body;

	// Step 2.2: Create the payment from the confirmation token
	const payment = await whop.payments.create({
		account_id: process.env.WHOP_ACCOUNT_ID,
		variant_id: "plan_xxxxxxxxxxxxx",
		confirmation_token: confirmationToken,
		return_url: `https://yoursite.com/checkout/return?order=${orderId}`,
		metadata: { order_id: orderId },
	});

	// client_secret is safe to send to the browser. Your API key never is.
	res.json({
		id: payment.id,
		status: payment.status,
		client_secret: payment.client_secret,
	});
});

// Step 2.3: Fulfill from the webhook
// Read the raw body on this route only: parsing it first changes the bytes and the signature check fails.
app.post(
	"/api/webhooks/whop",
	express.raw({ type: "application/json" }),
	(req, res) => {
		const event = unwrapWebhook(req.body.toString("utf8"), {
			headers: req.headers,
			key: process.env.WHOP_WEBHOOK_SECRET!,
		});

		if (event.type === "payment.succeeded") {
			// metadata.order_id is the value you set when you created the payment.
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
# Step 2.1: Install the Whop SDK
import os

from fastapi import BackgroundTasks, FastAPI, Request, Response
from whop_sdk import Whop
from whop_sdk.lib.verify_webhook import unwrap

app = FastAPI()
client = Whop(token=os.environ["WHOP_API_KEY"])


@app.post("/api/pay")
async def pay(request: Request):
    body = await request.json()

    # Step 2.2: Create the payment from the confirmation token
    payment = client.payments.create(
        request={
            "account_id": os.environ["WHOP_ACCOUNT_ID"],
            "variant_id": "plan_xxxxxxxxxxxxx",
            "confirmation_token": body["confirmationToken"],
            "return_url": f"https://yoursite.com/checkout/return?order={body['orderId']}",
            "metadata": {"order_id": body["orderId"]},
        },
    )

    # client_secret is safe to send to the browser. Your API key never is.
    return {
        "id": payment.id,
        "status": payment.status,
        "client_secret": payment.client_secret,
    }


# Step 2.3: Fulfill from the webhook
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
    # metadata.order_id is the value you set when you created the payment.
    order_id = (payment.get("metadata") or {}).get("order_id")
    print(f"[PAYMENT SUCCEEDED] {payment['id']} for order {order_id}")
    # Mark the order paid in your database and grant access here.
```

## Next steps

* [Payment elements reference](https://docs.whop.com/elements/latest/payments/overview): every option, event, and method on the Payments handle.
* [Express checkout](https://docs.whop.com/developer/guides/express-checkout.md): add one-press Apple Pay and Google Pay next to the form.
* [Save payment methods](https://docs.whop.com/developer/guides/save-payment-methods.md): save a method without charging it, then charge it later from your server.
* [Embed a checkout](https://docs.whop.com/developer/guides/embed-checkout.md): skip the form and drop in Whop's whole checkout instead.


## Related topics

- [Embed a Checkout](https://docs.whop.com/developer/guides/embed-checkout.md)
- [Accept Payments](https://docs.whop.com/developer/guides/accept-payments.md)
- [Save Payment Methods](https://docs.whop.com/developer/guides/save-payment-methods.md)
