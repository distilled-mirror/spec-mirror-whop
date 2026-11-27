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

# Enable Apple Pay and Google Pay

> Host the Apple Pay verification file and register your domain so Apple Pay and Google Pay work in Whop Elements on your site.

Apple Pay and Google Pay let customers pay from the wallet already on their device. When a [Whop Elements](https://docs.whop.com/elements/latest/getting-started.md) checkout or payment element renders on your own site, both wallets require Apple Pay to be ready on the page's domain. One domain with Apple Pay ready clears both wallets for every element on that page.

> [!NOTE]
>
> Domain verification is only required for pages you host. Whop-hosted checkout
> pages and whop.com are already approved.

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

> [tab] Next.js
>
> Place the file in your `public` folder:
>
> ```
> public/
> └── .well-known/
>     └── apple-developer-merchantid-domain-association
> ```

> [tab] Nginx
>
> Add a location block to serve the file:
>
> ```nginx theme={null}
> location /.well-known/apple-developer-merchantid-domain-association {
>     alias /path/to/apple-developer-merchantid-domain-association;
>     default_type application/octet-stream;
> }
> ```

> [tab] Vercel
>
> Create a `vercel.json` with a rewrite rule that proxies to the file hosted on whop.com:
>
> ```json theme={null}
> {
>   "rewrites": [
>     {
>       "source": "/.well-known/apple-developer-merchantid-domain-association",
>       "destination": "https://whop.com/.well-known/apple-platform-integrator/apple-developer-merchantid-domain-association"
>     }
>   ]
> }
> ```

> [tab] Cloudflare Pages
>
> Place the file in your output directory:
>
> ```
> dist/
> └── .well-known/
>     └── apple-developer-merchantid-domain-association
> ```

### Step 3: Verify file access

Test that you can access the file by visiting:

```
https://<your-domain>/.well-known/apple-developer-merchantid-domain-association
```

The file should download or display its contents without any errors.

### Step 4: Register your domain

Register the domain from the dashboard or through the API. Either way, Whop checks that the file is reachable before registering the domain with Apple.

> [tab] Dashboard
>
> Open **Websites** in your dashboard and select the domain. Select **Fix Apple Pay**, follow the steps, then select **Check now**.

> [tab] API
>
> Add the domain with Apple Pay through the [Domains API](https://docs.whop.com/api-reference/domains/domain). If your account already has the domain, [update it](https://docs.whop.com/api-reference/domains/update-domain.md) with `apple_pay: {}` instead. Your API key needs the `domain:manage` permission.
>
> ```typescript TypeScript theme={null}
> import { WhopClient } from "@whop/sdk";
>
> const client = new WhopClient({ token: process.env.WHOP_API_KEY });
>
> const domain = await client.domains.create({
>   domain: "shop.example.com",
>   apple_pay: {},
> });
>
> console.log(domain.apple_pay?.state); // "pending" while Whop registers the domain with Apple
> ```
>
> ```python Python theme={null}
> import os
> from whop_sdk import Whop
>
> client = Whop(token=os.environ["WHOP_API_KEY"])
>
> domain = client.domains.create(domain="shop.example.com", apple_pay={})
>
> print(domain.apple_pay.state)  # "pending" while Whop registers the domain with Apple
> ```
>
> Whop registers the domain with Apple in the background. [Retrieve the domain](https://docs.whop.com/api-reference/domains/retrieve-domain.md) until `apple_pay.state` is `ready`, or `action_required` when Apple couldn't fetch the file. Once you host the file, register an `action_required` domain again with [Check Domain](https://docs.whop.com/api-reference/domains/check-domain.md):
>
> ```typescript TypeScript theme={null}
> await client.domains.check({ id: domain.id });
> ```
>
> ```python Python theme={null}
> client.domains.check(id=domain.id)
> ```
>
> The Domains API replaces the deprecated Payment Method Domains API. The [migration guide](https://docs.whop.com/developer/api/migrate-from-previous-versions.md#payment-method-domains) maps each of its requests to the Domains API.

## Troubleshooting

> [accordion] Apple Pay or Google Pay button doesn't appear
>
> * Make sure your domain shows as verified in **Payment domains** or returns `apple_pay.state: "ready"` from the [Domains API](https://docs.whop.com/api-reference/domains/retrieve-domain.md).
> * The [ExpressCheckoutElement](https://docs.whop.com/elements/latest/checkout/expressCheckout.md) renders only the wallets the buyer's device can pay with, and renders nothing where no wallet is available. Test Apple Pay in Safari on a real Apple device, not in a simulator.
> * If using **Framer**, load the Elements script through Custom Code in your site headers, not through the Embed component. Framer's Embed component uses a `srcdoc` iframe, which Safari treats as an insecure context and blocks Apple Pay.

> [accordion] Self-hosted file returns 404
>
> * Verify the file is in the correct location:
>   `/.well-known/apple-developer-merchantid-domain-association`.
> * Check that your web server serves files without extensions.
> * Make sure your hosting configuration doesn't block the `.well-known` directory.

## Next steps

> [card] [Express Checkout](https://docs.whop.com/developer/guides/express-checkout.md)
>
> Add one-press Apple Pay and Google Pay buttons to your site

> [card] [Checkout Links](https://docs.whop.com/payments/create-checkout-link.md)
>
> Create shareable checkout links for your products


## Related topics

- [Add Express Checkout](https://docs.whop.com/developer/guides/express-checkout.md)
- [Open a Wallet Sheet from Your Own Button](https://docs.whop.com/developer/guides/payment-request.md)
- [Migrate from Previous Versions](https://docs.whop.com/developer/api/migrate-from-previous-versions.md)
