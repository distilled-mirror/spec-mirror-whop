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

# Social Accounts API (Deprecated)

> Social Accounts is the former public name for External Accounts.

Existing integrations can keep calling these deprecated endpoints while they migrate; every response points to the matching External Accounts endpoint.

Use the External Accounts API for all new integrations. External account IDs keep their `sacc_` prefix, and the underlying resource is unchanged.

## Endpoints

| Endpoint | Request |
| - | - |
| [List Social Accounts](https://docs.whop.com/api-reference/social-accounts/list-social-accounts.md) | GET `/social_accounts` |
| [Create a Social Account](https://docs.whop.com/api-reference/social-accounts/create-a-social-account.md) | POST `/social_accounts` |
| [Refresh](https://docs.whop.com/api-reference/social-accounts/refresh.md) | POST `/social_accounts/{id}/refresh` |
| [Connect a Social Account](https://docs.whop.com/api-reference/social-accounts/connect-a-social-account.md) | POST `/social_accounts/connect` |
| [Delete a Social Account](https://docs.whop.com/api-reference/social-accounts/delete-a-social-account.md) | DELETE `/social_accounts/{id}` |

## Attributes

**id** `string` *required*

Unique identifier for the social account.

**external\_id** `string | null` *required*

The platform-specific ID for this social account.

**name** `string | null` *required*

The display name of the social account on the platform.

**parent\_social\_account** `object | null` *required*

The social account this one belongs to on the platform, such as the Facebook page that owns an Instagram account. Null when the social account stands on its own, or when it is a partner.

> [accordion] Properties
>
> **id** `string` *required*
>
> Social account ID, prefixed `sacc_`.
>
> **external\_id** `string | null` *required*
>
> The platform-specific ID for the parent social account.
>
> **name** `string | null` *required*
>
> The display name of the parent social account on the platform.
>
> **platform** `string` *required*
>
> The platform the parent social account exists on.
>
> Available options: `x`, `instagram`, `youtube`, `tiktok`, `facebook`, `discord`, `telegram`, `linkedin`, `snapchat`, `klaviyo`, `quickbooks`, `aimerce`, `stripe`
>
> **profile\_picture\_url** `string | null` *required*
>
> The URL where the profile picture of the parent social account can be
> accessed.
>
> **username** `string | null` *required*
>
> The username of the parent social account on the platform.
>
> **verified** `boolean` *required*
>
> Whether the parent social account is verified on the platform.

**partnership\_status** `string | null` *required*

Always `null`. Partnership ads are no longer available.

**platform** `string` *required*

The platform the social account exists on.

Available options: `x`, `instagram`, `youtube`, `tiktok`, `facebook`, `discord`, `telegram`, `linkedin`, `snapchat`, `klaviyo`, `quickbooks`, `aimerce`, `stripe`

**profile\_picture\_url** `string | null` *required*

The URL where the profile picture of the social account can be accessed.

**scopes** `string[]` *required*

Capabilities Whop retains specific to this social account. For example, Whop
may request the ability to run advertisements that use this social account's
identity, reflected by `advertise`. Personal profile connections have no
additional Whop scopes.

**url** `string | null` *required*

The URL where the social account can be accessed on the platform. Null while a
Whop-owned account is still being provisioned.

**username** `string | null` *required*

The username of the social account on the platform. Null while a Whop-owned
account is still being provisioned, and on platforms without usernames, such
as Klaviyo.

**verified** `boolean` *required*

Whether the social account is verified on the platform.

```json SocialAccount theme={null}
{
	"id": "sacc_xxxxxxxxxxxx",
	"external_id": "17841400000000000",
	"name": "Unlimited Wash Club",
	"parent_social_account": {
		"id": "sacc_yyyyyyyyyyyy",
		"platform": "facebook",
		"username": "shinetimedetailing",
		"name": "Unlimited Wash Club",
		"profile_picture_url": "https://img.whop.com/sacc_yyyyyyyyyyyy.png",
		"verified": true,
		"external_id": "112233445566778"
	},
	"platform": "instagram",
	"profile_picture_url": "https://img.whop.com/sacc_xxxxxxxxxxxx.png",
	"scopes": ["advertise"],
	"partnership_status": null,
	"url": "https://instagram.com/shinetimedetailing",
	"username": "shinetimedetailing",
	"verified": true
}
```


## Related topics

- [Delete a Social Account](https://docs.whop.com/api-reference/social-accounts/delete-a-social-account.md)
- [Refresh Social Account](https://docs.whop.com/api-reference/social-accounts/refresh.md)
- [Create a Social Account](https://docs.whop.com/api-reference/social-accounts/create-a-social-account.md)
