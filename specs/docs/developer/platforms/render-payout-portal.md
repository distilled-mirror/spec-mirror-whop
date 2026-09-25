> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Enable Connected Account Payouts

> Let your connected accounts manage their own payouts with Whop Elements or a hosted portal

Let your connected accounts complete Know Your Customer (KYC) verification, add payout methods, and withdraw their funds on their own. You can mount the [Whop Elements](/elements/latest/getting-started) wallet surfaces directly in your app or redirect users to a Whop-hosted portal.

## Embedded payout portal

### Server side implementation

To mount the wallet elements, generate an access token for the connected account on your server. The elements read that account's balance and payouts with it. See [Auth & API keys](/developer/guides/auth-scoping) for how to scope that token and how it compares to Whop's other credential types.

<CodeGroup>
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
  			company_id: accountId,
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
  		company_id: accountId,
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
          company_id=account_id
      )

      return jsonify({'token': access_token.token})
  ```
</CodeGroup>

<Card title="Create Access Token API" icon="code" href="/api-reference/access-tokens/create-access-token">
  See the full API reference for generating access tokens and all available
  parameters
</Card>

## Client side setup

<CodeGroup>
  ```bash npm theme={null}
  npm install @whop/elements-react @whop/elements
  ```

  ```bash pnpm theme={null}
  pnpm add @whop/elements-react @whop/elements
  ```

  ```html HTML theme={null}
  <script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>
  ```
</CodeGroup>

## Client side implementation

Create one `Wallet` handle for the connected account and mount the surfaces you need under it. The handle sends its access token to every element beneath it, so mint one token that carries every scope those elements read.

<CodeGroup>
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
</CodeGroup>

<Note>
  On iOS, the wallet SDK ships `BalanceElement`, `ListElement`, and `ActivityElement`. See [Getting Started with Elements](/elements/latest/getting-started) and pick the Swift tab.
</Note>

### Identity verification

A connected account must verify its identity before it can withdraw. Mount the [KYC element](/elements/latest/verifications/kyc) for the full in-page flow, which collects the details, runs the hosted provider session, and reports the result. Or mount the [RequiredActions element](/elements/latest/dashboard/required-actions) to show every outstanding action, verification included, and let the user start each one.

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

| Option        | Type         | Required                | Description                                                                                                    |
| ------------- | ------------ | ----------------------- | -------------------------------------------------------------------------------------------------------------- |
| `accountId`   | `string`     | Yes                     | The connected account's ID, prefixed `biz_`.                                                                   |
| `accessToken` | `string`     | Yes, on your own domain | The token from your server. Without it, reads use the viewer's whop.com session, which only works on whop.com. |
| `currency`    | `string`     | No                      | ISO 4217 code for amount fields. Defaults to `usd`.                                                            |
| `appearance`  | `Appearance` | No                      | Theme and per-part styling for every element under the handle. See [Appearance](/elements/latest/appearance).  |

Every option, event, and method is listed on the [Wallet reference](/elements/latest/wallet/overview).

### Scopes

The elements read with the token you mint, so grant every scope they need on that one token:

* [Balances](/elements/latest/wallet/balances) and [Activity](/elements/latest/wallet/activity) read `company:balance:read`.
* [Withdraw](/elements/latest/wallet/withdraw) needs `payout:withdraw_funds`, `payout:destination:read`, and `payout:create_destination` to add a bank.
* [KYC](/elements/latest/verifications/kyc) needs `identity:read` and `identity:write`.

A token created without `scoped_actions` inherits every permission of your API key.

### Refreshing the token

Access tokens expire after one hour by default and three hours at most. Set a fresh one on the handle before it expires:

```ts theme={null}
wallet.update({ accessToken: await fetchToken(accountId) });
```

In React, pass the new value as the `accessToken` prop.

<Note>
  `accessToken` is a value you set, not a callback the SDK calls. `Verifications` is the exception: it takes a `getToken` callback and calls it before the element mounts.
</Note>

## Hosted payout portal

Instead of embedding the payout portal in your app, you can redirect users to a Whop-hosted payout portal. This is useful when you don't want to build a custom UI or need a quick integration.

Create an account link and redirect the user to the returned URL:

<CodeGroup>
  ```typescript TypeScript theme={null}
  import { WhopClient } from "@whop/sdk";

  const client = new WhopClient({
  	token: "Account API Key",
  });

  const accountLink = await client.accountLinks.create({
  	company_id: "biz_xxxxxxxxxxxxx",
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
      company_id="biz_xxxxxxxxxxxxx",
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
              company_id: "biz_xxxxxxxxxxxxx".to_string(),
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
      CompanyID:  "biz_xxxxxxxxxxxxx",
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
</CodeGroup>

In this example:

* `company_id` is the platform or connected account
* `use_case` specifies the portal type
* `return_url` is where Whop redirects the user when they want to return to your site
* `refresh_url` is where Whop redirects the user if the session expires

### Available use cases

| Use case             | Description                                             |
| -------------------- | ------------------------------------------------------- |
| `account_onboarding` | KYC and identity verification                           |
| `payouts_portal`     | Payouts, payout methods, KYC, and identity verification |

After creating the account link, redirect the user to the `url` returned in the response. The user completes the payout flow on the Whop-hosted portal, then Whop redirects them to your `return_url`.

## Related resources

<CardGroup cols={2}>
  <Card title="Wallet elements" icon="wallet" href="/elements/latest/wallet/overview">
    Every wallet surface with its options, events, and scopes
  </Card>

  <Card title="Pay connected accounts" icon="arrow-right-arrow-left" href="/developer/platforms/collect-payments-for-connected-accounts">
    Transfer funds to connected accounts
  </Card>
</CardGroup>
