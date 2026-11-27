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

# Set Up Payouts

> Set up Whop Payments so you can withdraw your balance to your bank account, manually or automatically.

Set up payouts to send your earnings to your bank account or preferred payout method. With Whop, you can set up manual, recurring, or instant payouts to your bank account through Whop Payments. You can also get direct payouts to your PayPal and Coinbase Commerce accounts.

## Enable withdrawals with Whop Payments

To withdraw, you'll first need to set up Whop Payments:

**1. Go to Balances**

Go to **Dashboard** > **Balances**

**2. Set up Whop Payments**

Select **Set up Whop Payments**

**3. Select your country**

Select the country of the bank account you pay out to. You can pay out and settle in a currency other than that country's local currency.

**4. Complete KYC**

Complete the KYC setup — fill in your details, link your bank, and upload ID

## Get started

**1. Go to Balances**

Go to your **Dashboard** > **Balances**

**2. Verify your identity**

Complete identity and account verification if prompted

**3. Add a payout method**

Add a **payout method** (bank account or other supported option)

**4. Set your schedule**

Choose your payout schedule (instant, daily, or weekly, depending on your account)

After setup, Whop sends sales funds according to your schedule and reserve policy.

## Check your balance

You can view and withdraw your earnings from either your **Dashboard** or your **Profile**.

### Through your dashboard

1. Go to **Dashboard** > **Balances**
2. Your **Balance** and **Withdrawals** are here

Your Balance includes:

* The amount you can withdraw now
* The balance that'll be available to pay out soon (when a customer pays, it can take a couple of days for the money to reach your account)
* The date your pending balance will be available on

### Through your profile

View your payouts at **Profile** > **Balance**.

## How to withdraw

You can withdraw from your **Dashboard** or **Profile** at any time, as long as you meet the minimum withdrawal threshold of \$10 (if applicable).

### Payout through the dashboard

**1. Go to Balances**

Go to your **Dashboard** > **Balances**

**2. Select Withdraw**

Select **Withdraw**

**3. Enter amount**

Input the **Amount** you want to withdraw and select **Continue** — you can also select **Max** to withdraw the maximum amount available

**4. Confirm payout**

Review the details and select **Payout**

### Payout through your profile

**1. Go to Balance**

Go to your **Profile** > **Balance**

**2. Select Withdraw**

Select **Withdraw**

**3. Enter amount and confirm**

Input the **Amount** you want to withdraw and select **Payout**

## Supported countries and territories

Whop supports payouts to 170+ countries and territories:

Albania

Algeria

American Samoa

Andorra

Angola

Anguilla

Antarctica

Antigua and Barbuda

Argentina

Armenia

Aruba

Australia

Austria

Azerbaijan

Bahamas

Bahrain

Bangladesh

Barbados

Belgium

Belize

Benin

Bermuda

Bhutan

Bolivia

Bosnia and Herzegovina

Botswana

Brazil

British Indian Ocean Territory

British Virgin Islands

Brunei

Bulgaria

Burkina Faso

Cambodia

Cameroon

Canada

Cape Verde

Cayman Islands

Chad

Chile

China

Christmas Island

Cocos (Keeling) Islands

Colombia

Cook Islands

Costa Rica

Croatia

Curaçao

Cyprus

Czech Republic

Côte d'Ivoire

Denmark

Djibouti

Dominica

Dominican Republic

Ecuador

Egypt

El Salvador

Equatorial Guinea

Eswatini

Estonia

Ethiopia

Falkland Islands

Faroe Islands

Fiji

Finland

France

French Guiana

French Polynesia

Gabon

Gambia

Georgia

Germany

Ghana

Gibraltar

Greece

Greenland

Grenada

Guadeloupe

Guam

Guatemala

Guernsey

Guinea

Guinea-Bissau

Guyana

Honduras

Hong Kong

Hungary

Iceland

India

Indonesia

Ireland

Isle of Man

Israel

Italy

Jamaica

Japan

Jersey

Jordan

Kazakhstan

Kenya

Kiribati

Kosovo

Kuwait

Kyrgyzstan

Laos

Latvia

Lebanon

Liberia

Liechtenstein

Lithuania

Luxembourg

Macau

Madagascar

Malawi

Malaysia

Maldives

Mali

Malta

Marshall Islands

Martinique

Mauritania

Mauritius

Mayotte

Mexico

Micronesia

Moldova

Monaco

Mongolia

Montenegro

Montserrat

Morocco

Mozambique

Namibia

Nauru

Nepal

Netherlands

Netherlands Antilles

New Caledonia

New Zealand

Nicaragua

Niger

Nigeria

Niue

North Macedonia

Northern Mariana Islands

Norway

Oman

Pakistan

Palau

Panama

Paraguay

Peru

Philippines

Pitcairn Islands

Poland

Portugal

Puerto Rico

Qatar

Republic of the Congo

Romania

Rwanda

Réunion

Saint Barthélemy

Saint Helena

Saint Kitts and Nevis

Saint Lucia

Saint Martin

Saint Pierre and Miquelon

Saint Vincent and the Grenadines

Samoa

San Marino

Saudi Arabia

Senegal

Serbia

Seychelles

Sierra Leone

Singapore

Sint Maarten

Slovakia

Slovenia

Solomon Islands

South Africa

South Korea

Spain

Sri Lanka

Suriname

Svalbard and Jan Mayen

Sweden

Switzerland

São Tomé and Príncipe

Taiwan

Tajikistan

Tanzania

Thailand

Timor-Leste

Togo

Tokelau

Tonga

Trinidad and Tobago

Tunisia

Turkey

Turkmenistan

Turks and Caicos Islands

Tuvalu

U.S. Minor Outlying Islands

U.S. Virgin Islands

Uganda

Ukraine

United Arab Emirates

United Kingdom

United States

Uruguay

Uzbekistan

Vanuatu

Vatican City

Vietnam

Wallis and Futuna

Western Sahara

Zambia

Zimbabwe

Note that different payout options will be available depending on the country to which you're paying out.

> [!WARNING]
>
> Whop doesn't support payouts to sanctioned or restricted countries. See the full list of [sanctioned countries](https://docs.whop.com/trust-and-safety/trust-safety-overview/sanctioned-countries.md).

## Setting up automatic withdrawals

**1. Go to Balances**

Go to your **Dashboard** > **Balances**

**2. Open automatic withdrawals**

Select the three dots on the top right corner of the **Balances** page, then select **Automatic withdrawals**

**3. Choose frequency**

Choose **How often you want to get paid** and select **Continue**

**4. Set minimum balance**

Choose the minimum amount to keep in your account and select **Continue** — you should always have a minimum balance available for refunds or to pay out disputes

**5. Confirm**

Review the details and select **Confirm**

For payout statuses, failed payouts, and affiliate withdrawals, see [Troubleshoot payouts](https://docs.whop.com/manage-your-business/manage-payouts/troubleshoot-payouts.md).

## Crypto payout restrictions

Whop supports Egypt, Kenya, and Ghana. In light of the local regulatory landscape, Whop currently does not offer crypto capabilities in these markets, including crypto payouts and off-ramps.


## Related topics

- [Sanctioned Countries](https://docs.whop.com/trust-and-safety/trust-safety-overview/sanctioned-countries.md)
- [Whop Cards](https://docs.whop.com/whop-finance/cards.md)
- [Set Up Custom Affiliates](https://docs.whop.com/affiliates/setup-custom.md)
