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

# Email Addresses API

> An Email address is a mailbox on one of the account's domains, such as support@example.com. The domain needs the `email` capability in the `ready` state first.

A mailbox can belong to one team member, who alone can read it and send from it. Or the account can share it, and then `email:read` and `email:send` decide who can use it.

Use the Email addresses API to create, list, reassign and delete mailboxes. You can read the password that mail apps sign in with. You can also download a configuration profile that adds the mailbox to Mail on Apple devices. Deleting an address keeps its mail, and creating it again in the same account brings the mailbox back.

## Endpoints

| Endpoint | Request |
| - | - |
| [List Email Addresses](https://docs.whop.com/api-reference/email-addresses/list-email-addresses.md) | GET `/email_addresses` |
| [Retrieve Email Address](https://docs.whop.com/api-reference/email-addresses/retrieve-email-address.md) | GET `/email_addresses/{id}` |
| [Download Email Address Configuration Profile](https://docs.whop.com/api-reference/email-addresses/download-email-address-configuration-profile.md) | GET `/email_addresses/{id}/mobileconfig` |
| [Create Email Address](https://docs.whop.com/api-reference/email-addresses/create-email-address.md) | POST `/email_addresses` |
| [Reset Email Address Password](https://docs.whop.com/api-reference/email-addresses/reset-email-address-password.md) | POST `/email_addresses/{id}/reset_password` |
| [Update Email Address](https://docs.whop.com/api-reference/email-addresses/update-email-address.md) | PATCH `/email_addresses/{id}` |
| [Delete Email Address](https://docs.whop.com/api-reference/email-addresses/delete-email-address.md) | DELETE `/email_addresses/{id}` |

## Attributes

**id** `string` *required*

Email address ID, prefixed `emaddr_`.

**account\_id** `string` *required*

ID of the account the address belongs to, prefixed `biz_`.

**address** `string` *required*

The full address in lowercase, such as [support@example.com](mailto:support@example.com).

**created\_at** `string` *required*

When the address was created, as an ISO 8601 timestamp.

**domain\_id** `string` *required*

ID of the domain the address is on, prefixed `dom_`.

**password** `string | null` *required*

The password mail apps sign in to the mailbox with over IMAP and SMTP, using
the address as the username. `null` unless you can use the mailbox: you own
it, or nobody owns it and you have `email:read` and `email:send`.

**updated\_at** `string` *required*

When the address was last updated, as an ISO 8601 timestamp.

**user\_id** `string | null` *required*

ID of the team member who owns the mailbox, prefixed `user_`. Only they can
read it or send from it. `null` when the whole account shares it.

```json EmailAddress theme={null}
{
	"id": "emaddr_xxxxxxxxxxxxxx",
	"address": "support@shinetime.example",
	"account_id": "biz_xxxxxxxxxxxxxx",
	"domain_id": "dom_xxxxxxxxxxxxxx",
	"user_id": null,
	"password": null,
	"created_at": "2026-08-20T13:05:00.000Z",
	"updated_at": "2026-08-20T13:05:00.000Z"
}
```


## Related topics

- [Delete Email Address](https://docs.whop.com/api-reference/email-addresses/delete-email-address.md)
- [Retrieve Email Address](https://docs.whop.com/api-reference/email-addresses/retrieve-email-address.md)
- [Update Email Address](https://docs.whop.com/api-reference/email-addresses/update-email-address.md)
