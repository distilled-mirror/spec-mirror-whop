> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Express Checkout

> Add one-press Apple Pay and Google Pay to your site with the payment request resource or the express checkout element

Express checkout lets a buyer pay with the wallet already on their device. One press opens the Apple Pay or Google Pay sheet, the sheet collects what the purchase still needs, and the payment settles without a form. Whop Elements gives you two ways to add it. The payment request resource opens the sheet from your own button and hands you a token to confirm on your server. The express checkout element renders the wallet buttons for a plan and confirms the payment for you.

<Tip>
  [`CheckoutElement`](/elements/latest/checkout/checkout) already shows wallet buttons above its form, and the [payment elements](/elements/latest/payments/overview) offer the wallets as payment methods. Use this guide when you want wallet buttons on their own: a buy button on a product page, or a cart you price yourself.
</Tip>

## Choose your integration

|                            | Payment request                        | Express checkout element                |
| -------------------------- | -------------------------------------- | --------------------------------------- |
| **Effort**                 | High                                   | Low                                     |
| **Buttons**                | You render your own button             | Whop renders the wallet buttons         |
| **Price**                  | You pass `currency` and `amount`       | From the plan or checkout configuration |
| **Shipping and repricing** | Your handlers answer each sheet change | Handled by the checkout session         |
| **Server code required**   | Yes                                    | No                                      |
| **Best for**               | A cart or total you compute yourself   | One-press purchase of a plan            |

## Before you start

Both paths need the same three things:

* **A verified payment method domain.** Wallets open only on pages whose domain you registered and verified with Whop. Follow [Enable Apple Pay and Google Pay](/payments/apple-pay) once per domain. Pages on whop.com are pre-approved.
* **An `https` page.** Apple and Google require a secure page for their sheets, and `returnUrl` must be `https` too (`http` only on localhost).
* **A device with a wallet.** Both paths show only the wallets the buyer's device can pay with, so test on a real device with a wallet set up, not in a simulator.

Install the packages for React, or load the script for plain JavaScript:

<CodeGroup>
  ```bash React theme={null}
  npm install @whop/elements-react @whop/elements
  ```

  ```html JavaScript theme={null}
  <script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>
  ```
</CodeGroup>

<Tabs>
  <Tab title="Payment request">
    [`PaymentRequest`](/elements/latest/payments/paymentRequest) is the wallet sheet without an element. You render the button, name the amount, and answer the sheet's shipping and billing changes. The sheet hands back a confirmation token that your server confirms with the Payments API.

    ### Step 1: Create the request and check availability

    Create the request with your account, the currency, and the amount in minor units. Await `canMakePayment()` before you show a button. It reports which wallets this device, this account, and this domain can pay with, and it primes the sheet.

    <CodeGroup>
      ```tsx React theme={null}
      import { useEffect, useRef, useState } from "react";
      import { WhopElements, useWhop } from "@whop/elements-react";
      import { loadWhop } from "@whop/elements";
      import type { PaymentRequestResource } from "@whop/elements/payments";

      type Wallet = "apple_pay" | "google_pay";

      export function ProductPage() {
        return (
          <WhopElements elements={loadWhop()}>
            <WalletButtons />
          </WhopElements>
        );
      }

      function WalletButtons() {
        const whop = useWhop();
        const requestRef = useRef<PaymentRequestResource | null>(null);
        const [wallets, setWallets] = useState<Wallet[]>([]);
        const [error, setError] = useState<string | null>(null);

        useEffect(() => {
          if (!whop) return;
          const paymentRequest = whop.payments.paymentRequest.create({
            accountId: "biz_xxxxxxxxxxxxx",
            currency: "usd",
            amount: 1000,
            lineItems: [{ label: "Pro plan", amount: 1000 }],
          });
          requestRef.current = paymentRequest;
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
            body: JSON.stringify({ confirmationToken: ctok }),
          });
          const payment = await response.json();
          if (payment.status === "paid") {
            window.location.assign("/checkout/complete");
            return;
          }

          const result = await whop.payments.handleNextAction({
            clientSecret: payment.client_secret,
          });
          if (result.redirected) return;
          if (result.status === "succeeded") {
            window.location.assign("/checkout/complete");
            return;
          }
          if (result.status === "processing") {
            window.location.assign("/checkout/pending");
            return;
          }
          setError(result.lastPaymentError?.message ?? "The payment wasn't completed. Try again.");
        }

        return (
          <>
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

      ```html JavaScript theme={null}
      <button id="apple-pay" hidden>Pay with Apple Pay</button>
      <button id="google-pay" hidden>Pay with Google Pay</button>
      <p id="error" role="alert"></p>

      <script type="module">
        const whop = window.WhopElements();
        const paymentRequest = whop.payments.paymentRequest.create({
          accountId: "biz_xxxxxxxxxxxxx",
          currency: "usd",
          amount: 1000,
          lineItems: [{ label: "Pro plan", amount: 1000 }],
        });

        const { applePay, googlePay } = await paymentRequest.canMakePayment();
        document.querySelector("#apple-pay").hidden = !applePay;
        document.querySelector("#google-pay").hidden = !googlePay;

        const errorLine = document.querySelector("#error");

        async function pay(wallet) {
          errorLine.textContent = "";

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
            body: JSON.stringify({ confirmationToken: ctok }),
          });
          const payment = await response.json();
          if (payment.status === "paid") {
            window.location.assign("/checkout/complete");
            return;
          }

          const result = await whop.payments.handleNextAction({
            clientSecret: payment.client_secret,
          });
          if (result.redirected) return;
          if (result.status === "succeeded") {
            window.location.assign("/checkout/complete");
            return;
          }
          if (result.status === "processing") {
            window.location.assign("/checkout/pending");
            return;
          }
          errorLine.textContent = result.lastPaymentError?.message ?? "The payment wasn't completed. Try again.";
        }

        document.querySelector("#apple-pay").addEventListener("click", () => pay("apple_pay"));
        document.querySelector("#google-pay").addEventListener("click", () => pay("google_pay"));
      </script>
      ```
    </CodeGroup>

    <Note>
      Apple and Google each require their own button artwork on a custom button. Follow the [Apple Pay button guidelines](https://developer.apple.com/design/human-interface-guidelines/apple-pay) and the [Google Pay brand guidelines](https://developers.google.com/pay/api/web/guides/brand-guidelines).
    </Note>

    ### Step 2: Open the sheet from your button

    Call `show(type)` first thing in the button's event handler, before any `await`. Apple refuses a sheet opened outside that user gesture, which is why `canMakePayment()` ran ahead of time. The sheet asks for the buyer's email unless you pass one: `show("apple_pay", { email })` fills it from a field your page already collected.

    `show` resolves with the result once the buyer authorizes:

    | Field      | What it holds                                                                         |
    | ---------- | ------------------------------------------------------------------------------------- |
    | `ctok`     | The single-use confirmation token, prefixed `ctok_`. Send it to your server.          |
    | `type`     | `apple_pay` or `google_pay`.                                                          |
    | `payer`    | The buyer's `email`, `name`, `phone`, and billing `country`, as the wallet gave them. |
    | `shipping` | The `address` and chosen `option` when you requested shipping, otherwise `null`.      |

    A buyer who dismisses the sheet rejects `show` with a `payment_request_cancelled` error and fires `onCancel`. The sheet closes with a checkmark as soon as the token mints, before your server confirms, so show your server's result on your page.

    ### Step 3: Confirm the payment on your server

    Create the payment with the confirmation token. Whop resolves the buyer from the token's email, charges the plan, and returns the payment with a `client_secret` the browser uses to finish any pending step.

    <CodeGroup>
      ```typescript TypeScript theme={null}
      import { WhopClient } from "@whop/sdk";

      const client = new WhopClient({ token: process.env.WHOP_API_KEY });

      export async function POST(request: Request) {
        const { confirmationToken } = await request.json();

        const payment = await client.payments.create({
          account_id: "biz_xxxxxxxxxxxxx",
          plan: { currency: "usd", initial_price: 10.0, plan_type: "one_time" },
          confirmation_token: confirmationToken,
          return_url: "https://yoursite.com/checkout/complete",
          metadata: { order_id: "order_12345" },
        });

        return Response.json({
          id: payment.id,
          status: payment.status,
          client_secret: payment.client_secret,
        });
      }
      ```

      ```python Python theme={null}
      import os
      from flask import Flask, jsonify, request
      from whop_sdk import Whop

      app = Flask(__name__)
      client = Whop(token=os.environ["WHOP_API_KEY"])

      @app.post("/api/pay")
      def pay():
          payment = client.payments.create(
              request={
                  "account_id": "biz_xxxxxxxxxxxxx",
                  "plan": {"currency": "usd", "initial_price": 10.0, "plan_type": "one_time"},
                  "confirmation_token": request.get_json()["confirmationToken"],
                  "return_url": "https://yoursite.com/checkout/complete",
                  "metadata": {"order_id": "order_12345"},
              },
          )
          return jsonify(id=payment.id, status=payment.status, client_secret=payment.client_secret)
      ```

      ```ruby Ruby theme={null}
      require "whop_sdk"

      client = Whop_sdk::Client.new(token: ENV.fetch("WHOP_API_KEY"))

      post "/api/pay" do
        confirmation_token = JSON.parse(request.body.read)["confirmationToken"]

        payment = client.payments.create(
          account_id: "biz_xxxxxxxxxxxxx",
          plan: { currency: "usd", initial_price: 10.0, plan_type: "one_time" },
          confirmation_token: confirmation_token,
          return_url: "https://yoursite.com/checkout/complete",
          metadata: { order_id: "order_12345" },
        )

        { id: payment.id, status: payment.status, client_secret: payment.client_secret }.to_json
      end
      ```
    </CodeGroup>

    Charge what the sheet showed. The request's `amount` is in minor units, so `1000` on the sheet is an `initial_price` of `10.0` here. Pass `plan_id` for a plan you already created, or an inline `plan` to find or create one for this payment. `metadata` comes back on the payment and its webhook, which is how you tie the charge to your own order. For physical goods, send the result's `shipping.address` along with the token and pass it as `shipping_address`. Its `recipient` becomes `name`. The `line1`, `line2`, `city`, `state`, `postal_code`, and `country` fields keep their names.

    ### Step 4: Finish the payment

    A payment that comes back `paid` is done. Any other status means the buyer still has a step, such as 3D Secure, or the charge is still being decided. Pass the payment's `client_secret` to `handleNextAction`. It runs an inline step in a dialog and resolves with `redirected: false`, or sends the buyer to your `returnUrl` and resolves with `redirected: true`. Branch on the `status` it returns: `succeeded` is paid, `processing` is still being decided, and anything else needs another try. A dismissed dialog leaves the payment at `requires_action` with no error, so a missing `lastPaymentError` isn't success.

    Whether the buyer pays inline or comes back through `returnUrl`, fulfill from the `payment.succeeded` webhook below, never from the browser.

    ### Collect a shipping address

    Set `requestShipping` to ask the sheet for a shipping address and offer `shippingOptions`. The sheet reports each change with a redacted address (city, state, postal code, and country) before authorization. Answer every change with `updateWith` exactly once, or the sheet stalls until the wallet times it out. The resource doesn't calculate shipping: your handler sets the new `amount`.

    ```ts theme={null}
    const paymentRequest = whop.payments.paymentRequest.create({
      accountId: "biz_xxxxxxxxxxxxx",
      currency: "usd",
      amount: 1000,
      requestShipping: true,
      shippingOptions: [
        { id: "standard", label: "Standard", detail: "5-7 business days", amount: 0 },
        { id: "express", label: "Express", detail: "1-2 business days", amount: 900 },
      ],
    });

    paymentRequest.onShippingAddressChange((event) => {
      if (event.address.country !== "US") {
        event.updateWith({
          errors: [{ code: "shipping_address_unserviceable", message: "We only ship within the US." }],
        });
        return;
      }
      event.updateWith({ amount: 1000 });
    });

    paymentRequest.onShippingOptionChange((event) => {
      event.updateWith({ amount: event.option.id === "express" ? 1900 : 1000 });
    });
    ```

    The result's `shipping.address` is the full address once the buyer authorizes, and `shipping.option` is the option they picked. `onBillingAddressChange` works the same way for tax: it fires at sheet open and on card switches with the redacted billing address, and you answer `updateWith({ amount })` or `updateWith({})` to keep the total.

    For every option, method, and event, see the [PaymentRequest reference](/elements/latest/payments/paymentRequest).
  </Tab>

  <Tab title="Express checkout element">
    [`ExpressCheckoutElement`](/elements/latest/checkout/expressCheckout) renders Apple Pay and Google Pay buttons for a plan. The wallet sheet collects what the purchase still needs: the buyer's email, a phone number where the seller collects one, and a shipping address for physical goods. Whop confirms the payment against the checkout session, so there's no server code to write.

    ### Step 1: Mount the buttons

    Mount the element inside a `Checkout` handle. Pass the `plan` to sell, or a `checkoutConfiguration` you [created on your server](/developer/guides/accept-payments#option-2-take-payments-on-your-own-site) with an inline plan and your own `metadata`. Set `returnUrl` to the page you host over `https` where the buyer lands after paying.

    <CodeGroup>
      ```tsx React theme={null}
      import { WhopElements, Checkout, ExpressCheckoutElement } from "@whop/elements-react";
      import { loadWhop } from "@whop/elements";

      export function BuyButton() {
        return (
          <WhopElements elements={loadWhop()}>
            <Checkout
              plan="plan_xxxxxxxxxxxxx"
              returnUrl="https://yoursite.com/checkout/complete"
            >
              <ExpressCheckoutElement layout="horizontal" />
            </Checkout>
          </WhopElements>
        );
      }
      ```

      ```html JavaScript theme={null}
      <div id="express-checkout"></div>

      <script type="module">
        const checkout = window.WhopElements().checkout.create({
          plan: "plan_xxxxxxxxxxxxx",
          returnUrl: "https://yoursite.com/checkout/complete",
        });
        checkout.create("expressCheckout", { layout: "horizontal" }).mount("#express-checkout");
      </script>
      ```
    </CodeGroup>

    The element renders only the wallets the buyer's device can pay with, best-native-first for the browser. Where no wallet is available it renders nothing, so keep a regular checkout path on the page.

    <Note>
      `ExpressCheckoutElement` and `CheckoutElement` are alternatives inside one `Checkout` handle. Mount one at a time. For wallet buttons above a full checkout form, mount `CheckoutElement` alone. It already includes them.
    </Note>

    ### Step 2: Let the payment settle

    A press opens the wallet sheet. When the buyer authorizes, Whop confirms the payment against the checkout session, and the sheet's checkmark follows that confirm, never precedes it. A finished checkout redirects the current tab to `returnUrl`, including the page that contains the element. Without a `returnUrl`, the buyer rests on the element's own outcome line.

    A payment that still needs a step, such as 3D Secure, runs it in a dialog when the step can be framed. A full-page step, such as a bank redirect, leaves for it and returns the buyer to `returnUrl`. If the buyer dismisses a pending step, the element offers to finish it.

    Fulfill from the `payment.succeeded` webhook below, never from the browser.

    ### Step 3: Customize the buttons

    * `wallets` filters which wallets may render, for example `["apple_pay"]`. The element never shows a wallet the device or the checkout's payment method configuration can't back.
    * `layout` stacks the buttons. `auto` follows the container width, `horizontal` and `vertical` force one arrangement.
    * `promoCode`, `affiliateCode`, `attribution`, and `metadata` on the `Checkout` handle record where the sale came from and your own reference data on the order.
    * `appearance` themes the element's outcome and error lines. See [Appearance](/elements/latest/appearance).

    For every option, event, and method, see the [ExpressCheckoutElement reference](/elements/latest/checkout/expressCheckout).
  </Tab>
</Tabs>

## Handle payment webhooks

Fulfill orders from the `payment.succeeded` webhook on your server, never from the browser. A wallet payment reports `apple_pay` or `google_pay` in `payment_method_type`, and the `metadata` you attached comes back on the event. The [accept payments guide](/developer/guides/accept-payments#handle-payment-webhooks) shows a handler and a sample payload.

## Next steps

<CardGroup cols={2}>
  <Card title="Accept payments" icon="credit-card" href="/developer/guides/accept-payments">
    Checkout links, the Checkout element, and the payment elements
  </Card>

  <Card title="Enable Apple Pay and Google Pay" icon="apple" href="/payments/apple-pay">
    Verify your domain as a payment method domain
  </Card>

  <Card title="Webhooks" icon="webhook" href="/developer/guides/webhooks">
    Handle payment events in real-time
  </Card>

  <Card title="Appearance" icon="palette" href="/elements/latest/appearance">
    Theme the elements to match your site
  </Card>
</CardGroup>
