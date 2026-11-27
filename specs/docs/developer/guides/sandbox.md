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

# Test in the Sandbox

> Point the Whop SDK and Elements at the sandbox, create test API keys and webhooks, simulate payments with test cards, and review sandbox limitations.

Whop provides a sandbox environment for testing your integration without affecting production data or making real payments.

## Sandbox URLs

| Environment | URL |
| - | - |
| Frontend | `https://sandbox.whop.com` |
| API | `https://sandbox-api.whop.com/api/v1` |

Create your sandbox account and API keys at [sandbox.whop.com](https://sandbox.whop.com).

## Software development kit configuration

To use the sandbox environment with the Whop SDK, pass the `Sandbox` environment. An environment names every host the SDK talks to, so every request moves to the sandbox at once.

```typescript TypeScript theme={null}
import { WhopClient, WhopEnvironment } from "@whop/sdk";

const client = new WhopClient({
	token: process.env["WHOP_API_KEY"],
	environment: WhopEnvironment.Sandbox,
});
```

```python Python theme={null}
import os
from whop_sdk import Whop, WhopEnvironment

client = Whop(
    token=os.environ.get("WHOP_API_KEY"),
    environment=WhopEnvironment.SANDBOX,
)
```

```ruby Ruby theme={null}
require "whop_sdk"

client = Whop_sdk::Client.new(
  token: ENV.fetch("WHOP_API_KEY"),
  environment: Whop_sdk::Environment::SANDBOX,
)
```

```rust Rust theme={null}
use whop_sdk::prelude::*;

let config = ClientConfig {
    token: Some(std::env::var("WHOP_API_KEY").unwrap()),
    base_url: "https://sandbox-api.whop.com/api/v1".to_string(),
    ..Default::default()
};
let client = Whop::new(config).expect("Failed to build client");
```

```go Go theme={null}
import (
    "os"

    whopsdk "github.com/whopio/whopsdk-go/v2"
    "github.com/whopio/whopsdk-go/v2/client"
    "github.com/whopio/whopsdk-go/v2/option"
)

client := client.NewWhop(
    option.WithToken(os.Getenv("WHOP_API_KEY")),
    option.WithEnvironment(whopsdk.Environments.Sandbox),
)
```

> [!NOTE]
>
> The `Sandbox` environment arrived in SDK 2.0. On 1.x, point the client at `https://sandbox-api.whop.com/api/v1` instead: `baseUrl` in TypeScript, `base_url` in Python and Ruby, `option.WithBaseURL` in Go. TypeScript, Ruby and Go keep that override in 2.0, but it sends every request to one host. Operations that must reach a different host are then refused, so use `environment`. The 2.0 Python client has no `base_url` parameter. The Rust SDK is still 1.x and keeps `base_url`.

## Elements in the sandbox

To point [Whop Elements](https://docs.whop.com/elements/latest/getting-started.md) at the sandbox, set `environment` when you create the SDK instance. This option is the only way to change where the elements send what a buyer types.

The Elements property for selecting a variant remains named `plan` for compatibility.

```tsx React theme={null}
import { WhopElements, Payments, PaymentElement, BrandingElement } from "@whop/elements-react";
import { loadWhop } from "@whop/elements";

function App() {
	return (
		<WhopElements elements={loadWhop()} environment="sandbox">
			<Payments accountId="biz_xxxxxxxxxxxxx" plan="plan_xxxxxxxxxxxxx">
				<PaymentElement />
				<BrandingElement />
			</Payments>
		</WhopElements>
	);
}
```

```html JavaScript theme={null}
<script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>
<script type="module">
  const whop = window.WhopElements({ environment: "sandbox" });
  const payments = whop.payments.create({
    accountId: "biz_xxxxxxxxxxxxx",
    plan: "plan_xxxxxxxxxxxxx",
  });
  payments.create("payment").mount("#payment");
  payments.create("branding").mount("#branding");
</script>
```

## Application programming interface keys and webhooks

API keys and webhooks work the same in sandbox as they do in production:

* Create API keys at [sandbox.whop.com/dashboard/developer](https://sandbox.whop.com/dashboard/developer)
* Configure webhooks to receive events from sandbox
* Use the same authentication headers (`Authorization: Bearer YOUR_API_KEY`)

## Test cards

Use the following test card numbers to simulate payments in sandbox:

| Card Number | Description |
| - | - |
| `4242 4242 4242 4242` | Successful payment |
| `4000 0000 0000 0002` | Declined payment |
| `4000 0000 0000 0341` | Card setup succeeds, but the processor declines later charges |
| `5385 3083 6013 5181` | Requires 3D Secure (enter `Checkout1!` on the 3DS screen) |

In production, a declined payment carries a code from the card issuer. [Payment decline codes explained](https://docs.whop.com/payments-and-billing/payment-issues/payment-decline-reasons.md) lists what each one means.

For all test cards:

* Use any future expiration date (e.g., 12/34)
* Use any 3-digit card verification code (e.g., 123)
* Use any billing address

Use `4000 0000 0000 0341` to test recovery flows where you save a card
successfully, but a future charge for that saved card fails.

## Payment methods in the sandbox

To test card payments, use the [test cards](#test-cards) above.

These methods also complete in the sandbox through each provider's own test flow: PayPal, Bancontact, iDEAL, Cash App Pay, Pix, and Boleto.

These methods aren't supported in the sandbox yet:

* Apple Pay and Google Pay
* Bank accounts (ACH)
* SEPA Direct Debit
* EU bank transfers and wire transfers
* Alipay, WeChat Pay, and MobilePay

## Known limitations

> [!WARNING]
>
> The following features aren't available in the sandbox:

* Payouts
* Crypto deposits and on-chain transfers
* Discord direct messages and role sync
* Up-to-date search results
* Some scheduled and background jobs that run in production, such as fraud reviews and digest emails


## Related topics

- [Auth & API Keys](https://docs.whop.com/developer/guides/auth-scoping.md)
- [Whop API Core Concepts](https://docs.whop.com/developer/concepts.md)
- [Embed a Checkout](https://docs.whop.com/developer/guides/embed-checkout.md)
