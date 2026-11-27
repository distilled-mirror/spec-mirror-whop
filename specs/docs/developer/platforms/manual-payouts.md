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

# Manual Payouts to Subaccounts

> Send payouts to subaccounts from your platform balance: onboard them through KYC, add a payout method, get the default method, and create a payout.

Send payouts directly to your subaccounts from your platform balance. Subaccounts must complete identity verification and add a payout method before they can receive funds.

**1. Complete KYC verification**

Before a user can receive payouts, they must complete identity verification (KYC). Use the hosted account onboarding flow to guide users through this process:

```typescript TypeScript theme={null}
import { WhopClient } from "@whop/sdk";

const client = new WhopClient({
  token: "Account API Key",
});

const accountLink = await client.accountLinks.create({
  account_id: "biz_xxxxxxxxxxxxx",
  use_case: "account_onboarding",
  return_url: "https://yourapp.com/onboarding/complete",
  refresh_url: "https://yourapp.com/onboarding/refresh",
});

// Redirect the user to complete KYC
console.log(accountLink.url);
```

```python Python theme={null}
from whop_sdk import Whop

client = Whop(
    token="Account API Key",
)

account_link = client.account_links.create(
    account_id="biz_xxxxxxxxxxxxx",
    use_case="account_onboarding",
    return_url="https://yourapp.com/onboarding/complete",
    refresh_url="https://yourapp.com/onboarding/refresh",
)

# Redirect the user to complete KYC
print(account_link.url)
```

```rust Rust theme={null}
use whop_sdk::prelude::*;

let config = ClientConfig {
    token: Some("Account API Key".to_string()),
    ..Default::default()
};
let client = Whop::new(config).expect("Failed to build client");

let account_link = client
    .account_links
    .create(
        &CreateAccountLinksRequest {
            account_id: "biz_xxxxxxxxxxxxx".to_string(),
            use_case: AccountLinkUseCases::AccountOnboarding,
            return_url: "https://yourapp.com/onboarding/complete".to_string(),
            refresh_url: "https://yourapp.com/onboarding/refresh".to_string(),
        },
        None,
    )
    .await?;

// Redirect the user to complete KYC
println!("{}", account_link.url);
```

```go Go theme={null}
import (
    "context"
    "fmt"
    "log"

    whopsdk "github.com/whopio/whopsdk-go/v2"
    "github.com/whopio/whopsdk-go/v2/client"
    "github.com/whopio/whopsdk-go/v2/option"
)

client := client.NewWhop(option.WithToken("Account API Key"))

accountLink, err := client.AccountLinks.Create(context.TODO(), &whopsdk.CreateAccountLinksRequest{
    AccountID:  "biz_xxxxxxxxxxxxx",
    UseCase:    whopsdk.AccountLinkUseCasesAccountOnboarding,
    ReturnURL:  "https://yourapp.com/onboarding/complete",
    RefreshURL: "https://yourapp.com/onboarding/refresh",
})
if err != nil {
    log.Fatal(err)
}

// Redirect the user to complete KYC
fmt.Println(accountLink.URL)
```

Redirect the user to the `url` returned in the response. After completing verification, they will be redirected back to your `return_url`.

**2. Add a payout method**

Before creating a payout, the account needs a payout method. Mount the [Withdraw element](https://docs.whop.com/elements/latest/wallet/withdraw.md) with an access token for the subaccount. It lists the account's payout methods and collects a new one when none exists:

```tsx React theme={null}
"use client";

import { WhopElements, Wallet, WithdrawElement } from "@whop/elements-react";
import { loadWhop } from "@whop/elements";

export function AddPayoutMethod({
  accountId,
  accessToken,
}: {
  accountId: string;
  accessToken: string;
}) {
  return (
    <WhopElements elements={loadWhop()}>
      <Wallet accountId={accountId} accessToken={accessToken}>
        <WithdrawElement />
      </Wallet>
    </WhopElements>
  );
}
```

Mint the `accessToken` on your server for the subaccount, as shown in [Enable Subaccount Payouts](https://docs.whop.com/developer/platforms/render-payout-portal.md). You can also send the user to the [hosted payout portal](https://docs.whop.com/developer/platforms/render-payout-portal.md#hosted-payout-portal) instead. To accept a PayPal account only after its holder logs in to prove they own it, [connect a PayPal payout method](https://docs.whop.com/developer/guides/connect-paypal-payout-method.md).

**3. Get the default payout method**

List the subaccount's payout methods and find the one with `is_default: true`:

```typescript TypeScript theme={null}
import { WhopClient } from "@whop/sdk";

const client = new WhopClient({
  token: "Account API Key",
});

const payoutMethods = await client.payoutMethods.listPayoutMethod({
  account_id: "biz_xxxxxxxxxxxxx",
});

const defaultMethod = payoutMethods.data.find((method) => method.is_default);
console.log(defaultMethod?.id);
```

```python Python theme={null}
from whop_sdk import Whop

client = Whop(
    token="Account API Key",
)

payout_methods = client.payout_methods.list_payout_method(
    account_id="biz_xxxxxxxxxxxxx",
)

default_method = next((m for m in payout_methods.items if m.is_default), None)
print(default_method.id)
```

```rust Rust theme={null}
use whop_sdk::prelude::*;

let config = ClientConfig {
    token: Some("Account API Key".to_string()),
    ..Default::default()
};
let client = Whop::new(config).expect("Failed to build client");

let payout_methods = client
    .payout_methods
    .list_payout_method(
        &ListPayoutMethodQueryRequest {
            account_id: "biz_xxxxxxxxxxxxx".to_string(),
            ..Default::default()
        },
        None,
    )
    .await?;

let default_method = payout_methods.data.iter().find(|method| method.is_default);
println!("{:?}", default_method.map(|method| &method.id));
```

```go Go theme={null}
import (
    "context"
    "fmt"
    "log"

    whopsdk "github.com/whopio/whopsdk-go/v2"
    "github.com/whopio/whopsdk-go/v2/client"
    "github.com/whopio/whopsdk-go/v2/option"
)

client := client.NewWhop(option.WithToken("Account API Key"))

page, err := client.PayoutMethods.ListPayoutMethod(context.TODO(), &whopsdk.ListPayoutMethodRequest{
    AccountID: "biz_xxxxxxxxxxxxx",
})
if err != nil {
    log.Fatal(err)
}

var defaultMethod *whopsdk.PayoutMethodListItem
for _, method := range page.Results {
    if method.IsDefault {
        defaultMethod = method
        break
    }
}
if defaultMethod != nil {
    fmt.Println(defaultMethod.ID)
}
```

**4. Create a payout**

API version `2026-11-03` and later requires a quote. First call `POST /payouts/quotes` with the account, payout method, amount, currency, and speed you will submit:

```bash theme={null}
curl https://api.whop.com/api/v1/payouts/quotes \
  -H "Authorization: Bearer $WHOP_API_KEY" \
  -H "Api-Version-Date: 2026-11-03" \
  -H "Idempotency-Key: payout-quote-unique-key" \
  -H "Content-Type: application/json" \
  -d '{"account_id":"biz_xxxxxxxxxxxxx","payout_method_id":"potk_xxxxxxxxxxxxx","amount":100,"currency":"usd","speed":"standard"}'
```

Replace `QUOTE_TOKEN_FROM_RESPONSE` below with the returned `quote_token` and submit before `expires_at`. If a payout request times out, retry with the same payout `Idempotency-Key` and original token. Use a different key for the quote request.

```typescript TypeScript theme={null}
import { WhopClient } from "@whop/sdk";

const client = new WhopClient({
  token: "Account API Key",
});

const payout = await client.payouts.create({
  account_id: "biz_xxxxxxxxxxxxx",
  amount: 100.0,
  currency: "usd",
  payout_method_id: "potk_xxxxxxxxxxxxx",
  quote_token: "QUOTE_TOKEN_FROM_RESPONSE",
});

console.log(payout.id);
```

```python Python theme={null}
from whop_sdk import Whop

client = Whop(
    token="Account API Key",
)

payout = client.payouts.create(
    request={
        "account_id": "biz_xxxxxxxxxxxxx",
        "amount": 100.0,
        "currency": "usd",
        "payout_method_id": "potk_xxxxxxxxxxxxx",
        "quote_token": "QUOTE_TOKEN_FROM_RESPONSE",
    },
)

print(payout.id)
```

```rust Rust theme={null}
use whop_sdk::prelude::*;

let config = ClientConfig {
    token: Some("Account API Key".to_string()),
    ..Default::default()
};
let client = Whop::new(config).expect("Failed to build client");

let payout = client
    .payouts
    .create(
        &CreatePayoutsRequestBody::Value(json!({
            "account_id": "biz_xxxxxxxxxxxxx",
            "amount": 100.0,
            "currency": "usd",
            "payout_method_id": "potk_xxxxxxxxxxxxx",
            "quote_token": "QUOTE_TOKEN_FROM_RESPONSE",
        })),
        None,
    )
    .await?;

println!("{}", payout.id);
```

```go Go theme={null}
import (
    "context"
    "fmt"
    "log"

    whopsdk "github.com/whopio/whopsdk-go/v2"
    "github.com/whopio/whopsdk-go/v2/client"
    "github.com/whopio/whopsdk-go/v2/option"
)

client := client.NewWhop(option.WithToken("Account API Key"))

payout, err := client.Payouts.Create(context.TODO(), &whopsdk.CreatePayoutsRequestBody{
    Unknown: map[string]any{
        "account_id":       "biz_xxxxxxxxxxxxx",
        "amount":           100.0,
        "currency":         "usd",
        "payout_method_id": "potk_xxxxxxxxxxxxx",
        "quote_token": "QUOTE_TOKEN_FROM_RESPONSE",
    },
})
if err != nil {
    log.Fatal(err)
}

fmt.Println(payout.ID)
```

In this example:

* `account_id` is the subaccount to pay out
* `amount` is the payout amount (100.00 USD) - fees will be deducted from this amount
* `currency` is the ISO currency code
* `payout_method_id` is the ID of the payout method to use (from step 3)

The request will return an error if the amount exceeds the available balance.

### Cover payout fees for your subaccounts

By default, payout fees are deducted from the subaccount's balance. You can choose to cover these fees from your platform balance instead by setting `platform_covers_fees` to `true` on both the quote request and the payout request:

```typescript TypeScript theme={null}
const payout = await client.payouts.create({
  account_id: "biz_xxxxxxxxxxxxx",
  amount: 100.0,
  currency: "usd",
  payout_method_id: "potk_xxxxxxxxxxxxx",
  quote_token: "QUOTE_TOKEN_FROM_RESPONSE",
  platform_covers_fees: true,
});
```

```python Python theme={null}
payout = client.payouts.create(
    request={
        "account_id": "biz_xxxxxxxxxxxxx",
        "amount": 100.0,
        "currency": "usd",
        "payout_method_id": "potk_xxxxxxxxxxxxx",
        "quote_token": "QUOTE_TOKEN_FROM_RESPONSE",
        "platform_covers_fees": True,
    },
)
```

```rust Rust theme={null}
client
    .payouts
    .create(
        &CreatePayoutsRequestBody::Value(json!({
            "account_id": "biz_xxxxxxxxxxxxx",
            "amount": 100.0,
            "currency": "usd",
            "payout_method_id": "potk_xxxxxxxxxxxxx",
            "quote_token": "QUOTE_TOKEN_FROM_RESPONSE",
            "platform_covers_fees": true
        })),
        None,
    )
    .await?;
```

```go Go theme={null}
_, err := client.Payouts.Create(context.TODO(), &whopsdk.CreatePayoutsRequestBody{
    Unknown: map[string]any{
        "account_id":           "biz_xxxxxxxxxxxxx",
        "amount":               100.0,
        "currency":             "usd",
        "payout_method_id":     "potk_xxxxxxxxxxxxx",
        "quote_token": "QUOTE_TOKEN_FROM_RESPONSE",
        "platform_covers_fees": true,
    },
})
if err != nil {
    log.Fatal(err)
}
```

When `platform_covers_fees` is `true`, the subaccount receives the full payout amount and the fee is debited from your platform's balance instead. This requires your platform's fee coverage policy to cover the payout method, or an API key authorized to manage your subaccounts' fees.

## API reference

> [card] [Create Payout API](https://docs.whop.com/api-reference/payouts/create-payout.md)
>
> See the full API reference for creating payouts

## Related resources

> [card] [Pay subaccounts](https://docs.whop.com/developer/platforms/collect-payments-for-subaccounts.md)
>
> Transfer funds to your subaccounts

> [card] [Render payout portal](https://docs.whop.com/developer/platforms/render-payout-portal.md)
>
> Let your users withdraw their funds


## Related topics

- [Enable Subaccount Payouts](https://docs.whop.com/developer/platforms/render-payout-portal.md)
- [Example Integration](https://docs.whop.com/developer/guides/example-integration.md)
- [Platform Quickstart](https://docs.whop.com/developer/platforms/quickstart.md)
