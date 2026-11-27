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

# Track Visitors

> Sites on whop.site include the Whop pixel with automatic page view and checkout tracking. Add whop.track calls for leads, bookings, and custom events.

Whop injects the pixel into every HTML page your site serves, scoped to the app's business, and fires a page view. There is no snippet to paste and no account ID to configure.

| Tracked automatically | You add a `whop.track` call |
| - | - |
| Page views | Lead forms, call bookings, applications |
| Whop checkout views | Quizzes, VSL completions, anything on your own pages |
| Purchases, subscriptions, trials | Steps Whop's payment data can't see |

## Track your own events

`window.whop` exists before your app code runs, so call it directly:

```javascript theme={null}
whop.track("lead");
whop.track("schedule", { value: 50, currency: "USD" });
whop.track("watched_vsl");
```

Standard event names — `lead`, `schedule`, `submit_application`, `contact`, `complete_registration`, `view_content`, `add_to_cart`, `initiate_checkout` — each optionally take a `value` and a `currency`. Any other name works too, and shows up under that name in reporting.

Fire the event where the action completes: in the success handler if the page stays put, or on the confirmation page if it redirects.

> [!TIP]
>
> Pass an `event_id` on anything that could arrive twice — a retry, a refresh, or the same conversion mirrored from your server. Whop counts each name and ID pair once. Use an ID your system already has for that one action, never a constant like `"lead"`.

## Track products and carts

Tracking tools match a sale back to the product a visitor looked at. Tell the pixel which Whop product and variant each view, cart add and checkout start is for. Stores Whop AI builds already do this on their product and checkout pages. A site you build yourself or upload with the CLI adds three calls and four attributes.

Fire `view_content` when a product page shows a variant, again when the shopper picks another one, `add_to_cart` when they add it, and `initiate_checkout` when checkout opens:

```javascript theme={null}
whop.track("view_content", {
  variant_id: "plan_xxxxxxxxxxxxx",
  product_id: "prod_xxxxxxxxxxxxx",
  value: 49,
  currency: "USD",
});
```

* `variant_id` is the variant's ID, prefixed `plan_`, and `product_id` is its product's ID, prefixed `prod_`.
* `value` is the price the page shows, in the currency's major units (`49` for \$49.00), and `currency` is its ISO 4217 code. Leave both out when the page shows no price.
* A checkout for several variants sends `initiate_checkout` with its total, and leaves out `variant_id` and `product_id`.

Mark the element that shows the product with the same four values for the variant on screen, so a tracking script can read them at any moment:

```html theme={null}
<section
  data-whop-product-id="prod_xxxxxxxxxxxxx"
  data-whop-variant-id="plan_xxxxxxxxxxxxx"
  data-whop-price="49"
  data-whop-currency="USD"
>
```

Update the attributes whenever the shopper picks another variant or the shown price changes. Purchases need nothing from you: Whop records them from its own payments.

## Where the data lands

Open **Websites** in your [dashboard](https://whop.com/dashboard) for visitors, page views, and attributed revenue per domain, or the [pixel dashboard](https://whop.com/dashboard) for the full event breakdown. First events appear about a minute after the visit.

> [!NOTE]
>
> Installing the pixel snippet yourself is still supported — the platform one checks for an existing `window.whop` and no-ops, so nothing double-counts. Do that when your site needs a scope other than its own business.

For a site on your own domain rather than `whop.site`, install the snippet by hand: [Install the Whop pixel](https://docs.whop.com/developer/ads/pixel.md).

## Next steps

> [card] [Pixel reference](https://docs.whop.com/developer/ads/pixel.md)
>
> Every event field, customer matching, and server-side sends.

> [card] [Accept payments](https://docs.whop.com/developer/guides/accept-payments.md)
>
> Give the pixel conversions to attribute.


## Related topics

- [Website Hosting on Whop](https://docs.whop.com/developer/websites/hosting.md)
- [Build a Website with the CLI](https://docs.whop.com/developer/websites/quickstart.md)
- [Deploy a Blueprint Business](https://docs.whop.com/developer/websites/blueprints.md)
