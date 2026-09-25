> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Enable Apple Pay and Google Pay

> Enable Apple Pay and Google Pay on your own domain by verifying it as a payment method domain

Apple Pay and Google Pay let customers pay from the wallet already on their device. When a [Whop Elements](/elements/latest/getting-started) checkout or payment element renders on your own site, both wallets require the page's domain to be a verified payment method domain. One verified domain clears both wallets for every element on that page.

<Note>
  Domain verification is only required for pages you host. Whop-hosted checkout
  pages and whop.com are already approved.
</Note>

## Prerequisites

Before setting up Apple Pay, check that you have:

* A domain where you're mounting Whop Elements
* The ability to host a file on that domain (to serve the Apple Pay verification file)

## Self-hosted verification

To verify your domain, serve the Apple Pay merchant ID domain association file at a specific path on your domain, then register the domain with Whop.

### Step 1: Download the verification file

Download the [Apple Pay verification file](https://whop.com/.well-known/apple-platform-integrator/apple-developer-merchantid-domain-association).

### Step 2: Host the file

Host this file at the following path on your domain:

```
https://<your-domain>/.well-known/apple-developer-merchantid-domain-association
```

The file must be:

* Served over HTTPS
* Accessible without authentication
* Served with the correct content (no modifications)

<Tabs>
  <Tab title="Next.js">
    Place the file in your `public` folder:

    ```
    public/
    └── .well-known/
        └── apple-developer-merchantid-domain-association
    ```
  </Tab>

  <Tab title="Nginx">
    Add a location block to serve the file:

    ```nginx theme={null}
    location /.well-known/apple-developer-merchantid-domain-association {
        alias /path/to/apple-developer-merchantid-domain-association;
        default_type application/octet-stream;
    }
    ```
  </Tab>

  <Tab title="Vercel">
    Create a `vercel.json` with a rewrite rule that proxies to the file hosted on whop.com:

    ```json theme={null}
    {
      "rewrites": [
        {
          "source": "/.well-known/apple-developer-merchantid-domain-association",
          "destination": "https://whop.com/.well-known/apple-platform-integrator/apple-developer-merchantid-domain-association"
        }
      ]
    }
    ```
  </Tab>

  <Tab title="Cloudflare Pages">
    Place the file in your output directory:

    ```
    dist/
    └── .well-known/
        └── apple-developer-merchantid-domain-association
    ```
  </Tab>
</Tabs>

### Step 3: Verify file access

Test that you can access the file by visiting:

```
https://<your-domain>/.well-known/apple-developer-merchantid-domain-association
```

The file should download or display its contents without any errors.

### Step 4: Register your domain

Register the domain from the dashboard or through the API. Either way, Whop checks that the file is reachable before registering the domain with Apple.

<Tabs>
  <Tab title="Dashboard">
    Navigate to your [checkout settings](https://whop.com/dashboard/settings/checkout/) and find the **Apple Pay and Google Pay for embedded checkout** section. Select **Configure** to open **Payment domains**.

    <Frame>
      <img src="https://mintcdn.com/whop/qJMsh85qcrvhDnDi/images/apple-pay/payment-domains-settings.png?fit=max&auto=format&n=qJMsh85qcrvhDnDi&q=85&s=2797c1311a2bbd28cdef5e86929eb25f" alt="Payment domains settings showing the Configure button" width="2868" height="1654" data-path="images/apple-pay/payment-domains-settings.png" />
    </Frame>

    Select **Add domain**, choose **Self-hosted verification**, and enter your domain.

    <Frame>
      <img src="https://mintcdn.com/whop/qJMsh85qcrvhDnDi/images/apple-pay/add-domain-self-hosted.png?fit=max&auto=format&n=qJMsh85qcrvhDnDi&q=85&s=cbb7f1aa2b9d605470356ee4d058f7cd" alt="Add domain dialog for self-hosted verification" width="2608" height="1246" data-path="images/apple-pay/add-domain-self-hosted.png" />
    </Frame>

    A domain listed as **Needs verification** is still waiting on the file. Fix the hosting, then select **Verify domain**.
  </Tab>

  <Tab title="API">
    Register the hostname with the [Payment Method Domains API](/api-reference/beta/payment-method-domains/payment-method-domain). Whop attempts verification inline and returns `verified` when Apple fetched the file, or `pending` when it couldn't.

    <CodeGroup>
      ```typescript TypeScript theme={null}
      import { WhopClient } from "@whop/sdk";

      const client = new WhopClient({ token: process.env.WHOP_API_KEY });

      const domain = await client.paymentMethodDomains.create({
        hostname: "shop.example.com",
      });

      console.log(domain.status); // "verified", or "pending" until the file is reachable
      ```

      ```python Python theme={null}
      import os
      from whop_sdk import Whop

      client = Whop(token=os.environ["WHOP_API_KEY"])

      domain = client.payment_method_domains.create(hostname="shop.example.com")

      print(domain.status)  # "verified", or "pending" until the file is reachable
      ```
    </CodeGroup>

    Once you host the file, re-run verification for a `pending` domain with [Verify Payment Method Domain](/api-reference/beta/payment-method-domains/verify-payment-method-domain):

    <CodeGroup>
      ```typescript TypeScript theme={null}
      await client.paymentMethodDomains.verify({ id: domain.id });
      ```

      ```python Python theme={null}
      client.payment_method_domains.verify(id=domain.id)
      ```
    </CodeGroup>
  </Tab>
</Tabs>

## Troubleshooting

<AccordionGroup>
  <Accordion title="Apple Pay or Google Pay button doesn't appear">
    * Make sure your domain shows as verified in **Payment domains** or returns `status: "verified"` from the API.
    * The [ExpressCheckoutElement](/elements/latest/checkout/expressCheckout) renders only the wallets the buyer's device can pay with, and renders nothing where no wallet is available. Test Apple Pay in Safari on a real Apple device, not in a simulator.
    * If using **Framer**, load the Elements script through Custom Code in your site headers, not through the Embed component. Framer's Embed component uses a `srcdoc` iframe, which Safari treats as an insecure context and blocks Apple Pay.
  </Accordion>

  <Accordion title="Self-hosted file returns 404">
    * Verify the file is in the correct location:
      `/.well-known/apple-developer-merchantid-domain-association`.
    * Check that your web server serves files without extensions.
    * Make sure your hosting configuration doesn't block the `.well-known` directory.
  </Accordion>
</AccordionGroup>

## Next steps

<CardGroup cols={2}>
  <Card title="Express Checkout" icon="apple" href="/developer/guides/express-checkout">
    Add one-press Apple Pay and Google Pay buttons to your site
  </Card>

  <Card title="Checkout Links" icon="link" href="/payments/create-checkout-link">
    Create shareable checkout links for your products
  </Card>
</CardGroup>
