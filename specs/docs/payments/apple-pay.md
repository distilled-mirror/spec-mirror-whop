> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Enable Apple Pay

> Enable Apple Pay and Google Pay for your embedded checkout by verifying your domain

Apple Pay lets customers pay using their Apple Wallet, providing a seamless checkout experience on Safari and iOS devices. To enable Apple Pay and Google Pay on your embedded checkout, you need to verify ownership of your domain. The same verified domain clears both wallets for a page.

<Note>
  Domain verification is only required for [embedded
  checkout](/payments/checkout-embed). Whop-hosted checkout pages already
  support Apple Pay without any additional setup.
</Note>

## Prerequisites

Before setting up Apple Pay, check that you have:

* A domain where you're hosting the embedded checkout
* The ability to host a file on that domain (to serve the Apple Pay verification file)
* `@whop/checkout@0.0.43` or later if using the `hideSubmitButton` option in React

## Self-hosted verification

To verify your domain, serve the Apple Pay merchant ID domain association file at a specific path on your domain, then register the domain in your checkout settings.

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

### Step 4: Open payment domains settings

Navigate to your [checkout settings](https://whop.com/dashboard/settings/checkout/) and find the **Apple Pay and Google Pay for embedded checkout** section. Select **Configure** to open the domain management panel.

<Frame>
  <img src="https://mintcdn.com/whop/qJMsh85qcrvhDnDi/images/apple-pay/payment-domains-settings.png?fit=max&auto=format&n=qJMsh85qcrvhDnDi&q=85&s=2797c1311a2bbd28cdef5e86929eb25f" alt="Payment domains settings showing the Configure button" width="2868" height="1654" data-path="images/apple-pay/payment-domains-settings.png" />
</Frame>

### Step 5: Register your domain

Select the **+** button (or **Add payment domain** if no domains exist yet). From the dropdown menu, select **Self-hosted verification**.

<Frame>
  <img src="https://mintcdn.com/whop/qJMsh85qcrvhDnDi/images/apple-pay/dropdown-self-hosted-verification.png?fit=max&auto=format&n=qJMsh85qcrvhDnDi&q=85&s=e9f392d465afa3a683828dc386f7a3bc" alt="Add domain dropdown menu with self-hosted option" width="1262" height="902" data-path="images/apple-pay/dropdown-self-hosted-verification.png" />
</Frame>

Enter your domain. Whop will verify access to the file before registering your domain with Apple.

<Frame>
  <img src="https://mintcdn.com/whop/qJMsh85qcrvhDnDi/images/apple-pay/add-domain-self-hosted.png?fit=max&auto=format&n=qJMsh85qcrvhDnDi&q=85&s=cbb7f1aa2b9d605470356ee4d058f7cd" alt="Add domain dialog for self-hosted verification" width="2608" height="1246" data-path="images/apple-pay/add-domain-self-hosted.png" />
</Frame>

## Troubleshooting

<AccordionGroup>
  <Accordion title="Apple Pay button doesn't appear">
    * Make sure your domain is fully verified in the payment domains settings.
    * Check that you're using `@whop/checkout@0.0.43` or later.
    * Use a supported browser (Safari) and device (iOS or macOS).
    * Test on an actual Apple device, not in a simulator.
    * If using **Framer**, make sure you are loading the checkout SDK via Custom Code in your site headers, not via the Embed component. Framer's Embed component uses a `srcdoc` iframe which Safari treats as an insecure context, blocking Apple Pay. See the [Framer checkout guide](/payments/checkout-embed) for the correct setup.
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
  <Card title="Embedded Checkout" icon="code" href="/payments/checkout-embed">
    Learn how to embed Whop checkout on your website
  </Card>

  <Card title="Checkout Links" icon="link" href="/payments/create-checkout-link">
    Create shareable checkout links for your products
  </Card>
</CardGroup>
