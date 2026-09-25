> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Save Payment Methods

> Save customer payment methods to charge them later

Use saved payment methods to charge customers automatically for subscriptions, renewals, or usage-based billing. The customer saves their card once using a Whop hosted or embedded flow, and you can charge it any time after.

<Tip>
  Pair this guide with [webhooks](/developer/guides/webhooks): listen for `setup_intent.succeeded` to confirm the save, then for `payment.succeeded` / `payment.failed` to track future charges.
</Tip>

## Pick your save flow

Two paths, depending on whether the user is also paying right now.

|                           | Setup mode (collect-only)                                                                                                                                                                                                                                                                                                                                                                                            | Save during checkout                                                                                                 |
| ------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| **Charges the user now**  | No                                                                                                                                                                                                                                                                                                                                                                                                                   | Yes                                                                                                                  |
| **Use case**              | Free trial signup, on-file card before usage-based billing                                                                                                                                                                                                                                                                                                                                                           | Subscription that renews after the first paid checkout                                                               |
| **How**                   | Mount the [payment elements](/elements/latest/payments/overview) in `mode: "setup"` and [create a setup intent](/api-reference/beta/setup-intents/create-setup-intent) with the confirmation token, or open a setup-mode [checkout configuration](/api-reference/beta/checkout-configurations/create-a-checkout-configuration) as a hosted checkout or in the [Checkout element](/elements/latest/checkout/overview) | Mount the payment elements with `setupFutureUsage: "off_session"` and create the payment with the confirmation token |
| **Webhook to listen for** | `setup_intent.succeeded`                                                                                                                                                                                                                                                                                                                                                                                             | `payment.succeeded` (its `payment_method_id` is the saved method)                                                    |

The rest of this page covers setup mode end-to-end, then shows how to charge a saved method later.

## Save a payment method with the payment elements

Build the save form on the [payment elements](/elements/latest/payments/overview) in `mode: "setup"`. The element offers only methods that can be saved, shows the buyer the future-use consent the save requires, and hands you a confirmation token. You turn that token into a setup intent from your server, and the setup intent's `client_secret` finishes any step the buyer still owes.

<Steps>
  <Step title="Mount the form in setup mode">
    Pass `mode="setup"` and the currency the saved method will be used with. There is no amount, so amount limits don't apply. Set `returnUrl` to a page you host over `https`. Bank enrollments and some 3D Secure flows bring the buyer back there.

    <CodeGroup>
      ```tsx React theme={null}
      import { useState } from "react";
      import {
        WhopElements,
        Payments,
        EmailElement,
        PaymentElement,
        BrandingElement,
        usePayments,
        useWhop,
      } from "@whop/elements-react";
      import { loadWhop } from "@whop/elements";

      export function SavePaymentMethodPage() {
        return (
          <WhopElements elements={loadWhop()}>
            <Payments
              accountId="biz_XXXXXXXX"
              mode="setup"
              currency="usd"
              returnUrl="https://yoursite.com/billing/saved"
            >
              <SaveForm />
            </Payments>
          </WhopElements>
        );
      }

      function SaveForm() {
        const payments = usePayments();
        const whop = useWhop();
        const [ready, setReady] = useState(false);
        const [error, setError] = useState<string | null>(null);

        async function save() {
          if (!payments || !whop) return;
          setError(null);

          const { confirmationToken } = await payments.createConfirmationToken({});

          const response = await fetch("/api/save-payment-method", {
            method: "POST",
            headers: { "Content-Type": "application/json" },
            body: JSON.stringify({ confirmationToken }),
          });
          const setupIntent = await response.json();
          if (setupIntent.status === "succeeded") {
            window.location.assign("/billing/saved");
            return;
          }
          if (setupIntent.status === "canceled") {
            setError("The payment method couldn't be saved. Try a different one.");
            return;
          }
          if (setupIntent.status === "processing") {
            window.location.assign("/billing/pending");
            return;
          }

          const result = await whop.payments.handleNextAction({
            clientSecret: setupIntent.client_secret,
          });
          if (result.redirected) return;
          if (result.status === "succeeded") {
            window.location.assign("/billing/saved");
            return;
          }
          if (result.status === "processing") {
            window.location.assign("/billing/pending");
            return;
          }
          setError(result.lastPaymentError?.message ?? "The payment method wasn't saved. Try again.");
        }

        return (
          <>
            <EmailElement />
            <PaymentElement onChange={(event) => setReady(event.complete)} />
            <BrandingElement />
            <button disabled={!ready} onClick={save}>
              Save payment method
            </button>
            {error && <p role="alert">{error}</p>}
          </>
        );
      }
      ```

      ```html JavaScript theme={null}
      <script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>

      <div id="email"></div>
      <div id="payment"></div>
      <div id="branding"></div>
      <button id="save" disabled>Save payment method</button>
      <p id="error" role="alert"></p>

      <script type="module">
        const whop = window.WhopElements();
        const payments = whop.payments.create({
          accountId: "biz_XXXXXXXX",
          mode: "setup",
          currency: "usd",
          returnUrl: "https://yoursite.com/billing/saved",
        });

        const saveButton = document.querySelector("#save");
        const errorLine = document.querySelector("#error");

        payments.create("email").mount("#email");
        payments
          .create("payment", { onChange: (event) => (saveButton.disabled = !event.complete) })
          .mount("#payment");
        payments.create("branding").mount("#branding");

        saveButton.addEventListener("click", async () => {
          errorLine.textContent = "";

          const { confirmationToken } = await payments.createConfirmationToken({});

          const response = await fetch("/api/save-payment-method", {
            method: "POST",
            headers: { "Content-Type": "application/json" },
            body: JSON.stringify({ confirmationToken }),
          });
          const setupIntent = await response.json();
          if (setupIntent.status === "succeeded") {
            window.location.assign("/billing/saved");
            return;
          }
          if (setupIntent.status === "canceled") {
            errorLine.textContent = "The payment method couldn't be saved. Try a different one.";
            return;
          }
          if (setupIntent.status === "processing") {
            window.location.assign("/billing/pending");
            return;
          }

          const result = await whop.payments.handleNextAction({
            clientSecret: setupIntent.client_secret,
          });
          if (result.redirected) return;
          if (result.status === "succeeded") {
            window.location.assign("/billing/saved");
            return;
          }
          if (result.status === "processing") {
            window.location.assign("/billing/pending");
            return;
          }
          errorLine.textContent = result.lastPaymentError?.message ?? "The payment method wasn't saved. Try again.";
        });
      </script>
      ```
    </CodeGroup>

    The token carries the consent the element displayed. A token collected from a payment-mode form is refused by the setup intent, so mount in `mode="setup"` when the buyer isn't paying.
  </Step>

  <Step title="Create the setup intent on your server">
    [Create a setup intent](/api-reference/beta/setup-intents/create-setup-intent) with the confirmation token. Whop resolves the buyer from the token's email and answers `201 Created` with the setup intent. It carries the `status`, a `client_secret` for any step the buyer still owes, and once it has `succeeded`, the saved method as `payment_method_id`. Attach `metadata` to tie the saved method to a customer in your system. It comes back on the webhook. Send an [`Idempotency-Key`](/developer/api/idempotency) so a retried request never saves the method twice.

    <CodeGroup>
      ```typescript TypeScript theme={null}
      import { WhopClient } from "@whop/sdk";

      const client = new WhopClient({ token: process.env.WHOP_API_KEY });

      export async function POST(request: Request) {
        const { confirmationToken } = await request.json();

        const setupIntent = await client.setupIntents.create({
          account_id: "biz_XXXXXXXX",
          confirmation_token: confirmationToken,
          currency: "usd",
          return_url: "https://yoursite.com/billing/saved",
          metadata: { customer_id: "my_internal_user_id" },
        });

        return Response.json({
          id: setupIntent.id,
          status: setupIntent.status,
          client_secret: setupIntent.client_secret,
        });
      }
      ```

      ```python Python theme={null}
      import os
      from flask import Flask, jsonify, request
      from whop_sdk import Whop

      app = Flask(__name__)
      client = Whop(token=os.environ["WHOP_API_KEY"])

      @app.post("/api/save-payment-method")
      def save_payment_method():
          setup_intent = client.setup_intents.create(
              request={
                  "account_id": "biz_XXXXXXXX",
                  "confirmation_token": request.get_json()["confirmationToken"],
                  "currency": "usd",
                  "return_url": "https://yoursite.com/billing/saved",
                  "metadata": {"customer_id": "my_internal_user_id"},
              },
          )
          return jsonify(id=setup_intent.id, status=setup_intent.status, client_secret=setup_intent.client_secret)
      ```

      ```ruby Ruby theme={null}
      require "whop_sdk"

      client = Whop_sdk::Client.new(token: ENV.fetch("WHOP_API_KEY"))

      post "/api/save-payment-method" do
        confirmation_token = JSON.parse(request.body.read)["confirmationToken"]

        setup_intent = client.setup_intents.create(
          account_id: "biz_XXXXXXXX",
          confirmation_token: confirmation_token,
          currency: "usd",
          return_url: "https://yoursite.com/billing/saved",
          metadata: { customer_id: "my_internal_user_id" },
        )

        { id: setup_intent.id, status: setup_intent.status, client_secret: setup_intent.client_secret }.to_json
      end
      ```
    </CodeGroup>
  </Step>

  <Step title="Finish any pending step">
    A setup intent that comes back `succeeded` has the method on file. `requires_action` means the buyer still has a step like 3D Secure or a bank enrollment. Pass the `client_secret` to `handleNextAction`, the same call the payment flow uses. It runs an inline step in a dialog or sends the buyer to your `returnUrl`. Branch on the `status` it returns: `succeeded` saved the method, `processing` is still deciding, and anything else needs another try. A dismissed dialog leaves the setup at `requires_action` with no error, and `lastPaymentError` carries the reason when the attempt fails. `processing` means the processor is still deciding, so show a pending state and let the webhook below confirm the save. `canceled` means the buyer abandoned the step or the provider refused the method: `last_setup_error` carries the provider's reason, and stays `null` when the buyer walked away. To read the state from your server instead, [retrieve the setup intent](/api-reference/beta/setup-intents/retrieve-setup-intent) and branch on the same `status`, or poll the lighter [Retrieve setup status](/api-reference/beta/setup-intents/retrieve-setup-status) while a step is pending.
  </Step>

  <Step title="Handle completion">
    Listen for the `setup_intent.succeeded` webhook to get the payment method ID and your `metadata` back. It's the only signal that survives a closed tab, and the same handler serves the hosted flow below. The setup intent also carries `payment_instrument` for display: a name, the standard icon set, and for a card its brand, last four, and expiry. You can show the saved method without another request. Webhooks pinned before API version 2026-09-22-1 carry the saved method as `payment_method.id` instead.

    <Warning>
      `webhooks.unwrap` isn't available in the current `@whop/sdk`. Both webhook handlers on this page are kept for reference and won't run as written. See [Verify and handle events](/developer/guides/webhooks#verify-and-handle-events) for the current verification path.
    </Warning>

    ```typescript theme={null}
    import { waitUntil } from "@vercel/functions";
    import type { NextRequest } from "next/server";
    import { whopsdk } from "@/lib/whop-sdk";

    export async function POST(request: NextRequest): Promise<Response> {
      const requestBodyText = await request.text();
      const headers = Object.fromEntries(request.headers);
      const webhookData = whopsdk.webhooks.unwrap(requestBodyText, { headers });

      if (webhookData.type === "setup_intent.succeeded") {
        waitUntil(handleSetupSucceeded(webhookData.data));
      }

      return new Response("OK", { status: 200 });
    }

    async function handleSetupSucceeded(setupIntent) {
      console.log("Payment method ID:", setupIntent.payment_method_id);
      console.log("Saved method:", setupIntent.payment_instrument.display_name);
      console.log("Metadata:", setupIntent.metadata);
    }
    ```

    The payment method is now saved and authorized for this member. Charge it with the steps in [Charge a saved payment method](#charge-a-saved-payment-method).
  </Step>
</Steps>

## Save a payment method with a hosted checkout

<Steps>
  <Step title="Create a checkout configuration in setup mode">
    [Create a checkout configuration](/api-reference/beta/checkout-configurations/create-a-checkout-configuration) without a plan to collect payment details without charging. Add metadata to be able to link the member and payment method to a customer in your system.

    <CodeGroup>
      ```typescript TypeScript theme={null}
      const checkoutConfiguration = await whopsdk.checkoutConfigurations.create({
        account_id: "biz_XXXXXX",
        mode: "setup",
        redirect_url: "https://mywebsite.com/return_location",
        metadata: { customer_id: "my_internal_user_id" },
      });
      ```

      ```python Python theme={null}
      checkout_configuration = whopsdk.checkout_configurations.create(
          account_id="biz_XXXXXX",
          mode="setup",
          redirect_url="https://mywebsite.com/return_location",
          metadata={"customer_id": "my_internal_user_id"},
      )
      ```

      ```ruby Ruby theme={null}
      checkout_configuration = whopsdk.checkout_configurations.create(
        account_id: "biz_XXXXXX",
        mode: "setup",
        redirect_url: "https://mywebsite.com/return_location",
        metadata: { customer_id: "my_internal_user_id" },
      )
      ```

      ```rust Rust theme={null}
      let checkout_configuration = client
          .checkout_configurations
          .create(
              &CreateCheckoutConfigurationsRequest {
                  account_id: Some("biz_XXXXXX".to_string()),
                  mode: Some(CreateCheckoutConfigurationsRequestMode::Setup),
                  redirect_url: Some("https://mywebsite.com/return_location".to_string()),
                  metadata: Some(HashMap::from([(
                      "customer_id".to_string(),
                      json!("my_internal_user_id"),
                  )])),
                  ..Default::default()
              },
              None,
          )
          .await?;
      ```

      ```go Go theme={null}
      checkoutConfiguration, err := client.CheckoutConfigurations.Create(context.TODO(), &whopsdk.CreateCheckoutConfigurationsRequest{
          AccountID:   whopsdk.String("biz_XXXXXX"),
          Mode:        whopsdk.CreateCheckoutConfigurationsRequestModeSetup.Ptr(),
          RedirectURL: whopsdk.String("https://mywebsite.com/return_location"),
          Metadata:    map[string]any{"customer_id": "my_internal_user_id"},
      })
      if err != nil {
          log.Fatal(err)
      }
      fmt.Println(checkoutConfiguration.ID)
      ```
    </CodeGroup>
  </Step>

  <Step title="Direct the user to checkout">
    Mount the Checkout element or redirect the user to save their payment method.

    <Tabs>
      <Tab title="Embedded">
        ```tsx theme={null}
        import { WhopElements, Checkout, CheckoutElement } from "@whop/elements-react";
        import { loadWhop } from "@whop/elements";

        export default function SavePayment({ configurationId }: { configurationId: string }) {
          return (
            <WhopElements elements={loadWhop()}>
              <Checkout
                checkoutConfiguration={configurationId}
                returnUrl="https://yoursite.com/setup/complete"
              >
                <CheckoutElement />
              </Checkout>
            </WhopElements>
          );
        }
        ```

        A setup-mode configuration mounts the same element as a payment-method
        save: nothing is charged, and the finished checkout redirects to
        `returnUrl` with `setup_intent_id` in the query string. Confirm the save
        from the `setup_intent.succeeded` webhook rather than from the redirect.
      </Tab>

      <Tab title="Redirect">
        ```typescript theme={null}
        window.location.href = checkoutConfiguration.purchase_url;
        ```
      </Tab>
    </Tabs>
  </Step>

  <Step title="Handle completion">
    Listen for the `setup_intent.succeeded` webhook to get the payment method ID. The setup intent carries `checkout_configuration_id` and the configuration's `metadata`, which you can use to link the member and payment method to a customer in your system.

    <Note>
      Confirm the save from the webhook rather than from the buyer arriving at your `redirect_url`. It's the only signal that survives a closed tab.
    </Note>

    <Tip>
      The example uses `waitUntil` from `@vercel/functions` to run the handler after responding `200`. On other runtimes (Bun, Cloudflare Workers, Fastify, Hono) swap it for your framework's equivalent background-task primitive or a job queue. See the [webhooks guide](/developer/guides/webhooks) for Python and Ruby handler equivalents.
    </Tip>

    ```typescript theme={null}
    import { waitUntil } from "@vercel/functions";
    import type { NextRequest } from "next/server";
    import { whopsdk } from "@/lib/whop-sdk";

    export async function POST(request: NextRequest): Promise<Response> {
      const requestBodyText = await request.text();
      const headers = Object.fromEntries(request.headers);
      const webhookData = whopsdk.webhooks.unwrap(requestBodyText, { headers });

      if (webhookData.type === "setup_intent.succeeded") {
        waitUntil(handleSetupSucceeded(webhookData.data));
      }

      return new Response("OK", { status: 200 });
    }

    async function handleSetupSucceeded(setupIntent) {
      console.log("Payment method ID:", setupIntent.payment_method_id);
      console.log("Member ID:", setupIntent.member_id);
      console.log("Checkout configuration ID:", setupIntent.checkout_configuration_id);
      console.log("Metadata:", setupIntent.metadata);
    }
    ```

    The payment method is now saved and authorized for this member.
  </Step>
</Steps>

## Charge a saved payment method

<Steps>
  <Step title="Get the payment method">
    [List saved payment methods](/api-reference/payment-methods/list-payment-methods) for a member, or use the `payment_method_id` from the setup intent in the previous step.

    <CodeGroup>
      ```typescript TypeScript theme={null}
      const payment_methods = await whopsdk.paymentMethods.list({
        member_id: "mber_XXXXXXXX",
      });

      const payment_method = payment_methods.data[0];
      ```

      ```python Python theme={null}
      payment_methods = whopsdk.payment_methods.list(member_id="mber_XXXXXXXX")
      payment_method = payment_methods.items[0]
      ```

      ```ruby Ruby theme={null}
      # list returns an Enumerable that pages for you; there is no `.data` on it
      payment_method = whopsdk.payment_methods.list(member_id: "mber_XXXXXXXX").first
      ```

      ```rust Rust theme={null}
      let payment_methods = client
          .payment_methods
          .list(
              &PaymentMethodsListQueryRequest {
                  member_id: Some("mber_XXXXXXXX".to_string()),
                  ..Default::default()
              },
              None,
          )
          .await?;

      let payment_method = &payment_methods.data[0];
      ```

      ```go Go theme={null}
      page, err := client.PaymentMethods.List(context.TODO(), &whopsdk.ListPaymentMethodsRequest{
          MemberID: whopsdk.String("mber_XXXXXXXX"),
      })
      if err != nil {
          log.Fatal(err)
      }

      paymentMethod := page.Results[0]
      fmt.Println(paymentMethod)
      ```
    </CodeGroup>
  </Step>

  <Step title="Create an off-session payment">
    Charge the payment method without customer interaction. The [create payment endpoint](/api-reference/payments/create-payment) returns a payment object immediately and processes the charge asynchronously.

    <CodeGroup>
      ```typescript TypeScript theme={null}
      const payment = await whopsdk.payments.create({
        account_id: "biz_XXXXXXXX",
        plan_id: "plan_XXXXXXXX",
        member_id: "mber_XXXXXXXX",
        payment_method_id: "payt_XXXXXXXXX",
      });

      console.log("Payment:", payment.id);
      ```

      ```python Python theme={null}
      payment = whopsdk.payments.create(
          request={
              "account_id": "biz_XXXXXXXX",
              "plan_id": "plan_XXXXXXXX",
              "member_id": "mber_XXXXXXXX",
              "payment_method_id": "payt_XXXXXXXXX",
          },
      )

      print("Payment:", payment.id)
      ```

      ```ruby Ruby theme={null}
      payment = whopsdk.payments.create(
        account_id: "biz_XXXXXXXX",
        plan_id: "plan_XXXXXXXX",
        member_id: "mber_XXXXXXXX",
        payment_method_id: "payt_XXXXXXXXX",
      )

      puts "Payment: #{payment.id}"
      ```

      ```rust Rust theme={null}
      let payment = client
          .payments
          .create(
              &CreatePaymentsRequestBody::CreatePaymentsRequestBodyOne(
                  CreatePaymentsRequestBodyOne::builder()
                      .account_id("biz_XXXXXXXX")
                      .plan_id("plan_XXXXXXXX")
                      .member_id("mber_XXXXXXXX")
                      .payment_method_id("payt_XXXXXXXXX")
                      .build()
                      .unwrap(),
              ),
              None,
          )
          .await?;

      println!("Payment: {}", payment.payment_fields.id);
      ```

      ```go Go theme={null}
      payment, err := client.Payments.Create(context.TODO(), &whopsdk.CreatePaymentsRequest{
          CreatePaymentsRequestOne: &whopsdk.CreatePaymentsRequestOne{
              AccountID:       "biz_XXXXXXXX",
              PlanID:          "plan_XXXXXXXX",
              MemberID:        "mber_XXXXXXXX",
              PaymentMethodID: whopsdk.String("payt_XXXXXXXXX"),
          },
      })
      if err != nil {
          log.Fatal(err)
      }

      fmt.Println("Payment:", payment.ID)
      ```
    </CodeGroup>

    <Tip>
      To apply a discount, pass the ID of one of your [promo codes](/manage-your-business/growth-marketing/promo-codes) as `promo_code_id` and Whop calculates the discounted price for you. The promo code must be active and valid for the plan, and the plan must belong to a product.
    </Tip>
  </Step>

  <Step title="Handle payment events">
    Listen for payment webhooks to track success or failure.

    ```typescript theme={null}
    if (webhookData.type === "payment.succeeded") {
      await fulfillOrder(webhookData.data);
    }

    if (webhookData.type === "payment.failed") {
      await notifyCustomer(webhookData.data.customer_email, webhookData.data.failure_message);
    }
    ```
  </Step>
</Steps>

## Save during checkout

To save a payment method while charging the buyer, build the checkout on the [payment elements](/elements/latest/payments/overview) and pass `setupFutureUsage: "off_session"` on the `Payments` handle. The elements show the buyer the save consent and record it on the confirmation token, and the payment vaults the method only when that consent is attested.

<Steps>
  <Step title="Mount the form with save consent">
    ```tsx theme={null}
    <WhopElements elements={loadWhop()}>
      <Payments
        accountId="biz_XXXXXXXX"
        plan="plan_XXXXXXXX"
        setupFutureUsage="off_session"
        returnUrl="https://yoursite.com/checkout/complete"
      >
        <EmailElement />
        <PaymentElement onChange={(event) => setReady(event.complete)} />
        <BrandingElement />
      </Payments>
    </WhopElements>
    ```

    [Accept Payments](/developer/guides/accept-payments#option-2-take-payments-on-your-own-site) walks through the pay button, `createConfirmationToken`, and `handleNextAction`.
  </Step>

  <Step title="Create the payment with the confirmation token">
    The create response is the payment as opened, not its outcome. The saved method's `payment_method_id` arrives on the `payment.succeeded` webhook once the charge completes.

    <CodeGroup>
      ```typescript TypeScript theme={null}
      const payment = await whopsdk.payments.create({
        account_id: "biz_XXXXXXXX",
        plan_id: "plan_XXXXXXXX",
        confirmation_token: confirmationToken,
        return_url: "https://yoursite.com/checkout/complete",
      });

      console.log("Payment:", payment.id, payment.status);
      ```

      ```python Python theme={null}
      payment = whopsdk.payments.create(
          request={
              "account_id": "biz_XXXXXXXX",
              "plan_id": "plan_XXXXXXXX",
              "confirmation_token": confirmation_token,
              "return_url": "https://yoursite.com/checkout/complete",
          },
      )

      print("Payment:", payment.id, payment.status)
      ```

      ```ruby Ruby theme={null}
      payment = whopsdk.payments.create(
        account_id: "biz_XXXXXXXX",
        plan_id: "plan_XXXXXXXX",
        confirmation_token: confirmation_token,
        return_url: "https://yoursite.com/checkout/complete",
      )

      puts "Payment: #{payment.id} #{payment.status}"
      ```
    </CodeGroup>
  </Step>

  <Step title="Charge it later">
    `payment_method_id` is the `payt_` method to use in [Charge a saved payment method](#charge-a-saved-payment-method). It stays `null` when nothing was saved. The `payment.succeeded` webhook carries the same field, and it's the path that survives a closed tab.
  </Step>
</Steps>

## Next steps

<CardGroup cols={2}>
  <Card title="Accept payments" href="/developer/guides/accept-payments">
    One-time and subscription checkouts to pair with your save flow.
  </Card>

  <Card title="Checkout element" href="/elements/latest/checkout/overview">
    Mount Whop checkout on your own site with Whop Elements.
  </Card>

  <Card title="Listen to webhooks" href="/developer/guides/webhooks">
    Track `setup_intent.succeeded`, `payment.succeeded`, and `payment.failed`.
  </Card>

  <Card title="Billing portal" href="/payments-and-billing/manage-billing/billing-portal">
    Let customers manage and remove their saved payment methods.
  </Card>
</CardGroup>
