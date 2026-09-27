> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Embed a Checkout

> Drop Whop's whole checkout into your page with the Checkout element, straight from the browser, and fulfill from a webhook

export const guide = {
  "title": "Embed a checkout",
  "description": "Drop Whop's whole checkout into your own page with the Checkout element. It runs entirely in the browser. Your server only listens for the webhook that says the buyer paid.",
  "categoryOrder": ["frontend", "backend"],
  "steps": [{
    "title": "Embed the checkout",
    "subSteps": [[{
      "match": {
        "frontend": "react"
      },
      "title": "Install the packages",
      "body": "Install the Whop Elements packages for React.",
      "install": {
        "npm": "npm install @whop/elements-react @whop/elements",
        "pnpm": "pnpm add @whop/elements-react @whop/elements"
      }
    }, {
      "match": {
        "frontend": "html"
      },
      "title": "Add the script tag",
      "body": "Load the Whop Elements script on the page that hosts the checkout."
    }], {
      "title": "Mount the Checkout element",
      "body": ["Mount `CheckoutElement` inside a `Checkout` handle and pass the `plan` the buyer is purchasing. The element opens the checkout session itself, prices the order, and collects everything the seller set up. It needs no server code and no API key on the page: the plan defines the price, so the browser never asserts an amount.", "Pass `metadata`, such as your own order ID, on the handle. Whop copies it to the payment and to every webhook about it. Set `returnUrl` to a page you host over `https`. The buyer lands there after an off-site payment step, such as 3D Secure, a bank page, or a financing application. Add your own query parameters to it, such as the order ID. They survive the round trip.", "To attach an affiliate code, a promo code, or campaign attribution, pass `affiliateCode`, `promoCode`, or `attribution` on the handle. A [checkout configuration](/api-reference/beta/checkout-configurations/create-a-checkout-configuration) created on your server is only needed when you want those presets kept out of client code."]
    }, {
      "title": "Record the completion",
      "body": ["`onComplete` fires once when the checkout completes inside the element, before any navigation to `returnUrl`. Use it for analytics and ad pixels.", "Don't fulfill from it. A checkout restored on a later page load fires it again for the same result, and an off-site payment step completes on the return page instead."]
    }]
  }, {
    "title": "Handle the return page",
    "subSteps": [{
      "title": "Read the outcome from the URL",
      "body": ["The buyer arrives at `returnUrl` **whatever happened** during the off-site step. Whop appends two query parameters. `payment` is the `pay_` ID. `status` is `succeeded` when the buyer paid, and `failed` or `canceled` when they didn't. A financing application the lender declined lands here with `status=canceled`, exactly as an approved one lands with `status=succeeded`.", "Your own parameters, such as `order`, stay on the URL. Card declines never reach this page, because the element shows them inline. The return page is the one place where your site sees a failed off-site payment."]
    }, {
      "title": "Show the right face",
      "body": ["Branch on `status`. `succeeded` gets the thank-you page. `failed` and `canceled` get a message that the buyer wasn't charged and a link back to the checkout. Anything else, such as `processing`, means the charge is still pending. Tell the buyer you'll confirm by email.", "Treat the page as a display of the outcome, not as proof of payment. Anyone can type `status=succeeded` into a URL. Access, downloads, and order fulfillment come from the `payment.succeeded` webhook on your server."]
    }]
  }, {
    "title": "Fulfill from your server",
    "subSteps": [[{
      "match": {
        "backend": "nextjs"
      },
      "title": "Install the Whop SDK",
      "body": "The webhook helper ships with the Whop SDK. Install it on the server that receives the webhook.",
      "install": {
        "npm": "npm install @whop/sdk @vercel/functions",
        "pnpm": "pnpm add @whop/sdk @vercel/functions"
      }
    }, {
      "match": {
        "backend": "express"
      },
      "title": "Install the Whop SDK",
      "body": "The webhook helper ships with the Whop SDK. Install it on the server that receives the webhook.",
      "install": {
        "npm": "npm install @whop/sdk express",
        "pnpm": "pnpm add @whop/sdk express"
      }
    }, {
      "match": {
        "backend": "python"
      },
      "title": "Install the Whop SDK",
      "body": "The webhook helper ships with the Whop SDK. Install it on the server that receives the webhook.",
      "install": {
        "pip": "pip install whop-sdk fastapi",
        "poetry": "poetry add whop-sdk fastapi"
      }
    }], {
      "title": "Handle the payment webhook",
      "body": ["Whop sends `payment.succeeded` to your webhook endpoint once the buyer has paid. Verify the signature with the SDK helper, then do the fulfillment work: mark the order paid, grant access, and send the email. The payment carries the `metadata` you set on the Checkout handle.", "Fulfillment belongs here and nowhere else. The buyer can close the browser before your return page loads, a redirect can fail, and anyone can type a query parameter by hand. Follow the [Webhooks guide](/developer/guides/webhooks) to create the webhook and store its `ws_` secret as `WHOP_WEBHOOK_SECRET`."]
    }]
  }]
};

export const code = {
  frontend: {
    react: [{
      code: `// [step:1.1:start]
import { Checkout, CheckoutElement, WhopElements } from "@whop/elements-react";
import { loadWhop } from "@whop/elements";
// [step:1.1:end]

export function CheckoutPage({ orderId }: { orderId: string }) {
	return (
		<WhopElements elements={loadWhop()}>
			{/* [step:1.2:start] */}
			<Checkout
				plan="plan_xxxxxxxxxxxxx"
				metadata={{ order_id: orderId }}
				returnUrl={\`https://yoursite.com/checkout/return?order=\${orderId}\`}
				// [step:1.2:end]
				// [step:1.3:start]
				onComplete={(completion) => {
					if (completion.result === "payment") {
						// Fire analytics or ad pixels here. Fulfill from the webhook, not from this callback.
						console.log("Checkout completed", completion.paymentId);
					}
				}}
				// [step:1.3:end]
			>
				{/* [step:1.2] */}
				<CheckoutElement />
			</Checkout>
		</WhopElements>
	);
}
`,
      filename: "CheckoutPage.tsx",
      language: "tsx"
    }, {
      code: `export function CheckoutReturn() {
	// [step:2.1:start]
	// Whop appends \`payment\` and \`status\` to your returnUrl. Your own parameters (here \`order\`) survive.
	const params = new URLSearchParams(window.location.search);
	const paymentId = params.get("payment");
	const status = params.get("status");
	const orderId = params.get("order");
	// [step:2.1:end]

	// [step:2.2:start]
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
				<a href={\`/checkout?order=\${orderId}\`}>Back to checkout</a>
			</section>
		);
	}

	return (
		<section>
			<h1>Confirming your payment</h1>
			<p>Your payment is still being processed. We will email you as soon as it is confirmed.</p>
		</section>
	);
	// [step:2.2:end]
}
`,
      filename: "CheckoutReturn.tsx",
      language: "tsx"
    }],
    html: [{
      code: `<!DOCTYPE html>
<html>
  <head>
    <!-- [step:1.1] -->
    <script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>
  </head>
  <body>
    <div id="checkout"></div>

    <script type="module">
      const orderId = new URLSearchParams(window.location.search).get("order");

      // [step:1.2:start]
      const checkout = window.WhopElements().checkout.create({
        plan: "plan_xxxxxxxxxxxxx",
        metadata: { order_id: orderId },
        returnUrl: \`https://yoursite.com/checkout/return?order=\${orderId}\`,
        // [step:1.2:end]
        // [step:1.3:start]
        onComplete: (completion) => {
          if (completion.result === "payment") {
            // Fire analytics or ad pixels here. Fulfill from the webhook, not from this callback.
            console.log("Checkout completed", completion.paymentId);
          }
        },
        // [step:1.3:end]
      });
      // [step:1.2]
      checkout.create("checkout").mount("#checkout");
    </script>
  </body>
</html>
`,
      filename: "checkout.html",
      language: "html"
    }, {
      code: `<!DOCTYPE html>
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
      // [step:2.1:start]
      // Whop appends \`payment\` and \`status\` to your returnUrl. Your own parameters (here \`order\`) survive.
      const params = new URLSearchParams(window.location.search);
      const paymentId = params.get("payment");
      const status = params.get("status");
      const orderId = params.get("order");
      // [step:2.1:end]

      // [step:2.2:start]
      if (status === "succeeded") {
        document.querySelector("#payment-id").textContent = paymentId;
        document.querySelector("#succeeded").hidden = false;
      } else if (status === "failed" || status === "canceled") {
        document.querySelector("#retry").href = \`/checkout?order=\${orderId}\`;
        document.querySelector("#failed").hidden = false;
      } else {
        document.querySelector("#pending").hidden = false;
      }
      // [step:2.2:end]
    </script>
  </body>
</html>
`,
      filename: "return.html",
      language: "html"
    }]
  },
  backend: {
    nextjs: [{
      code: `// [step:3.1:start]
import { waitUntil } from "@vercel/functions";
import { unwrapWebhook } from "@whop/sdk/helpers";
import type { NextRequest } from "next/server";
// [step:3.1:end]

export async function POST(request: NextRequest) {
	// [step:3.2:start]
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
	// [step:3.2:end]

	// Respond in less than 5 seconds, or Whop retries.
	return new Response("OK", { status: 200 });
}

async function fulfillOrder(payment: Record<string, any>) {
	// [step:3.2:start]
	// metadata.order_id is the value you set on the Checkout handle.
	const orderId = payment.metadata?.order_id;
	console.log(\`[PAYMENT SUCCEEDED] \${payment.id} for order \${orderId}\`);
	// Mark the order paid in your database and grant access here.
	// [step:3.2:end]
}
`,
      filename: "app/api/webhooks/whop/route.ts",
      language: "typescript"
    }],
    express: [{
      code: `// [step:3.1:start]
import { unwrapWebhook } from "@whop/sdk/helpers";
import express from "express";

const app = express();
// [step:3.1:end]

// [step:3.2:start]
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
			console.log(\`[PAYMENT SUCCEEDED] \${event.data.id} for order \${orderId}\`);
			// Mark the order paid in your database and grant access here.
		}

		// Respond in less than 5 seconds, or Whop retries.
		res.sendStatus(200);
	},
);
// [step:3.2:end]

app.listen(3000);
`,
      filename: "server.ts",
      language: "typescript"
    }],
    python: [{
      code: `# [step:3.1:start]
import os

from fastapi import BackgroundTasks, FastAPI, Request, Response
from whop_sdk.lib.verify_webhook import unwrap

app = FastAPI()
# [step:3.1:end]


# [step:3.2:start]
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
# [step:3.2:end]
`,
      filename: "server.py",
      language: "python"
    }]
  }
};

Drop Whop's whole checkout into your own page with the Checkout element. It runs entirely in the browser. Your server only listens for the webhook that says the buyer paid.

The files under **Code** carry `Step X.Y` comments that point back to the steps below. Pick one server implementation and one client implementation. The list includes the files for every option.

## 1. Embed the checkout

### 1.1 Install the packages (React)

Install the Whop Elements packages for React.

```bash npm theme={null}
npm install @whop/elements-react @whop/elements
```

```bash pnpm theme={null}
pnpm add @whop/elements-react @whop/elements
```

### 1.1 Add the script tag (JavaScript)

Load the Whop Elements script on the page that hosts the checkout.

### 1.2 Mount the Checkout element

Mount `CheckoutElement` inside a `Checkout` handle and pass the `plan` the buyer is purchasing. The element opens the checkout session itself, prices the order, and collects everything the seller set up. It needs no server code and no API key on the page: the plan defines the price, so the browser never asserts an amount.

Pass `metadata`, such as your own order ID, on the handle. Whop copies it to the payment and to every webhook about it. Set `returnUrl` to a page you host over `https`. The buyer lands there after an off-site payment step, such as 3D Secure, a bank page, or a financing application. Add your own query parameters to it, such as the order ID. They survive the round trip.

To attach an affiliate code, a promo code, or campaign attribution, pass `affiliateCode`, `promoCode`, or `attribution` on the handle. A [checkout configuration](/api-reference/beta/checkout-configurations/create-a-checkout-configuration) created on your server is only needed when you want those presets kept out of client code.

### 1.3 Record the completion

`onComplete` fires once when the checkout completes inside the element, before any navigation to `returnUrl`. Use it for analytics and ad pixels.

Don't fulfill from it. A checkout restored on a later page load fires it again for the same result, and an off-site payment step completes on the return page instead.

## 2. Handle the return page

### 2.1 Read the outcome from the URL

The buyer arrives at `returnUrl` **whatever happened** during the off-site step. Whop appends two query parameters. `payment` is the `pay_` ID. `status` is `succeeded` when the buyer paid, and `failed` or `canceled` when they didn't. A financing application the lender declined lands here with `status=canceled`, exactly as an approved one lands with `status=succeeded`.

Your own parameters, such as `order`, stay on the URL. Card declines never reach this page, because the element shows them inline. The return page is the one place where your site sees a failed off-site payment.

### 2.2 Show the right face

Branch on `status`. `succeeded` gets the thank-you page. `failed` and `canceled` get a message that the buyer wasn't charged and a link back to the checkout. Anything else, such as `processing`, means the charge is still pending. Tell the buyer you'll confirm by email.

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

Whop sends `payment.succeeded` to your webhook endpoint once the buyer has paid. Verify the signature with the SDK helper, then do the fulfillment work: mark the order paid, grant access, and send the email. The payment carries the `metadata` you set on the Checkout handle.

Fulfillment belongs here and nowhere else. The buyer can close the browser before your return page loads, a redirect can fail, and anyone can type a query parameter by hand. Follow the [Webhooks guide](/developer/guides/webhooks) to create the webhook and store its `ws_` secret as `WHOP_WEBHOOK_SECRET`.

## Code

### Client: React — `CheckoutPage.tsx`

```tsx CheckoutPage.tsx theme={null}
// Step 1.1: Install the packages
import { Checkout, CheckoutElement, WhopElements } from "@whop/elements-react";
import { loadWhop } from "@whop/elements";

export function CheckoutPage({ orderId }: { orderId: string }) {
	return (
		<WhopElements elements={loadWhop()}>
			{/* Step 1.2: Mount the Checkout element */}
			<Checkout
				plan="plan_xxxxxxxxxxxxx"
				metadata={{ order_id: orderId }}
				returnUrl={`https://yoursite.com/checkout/return?order=${orderId}`}
				// Step 1.3: Record the completion
				onComplete={(completion) => {
					if (completion.result === "payment") {
						// Fire analytics or ad pixels here. Fulfill from the webhook, not from this callback.
						console.log("Checkout completed", completion.paymentId);
					}
				}}
			>
				{/* Step 1.2: Mount the Checkout element */}
				<CheckoutElement />
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

### Client: JavaScript — `checkout.html`

```html checkout.html theme={null}
<!DOCTYPE html>
<html>
  <head>
    <!-- Step 1.1: Add the script tag -->
    <script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>
  </head>
  <body>
    <div id="checkout"></div>

    <script type="module">
      const orderId = new URLSearchParams(window.location.search).get("order");

      // Step 1.2: Mount the Checkout element
      const checkout = window.WhopElements().checkout.create({
        plan: "plan_xxxxxxxxxxxxx",
        metadata: { order_id: orderId },
        returnUrl: `https://yoursite.com/checkout/return?order=${orderId}`,
        // Step 1.3: Record the completion
        onComplete: (completion) => {
          if (completion.result === "payment") {
            // Fire analytics or ad pixels here. Fulfill from the webhook, not from this callback.
            console.log("Checkout completed", completion.paymentId);
          }
        },
      });
      // Step 1.2: Mount the Checkout element
      checkout.create("checkout").mount("#checkout");
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

* [Checkout element reference](/elements/latest/checkout/overview): every option, event, and method on the Checkout handle.
* [Build a checkout with payment elements](/developer/guides/payment-elements): design the form yourself and confirm the payment from your server.
* [Webhooks](/developer/guides/webhooks): create the endpoint, store the signing secret, and handle retries.
* [Test in the sandbox](/developer/guides/sandbox): run a test charge with cards that succeed, fail, and require action.
