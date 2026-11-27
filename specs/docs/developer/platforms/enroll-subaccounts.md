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

# Enroll Subaccounts

> Create subaccounts for the businesses or people on your platform, and send them a link to finish onboarding.

To enroll a subaccount on your platform, create an Account object for them under your platform account. That account can then accept direct charges, receive transfers, and use Whop payout flows.

## Example

Create an account for the subaccount:

```typescript TypeScript theme={null}
import { WhopClient } from "@whop/sdk";

const client = new WhopClient({
	token: "Account API Key",
});

const account = await client.accounts.create({
	email: "merchant@example.com",
	title: "Shine Time North",
	external_id: "user_12345",
	metadata: {
		seller_tier: "gold",
	},
});

console.log(account.id);
```

```python Python theme={null}
from whop_sdk import Whop

client = Whop(
    token="Account API Key",
)

account = client.accounts.create(
    email="merchant@example.com",
    title="Shine Time North",
    external_id="user_12345",
    metadata={
        "seller_tier": "gold",
    },
)

print(account.id)
```

```rust Rust theme={null}
use whop_sdk::prelude::*;

let config = ClientConfig {
    token: Some("Account API Key".to_string()),
    ..Default::default()
};
let client = Whop::new(config).expect("Failed to build client");

let account = client
    .accounts
    .create(
        &CreateAccountsRequest {
            email: Some("merchant@example.com".to_string()),
            title: "Shine Time North".to_string(),
            external_id: Some("user_12345".to_string()),
            metadata: Some(HashMap::from([
                ("seller_tier".to_string(), json!("gold")),
            ])),
            ..Default::default()
        },
        None,
    )
    .await?;

println!("{}", account.id);
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

account, err := client.Accounts.Create(context.TODO(), &whopsdk.CreateAccountsRequest{
    Email: whopsdk.String("merchant@example.com"),
    Title: "Shine Time North",
    ExternalID: whopsdk.String("user_12345"),
    Metadata: map[string]any{
        "seller_tier": "gold",
    },
})
if err != nil {
    log.Fatal(err)
}

fmt.Println(account.ID)
```

In this example:

* `email` is the subaccount's email address. Whop uses it to identify the subaccount and send important notifications.
* The Account API key determines which platform account owns the subaccount.
* `title` is the required display name for the subaccount.
* `external_id` is your platform's own ID for this subaccount, for example your store ID. Each ID maps to one subaccount, and you can look the account up by it later.
* `metadata` contains custom key-value pairs, such as `seller_tier`, a classification or tier level for the subaccount (e.g., "bronze," "silver," or "gold").
* The response includes an `account.id` that you can use to reference this subaccount in future API calls

> [!TIP]
>
> The email address should belong to the subaccount (business or
> individual) who will be managing the account. This person will receive account
> setup and payment notifications.

## Find a subaccount by your ID

List your subaccounts with `external_id` to get the account for one of your records. End the value with `%` to get every account whose ID starts with a namespace you chose, such as `shopline:%`. Matching is case-sensitive, `_` matches literally, and `%` works only at the end, so IDs you create can't contain it.

```typescript TypeScript theme={null}
const accounts = await client.accounts.list({ external_id: "user_12345" });

console.log(accounts.data[0]?.id);
```

Update Account sets or clears `external_id` on an existing subaccount. Only your platform can change it. The subaccount can read it but can't edit it.

## Reusing an email address

Several subaccounts can share one email address, as long as each one has a distinct `external_id`. Creating an account is idempotent on the email and `external_id` pair:

* **New `external_id`**: Whop creates a new subaccount, even when the email is already in use.
* **Same email and `external_id`**: Whop returns the existing subaccount instead of creating a duplicate, so retries are safe.
* **`external_id` already used with another email**: Whop returns a `409` instead of creating a second account with that ID. To move the ID to the new account in the same request, see [Replace the account behind an ID](#replace-the-account-behind-an-id).
* **No `external_id`**: Whop returns the existing subaccount for that email that has no `external_id`, if there is one.

Accounts that share an email are managed by the same Whop user, the person who receives that inbox.

> [!NOTE]
>
> Earlier versions of this guide put `external_id` inside `metadata`. Whop still accepts it there on every API
> version, sets `external_id` from it, and returns it in `metadata`. It follows the same rules as the top-level field,
> including the `409` when another subaccount already has it, so send `reassign_external_id: true` to reuse one.
> Changing `metadata.external_id` later doesn't change `external_id`. To change it, use Update Account. New
> integrations should send the top-level field.

## Replace the account behind an ID

When you reissue an account on your side, for example a store that moves to a new owner, create the new subaccount with the same `external_id` and `reassign_external_id: true`. Whop moves the ID to the new subaccount in the same request instead of returning a `409`.

```typescript TypeScript theme={null}
const account = await client.accounts.create({
	email: "new-owner@example.com",
	external_id: "user_12345",
	reassign_external_id: true,
});
```

* The subaccount that had the ID isn't deleted or changed in any other way. It stays active with `external_id` set to `null`, keeps its `metadata` and balance, and sends an `account.updated` webhook. Delete it yourself if you no longer need it.
* If the create fails, the ID stays where it was.
* The request needs the `company:update` permission on your platform account as well as `company:create_child`, because it changes an existing subaccount.
* A request with the same email and `external_id` as an existing subaccount returns that subaccount, even when it sets the flag.
* The ID only moves between your own subaccounts.

To move an ID between two subaccounts that already exist, call Update Account twice: clear `external_id` on the old one, then set it on the other. Update Account has no `reassign_external_id`, and setting the ID before clearing it returns a `409`.

## Finish onboarding

If the recipient doesn't already have a Whop account, send them an account onboarding link after you create the account. They complete Know Your Customer (KYC) verification through that link, and then the account can receive transfers and other payout flows.

```typescript TypeScript theme={null}
const accountLink = await client.accountLinks.create({
	account_id: account.id,
	refresh_url: "https://yourapp.com/onboarding/refresh",
	return_url: "https://yourapp.com/onboarding/complete",
	use_case: "account_onboarding",
});

console.log(accountLink.url);
```

```python Python theme={null}
account_link = client.account_links.create(
    account_id=account.id,
    refresh_url="https://yourapp.com/onboarding/refresh",
    return_url="https://yourapp.com/onboarding/complete",
    use_case="account_onboarding",
)

print(account_link.url)
```

```rust Rust theme={null}
let account_link = client
    .account_links
    .create(
        &CreateAccountLinksRequest {
            account_id: "biz_xxxxxxxxxxxxx".to_string(),
            refresh_url: "https://yourapp.com/onboarding/refresh".to_string(),
            return_url: "https://yourapp.com/onboarding/complete".to_string(),
            use_case: AccountLinkUseCases::AccountOnboarding,
        },
        None,
    )
    .await?;

println!("{}", account_link.url);
```

```go Go theme={null}
accountLink, err := client.AccountLinks.Create(context.TODO(), &whopsdk.CreateAccountLinksRequest{
    AccountID:  "biz_xxxxxxxxxxxxx",
    RefreshURL: "https://yourapp.com/onboarding/refresh",
    ReturnURL:  "https://yourapp.com/onboarding/complete",
    UseCase:    whopsdk.AccountLinkUseCasesAccountOnboarding,
})
if err != nil {
    log.Fatal(err)
}

fmt.Println(accountLink.URL)
```

After the recipient completes onboarding, you can:

* create checkout flows for them with application fees.
* transfer funds to their account balance.
* let them withdraw funds with the payout portal.

## Getting started

Before you can enroll subaccounts, you need to set up your platform account:

1. **Sign up for a platform account**: Create an account at [whop.com/dashboard](https://whop.com/dashboard)
2. **Generate an Account API key**: Go to your developer settings page and generate an Account API key
3. **Use the API key for authentication**: This API key is how you authenticate with the Current API and control your subaccounts

The Account API key provides the necessary permissions to create and manage subaccounts under your platform account.

## Custom metadata

You can attach custom metadata to accounts when creating them. Metadata lets you store additional information about each subaccount as key-value pairs. This is useful for:

* Tracking subaccount tiers or classifications
* Linking to your platform's database records

Whop stores metadata on the Account object and returns it on every read, but you can't filter accounts by it. Put the identifier you look accounts up by in `external_id` instead.

## API reference

> [card] [Create Account API](https://docs.whop.com/api-reference/accounts/create-account.md)
>
> See the full API reference for creating accounts and all available parameters

## Next steps

After creating an Account for a subaccount:

> [card] [Collect and split payments](https://docs.whop.com/developer/platforms/collect-payments-for-subaccounts.md)
>
> Choose between direct charges and transfers

> [card] [Render payout portal](https://docs.whop.com/developer/platforms/render-payout-portal.md)
>
> Let subaccounts withdraw their funds


## Related topics

- [Collect Payments for Subaccounts](https://docs.whop.com/developer/platforms/collect-payments-for-subaccounts.md)
- [Sync Users to Embedded Chat](https://docs.whop.com/developer/guides/chat/sync-users.md)
- [Platform Quickstart](https://docs.whop.com/developer/platforms/quickstart.md)
