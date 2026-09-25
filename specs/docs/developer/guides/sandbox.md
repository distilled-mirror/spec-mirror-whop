> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Test in the Sandbox

> Test your integration in a safe environment before going live

Whop provides a sandbox environment for testing your integration without affecting production data or making real payments.

## Sandbox URLs

| Environment | URL                                   |
| ----------- | ------------------------------------- |
| Frontend    | `https://sandbox.whop.com`            |
| API         | `https://sandbox-api.whop.com/api/v1` |

Create your sandbox account and API keys at [sandbox.whop.com](https://sandbox.whop.com).

## Software development kit configuration

To use the sandbox environment with the Whop SDK, pass the `Sandbox` environment. An environment names every host the SDK talks to, so every request moves to the sandbox at once.

<CodeGroup>
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
</CodeGroup>

<Note>
  The `Sandbox` environment arrived in SDK 2.0. On 1.x, point the client at `https://sandbox-api.whop.com/api/v1` instead: `baseUrl` in TypeScript, `base_url` in Python and Ruby, `option.WithBaseURL` in Go. TypeScript, Ruby and Go keep that override in 2.0, but it sends every request to one host. Operations that must reach a different host are then refused, so use `environment`. The 2.0 Python client has no `base_url` parameter. The Rust SDK is still 1.x and keeps `base_url`.
</Note>

## Elements in the sandbox

To point [Whop Elements](/elements/latest/getting-started) at the sandbox, set `environment` when you create the SDK instance. This option is the only way to change where the elements send what a buyer types.

<Note>
  The sandbox environment isn't available for Whop Elements yet. The samples below run as written once it is.
</Note>

<CodeGroup>
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
</CodeGroup>

## Application programming interface keys and webhooks

API keys and webhooks work the same in sandbox as they do in production:

* Create API keys at [sandbox.whop.com/dashboard/developer](https://sandbox.whop.com/dashboard/developer)
* Configure webhooks to receive events from sandbox
* Use the same authentication headers (`Authorization: Bearer YOUR_API_KEY`)

## Test cards

Use the following test card numbers to simulate payments in sandbox:

| Card Number           | Description                                                   |
| --------------------- | ------------------------------------------------------------- |
| `4242 4242 4242 4242` | Successful payment                                            |
| `4000 0000 0000 0002` | Declined payment                                              |
| `4000 0000 0000 0341` | Card setup succeeds, but the processor declines later charges |
| `5385 3083 6013 5181` | Requires 3D Secure (enter `Checkout1!` on the 3DS screen)     |

For all test cards:

* Use any future expiration date (e.g., 12/34)
* Use any 3-digit card verification code (e.g., 123)
* Use any billing address

Use `4000 0000 0000 0341` to test recovery flows where you save a card
successfully, but a future charge for that saved card fails.

## Known limitations

The sandbox environment has some limitations compared to production:

<Warning>
  The following features aren't available or may not work as expected in
  sandbox:
</Warning>

* **Payouts** - Payout functionality isn't available yet
* **Apps & Messaging** - Don't use apps or messaging features in sandbox
* **Alternative payment methods** - The sandbox supports only card payments (no Apple Pay, Google Pay, etc.)
