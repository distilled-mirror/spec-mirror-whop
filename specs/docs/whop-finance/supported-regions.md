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

# Whop Card Supported Regions

> Supported U.S. states and unsupported countries for consumer and corporate Whop Cards

## Supported states

Whop Cards support consumers and business entities in the following U.S. states. For consumers, eligibility depends on your state of residence. For business entities, eligibility depends on your business's state of registration.

### Consumers

Consumers who reside in any of these 31 states can apply for a Whop Card:

* Alabama
* Alaska
* Arkansas
* California
* Colorado
* Connecticut
* Florida
* Hawaii
* Illinois
* Indiana
* Iowa
* Kansas
* Kentucky
* Maine
* Massachusetts
* Michigan
* Minnesota
* Montana
* Nebraska
* New Hampshire
* New Jersey
* New York
* North Carolina
* Oklahoma
* Pennsylvania
* South Carolina
* Texas
* Utah
* Virginia
* West Virginia
* Wyoming

### Business entities

Business entities registered in any of these 41 states can apply for Whop Cards:

* Alabama
* Alaska
* Arizona
* Arkansas
* California
* Colorado
* Connecticut
* Delaware
* Florida
* Georgia
* Hawaii
* Illinois
* Indiana
* Iowa
* Kansas
* Kentucky
* Louisiana
* Maine
* Maryland
* Massachusetts
* Michigan
* Minnesota
* Montana
* Nebraska
* New Hampshire
* New Jersey
* New York
* North Carolina
* Ohio
* Oklahoma
* Oregon
* Pennsylvania
* Rhode Island
* South Carolina
* Texas
* Utah
* Virginia
* Washington
* West Virginia
* Wisconsin
* Wyoming

## Unsupported corporate countries

Whop can't issue cards to business entities registered in these countries and regions:

* Algeria
* Angola
* Belarus
* Bulgaria
* Burkina Faso
* Cameroon
* China (Mainland)
* Croatia
* Cuba
* Democratic Republic of Congo
* Haiti
* Hong Kong
* India
* Iran
* Iraq
* Israel
* Lebanon
* Mali
* Monaco
* Mozambique
* Myanmar
* Namibia
* Nepal
* Nicaragua
* North Korea
* Russia
* South Sudan
* Syria
* Tanzania
* Turkey
* Ukraine
* Venezuela
* Vietnam
* Yemen

## Unsupported consumer countries

Whop can't issue cards to consumers residing in these countries and regions:

* Belarus
* China (Mainland)
* Cuba
* Hong Kong
* India
* Iran
* Iraq
* Israel
* Myanmar
* Nepal
* Nicaragua
* North Korea
* Russia
* Syria
* Turkey
* Ukraine
* Venezuela
* Vietnam


## Related topics

- [Whop Cards](https://docs.whop.com/whop-finance/cards.md)
- [Whop Partner Program Guide](https://docs.whop.com/partners.md)
- [Regional Fees](https://docs.whop.com/payments-and-billing/fees/regional-fees.md)
