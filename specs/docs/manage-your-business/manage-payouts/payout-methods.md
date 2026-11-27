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

# Payout Methods

> Add a payout method such as a bank account or PayPal, and compare standard and instant payouts.

Payout methods are the bank accounts or other destinations where Whop sends your earnings.

## Payout options

Whop supports payouts to your bank account, mobile wallet, or crypto wallet in 170+ countries and territories. Depending on your country, all eligible payout methods will show up on the modal when you go to withdraw from the **[Balances](https://whop.com/dashboard/balance/)** page of your Whop dashboard. Different countries are eligible for different payout options.

Standard payouts take up to 5 business days. You can also withdraw your balance through **instant payout methods**, which take only a few minutes. Instant payouts let you withdraw pending funds from payments that haven't fully settled in your Whop balance. See [Instant withdrawals](https://docs.whop.com/manage-your-business/manage-payouts/instant-withdrawals.md) for how much you can withdraw instantly and how the daily limit works.

> [!WARNING]
>
> Whop may flag some withdrawals for compliance review based on different risk
> factors. The Trust & Safety team manually reviews and releases these
> withdrawals. When a payout requires review, the Instant payout option doesn't
> appear because the payout won't be instant.

## Add a payout method

1. Go to your **Dashboard** > **Balances** > **Payout methods**
2. Select **Add payout method**
3. Enter your bank details or connect via the supported flow
4. Confirm and set as default if you want new payouts to use it

You can have multiple payout methods and sometimes choose which one to use per payout.

## International payouts

You can pay out and settle in a currency other than your country's local currency. The bank account must accept the currency of the payout.

On Whop Payments, Whop is the *Merchant of Record*. A sale settles in its plan's currency unless you set a preferred settlement currency. You can settle new sales in United States dollars, euros, British pounds, Canadian dollars, or Australian dollars. That currency doesn't have to be local to your country. Whop charges 0.5% on each sale it converts. Money already in your balance stays in its current currency.

* Whop doesn't convert a United States dollar payment into your local currency before the payout.
* You can send the payout to a bank account in another currency, such as United States dollars, when your country offers that payout method.

Open your dashboard settings and select **Payments**. Under **Preferred settlement currency**, select a currency in **Settle every new sale in**. Then select **Save changes**. [Preferred settlement currency](https://docs.whop.com/developer/payouts/preferred-settlement-currency.md) covers refunds and which sales convert.

> [!TIP]
>
> You can change the payout country for an existing Whop. To switch payout
> countries, navigate to your payouts and select the 3 dots on the top right,
> then select **Change payout country**.

## Payouts to a digital wallet

To receive PayPal payouts, customers must buy using PayPal (no post-purchase transfer to PayPal).

### Setting up payouts to your PayPal

1. Go to your **Dashboard** > **Balances** > **Additional payment methods**
2. Select **Setup** next to the **PayPal** option
3. Follow the on-screen instructions to link your PayPal account

> [!TIP]
>
> For PayPal to appear at checkout, check the PayPal box under Payment options
> when you create a checkout link.

![PayPal](https://mintcdn.com/whop/gkU9N_oxv5Gb4qnv/images/paypalpopup.png?fit=max\&auto=format\&n=gkU9N_oxv5Gb4qnv\&q=85\&s=66b2f9849feb3d72ce18900852515541)

## Coinbase payouts

**Important notes:**

* Withdrawals must go to a Coinbase wallet
* To receive Coinbase payouts, customers must buy using crypto (no post-purchase transfer to Coinbase)

### Setting up payouts to your Coinbase crypto wallet

1. Go to your **Dashboard** > **Balances** > **Additional payment methods**
2. Select **Setup** next to the **Coinbase** option
3. Follow the on-screen instructions to link your Coinbase Commerce account
4. You will need to log into your Coinbase account and copy your API key and Webhook secret to paste into Whop

**For Coinbase to appear at checkout:** Check the **Crypto** box under **Payment options** when you create a checkout link.

> [!TIP]
>
> To get paid with crypto, you have to set up a one-time pricing plan.

![Coinbase](https://mintcdn.com/whop/gkU9N_oxv5Gb4qnv/images/coinbasepopup.png?fit=max\&auto=format\&n=gkU9N_oxv5Gb4qnv\&q=85\&s=1054b235fdef59fe7509786a5ca31537)

## Crypto payout restrictions

Whop supports Egypt, Kenya, and Ghana. In light of the local regulatory landscape, Whop currently does not offer crypto capabilities in these markets, including crypto payouts and off-ramps.


## Related topics

- [Set Up Payouts](https://docs.whop.com/manage-your-business/manage-payouts/set-up-payouts.md)
- [Instant Withdrawals](https://docs.whop.com/manage-your-business/manage-payouts/instant-withdrawals.md)
- [Payment Processing and Payout Fees](https://docs.whop.com/fees.md)
