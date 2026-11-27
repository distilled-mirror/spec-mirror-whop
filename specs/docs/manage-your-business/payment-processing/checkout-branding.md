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

# Checkout Branding

> Customize the look and feel of your checkout pages with button colors, fonts, and border styles

Checkout branding lets you match your checkout pages to your brand. Choose a background color, button color, font, and border style at the account level, and optionally override them per checkout link or per checkout session via the API.

> [card] [Try the checkout playground](https://whop.com/checkout-playground)
>
> Preview background color, button color, font, and border style on a live checkout before you apply them.

## What you can customize

| Setting | Description |
| - | - |
| **Background color** | A hex color code applied to the order summary panel on the checkout page (e.g. `#F4F4F5`) |
| **Button color** | A hex color code for the button color (e.g. `#FF5733`) |
| **Font family** | `system` (default), `roboto`, or `open_sans` |
| **Border style** | `rounded` (default), `pill`, or `rectangular` |

## Global branding

Global branding applies to every checkout link for your business. Individual links inherit these settings unless they have their own overrides.

1. Go to **Dashboard** > **Settings** > **Checkout Branding**
2. Choose your **background color**, **button color**, **font**, and **border style**
3. Use the live preview to see how your checkout will look
4. Select **Save**

## Per-link overrides

Override the global branding on a specific checkout link. Useful when a particular product or campaign needs a different look.

1. Go to **Dashboard** > **Checkout Links**
2. Edit the checkout link you want to customize
3. In the **Checkout branding** section, adjust any setting
4. Save your changes

Any setting left on **Global default** inherits from account branding. Only the settings you explicitly change are overridden for that link.

> [!NOTE]
>
> Per-link branding takes precedence over global branding. If you update your
> global settings later, any link with its own overrides keeps its custom values
> for the overridden settings.

## Customize individual checkout sessions

Set branding on individual checkout sessions using the `checkoutStyling` parameter when creating a checkout session. Session-level branding takes the highest precedence.

The resolution order is: **checkout session** > **checkout link (plan)** > **account defaults**.

```graphql theme={null}
mutation {
	createCheckoutSession(
		input: {
			planId: "plan_XXXXXXXXX"
			checkoutStyling: {
				backgroundColor: "#F4F4F5"
				buttonColor: "#FF5733"
				fontFamily: roboto
				borderStyle: pill
			}
		}
	) {
		id
		purchaseUrl
		checkoutStyling {
			backgroundColor
			buttonColor
			fontFamily
			borderStyle
		}
	}
}
```

### Parameters

| Field | Type | Description |
| - | - | - |
| `backgroundColor` | String | A hex color code (e.g. `#F4F4F5`). `null` to inherit. |
| `buttonColor` | String | A hex color code (e.g. `#FF5733`). `null` to inherit. |
| `fontFamily` | enum | `system`, `roboto`, or `open_sans`. `null` to inherit. |
| `borderStyle` | enum | `rounded`, `pill`, or `rectangular`. `null` to inherit. |

Pass `null` for any field to inherit from the next level in the resolution chain.

## Add a known email address

Control the email field on checkout pages using URL parameters. This is useful when you already know a customer's email from a landing page or customer relationship management system. You can skip collecting the email address again.

| Parameter | Effect |
| - | - |
| `email` | Fills in the email field |
| `email.disabled=1` | Locks the email field so the customer can't edit it |
| `email.hidden=1` | Hides the email field entirely |

### Examples

Prefill and lock:

```
https://whop.com/checkout/plan_XXXXXXXXX?email=customer@example.com&email.disabled=1
```

Prefill and hide:

```
https://whop.com/checkout/plan_XXXXXXXXX?email=customer@example.com&email.hidden=1
```

> [!NOTE]
>
> Provide the `email` parameter to use `email.hidden` or `email.disabled`. If you enable
> both options, checkout hides the field.

When you mount the [Checkout element](https://docs.whop.com/elements/latest/checkout/checkout.md) on your own site, pass `buyerEmail` to prefill the address and `lockBuyerEmail` to keep the buyer from editing it.

## Add a known phone number and address

Fill in the customer's phone number, billing details, and shipping address with URL parameters. The customer can still edit every prefilled field.

| Parameter | Effect |
| - | - |
| `phone` | Fills in the phone number, including the billing phone for payment methods that need one |
| `name` | Fills in the billing name |
| `address.line1`, `address.line2`, `address.city` | Fill in the billing address |
| `address.state`, `address.postal_code`, `address.country` | Fill in the billing address |
| `shipping.name` | Fills in the shipping recipient's name |
| `shipping.address.line1`, `shipping.address.city`, … | Fill in the shipping address, with the same fields as the billing address |

Include the country code in `phone` and encode the plus sign as `%2B`, for example `phone=%2B14155552671`. Use a two-letter country code for `country`, such as `US`.

### Example

```
https://whop.com/checkout/plan_XXXXXXXXX?email=customer@example.com&phone=%2B14155552671&name=Ada+Lovelace&address.country=US&address.postal_code=94107
```

## Customer emails

Whop emails your customers about their purchases and subscriptions. The sender name is your business name followed by **via Whop**, for example **Acme via Whop**. The sender address is `no-reply@businesses.whop.com`. If you've added a business logo, it appears at the top of each email.

Subaccounts that a platform creates through the API start with customer emails turned off. The platform can turn them on with the `send_customer_emails` field.

| Email | When Whop sends it |
| - | - |
| **Order confirmation** | After a one-time purchase or the first payment on a membership. Free purchases don't get one. |
| **Renewal receipt** | After every renewal charge, including the first charge after a free trial, and after a charge for a plan change. It shows the amount, the card statement descriptor when Whop has it, and a link to update the card or cancel. |
| **Upcoming renewal** | Once per billing period, before the subscription renews. Free trials longer than 7 days get it 7 days before the first charge. Plans billed every 6 months or less often get it 30 days before renewal. Everything else gets it about 24 hours before renewal. Whop skips it if the member has canceled or you paused their payments. |
| **Payment failed** | When a subscription payment fails and [Whop will retry it](https://docs.whop.com/payments/payment-declines.md). Whop doesn't send it for the final failed attempt that cancels the membership. Some payment methods, such as PayPal, don't get this email. |
| **Membership canceled** | When you or the member cancels a membership, even if access continues until the end of the billing period. |
| **Refund** | When you refund a payment. |

For a free trial, the upcoming renewal email is the trial-ending reminder. Whop doesn't send a separate welcome email. To welcome new members by email, set up a [**User joins** message](https://docs.whop.com/manage-your-business/growth-marketing/automated-messaging.md) and check **Send email to user when triggered**.

### Turn off customer emails

1. Go to **Dashboard** > **Settings** > **Checkout**
2. Turn off **Send transactional emails to your members**

> [!WARNING]
>
> When customer emails are off, your customers don't get order confirmations, renewal receipts, renewal or trial reminders, or payment failure notices from Whop. These emails help customers recognize charges, which can prevent refund requests and disputes.

This stops the emails in this table. Invoice emails and notices about billing schedule changes still go out, and so do your automated messages.

You can't edit the wording of Whop's emails. To send your own emails, trigger them from [webhooks](https://docs.whop.com/developer/guides/webhooks.md).


## Related topics

- [Access Higher Checkout Links](https://docs.whop.com/manage-your-business/payment-processing/access-higher-checkout-links.md)
- [Create Checkout Links for Your Products](https://docs.whop.com/payments/create-checkout-link.md)
- [Create a Checkout Link to Accept Payments](https://docs.whop.com/manage-your-business/payment-processing/create-checkout-link.md)
