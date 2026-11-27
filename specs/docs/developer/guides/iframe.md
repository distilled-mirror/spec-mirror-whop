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

# Iframe SDK

> Use the @whop/iframe SDK to talk to Whop from your embedded app, such as opening a link or starting an in-app purchase.

Whop embeds apps into the site using iframes. This SDK provides a type-safe way for you to communicate with the Whop application through a request-and-response API that uses `window.postMessage`.

Since this package relies on `window.postMessage`, it only works in **Client Components** or client-side JavaScript.

### Relevant packages

* `@whop/iframe` - The main package for the iframe SDK.
* `@whop/react` - A React wrapper for Whop Apps including helpers for the iframe SDK.

***

## Setup

The main function exported from the `@whop/iframe` package is the `createSdk` function. When called, this function sets up a listener for messages from the main Whop site, using `window.on('message', ...)`. It's also exposed through the `WhopIframeSdkProvider` component from `@whop/react`.

### React

If you use React, add the `WhopIframeSdkProvider` component from `@whop/react` to provide the iframe SDK to all child components.

**1. Mount provider in root layout**

```javascript theme={null}
// app/layout.tsx
import { WhopIframeSdkProvider } from "@whop/react";

export default function RootLayout({
  children,
}: {
  children: React.ReactNode,
}) {
  return (
    <html lang="en">
      <body>
        <WhopIframeSdkProvider>{children}</WhopIframeSdkProvider>
      </body>
    </html>
  );
}
```

**2. Consume the iframe SDK in a component**

```javascript theme={null}
// components/example.tsx
import { useIframeSdk } from "@whop/react";

export const Example = () => {
  const iframeSdk = useIframeSdk();

  return (
    <button
      onClick={() => iframeSdk.openExternalUrl({ url: "https://example.com" })}
    >
      Open External URL
    </button>
  );
};
```

### Other frameworks

For other frameworks, you can use the `createSdk` function from `@whop/iframe` to create an instance of the iframe SDK.

**1. Create the iframe SDK instance**

```javascript theme={null}
// lib/iframe-sdk.ts
import { createSdk } from "@whop/iframe";

export const iframeSdk = createSdk({
  appId: process.env.NEXT_PUBLIC_WHOP_APP_ID,
});
```

**2. Use the iframe SDK instance**

```javascript theme={null}
// index.ts
import { iframeSdk } from "@/lib/iframe-sdk";

const navigationButtonElement = document.querySelector("button");

if (navigationButtonElement) {
  navigationButtonElement.addEventListener("click", () => {
    iframeSdk.openExternalUrl({ url: "https://example.com" });
  });
}
```

> [!TIP]
>
> The SDK and iframe are now set up.

## Opening external links

After you initialize the iframe SDK, you can use it to open external links. This closes your app and navigates to a new website.

```javascript React theme={null}
"use client";
import { useIframeSdk } from "@whop/react";

export default function Home() {
	const iframeSdk = useIframeSdk();

    function openLink() {
    	iframeSdk.openExternalUrl({ url: "https://google.com" });
    }

    return <button onClick={openLink}>Click me to open Google</button>;

}

```

```javascript Vanilla JS theme={null}
import { iframeSdk } from "@/lib/iframe-sdk";

const navigationButtonElement = document.querySelector("button");

if (navigationButtonElement) {
  navigationButtonElement.addEventListener("click", () => {
    iframeSdk.openExternalUrl({ url: "https://google.com" });
  });
}
```

### Opening user profiles

If you want to display a Whop user profile, you can use the `openExternalUrl` method and pass their profile page link which looks like `https://whop.com/@username`.

The Whop app will intercept this and display a modal containing their user profile instead of navigating away.

```javascript React theme={null}
"use client";
import { useIframeSdk } from "@whop/react";

export default function Home() {
	const iframeSdk = useIframeSdk();

    function openProfile() {
    	iframeSdk.openExternalUrl({ url: "https://whop.com/@j" });
    }

    return <button onClick={openProfile}>View Profile</button>;

}

```

```javascript Vanilla JS theme={null}
import { iframeSdk } from "@/lib/iframe-sdk";

const profileButtonElement = document.querySelector("button");

if (profileButtonElement) {
  profileButtonElement.addEventListener("click", () => {
    iframeSdk.openExternalUrl({ url: "https://whop.com/@j" });
  });
}
```

> [!NOTE]
>
> You can also use a user ID instead of username. The final link should look
> like this: `https://whop.com/@user_XXXXXXXX`

## In-app purchases

The iframe SDK provides a method to trigger in-app purchases directly within your app. This lets you accept payments for one-time purchases or subscriptions using Whop's checkout.

To process in-app purchases, you'll need to:

1. Create a checkout configuration on your server (with variant details in the compatibility `plan` field and your metadata)
2. Use the `inAppPurchase` method from the iframe SDK to open the payment modal
3. Handle the response and validate the payment via webhooks

```tsx React theme={null}
"use client";
import { useIframeSdk } from "@whop/react";
import { useState } from "react";

export default function PaymentButton() {
    const iframeSdk = useIframeSdk();
    const [paymentId, setPaymentId] = useState<string>();

    async function handlePurchase() {
        // 1. Create checkout configuration on server
        //    (in this example this is a nextjs server action, but it could also be a plain API route)
        const checkoutConfiguration = await createCheckoutConfiguration(/* can pass options here */);

        // 2. Open payment modal
        const res = await iframeSdk.inAppPurchase({
            planId: checkoutConfiguration.plan.id,
            id: checkoutConfiguration.id
        });

        if (res.status === "ok") {
            setPaymentId(res.data.receipt_id);
        } else {
            // handle errors
        }
    }

    return <button onClick={handlePurchase}>Purchase</button>;

}

```

```javascript Vanilla JS theme={null}
import { iframeSdk } from "@/lib/iframe-sdk";

const paymentButton = document.querySelector("button#payment-button");

paymentButton.addEventListener(
    "click",
    async function onPaymentButtonClick() {
        // 1. Create checkout configuration on server
        const checkoutConfiguration = await createCheckoutConfiguration(/* can pass options here */);

        // 2. Open payment modal
        const res = await iframeSdk.inAppPurchase({
            planId: checkoutConfiguration.plan.id,
            id: checkoutConfiguration.id
        });

        if (res.status === "ok") {
            const paymentId = res.data.receipt_id;
            // Send this id to your backend to validate, or rely on webhooks.
            // User has purchased the item, and it can be unlocked.
        } else {
            // handle errors
        }
    }
);
```

> [!NOTE]
>
> For a complete guide on setting up in-app purchases including creating
> checkout configurations, handling webhooks, and validating payments, see the
> [Accept payments documentation](https://docs.whop.com/developer/guides/accept-payments.md).


## Related topics

- [Frosted UI](https://docs.whop.com/developer/guides/frosted_ui.md)
- [Authenticate Users in Whop Apps](https://docs.whop.com/developer/guides/authentication.md)
- [Auth & API Keys](https://docs.whop.com/developer/guides/auth-scoping.md)
