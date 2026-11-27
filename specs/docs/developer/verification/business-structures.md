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

# Business Structures

> Look up the business_structure values each country supports when creating or updating a KYB or KYC verification for a business entity, by country or value.

`business_structure` is the legal entity type of a business. Provide it when you create or update a verification for a business entity. This includes a Know Your Business (KYB) verification or a Know Your Customer (KYC) verification for a creator who operates through a company.

Pick the structure that matches the business's legal form in its country of incorporation.

## By country

| Country | Supported structures |
| - | - |
| `AE` | `llc`, `sole_establishment`, `free_zone_llc`, `free_zone_establishment` |
| `AT` | `incorporated_partnership`, `unincorporated_partnership`, `public_corporation`, `private_corporation` |
| `AU` | `public_corporation`, `private_corporation`, `sole_proprietorship`, `private_partnership`, `trust`, `unincorporated_association` |
| `BE` | `incorporated_partnership`, `unincorporated_partnership`, `public_corporation`, `private_corporation` |
| `BG` | `incorporated_partnership`, `unincorporated_partnership`, `public_corporation`, `private_corporation` |
| `CA` | `private_corporation`, `public_corporation`, `private_partnership`, `sole_proprietorship`, `registered_charity` |
| `CH` | `incorporated_partnership`, `unincorporated_partnership`, `public_corporation`, `private_corporation` |
| `CY` | `incorporated_partnership`, `unincorporated_partnership`, `public_corporation`, `private_corporation` |
| `CZ` | `incorporated_partnership`, `unincorporated_partnership`, `public_corporation`, `private_corporation` |
| `DE` | `private_corporation`, `public_corporation`, `incorporated_partnership`, `unincorporated_partnership`, `incorporated_non_profit`, `unincorporated_non_profit` |
| `DK` | `incorporated_partnership`, `unincorporated_partnership`, `public_corporation`, `private_corporation` |
| `EE` | `incorporated_partnership`, `unincorporated_partnership`, `public_corporation`, `private_corporation` |
| `ES` | `incorporated_partnership`, `unincorporated_partnership`, `public_corporation`, `private_corporation` |
| `FI` | `incorporated_partnership`, `unincorporated_partnership`, `public_corporation`, `private_corporation` |
| `FR` | `incorporated_partnership`, `unincorporated_partnership`, `public_corporation`, `private_corporation` |
| `GB` | `private_corporation`, `incorporated_partnership`, `unincorporated_partnership`, `incorporated_non_profit`, `unincorporated_non_profit` |
| `GI` | `incorporated_partnership`, `unincorporated_partnership`, `public_corporation`, `private_corporation` |
| `GR` | `incorporated_partnership`, `unincorporated_partnership`, `public_corporation`, `private_corporation` |
| `HK` | `private_company`, `private_partnership`, `sole_proprietorship`, `incorporated_non_profit`, `unincorporated_non_profit` |
| `HR` | `incorporated_partnership`, `unincorporated_partnership`, `public_corporation`, `private_corporation` |
| `HU` | `incorporated_partnership`, `unincorporated_partnership`, `public_corporation`, `private_corporation` |
| `IE` | `incorporated_partnership`, `unincorporated_partnership`, `public_corporation`, `private_corporation` |
| `IT` | `incorporated_partnership`, `unincorporated_partnership`, `public_corporation`, `private_corporation` |
| `JP` | `sole_proprietorship` |
| `LI` | `incorporated_partnership`, `unincorporated_partnership`, `public_corporation`, `private_corporation` |
| `LT` | `incorporated_partnership`, `unincorporated_partnership`, `public_corporation`, `private_corporation` |
| `LU` | `incorporated_partnership`, `unincorporated_partnership`, `public_corporation`, `private_corporation` |
| `LV` | `incorporated_partnership`, `unincorporated_partnership`, `public_corporation`, `private_corporation` |
| `MT` | `incorporated_partnership`, `unincorporated_partnership`, `public_corporation`, `private_corporation` |
| `NL` | `incorporated_partnership`, `unincorporated_partnership`, `public_corporation`, `private_corporation`, `sole_proprietorship` |
| `NO` | `incorporated_partnership`, `unincorporated_partnership`, `public_corporation`, `private_corporation` |
| `NZ` | `public_corporation`, `private_corporation`, `sole_proprietorship`, `private_partnership`, `trust` |
| `PL` | `incorporated_partnership`, `unincorporated_partnership`, `public_corporation`, `private_corporation` |
| `PT` | `incorporated_partnership`, `unincorporated_partnership`, `public_corporation`, `private_corporation` |
| `RO` | `incorporated_partnership`, `unincorporated_partnership`, `public_corporation`, `private_corporation` |
| `SE` | `incorporated_partnership`, `unincorporated_partnership`, `public_corporation`, `private_corporation` |
| `SG` | `sole_proprietorship`, `private_partnership`, `private_company`, `public_company` |
| `SI` | `incorporated_partnership`, `unincorporated_partnership`, `public_corporation`, `private_corporation` |
| `SK` | `incorporated_partnership`, `unincorporated_partnership`, `public_corporation`, `private_corporation` |
| `TH` | `private_corporation`, `incorporated_partnership`, `unincorporated_partnership`, `sole_proprietorship` |
| `US` | `single_member_llc`, `multi_member_llc`, `private_corporation`, `private_partnership`, `public_corporation`, `public_partnership`, `sole_proprietorship`, `unincorporated_association`, `incorporated_non_profit`, `unincorporated_non_profit` |

> [!NOTE]
>
> Countries not listed have no structure values. Declare the business with `business_name` alone and omit `business_structure`.

## Values

The same data, keyed by `business_structure` value.

| Value | Entity type | Countries |
| - | - | - |
| `free_zone_establishment` | Free zone establishment | `AE` |
| `free_zone_llc` | Free zone limited liability company | `AE` |
| `incorporated_non_profit` | Incorporated non-profit | `DE`, `GB`, `HK`, `US` |
| `incorporated_partnership` | Incorporated partnership | `AT`, `BE`, `BG`, `CH`, `CY`, `CZ`, `DE`, `DK`, `EE`, `ES`, `FI`, `FR`, `GB`, `GI`, `GR`, `HR`, `HU`, `IE`, `IT`, `LI`, `LT`, `LU`, `LV`, `MT`, `NL`, `NO`, `PL`, `PT`, `RO`, `SE`, `SI`, `SK`, `TH` |
| `llc` | Limited liability company | `AE` |
| `multi_member_llc` | Multi-member limited liability company | `US` |
| `private_company` | Private company | `HK`, `SG` |
| `private_corporation` | Private corporation | `AT`, `AU`, `BE`, `BG`, `CA`, `CH`, `CY`, `CZ`, `DE`, `DK`, `EE`, `ES`, `FI`, `FR`, `GB`, `GI`, `GR`, `HR`, `HU`, `IE`, `IT`, `LI`, `LT`, `LU`, `LV`, `MT`, `NL`, `NO`, `NZ`, `PL`, `PT`, `RO`, `SE`, `SI`, `SK`, `TH`, `US` |
| `private_partnership` | Private partnership | `AU`, `CA`, `HK`, `NZ`, `SG`, `US` |
| `public_company` | Public company | `SG` |
| `public_corporation` | Public corporation | `AT`, `AU`, `BE`, `BG`, `CA`, `CH`, `CY`, `CZ`, `DE`, `DK`, `EE`, `ES`, `FI`, `FR`, `GI`, `GR`, `HR`, `HU`, `IE`, `IT`, `LI`, `LT`, `LU`, `LV`, `MT`, `NL`, `NO`, `NZ`, `PL`, `PT`, `RO`, `SE`, `SI`, `SK`, `US` |
| `public_partnership` | Public partnership | `US` |
| `registered_charity` | Registered charity | `CA` |
| `single_member_llc` | Single-member limited liability company | `US` |
| `sole_establishment` | Sole establishment | `AE` |
| `sole_proprietorship` | Sole proprietorship | `AU`, `CA`, `HK`, `JP`, `NL`, `NZ`, `SG`, `TH`, `US` |
| `trust` | Trust | `AU`, `NZ` |
| `unincorporated_association` | Unincorporated association | `AU`, `US` |
| `unincorporated_non_profit` | Unincorporated non-profit | `DE`, `GB`, `HK`, `US` |


## Related topics

- [Check Verification Status](https://docs.whop.com/developer/verification/retrieve.md)
- [Update or Delete a Verification](https://docs.whop.com/developer/verification/manage.md)
- [Handle Verification RFIs](https://docs.whop.com/developer/verification/rfis.md)
