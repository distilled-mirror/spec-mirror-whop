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

# SaaS

> Sell your software on Whop. Gate a web app by checking each user's access, or gate downloadable software with license keys.

Whop runs checkout, billing, and subscription management for your software. Before your app lets someone in, it checks with Whop that the customer has access right now. This page shows the two ways to run that check.

## Choose your integration

| | Access check | License keys |
| - | - | - |
| **Best for** | A web app where users sign in to your site | Desktop apps, scripts, and tools users download |
| **What identifies the customer** | Their Whop user ID, stored on your user record | A license key Whop issues with each purchase |
| **What your server calls** | `users.checkAccess` | `memberships.retrieve` and `memberships.update` |
| **Limit to one machine** | No | Yes, with a hardware ID in membership metadata |

Both paths share the same setup.

## Set up

### 1. Create your whop and a product

Start at [whop.com/sell](https://whop.com/dashboard/start/) to name your whop and pick its URL. Then go to **Products** in your dashboard and add a product with the price your customers pay. Copy the product ID (`prod_`) and its plan ID (`plan_`). You need both below.

### 2. Create an API key

In your dashboard, go to **Developer → API keys** and create an account API key. It acts on behalf of your business, so keep it on your server and never ship it in client code or a downloadable app. Store it as `WHOP_API_KEY`.

### 3. Install the SDK

```bash npm theme={null}
npm install @whop/sdk
```

```bash pip theme={null}
pip install whop-sdk
```

Create one client and reuse it. The examples on this page call it `client`.

```typescript TypeScript theme={null}
import { WhopClient } from "@whop/sdk";

export const client = new WhopClient({ token: process.env.WHOP_API_KEY });
```

```python Python theme={null}
import os

from whop_sdk import Whop

client = Whop(token=os.environ["WHOP_API_KEY"])
```

## Email login integration

Use this path when customers sign in to your own web app. Store each customer's Whop user ID on your user record, then check access with it before you serve paid features.

### 1. Add Whop columns to your user table

| Column | Type | Description |
| - | - | - |
| `whop_user_id` | string | The customer's Whop user ID, prefixed `user_`. |
| `whop_username` | string | Optional. The customer's Whop username. Show it in your UI and logs. |

### 2. Send signed-in users to checkout

Create a checkout configuration from your server and put your own user ID in `metadata`. Whop copies that metadata onto the membership, which is how the next step links the purchase back to your user. Then redirect the user to `purchase_url`.

```typescript TypeScript theme={null}
export async function startCheckout(userId: string) {
  const checkout = await client.checkoutConfigurations.create({
    variant_id: process.env.WHOP_VARIANT_ID!,
    metadata: { customer_id: userId },
    redirect_url: "https://yourapp.com/billing/return",
  });

  return checkout.purchase_url;
}
```

```python Python theme={null}
def start_checkout(user_id: str) -> str:
    checkout = client.checkout_configurations.create(
        variant_id=os.environ["WHOP_VARIANT_ID"],
        metadata={"customer_id": user_id},
        redirect_url="https://yourapp.com/billing/return",
    )

    return checkout.purchase_url
```

To keep buyers on your own page instead, [embed the Checkout element](https://docs.whop.com/developer/guides/embed-checkout.md) and pass the same `metadata` on its handle. [Accept payments](https://docs.whop.com/developer/guides/accept-payments.md) compares every checkout option.

### 3. Save the Whop user ID from the webhook

Open the [Developer tab of your dashboard](https://whop.com/dashboard/developer), select **Create webhook**, subscribe to `membership.activated`, and store the signing secret as `WHOP_WEBHOOK_SECRET`. The event's `data` is the membership, with the buyer's `user_id` and the `metadata` you set at checkout.

```typescript TypeScript theme={null}
// app/api/webhooks/whop/route.ts
import { unwrapWebhook } from "@whop/sdk/helpers";
import type { NextRequest } from "next/server";

export async function POST(request: NextRequest) {
  // Verify the raw body. Parsing it first changes the bytes and the signature check fails.
  const event = unwrapWebhook(await request.text(), {
    headers: Object.fromEntries(request.headers),
    key: process.env.WHOP_WEBHOOK_SECRET!,
  });

  if (event.type === "membership.activated") {
    const customerId = event.data.metadata?.customer_id;
    if (customerId && event.data.user_id) {
      await db.users.update(customerId, { whop_user_id: event.data.user_id });
    }
  }

  return new Response("OK", { status: 200 });
}
```

```python Python theme={null}
import os

from fastapi import FastAPI, Request, Response
from whop_sdk.lib.verify_webhook import unwrap

app = FastAPI()


@app.post("/api/webhooks/whop")
async def whop_webhook(request: Request):
    # Verify the raw body. Parsing it first changes the bytes and the signature check fails.
    event = unwrap(await request.body(), dict(request.headers), os.environ["WHOP_WEBHOOK_SECRET"])

    if event["type"] == "membership.activated":
        membership = event["data"]
        customer_id = (membership.get("metadata") or {}).get("customer_id")
        if customer_id and membership.get("user_id"):
            db.users.update(customer_id, whop_user_id=membership["user_id"])

    return Response(status_code=200)
```

`membership.activated` also fires on renewals, so make the write safe to repeat. [Webhooks](https://docs.whop.com/developer/guides/webhooks.md) covers retries and testing.

### 4. Check access before serving paid features

Ask Whop whether the user has access to your product. The answer follows renewals, cancellations, and refunds, so you don't need to track billing state yourself.

```typescript TypeScript theme={null}
export async function hasAccess(whopUserId: string) {
  const access = await client.users.checkAccess({
    id: whopUserId,
    resource_id: process.env.WHOP_PRODUCT_ID!,
  });

  return access.has_access;
}
```

```python Python theme={null}
def has_access(whop_user_id: str) -> bool:
    access = client.users.check_access(
        resource_id=os.environ["WHOP_PRODUCT_ID"],
        id=whop_user_id,
    )

    return access.has_access
```

`access_level` on the same response is `customer`, `admin`, or `no_access`.

### 5. Let customers manage billing

Link customers to their [Whop orders](https://whop.com/@me/settings/orders) to update their payment method or cancel. They sign in with the email they bought with.

## License key integration

Use this path for software that runs on the customer's machine. Each purchase comes with a unique license key. Your software asks for the key, and your server checks it with Whop.

### 1. Add the Software app to your product

[Install the Software app](https://whop.com/apps/app_jHH5YT7jHYQANi/install/) on your whop, open it, and add your software's name and download link. Buyers then see their license key and the download in their orders. The API returns a membership's `license_key` only when its product includes this app.

### 2. Validate keys from your server

Check keys from your own server, never from the software you ship. Your API key can read and change every membership in your business, so never embed it in a desktop app or script that users download. Your software sends the license key and a machine identifier to your server, and your server checks it with Whop.

To limit each key to one machine, store a hardware ID in the membership's metadata the first time someone uses the key, then compare it on every later check:

* **The key isn't bound yet** (no `hwid` in its metadata): save the machine's `hwid` and grant access.
* **The stored `hwid` matches:** grant access.
* **The stored `hwid` is different:** deny access. The key is in use on another machine until the customer resets it.

```typescript TypeScript theme={null}
// Add "past_due" if you let customers keep access while Whop retries a failed renewal.
const VALID_STATUSES = ["active", "trialing", "completed"];

export async function validateLicense(licenseKey: string, hwid: string) {
  // Retrieve accepts a license key in place of a membership ID. An unknown key throws a 404 error.
  const membership = await client.memberships.retrieve({ id: licenseKey });

  if (!VALID_STATUSES.includes(membership.status)) {
    return { valid: false, reason: "membership_inactive" };
  }

  const boundHwid = membership.metadata?.hwid;

  if (!boundHwid) {
    // Update merges these keys into the existing metadata.
    await client.memberships.update({ id: membership.id, metadata: { hwid } });
    return { valid: true };
  }

  if (boundHwid !== hwid) {
    return { valid: false, reason: "bound_to_another_machine" };
  }

  return { valid: true };
}
```

```python Python theme={null}
# Add "past_due" if you let customers keep access while Whop retries a failed renewal.
VALID_STATUSES = {"active", "trialing", "completed"}


def validate_license(license_key: str, hwid: str) -> dict:
    # Retrieve accepts a license key in place of a membership ID. An unknown key raises a 404 error.
    membership = client.memberships.retrieve(license_key)

    if membership.status not in VALID_STATUSES:
        return {"valid": False, "reason": "membership_inactive"}

    bound_hwid = (membership.metadata or {}).get("hwid")

    if not bound_hwid:
        # Update merges these keys into the existing metadata.
        client.memberships.update(membership.id, metadata={"hwid": hwid})
        return {"valid": True}

    if bound_hwid != hwid:
        return {"valid": False, "reason": "bound_to_another_machine"}

    return {"valid": True}
```

### 3. Reset a license key

Customers can reset their key from their orders. To reset it from your server, [update the membership](https://docs.whop.com/api-reference/memberships/update-membership.md) with an empty metadata object (`metadata: {}`). The next check from any machine binds the key again.

## Design your store page

Your store page is where people learn about your software before they buy. Select your whop name in the top left, then select **Design store page** to open the editor. Select **Edit details** to customize:

* **Name and headline:** say what the software does and who it's for.
* **Description:** focus on the problem it solves and what buyers get, such as updates, support, or a community.
* **Logo:** add a clean, simple logo.
* **Gallery images or video:** show the software in use. A short demo video helps buyers decide.
* **Category:** pick the category that helps people find your software when they browse Whop.

## Next steps

> [card] [Accept payments](https://docs.whop.com/developer/guides/accept-payments.md)
>
> Compare checkout links, the Checkout element, and payment elements.

> [card] [Webhooks](https://docs.whop.com/developer/guides/webhooks.md)
>
> Verify signatures, handle retries, and test events.

> [card] [Manage memberships](https://docs.whop.com/developer/guides/memberships.md)
>
> Cancel, pause, extend, and update memberships from your server.

> [card] [Sign in with Whop](https://docs.whop.com/developer/guides/oauth.md)
>
> Let customers sign in with their Whop account instead of a password.


## Related topics

- [Get Started with Whop](https://docs.whop.com/get-started.md)
- [What Are Whop Apps](https://docs.whop.com/whop-apps/what-are-whop-apps.md)
