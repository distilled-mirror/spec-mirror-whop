> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Open a Wallet Sheet from Your Own Button

> Open the Apple Pay or Google Pay sheet from a button you render, price the order yourself, and charge the confirmation token from your server

export const guide = {
  "title": "Open a wallet sheet from your own button",
  "description": "Use the payment request resource when you render the buy button yourself and price the order yourself. One press opens the Apple Pay or Google Pay sheet, the sheet hands you a confirmation token, and your server passes the token to the Payments API to charge.",
  "categoryOrder": ["frontend", "backend"],
  "steps": [{
    "title": "Build your wallet button",
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
      "body": "Load the Whop Elements script on the page that shows the button."
    }], {
      "title": "Create the payment request",
      "body": ["Create the request with your account, the currency, and the amount in minor units. `lineItems` are the rows the sheet lists under the total. Nothing mounts: the resource is the wallet sheet without an element, and the button is yours.", "Wallets open only on pages whose domain you registered as a payment method domain. Follow [Enable Apple Pay and Google Pay](/payments/apple-pay) once per domain, serve the page over `https`, and test on a real device with a wallet set up."]
    }, {
      "title": "Check which wallets can pay",
      "body": ["Await `canMakePayment()` before you show a button. It reports which wallets this device, this account, and this domain can pay with, and it primes the sheet so it can open synchronously later. Render a button only for a wallet that came back `true`.", "Apple and Google each require their own button artwork on a custom button. Follow the [Apple Pay button guidelines](https://developer.apple.com/design/human-interface-guidelines/apple-pay) and the [Google Pay brand guidelines](https://developers.google.com/pay/api/web/guides/brand-guidelines)."]
    }, {
      "title": "Open the sheet and send the token to your server",
      "body": ["Call `show(type)` first thing in the button's press handler, before any `await`. Apple refuses a sheet opened outside that user gesture, which is why `canMakePayment()` ran ahead of time. The sheet asks for the buyer's email unless you pass one with `show(\"apple_pay\", { email })`.", "`show` resolves once the buyer authorizes. `ctok` is the single-use confirmation token, `type` names the wallet, and `payer` carries the buyer's email, name, phone, and billing country as the wallet gave them. A buyer who dismisses the sheet rejects `show` with a `payment_request_cancelled` error. The sheet closes with a checkmark as soon as the token mints, before your server confirms, so show your server's result on your page.", "Post `ctok` to your server together with your own order reference. To collect a shipping address and offer shipping options, set `requestShipping` and answer the sheet's changes with `updateWith`. The [PaymentRequest reference](/elements/latest/payments/paymentRequest) covers those handlers."]
    }]
  }, {
    "title": "Charge from your server",
    "subSteps": [[{
      "match": {
        "backend": "nextjs"
      },
      "title": "Install the Whop SDK",
      "body": "Install the Whop SDK and create a client with your Account API key. The key stays on the server. The browser never sees it.",
      "install": {
        "npm": "npm install @whop/sdk @vercel/functions",
        "pnpm": "pnpm add @whop/sdk @vercel/functions"
      }
    }, {
      "match": {
        "backend": "express"
      },
      "title": "Install the Whop SDK",
      "body": "Install the Whop SDK and create a client with your Account API key. The key stays on the server. The browser never sees it.",
      "install": {
        "npm": "npm install @whop/sdk express",
        "pnpm": "pnpm add @whop/sdk express"
      }
    }, {
      "match": {
        "backend": "python"
      },
      "title": "Install the Whop SDK",
      "body": "Install the Whop SDK and create a client with your Account API key. The key stays on the server. The browser never sees it.",
      "install": {
        "pip": "pip install whop-sdk fastapi",
        "poetry": "poetry add whop-sdk fastapi"
      }
    }], {
      "title": "Create the payment from the confirmation token",
      "body": ["Pass the token to the Payments API. Whop resolves the buyer from the token's email, charges the plan, and returns the payment with a `client_secret`. Return `id`, `status`, and `client_secret` to the browser. The `client_secret` only unlocks this one payment, so it's safe to expose.", "Charge what the sheet showed. The request's `amount` is in minor units, so `1000` on the sheet is an `initial_price` of `10.0` here. Pass an inline `plan` to find or create one for this price, or `plan_id` for a plan you already created. Set `return_url` to the page the buyer should land on after a full-page step, and put your own order ID in `metadata` so the payment and its webhooks carry it."],
      "bullets": ["`plan` finds or creates the plan for this price. Use `plan_id` when the price already exists as a plan", "`confirmation_token` stands in for the wallet card the sheet authorized", "`metadata` ties the payment back to your own order", "For physical goods, send the result's `shipping.address` with the token and pass it as `shipping_address`"]
    }, {
      "title": "Fulfill from the webhook",
      "body": ["Whop sends `payment.succeeded` to your webhook endpoint once the charge succeeds. Verify the signature with the SDK helper, then do the fulfillment work: mark the order paid, grant access, and send the email. A wallet payment reports `apple_pay` or `google_pay` in `payment_method_type`.", "Fulfillment belongs here and nowhere else. The sheet's checkmark, the browser, and a query parameter can all say paid without the charge having settled. Follow the [Webhooks guide](/developer/guides/webhooks) to create the webhook and store its `ws_` secret as `WHOP_WEBHOOK_SECRET`."]
    }]
  }, {
    "title": "Finish the payment",
    "subSteps": [{
      "title": "Handle the next action",
      "body": ["A payment that comes back `paid` is complete. Anything else means the buyer still has a step, such as 3D Secure, or the charge is still pending. Pass the `client_secret` to `handleNextAction`. It runs an inline step in a dialog and resolves with `redirected: false`, or sends the buyer to your `return_url` and resolves with `redirected: true`.", "Branch on the `status` it returns. `succeeded` means the buyer paid, `processing` means the charge is still pending, and anything else needs another try. `lastPaymentError` carries the reason when there is one. A dismissed dialog leaves the payment at `requires_action` with no error, so a missing error never means success."]
    }, {
      "title": "Read the outcome from the URL",
      "body": ["A buyer who left for a full-page step arrives at `return_url` **whatever happened** there. Whop appends three query parameters. `payment` is the `pay_` ID. `status` is `succeeded` when the buyer paid, `failed` or `canceled` when they didn't, and a pending status such as `processing` while the charge is still undecided. `client_secret` lets the page poll the payment's status if it wants to wait for a decision.", "Your own parameters, such as `order`, stay on the URL. `handleNextAction` reports inline declines itself, so the return page is the one place where your site sees a failed off-site step."]
    }, {
      "title": "Show the right face",
      "body": ["Branch on `status`. `succeeded` gets the thank-you page. `failed` and `canceled` get a message that the buyer wasn't charged and a link back to the product. Anything else means the charge is still pending. Tell the buyer you'll confirm by email. The inline paths in the button handler send the buyer to the same page with the same parameters, so one page handles every ending.", "Treat the page as a display of the outcome, not as proof of payment. Anyone can type `status=succeeded` into a URL. Access, downloads, and order fulfillment come from the `payment.succeeded` webhook on your server."]
    }]
  }]
};

export const code = {
  frontend: {
    react: [{
      code: `// [step:1.1:start]
import { useEffect, useRef, useState } from "react";
import { WhopElements, useWhop } from "@whop/elements-react";
import { loadWhop } from "@whop/elements";
import type { PaymentRequestResource } from "@whop/elements/payments";
// [step:1.1:end]

type Wallet = "apple_pay" | "google_pay";

export function ProductPage({ orderId }: { orderId: string }) {
	return (
		<WhopElements elements={loadWhop()}>
			<WalletButtons orderId={orderId} />
		</WhopElements>
	);
}

function WalletButtons({ orderId }: { orderId: string }) {
	const whop = useWhop();
	const requestRef = useRef<PaymentRequestResource | null>(null);
	const [wallets, setWallets] = useState<Wallet[]>([]);
	const [error, setError] = useState<string | null>(null);

	useEffect(() => {
		if (!whop) return;
		// [step:1.2:start]
		// Amounts are in minor units: 1000 is \$10.00.
		const paymentRequest = whop.payments.paymentRequest.create({
			accountId: "biz_xxxxxxxxxxxxx",
			currency: "usd",
			amount: 1000,
			lineItems: [{ label: "Pro plan", amount: 1000 }],
		});
		requestRef.current = paymentRequest;
		// [step:1.2:end]
		// [step:1.3:start]
		paymentRequest.canMakePayment().then(({ applePay, googlePay }) => {
			setWallets([
				...(applePay ? (["apple_pay"] as const) : []),
				...(googlePay ? (["google_pay"] as const) : []),
			]);
		});
		// [step:1.3:end]
	}, [whop]);

	async function pay(wallet: Wallet) {
		const paymentRequest = requestRef.current;
		if (!paymentRequest || !whop) return;
		setError(null);

		// [step:1.4:start]
		// show() must run inside the press handler, before any await, or Apple refuses the sheet.
		let ctok: string;
		try {
			({ ctok } = await paymentRequest.show(wallet));
		} catch (err) {
			if ((err as Error).message !== "payment_request_cancelled") {
				setError("The wallet sheet couldn't complete. Try again.");
			}
			return;
		}

		const response = await fetch("/api/pay", {
			method: "POST",
			headers: { "Content-Type": "application/json" },
			body: JSON.stringify({ confirmationToken: ctok, orderId }),
		});
		const payment = await response.json();
		// [step:1.4:end]

		// [step:3.1:start]
		if (payment.status === "paid") {
			window.location.assign(\`/checkout/return?order=\${orderId}&status=succeeded&payment=\${payment.id}\`);
			return;
		}

		// Anything but paid means the buyer still has a step, or the charge is still being decided.
		const result = await whop.payments.handleNextAction({
			clientSecret: payment.client_secret,
		});
		// The buyer left for an off-site step; they come back on the return page.
		if (result.redirected) return;

		if (result.status === "succeeded") {
			window.location.assign(\`/checkout/return?order=\${orderId}&status=succeeded&payment=\${payment.id}\`);
			return;
		}
		if (result.status === "processing") {
			window.location.assign(\`/checkout/return?order=\${orderId}&status=processing&payment=\${payment.id}\`);
			return;
		}
		// A dismissed dialog leaves the payment at requires_action with no error, so a missing error never means success.
		setError(result.lastPaymentError?.message ?? "The payment wasn't completed. Try again.");
		// [step:3.1:end]
	}

	return (
		<>
			{/* [step:1.3:start] */}
			{wallets.map((wallet) => (
				<button key={wallet} onClick={() => pay(wallet)}>
					{wallet === "apple_pay" ? "Pay with Apple Pay" : "Pay with Google Pay"}
				</button>
			))}
			{/* [step:1.3:end] */}
			{error && <p role="alert">{error}</p>}
		</>
	);
}
`,
      filename: "WalletButtons.tsx",
      language: "tsx"
    }, {
      code: `export function CheckoutReturn() {
	// [step:3.2:start]
	// Whop appends \`payment\`, \`status\`, and \`client_secret\` to your returnUrl. Your own parameters (here \`order\`) survive.
	const params = new URLSearchParams(window.location.search);
	const paymentId = params.get("payment");
	const status = params.get("status");
	const orderId = params.get("order");
	// [step:3.2:end]

	// [step:3.3:start]
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
	// [step:3.3:end]
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
    <button id="apple-pay" hidden>Pay with Apple Pay</button>
    <button id="google-pay" hidden>Pay with Google Pay</button>
    <p id="error" role="alert"></p>

    <script type="module">
      const orderId = new URLSearchParams(window.location.search).get("order");

      // [step:1.2:start]
      // Amounts are in minor units: 1000 is \$10.00.
      const whop = window.WhopElements();
      const paymentRequest = whop.payments.paymentRequest.create({
        accountId: "biz_xxxxxxxxxxxxx",
        currency: "usd",
        amount: 1000,
        lineItems: [{ label: "Pro plan", amount: 1000 }],
      });
      // [step:1.2:end]

      // [step:1.3:start]
      const { applePay, googlePay } = await paymentRequest.canMakePayment();
      document.querySelector("#apple-pay").hidden = !applePay;
      document.querySelector("#google-pay").hidden = !googlePay;
      // [step:1.3:end]

      const errorLine = document.querySelector("#error");

      async function pay(wallet) {
        errorLine.textContent = "";

        // [step:1.4:start]
        // show() must run inside the press handler, before any await, or Apple refuses the sheet.
        let ctok;
        try {
          ({ ctok } = await paymentRequest.show(wallet));
        } catch (err) {
          if (err.message !== "payment_request_cancelled") {
            errorLine.textContent = "The wallet sheet couldn't complete. Try again.";
          }
          return;
        }

        const response = await fetch("/api/pay", {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify({ confirmationToken: ctok, orderId }),
        });
        const payment = await response.json();
        // [step:1.4:end]

        // [step:3.1:start]
        if (payment.status === "paid") {
          window.location.assign(\`/checkout/return?order=\${orderId}&status=succeeded&payment=\${payment.id}\`);
          return;
        }

        // Anything but paid means the buyer still has a step, or the charge is still being decided.
        const result = await whop.payments.handleNextAction({
          clientSecret: payment.client_secret,
        });
        // The buyer left for an off-site step; they come back on the return page.
        if (result.redirected) return;

        if (result.status === "succeeded") {
          window.location.assign(\`/checkout/return?order=\${orderId}&status=succeeded&payment=\${payment.id}\`);
          return;
        }
        if (result.status === "processing") {
          window.location.assign(\`/checkout/return?order=\${orderId}&status=processing&payment=\${payment.id}\`);
          return;
        }
        // A dismissed dialog leaves the payment at requires_action with no error, so a missing error never means success.
        errorLine.textContent = result.lastPaymentError?.message ?? "The payment wasn't completed. Try again.";
        // [step:3.1:end]
      }

      // [step:1.3:start]
      document.querySelector("#apple-pay").addEventListener("click", () => pay("apple_pay"));
      document.querySelector("#google-pay").addEventListener("click", () => pay("google_pay"));
      // [step:1.3:end]
    </script>
  </body>
</html>
`,
      filename: "buy.html",
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
      // [step:3.2:start]
      // Whop appends \`payment\`, \`status\`, and \`client_secret\` to your returnUrl. Your own parameters (here \`order\`) survive.
      const params = new URLSearchParams(window.location.search);
      const paymentId = params.get("payment");
      const status = params.get("status");
      const orderId = params.get("order");
      // [step:3.2:end]

      // [step:3.3:start]
      if (status === "succeeded") {
        document.querySelector("#payment-id").textContent = paymentId;
        document.querySelector("#succeeded").hidden = false;
      } else if (status === "failed" || status === "canceled") {
        document.querySelector("#retry").href = \`/checkout?order=\${orderId}\`;
        document.querySelector("#failed").hidden = false;
      } else {
        document.querySelector("#pending").hidden = false;
      }
      // [step:3.3:end]
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
      code: `// [step:2.1:start]
import { WhopClient } from "@whop/sdk";

const whop = new WhopClient({
	token: process.env.WHOP_API_KEY,
});
// [step:2.1:end]

export async function POST(request: Request) {
	const { confirmationToken, orderId } = await request.json();

	// [step:2.2:start]
	const payment = await whop.payments.create({
		account_id: process.env.WHOP_ACCOUNT_ID,
		// Charge what the sheet showed: 1000 minor units on the sheet is an initial_price of 10.0 here.
		plan: { currency: "usd", initial_price: 10.0, plan_type: "one_time" },
		confirmation_token: confirmationToken,
		return_url: \`https://yoursite.com/checkout/return?order=\${orderId}\`,
		metadata: { order_id: orderId },
	});

	// client_secret is safe to send to the browser. Your API key never is.
	return Response.json({
		id: payment.id,
		status: payment.status,
		client_secret: payment.client_secret,
	});
	// [step:2.2:end]
}
`,
      filename: "app/api/pay/route.ts",
      language: "typescript"
    }, {
      code: `import { waitUntil } from "@vercel/functions";
import { unwrapWebhook } from "@whop/sdk/helpers";
import type { NextRequest } from "next/server";

export async function POST(request: NextRequest) {
	// [step:2.3:start]
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
	// [step:2.3:end]

	// Respond in less than 5 seconds, or Whop retries.
	return new Response("OK", { status: 200 });
}

async function fulfillOrder(payment: Record<string, any>) {
	// [step:2.3:start]
	// metadata.order_id is the value you set when you created the payment.
	const orderId = payment.metadata?.order_id;
	console.log(\`[PAYMENT SUCCEEDED] \${payment.id} for order \${orderId}\`);
	// Mark the order paid in your database and grant access here.
	// [step:2.3:end]
}
`,
      filename: "app/api/webhooks/whop/route.ts",
      language: "typescript"
    }],
    express: [{
      code: `// [step:2.1:start]
import { WhopClient } from "@whop/sdk";
import { unwrapWebhook } from "@whop/sdk/helpers";
import express from "express";

const app = express();

const whop = new WhopClient({
	token: process.env.WHOP_API_KEY,
});
// [step:2.1:end]

app.post("/api/pay", express.json(), async (req, res) => {
	const { confirmationToken, orderId } = req.body;

	// [step:2.2:start]
	const payment = await whop.payments.create({
		account_id: process.env.WHOP_ACCOUNT_ID,
		// Charge what the sheet showed: 1000 minor units on the sheet is an initial_price of 10.0 here.
		plan: { currency: "usd", initial_price: 10.0, plan_type: "one_time" },
		confirmation_token: confirmationToken,
		return_url: \`https://yoursite.com/checkout/return?order=\${orderId}\`,
		metadata: { order_id: orderId },
	});

	// client_secret is safe to send to the browser. Your API key never is.
	res.json({
		id: payment.id,
		status: payment.status,
		client_secret: payment.client_secret,
	});
	// [step:2.2:end]
});

// [step:2.3:start]
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
			console.log(\`[PAYMENT SUCCEEDED] \${event.data.id} for order \${orderId}\`);
			// Mark the order paid in your database and grant access here.
		}

		// Respond in less than 5 seconds, or Whop retries.
		res.sendStatus(200);
	},
);
// [step:2.3:end]

app.listen(3000);
`,
      filename: "server.ts",
      language: "typescript"
    }],
    python: [{
      code: `# [step:2.1:start]
import os

from fastapi import BackgroundTasks, FastAPI, Request, Response
from whop_sdk import Whop
from whop_sdk.lib.verify_webhook import unwrap

app = FastAPI()
client = Whop(token=os.environ["WHOP_API_KEY"])
# [step:2.1:end]


@app.post("/api/pay")
async def pay(request: Request):
    body = await request.json()

    # [step:2.2:start]
    payment = client.payments.create(
        request={
            "account_id": os.environ["WHOP_ACCOUNT_ID"],
            # Charge what the sheet showed: 1000 minor units on the sheet is an initial_price of 10.0 here.
            "plan": {"currency": "usd", "initial_price": 10.0, "plan_type": "one_time"},
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
    # [step:2.2:end]


# [step:2.3:start]
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
# [step:2.3:end]
`,
      filename: "server.py",
      language: "python"
    }]
  }
};

Use the payment request resource when you render the buy button yourself and price the order yourself. One press opens the Apple Pay or Google Pay sheet, the sheet hands you a confirmation token, and your server passes the token to the Payments API to charge.

The files under **Code** carry `Step X.Y` comments that point back to the steps below. Pick one server implementation and one client implementation. The list includes the files for every option.

## 1. Build your wallet button

### 1.1 Install the packages (React)

Install the Whop Elements packages for React.

```bash npm theme={null}
npm install @whop/elements-react @whop/elements
```

```bash pnpm theme={null}
pnpm add @whop/elements-react @whop/elements
```

### 1.1 Add the script tag (JavaScript)

Load the Whop Elements script on the page that shows the button.

### 1.2 Create the payment request

Create the request with your account, the currency, and the amount in minor units. `lineItems` are the rows the sheet lists under the total. Nothing mounts: the resource is the wallet sheet without an element, and the button is yours.

Wallets open only on pages whose domain you registered as a payment method domain. Follow [Enable Apple Pay and Google Pay](/payments/apple-pay) once per domain, serve the page over `https`, and test on a real device with a wallet set up.

### 1.3 Check which wallets can pay

Await `canMakePayment()` before you show a button. It reports which wallets this device, this account, and this domain can pay with, and it primes the sheet so it can open synchronously later. Render a button only for a wallet that came back `true`.

Apple and Google each require their own button artwork on a custom button. Follow the [Apple Pay button guidelines](https://developer.apple.com/design/human-interface-guidelines/apple-pay) and the [Google Pay brand guidelines](https://developers.google.com/pay/api/web/guides/brand-guidelines).

### 1.4 Open the sheet and send the token to your server

Call `show(type)` first thing in the button's press handler, before any `await`. Apple refuses a sheet opened outside that user gesture, which is why `canMakePayment()` ran ahead of time. The sheet asks for the buyer's email unless you pass one with `show("apple_pay", \{ email \})`.

`show` resolves once the buyer authorizes. `ctok` is the single-use confirmation token, `type` names the wallet, and `payer` carries the buyer's email, name, phone, and billing country as the wallet gave them. A buyer who dismisses the sheet rejects `show` with a `payment_request_cancelled` error. The sheet closes with a checkmark as soon as the token mints, before your server confirms, so show your server's result on your page.

Post `ctok` to your server together with your own order reference. To collect a shipping address and offer shipping options, set `requestShipping` and answer the sheet's changes with `updateWith`. The [PaymentRequest reference](/elements/latest/payments/paymentRequest) covers those handlers.

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

Pass the token to the Payments API. Whop resolves the buyer from the token's email, charges the plan, and returns the payment with a `client_secret`. Return `id`, `status`, and `client_secret` to the browser. The `client_secret` only unlocks this one payment, so it's safe to expose.

Charge what the sheet showed. The request's `amount` is in minor units, so `1000` on the sheet is an `initial_price` of `10.0` here. Pass an inline `plan` to find or create one for this price, or `plan_id` for a plan you already created. Set `return_url` to the page the buyer should land on after a full-page step, and put your own order ID in `metadata` so the payment and its webhooks carry it.

* `plan` finds or creates the plan for this price. Use `plan_id` when the price already exists as a plan
* `confirmation_token` stands in for the wallet card the sheet authorized
* `metadata` ties the payment back to your own order
* For physical goods, send the result's `shipping.address` with the token and pass it as `shipping_address`

### 2.3 Fulfill from the webhook

Whop sends `payment.succeeded` to your webhook endpoint once the charge succeeds. Verify the signature with the SDK helper, then do the fulfillment work: mark the order paid, grant access, and send the email. A wallet payment reports `apple_pay` or `google_pay` in `payment_method_type`.

Fulfillment belongs here and nowhere else. The sheet's checkmark, the browser, and a query parameter can all say paid without the charge having settled. Follow the [Webhooks guide](/developer/guides/webhooks) to create the webhook and store its `ws_` secret as `WHOP_WEBHOOK_SECRET`.

## 3. Finish the payment

### 3.1 Handle the next action

A payment that comes back `paid` is complete. Anything else means the buyer still has a step, such as 3D Secure, or the charge is still pending. Pass the `client_secret` to `handleNextAction`. It runs an inline step in a dialog and resolves with `redirected: false`, or sends the buyer to your `return_url` and resolves with `redirected: true`.

Branch on the `status` it returns. `succeeded` means the buyer paid, `processing` means the charge is still pending, and anything else needs another try. `lastPaymentError` carries the reason when there is one. A dismissed dialog leaves the payment at `requires_action` with no error, so a missing error never means success.

### 3.2 Read the outcome from the URL

A buyer who left for a full-page step arrives at `return_url` **whatever happened** there. Whop appends three query parameters. `payment` is the `pay_` ID. `status` is `succeeded` when the buyer paid, `failed` or `canceled` when they didn't, and a pending status such as `processing` while the charge is still undecided. `client_secret` lets the page poll the payment's status if it wants to wait for a decision.

Your own parameters, such as `order`, stay on the URL. `handleNextAction` reports inline declines itself, so the return page is the one place where your site sees a failed off-site step.

### 3.3 Show the right face

Branch on `status`. `succeeded` gets the thank-you page. `failed` and `canceled` get a message that the buyer wasn't charged and a link back to the product. Anything else means the charge is still pending. Tell the buyer you'll confirm by email. The inline paths in the button handler send the buyer to the same page with the same parameters, so one page handles every ending.

Treat the page as a display of the outcome, not as proof of payment. Anyone can type `status=succeeded` into a URL. Access, downloads, and order fulfillment come from the `payment.succeeded` webhook on your server.

## Code

### Client: React — `WalletButtons.tsx`

```tsx WalletButtons.tsx theme={null}
// Step 1.1: Install the packages
import { useEffect, useRef, useState } from "react";
import { WhopElements, useWhop } from "@whop/elements-react";
import { loadWhop } from "@whop/elements";
import type { PaymentRequestResource } from "@whop/elements/payments";

type Wallet = "apple_pay" | "google_pay";

export function ProductPage({ orderId }: { orderId: string }) {
	return (
		<WhopElements elements={loadWhop()}>
			<WalletButtons orderId={orderId} />
		</WhopElements>
	);
}

function WalletButtons({ orderId }: { orderId: string }) {
	const whop = useWhop();
	const requestRef = useRef<PaymentRequestResource | null>(null);
	const [wallets, setWallets] = useState<Wallet[]>([]);
	const [error, setError] = useState<string | null>(null);

	useEffect(() => {
		if (!whop) return;
		// Step 1.2: Create the payment request
		// Amounts are in minor units: 1000 is $10.00.
		const paymentRequest = whop.payments.paymentRequest.create({
			accountId: "biz_xxxxxxxxxxxxx",
			currency: "usd",
			amount: 1000,
			lineItems: [{ label: "Pro plan", amount: 1000 }],
		});
		requestRef.current = paymentRequest;
		// Step 1.3: Check which wallets can pay
		paymentRequest.canMakePayment().then(({ applePay, googlePay }) => {
			setWallets([
				...(applePay ? (["apple_pay"] as const) : []),
				...(googlePay ? (["google_pay"] as const) : []),
			]);
		});
	}, [whop]);

	async function pay(wallet: Wallet) {
		const paymentRequest = requestRef.current;
		if (!paymentRequest || !whop) return;
		setError(null);

		// Step 1.4: Open the sheet and send the token to your server
		// show() must run inside the press handler, before any await, or Apple refuses the sheet.
		let ctok: string;
		try {
			({ ctok } = await paymentRequest.show(wallet));
		} catch (err) {
			if ((err as Error).message !== "payment_request_cancelled") {
				setError("The wallet sheet couldn't complete. Try again.");
			}
			return;
		}

		const response = await fetch("/api/pay", {
			method: "POST",
			headers: { "Content-Type": "application/json" },
			body: JSON.stringify({ confirmationToken: ctok, orderId }),
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
		setError(result.lastPaymentError?.message ?? "The payment wasn't completed. Try again.");
	}

	return (
		<>
			{/* Step 1.3: Check which wallets can pay */}
			{wallets.map((wallet) => (
				<button key={wallet} onClick={() => pay(wallet)}>
					{wallet === "apple_pay" ? "Pay with Apple Pay" : "Pay with Google Pay"}
				</button>
			))}
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

### Client: JavaScript — `buy.html`

```html buy.html theme={null}
<!DOCTYPE html>
<html>
  <head>
    <!-- Step 1.1: Add the script tag -->
    <script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>
  </head>
  <body>
    <button id="apple-pay" hidden>Pay with Apple Pay</button>
    <button id="google-pay" hidden>Pay with Google Pay</button>
    <p id="error" role="alert"></p>

    <script type="module">
      const orderId = new URLSearchParams(window.location.search).get("order");

      // Step 1.2: Create the payment request
      // Amounts are in minor units: 1000 is $10.00.
      const whop = window.WhopElements();
      const paymentRequest = whop.payments.paymentRequest.create({
        accountId: "biz_xxxxxxxxxxxxx",
        currency: "usd",
        amount: 1000,
        lineItems: [{ label: "Pro plan", amount: 1000 }],
      });

      // Step 1.3: Check which wallets can pay
      const { applePay, googlePay } = await paymentRequest.canMakePayment();
      document.querySelector("#apple-pay").hidden = !applePay;
      document.querySelector("#google-pay").hidden = !googlePay;

      const errorLine = document.querySelector("#error");

      async function pay(wallet) {
        errorLine.textContent = "";

        // Step 1.4: Open the sheet and send the token to your server
        // show() must run inside the press handler, before any await, or Apple refuses the sheet.
        let ctok;
        try {
          ({ ctok } = await paymentRequest.show(wallet));
        } catch (err) {
          if (err.message !== "payment_request_cancelled") {
            errorLine.textContent = "The wallet sheet couldn't complete. Try again.";
          }
          return;
        }

        const response = await fetch("/api/pay", {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify({ confirmationToken: ctok, orderId }),
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
        errorLine.textContent = result.lastPaymentError?.message ?? "The payment wasn't completed. Try again.";
      }

      // Step 1.3: Check which wallets can pay
      document.querySelector("#apple-pay").addEventListener("click", () => pay("apple_pay"));
      document.querySelector("#google-pay").addEventListener("click", () => pay("google_pay"));
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
		// Charge what the sheet showed: 1000 minor units on the sheet is an initial_price of 10.0 here.
		plan: { currency: "usd", initial_price: 10.0, plan_type: "one_time" },
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
		// Charge what the sheet showed: 1000 minor units on the sheet is an initial_price of 10.0 here.
		plan: { currency: "usd", initial_price: 10.0, plan_type: "one_time" },
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
            # Charge what the sheet showed: 1000 minor units on the sheet is an initial_price of 10.0 here.
            "plan": {"currency": "usd", "initial_price": 10.0, "plan_type": "one_time"},
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

* [PaymentRequest reference](/elements/latest/payments/paymentRequest): shipping and billing change handlers, `updateWith`, and every option on the resource.
* [Add express checkout](/developer/guides/express-checkout): let Whop render the wallet buttons for a plan and confirm the payment for you.
* [Enable Apple Pay and Google Pay](/payments/apple-pay): register and verify your domain as a payment method domain.
* [Webhooks](/developer/guides/webhooks): create the endpoint, store the signing secret, and handle retries.
