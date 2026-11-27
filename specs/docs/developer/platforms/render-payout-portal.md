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

# Enable Subaccount Payouts

> Let your subaccounts manage their own payouts with Whop Elements or a hosted portal

Let your subaccounts complete Know Your Customer (KYC) verification, add payout methods, and withdraw their funds on their own. You can mount the [Whop Elements](https://docs.whop.com/elements/latest/getting-started.md) wallet surfaces directly in your app or redirect users to a Whop-hosted portal.

## Embedded payout portal

### Server side implementation

To mount the wallet elements, generate an access token for the subaccount on your server. The elements read that account's balance and payouts with it. See [Auth & API keys](https://docs.whop.com/developer/guides/auth-scoping.md) for how to scope that token and how it compares to Whop's other credential types.

```tsx Next.JS theme={null}
// app/api/token/route.ts
import { WhopClient } from "@whop/sdk";
import type { NextRequest } from "next/server";

const whop = new WhopClient({
	token: process.env.WHOP_API_KEY,
});

export async function GET(request: NextRequest) {
	// Authenticate your user here
	const accountId = request.nextUrl.searchParams.get("accountId");

	if (!accountId) {
		return new Response(null, { status: 400 });
	}
	const tokenResponse = await whop.accessTokens
		.create({
			account_id: accountId,
		})
		.catch(() => {
			return null;
		});
	if (!tokenResponse) {
		return new Response(null, { status: 500 });
	}
	const token = tokenResponse.token;
	return Response.json({
		token,
	});
}
```

```typescript Express.js theme={null}
import express from "express";
import { WhopClient } from "@whop/sdk";

const app = express();
const client = new WhopClient({ token: "Account API Key" });

app.use(express.json());

app.post("/api/access-token", async (req, res) => {
	// Authenticate your user here
	const { accountId } = req.body;

	const { token } = await client.accessTokens.create({
		account_id: accountId,
	});

	res.json({ token });
});

app.listen(3000, () => {
	console.log("Server running on port 3000");
});
```

```python Python theme={null}
from whop_sdk import Whop
from flask import Flask, request, jsonify

app = Flask(__name__)
client = Whop(token="Account API Key")

@app.route('/api/access-token', methods=['POST'])
def create_access_token():
    data = request.get_json()
    # Authenticate your user here
    account_id = data.get('accountId')

    access_token = client.access_tokens.create(
        account_id=account_id
    )

    return jsonify({'token': access_token.token})
```

> [card] [Create Access Token API](https://docs.whop.com/api-reference/legacy/access-tokens/create-access-token.md)
>
> See the full API reference for generating access tokens and all available
> parameters

## Client side setup

```bash npm theme={null}
npm install @whop/elements-react @whop/elements
```

```bash pnpm theme={null}
pnpm add @whop/elements-react @whop/elements
```

```html HTML theme={null}
<script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>
```

## Client side implementation

Create one `Wallet` handle for the subaccount and mount the surfaces you need under it. The handle sends its access token to every element beneath it, so mint one token that carries every scope those elements read.

```tsx React theme={null}
"use client";

import { useEffect, useState } from "react";
import {
	ActivityElement,
	BalanceElement,
	Balances,
	Wallet,
	WhopElements,
	WithdrawElement,
} from "@whop/elements-react";
import { loadWhop } from "@whop/elements";

export function PayoutPortal({ accountId }: { accountId: string }) {
	const [accessToken, setAccessToken] = useState<string | null>(null);

	useEffect(() => {
		fetch(`/api/token?accountId=${accountId}`)
			.then((res) => res.json())
			.then((data) => setAccessToken(data.token));
	}, [accountId]);

	if (!accessToken) return null;

	return (
		<WhopElements elements={loadWhop()}>
			<Wallet accountId={accountId} accessToken={accessToken}>
				<Balances>
					<BalanceElement />
				</Balances>
				<WithdrawElement />
				<ActivityElement />
			</Wallet>
		</WhopElements>
	);
}
```

```html JavaScript theme={null}
<div id="balance"></div>
<div id="withdraw"></div>
<div id="activity"></div>

<script type="module">
  const accountId = "biz_xxxxxxxxxxxxx";
  const { token } = await (await fetch(`/api/token?accountId=${accountId}`)).json();

  const wallet = window.WhopElements().wallet.create({ accountId, accessToken: token });
  wallet.create("balances").create("balance").mount("#balance");
  wallet.create("withdraw").mount("#withdraw");
  wallet.create("activity").mount("#activity");
</script>
```

> [!NOTE]
>
> On iOS, the wallet SDK ships `BalanceElement`, `ListElement`, and `ActivityElement`. See [Getting Started with Elements](https://docs.whop.com/elements/latest/getting-started.md) and pick the Swift tab.

### Identity verification

A subaccount must verify its identity before it can withdraw. Mount the [KYC element](https://docs.whop.com/elements/latest/verifications/kyc.md) for the full in-page flow, which collects the details, runs the hosted provider session, and reports the result. Or mount the [RequiredActions element](https://docs.whop.com/elements/latest/dashboard/required-actions.md) to show every outstanding action, verification included, and let the user start each one.

```tsx React theme={null}
import { WhopElements, Verifications, KycElement } from "@whop/elements-react";
import { loadWhop } from "@whop/elements";

export function VerifyIdentity({ accountId }: { accountId: string }) {
	return (
		<WhopElements elements={loadWhop()}>
			<Verifications
				accountId={accountId}
				getToken={() =>
					fetch(`/api/token?accountId=${accountId}`)
						.then((res) => res.json())
						.then((data) => data.token)
				}
			>
				<KycElement onCompleted={() => console.log("Verified")} />
			</Verifications>
		</WhopElements>
	);
}
```

## `Wallet` options

| Option | Type | Required | Description |
| - | - | - | - |
| `accountId` | `string` | Yes | The subaccount's ID, prefixed `biz_`. |
| `accessToken` | `string` | Yes, on your own domain | The token from your server. Without it, reads use the viewer's whop.com session, which only works on whop.com. |
| `currency` | `string` | No | ISO 4217 code for amount fields. Defaults to `usd`. |
| `appearance` | `Appearance` | No | Theme and per-part styling for every element under the handle. See [Appearance](https://docs.whop.com/elements/latest/appearance.md). |

Every option, event, and method is listed on the [Wallet reference](https://docs.whop.com/elements/latest/wallet/overview).

### Scopes

The elements read with the token you mint, so grant every scope they need on that one token:

* [Balances](https://docs.whop.com/elements/latest/wallet/balances) and [Activity](https://docs.whop.com/elements/latest/wallet/activity.md) read `company:balance:read`.
* [Withdraw](https://docs.whop.com/elements/latest/wallet/withdraw.md) needs `payout:withdraw_funds`, `payout:destination:read`, and `payout:create_destination` to add a bank.
* [KYC](https://docs.whop.com/elements/latest/verifications/kyc.md) needs `identity:read` and `identity:write`.

A token created without `scoped_actions` inherits every permission of your API key.

### Refreshing the token

Access tokens expire after one hour by default and three hours at most. Set a fresh one on the handle before it expires:

```ts theme={null}
wallet.update({ accessToken: await fetchToken(accountId) });
```

In React, pass the new value as the `accessToken` prop.

> [!NOTE]
>
> `accessToken` is a value you set, not a callback the SDK calls. `Verifications` is the exception: it takes a `getToken` callback and calls it before the element mounts.

## Hosted payout portal

Instead of embedding the payout portal in your app, you can redirect users to a Whop-hosted payout portal. This is useful when you don't want to build a custom UI or need a quick integration.

Create an account link and redirect the user to the returned URL:

```typescript TypeScript theme={null}
import { WhopClient } from "@whop/sdk";

const client = new WhopClient({
	token: "Account API Key",
});

const accountLink = await client.accountLinks.create({
	account_id: "biz_xxxxxxxxxxxxx",
	use_case: "payouts_portal",
	return_url: "https://yourapp.com/payouts/complete",
	refresh_url: "https://yourapp.com/payouts/refresh",
});

// Redirect the user to the hosted portal
console.log(accountLink.url);
```

```python Python theme={null}
from whop_sdk import Whop

client = Whop(
    token="Account API Key",
)

account_link = client.account_links.create(
    account_id="biz_xxxxxxxxxxxxx",
    use_case="payouts_portal",
    return_url="https://yourapp.com/payouts/complete",
    refresh_url="https://yourapp.com/payouts/refresh",
)

# Redirect the user to the hosted portal
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
            use_case: AccountLinkUseCases::PayoutsPortal,
            return_url: "https://yourapp.com/payouts/complete".to_string(),
            refresh_url: "https://yourapp.com/payouts/refresh".to_string(),
        },
        None,
    )
    .await?;

// Redirect the user to the hosted portal
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
    UseCase:    whopsdk.AccountLinkUseCasesPayoutsPortal,
    ReturnURL:  "https://yourapp.com/payouts/complete",
    RefreshURL: "https://yourapp.com/payouts/refresh",
})
if err != nil {
    log.Fatal(err)
}

// Redirect the user to the hosted portal
fmt.Println(accountLink.URL)
```

In this example:

* `account_id` is the platform or subaccount
* `use_case` specifies the portal type
* `return_url` is where Whop redirects the user when they want to return to your site
* `refresh_url` is where Whop redirects the user if the session expires

### Available use cases

| Use case | Description |
| - | - |
| `account_onboarding` | KYC and identity verification |
| `payouts_portal` | Payouts, payout methods, KYC, and identity verification |

After creating the account link, redirect the user to the `url` returned in the response. The user completes the payout flow on the Whop-hosted portal, then Whop redirects them to your `return_url`.

## Related resources

> [card] [Wallet elements](https://docs.whop.com/elements/latest/wallet/overview)
>
> Every wallet surface with its options, events, and scopes

> [card] [Pay subaccounts](https://docs.whop.com/developer/platforms/collect-payments-for-subaccounts.md)
>
> Transfer funds to subaccounts


## Related topics

- [Manual Payouts to Subaccounts](https://docs.whop.com/developer/platforms/manual-payouts.md)
- [Example Integration](https://docs.whop.com/developer/guides/example-integration.md)
- [Enroll Subaccounts](https://docs.whop.com/developer/platforms/enroll-subaccounts.md)
