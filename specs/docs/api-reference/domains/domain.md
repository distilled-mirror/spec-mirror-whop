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

# Domains API

> A Domain is a hostname an account holds on Whop.

Its capabilities set what Whop does with it: `registration` buys the domain through Whop, runs its DNS, and renews it every year; `verification` connects a domain registered elsewhere once you publish the DNS records it returns, and expires if ownership is not proven within 48 hours; `website` serves one of the account's apps on it.

Use the Domains API to search for a domain to buy, buy or connect one, serve a website on it, and check its DNS, payment, and renewal state. Thirty days before a bought domain expires, Whop opens a renewal charge at `purchase_url` and, while `auto_renew` is on, charges the saved card. An unpaid domain stops serving when it expires but stays renewable at `purchase_url` until the registry's grace period ends, and is then removed.

## Endpoints

| Endpoint | Request |
| - | - |
| [List Domains](https://docs.whop.com/api-reference/domains/list-domains.md) | GET `/domains` |
| [Retrieve Domain](https://docs.whop.com/api-reference/domains/retrieve-domain.md) | GET `/domains/{id}` |
| [Create Domain](https://docs.whop.com/api-reference/domains/create-domain.md) | POST `/domains` |
| [Check Domain](https://docs.whop.com/api-reference/domains/check-domain.md) | POST `/domains/{id}/check` |
| [Update Domain](https://docs.whop.com/api-reference/domains/update-domain.md) | PATCH `/domains/{id}` |
| [Delete Domain](https://docs.whop.com/api-reference/domains/delete-domain.md) | DELETE `/domains/{id}` |

## Attributes

**id** `string | null` *required*

Domain ID, prefixed `dom_`. `null` when no domain on Whop has this hostname.

**account\_id** `string | null` *required*

ID of the account the domain belongs to, prefixed `biz_`.

**apple\_pay** `object | null` *required*

Offering Apple Pay at checkouts served from the domain. `null` when not enrolled or not visible to you.

> [accordion] Properties
>
> **state** `string` *required*
>
> `pending` while Whop registers the domain with Apple, `ready` once Apple verified it, and `action_required` when Apple couldn't; its issue says how to fix it. `releasing` while Whop removes it from Apple.
>
> Available options: `pending`, `ready`, `action_required`, `releasing`

**availability** `object | null` *required*

Whether you can register the hostname and what it costs. Set only when no domain on Whop has it.

> [accordion] Properties
>
> **available** `boolean` *required*
>
> Whether the domain can be registered now.
>
> **premium** `boolean` *required*
>
> Whether the registry charges more than its standard price for this domain.
>
> **price** `object | null` *required*
>
> What the first year of registration costs. `null` when the domain is not available.
>
> > [accordion] Properties
> >
> > **amount** `string` *required*
> >
> > The amount in major units, as an exact decimal string. For example, `"10.00"`
> > is ten dollars. It is a string so that no float rounds it in transit.
> >
> > **currency** `string` *required*
> >
> > Three-letter ISO 4217 currency code, lowercase.
> >
> > **decimals** `integer` *required*
> >
> > How many decimal places the amount carries. This is the precision the charge
> > itself runs at.
> >
> > **display\_decimals** `integer` *required*
> >
> > How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
>
> **renewal\_price** `object | null` *required*
>
> What each yearly renewal costs after the first year. `null` when the domain is not available.
>
> > [accordion] Properties
> >
> > **amount** `string` *required*
> >
> > The amount in major units, as an exact decimal string. For example, `"10.00"`
> > is ten dollars. It is a string so that no float rounds it in transit.
> >
> > **currency** `string` *required*
> >
> > Three-letter ISO 4217 currency code, lowercase.
> >
> > **decimals** `integer` *required*
> >
> > How many decimal places the amount carries. This is the precision the charge
> > itself runs at.
> >
> > **display\_decimals** `integer` *required*
> >
> > How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
>
> **score** `integer` *required*
>
> How desirable the domain is, from 0 to 100. Short, real-word names on
> well-known extensions score highest.
>
> **transfer\_price** `object | null` *required*
>
> What moving the domain to Whop costs if it's registered elsewhere, including the year it adds to the domain's registration. `null` when it's available or can't be transferred.
>
> > [accordion] Properties
> >
> > **amount** `string` *required*
> >
> > The amount in major units, as an exact decimal string. For example, `"10.00"`
> > is ten dollars. It is a string so that no float rounds it in transit.
> >
> > **currency** `string` *required*
> >
> > Three-letter ISO 4217 currency code, lowercase.
> >
> > **decimals** `integer` *required*
> >
> > How many decimal places the amount carries. This is the precision the charge
> > itself runs at.
> >
> > **display\_decimals** `integer` *required*
> >
> > How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.

**created\_at** `string | null` *required*

When the domain was created, as an ISO 8601 timestamp.

**domain** `string` *required*

Normalized hostname, such as checkout.example.com.

**email** `object | null` *required*

Sending and receiving email on the domain through Whop. `null` when not wanted, or when the domain belongs to an account you can't read.

> [accordion] Properties
>
> **state** `string` *required*
>
> `pending` while Whop sets the domain up with its email provider and publishes its DNS records, `ready` once the provider verified them, and `action_required` when it couldn't; its issue says what happens next. `releasing` while Whop removes email from the domain.
>
> Available options: `pending`, `ready`, `action_required`, `releasing`

**issues** `object[]` *required*

What needs attention, each tagged with its capability. Empty when nothing does or when the domain isn't yours.

> [accordion] Properties
>
> **capability** `string` *required*
>
> The capability the issue is about.
>
> Available options: `verification`, `registration`, `platform`, `subdomain`, `website`, `apple_pay`, `pixel`, `email`, `transfer`
>
> **code** `string` *required*
>
> What needs attention, as a stable code.
>
> Available options: `verification_required`, `verification_expired`, `ownership_conflict`, `ownership_lost`, `dns_required`, `provider_validation`, `certificate_pending`, `expiring_soon`, `account_unavailable`, `check_failed`, `purchase_expired`, `domain_unavailable`, `unsupported_tld`, `registration_unavailable`, `registration_failed`, `renewal_failed`, `payment_action_required`, `domain_on_whop`, `domain_transfer_unavailable`, `transfer_locked`, `transfer_auth_code_rejected`, `transfer_rejected`, `transfer_not_cancellable`, `domain_transfer_failed`, `transfer_out_locked`, `registrar_unavailable`, `apple_pay_unverified`, `pixel_not_seen`, `pixel_not_installed`, `pixel_waiting`, `pixel_unreachable`, `pixel_quiet`, `email_needs_whop_dns`, `email_unreachable`, `email_identity_in_use`, `email_verification_failed`, `subdomain_needs_whop_domain`, `root_domain_pending`, `root_domain_needs_action`
>
> **code\_block** `string | null` *required*
>
> Code to add to the site to fix the issue, ready to copy. `null` when the issue
> needs none.
>
> **dns\_records** `object[]` *required*
>
> DNS records to publish to fix the issue. Empty when the issue needs no DNS change.
>
> > [accordion] Properties
> >
> > **name** `string` *required*
> >
> > Full hostname where the record must be published.
> >
> > **type** `string` *required*
> >
> > DNS record type.
> >
> > Available options: `TXT`, `CNAME`, `A`, `AAAA`
> >
> > **value** `string` *required*
> >
> > DNS record content.
>
> **message** `string` *required*
>
> What needs attention and how to fix it.

**metadata** `object` *required*

Custom string keys and values attached to this domain. Empty when the domain
isn't yours.

**owned\_by** `object | null` *required*

The account that has proven it owns this hostname on Whop, which can be your own. `null` when no account has.

> [accordion] Properties
>
> **id** `string` *required*
>
> ID of the account that owns the domain, prefixed `biz_`.
>
> **logo\_url** `string | null` *required*
>
> Account logo image URL.
>
> **route** `string` *required*
>
> Account public route identifier.
>
> **title** `string` *required*
>
> Account display name.

**pixel** `object | null` *required*

Tracking page views on the domain with the Whop pixel. `null` when not tracked or not visible to you.

> [accordion] Properties
>
> **state** `string` *required*
>
> `ready` while the Whop pixel recorded a page view on the domain in the last 7 days. `pending` until a pixel you asked for records its first one, and `action_required` once it goes 7 days without one; its issue says what to do.
>
> Available options: `pending`, `ready`, `action_required`

**platform** `object | null` *required*

An address on a zone Whop runs, such as example.whop.site, that follows its app's route. `null` for other domains or when not visible to you.

> [accordion] Properties
>
> **state** `string` *required*
>
> Whether Whop holds this address for the account. A platform address is ready as soon as its route is claimed.
>
> Available options: `pending`, `ready`, `action_required`, `releasing`

**public\_record** `object | null` *required*

The domain's public registration record (RDAP), read when you retrieve it by hostname. `null` for domains on Whop, available domains, or a record that couldn't be read.

> [accordion] Properties
>
> **dnssec** `boolean | null` *required*
>
> Whether the domain's delegation is signed with DNSSEC, or `null` when the
> registry doesn't say.
>
> **expires\_at** `string | null` *required*
>
> When the current registration expires, as an ISO 8601 timestamp, or `null`
> when the record omits it.
>
> **name\_servers** `string[]` *required*
>
> The name servers the domain delegates to, lowercase, such as
> `ns1.example.com`. Empty when the record lists none.
>
> **registered\_at** `string | null` *required*
>
> When the domain was first registered, as an ISO 8601 timestamp, or `null` when
> the record omits it.
>
> **registrant** `object | null` *required*
>
> Who the domain is registered to, often a privacy service or `Redacted For Privacy`. `null` when the record publishes no registrant.
>
> > [accordion] Properties
> >
> > **address** `string | null` *required*
> >
> > Registrant postal address as published, with lines separated by line breaks.
> > Often only a state or region. `null` when withheld.
> >
> > **contact\_url** `string | null` *required*
> >
> > A web form for contacting the registrant, published when the registrar
> > withholds their email. `null` when there is none.
> >
> > **country** `string | null` *required*
> >
> > Registrant country as an ISO 3166-1 alpha-2 code, such as `US`, or `null` when
> > withheld.
> >
> > **email** `string | null` *required*
> >
> > Registrant email address, often a privacy forwarding address, or `null` when
> > withheld.
> >
> > **name** `string | null` *required*
> >
> > Registrant name, or `null` when withheld.
> >
> > **organization** `string | null` *required*
> >
> > Registrant organization, often a privacy service, or `null` when withheld.
> >
> > **phone** `string | null` *required*
> >
> > Registrant phone number, or `null` when withheld.
>
> **registrar** `object | null` *required*
>
> The registrar that manages the domain, or `null` when the record doesn't name one.
>
> > [accordion] Properties
> >
> > **iana\_id** `string | null` *required*
> >
> > The registrar's IANA registrar ID, or `null` when the record omits it.
> >
> > **name** `string` *required*
> >
> > Registrar name, such as `GoDaddy.com, LLC`.
> >
> > **url** `string | null` *required*
> >
> > The registrar's website, or `null` when the record omits it.
>
> **statuses** `string[]` *required*
>
> The domain's registry statuses in RDAP form, such as `client transfer
>           	prohibited`. Empty when the record lists none.
>
> **updated\_at** `string | null` *required*
>
> When the registration record last changed, as an ISO 8601 timestamp, or `null` when the record omits it.

**registration** `object | null` *required*

Buying the domain through Whop, renewing it, and running its DNS. `null` when not wanted or not visible to you.

> [accordion] Properties
>
> **amount\_due** `object | null` *required*
>
> What the domain owes now. `null` when nothing is owed.
>
> > [accordion] Properties
> >
> > **amount** `string` *required*
> >
> > The amount in major units, as an exact decimal string. For example, `"10.00"`
> > is ten dollars. It is a string so that no float rounds it in transit.
> >
> > **currency** `string` *required*
> >
> > Three-letter ISO 4217 currency code, lowercase.
> >
> > **decimals** `integer` *required*
> >
> > How many decimal places the amount carries. This is the precision the charge
> > itself runs at.
> >
> > **display\_decimals** `integer` *required*
> >
> > How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
>
> **auto\_renew** `boolean` *required*
>
> Whether Whop charges the saved payment method to renew the domain before it
> expires.
>
> **expires\_at** `string | null` *required*
>
> When the registration expires unless renewed, as an ISO 8601 timestamp. `null`
> until it's registered.
>
> **method** `string` *required*
>
> How the domain came to Whop: `registration` when it was bought through Whop, `transfer` when it was moved from another registrar.
>
> Available options: `registration`, `transfer`
>
> **payment\_method\_id** `string | null` *required*
>
> ID of the saved card charged for the domain, prefixed `payt_`. `null` when
> none is set.
>
> **phase** `string` *required*
>
> Where the registration is: awaiting payment, ordering at the registrar, transferring from another registrar, delegating DNS to Whop, held, expired, or releasing.
>
> Available options: `awaiting_payment`, `ordering`, `transferring`, `delegating`, `held`, `expired`, `failed`, `releasing`
>
> **purchase\_url** `string | null` *required*
>
> Link to pay what the domain owes now: its registration or transfer, or a
> yearly renewal. `null` when nothing is owed.
>
> **renewal\_price** `object | null` *required*
>
> What each yearly renewal costs at the registrar's current price. Set when the response is a single domain; `null` in lists, while the domain is being released, or when the price can't be read right now.
>
> > [accordion] Properties
> >
> > **amount** `string` *required*
> >
> > The amount in major units, as an exact decimal string. For example, `"10.00"`
> > is ten dollars. It is a string so that no float rounds it in transit.
> >
> > **currency** `string` *required*
> >
> > Three-letter ISO 4217 currency code, lowercase.
> >
> > **decimals** `integer` *required*
> >
> > How many decimal places the amount carries. This is the precision the charge
> > itself runs at.
> >
> > **display\_decimals** `integer` *required*
> >
> > How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
>
> **state** `string` *required*
>
> How far registering and keeping the domain has got.
>
> Available options: `pending`, `ready`, `action_required`, `releasing`

**stats** `object | null` *required*

Traffic on the domain. `null` when the domain isn't on Whop or isn't visible to you.

> [accordion] Properties
>
> **unique\_visitors\_last\_7\_days** `integer` *required*
>
> People who viewed a page on the domain with the Whop pixel in the last 7 days, refreshed hourly.

**status** `string | null` *required*

The most pressing state of the domain's capabilities: `releasing`, then `action_required`, then `pending`, then `ready`. `idle` when it wants nothing.

Available options: `idle`, `pending`, `ready`, `action_required`, `releasing`

**subdomain** `object | null` *required*

A subdomain of a domain this account bought through Whop, whose DNS Whop runs. Whop removes it when the account no longer holds the root domain. `null` for other domains or when not visible to you.

> [accordion] Properties
>
> **state** `string` *required*
>
> Whether Whop has published the subdomain's DNS records in its root domain's zone. It stays `pending` until the root domain is set up.
>
> Available options: `pending`, `ready`, `action_required`, `releasing`

**transfer** `object | null` *required*

Moving the domain out of this account, to another Whop account or another registrar. `null` when not turned on or not visible to you.

> [accordion] Properties
>
> **code** `string | null` *required*
>
> The code that takes the domain, shown while `state` is `ready`. For an
> `internal` move it starts `whop_dt_`. The receiving Whop account enters it as
> `registration.auth_code` when it creates or updates the domain. For an
> `external` move it is the registrar's auth code, which you give to the new
> registrar. Only a single-domain response shows it, and only to a caller who
> can manage the domain. Otherwise it is `null`.
>
> **destination** `string` *required*
>
> Where the domain is moving. `internal` is another Whop account. `external` is another registrar.
>
> Available options: `internal`, `external`
>
> **state** `string` *required*
>
> `ready` while the code waits to be used. For `internal`, `pending` once another account uses it. Whop then removes the domain's email, Apple Pay and pixel from this account. Then it moves the registration and removes the domain from this account. For `external`, `pending` while Whop unlocks the domain at its registrar and turns off everything Whop runs on it. The domain leaves this account when the transfer completes. `releasing` while Whop turns the transfer off.
>
> Available options: `ready`, `pending`, `releasing`

**updated\_at** `string | null` *required*

When the domain was last updated, as an ISO 8601 timestamp.

**verification** `object | null` *required*

Proving you control the domain's DNS with a TXT record. `null` when not wanted or not visible to you.

> [accordion] Properties
>
> **state** `string` *required*
>
> How far proving verification has got.
>
> Available options: `pending`, `ready`, `action_required`, `releasing`
>
> **verification\_expires\_at** `string | null` *required*
>
> When an unproven claim is removed, 48 hours after it was made, as an ISO 8601 timestamp.

**website** `object | null` *required*

Serving a Whop website on the domain. `null` when not wanted.

> [accordion] Properties
>
> **app** `object | null` *required*
>
> The app the domain serves. `null` until you choose one; the domain shows a placeholder page meanwhile.
>
> > [accordion] Properties
> >
> > **id** `string` *required*
> >
> > App ID, prefixed `app_`.
> >
> > **base\_url** `string | null` *required*
> >
> > The production base URL where the app is hosted. `null` if no base URL is configured or the domain isn't yours.
>
> **state** `string` *required*
>
> How far serving the website has got.
>
> Available options: `pending`, `ready`, `action_required`, `releasing`

```json Domain theme={null}
{
	"id": "dom_M6b2X9k4R7t1Wp",
	"domain": "shop.example.com",
	"status": "pending",
	"account_id": "biz_J8n2R5p9T1w4Kx",
	"verification": {
		"state": "pending",
		"verification_expires_at": "2026-09-10T12:00:00.000Z"
	},
	"registration": null,
	"platform": null,
	"subdomain": null,
	"website": {
		"state": "pending",
		"app": {
			"id": "app_P9q3K7m2W8v1Bx",
			"base_url": "https://shop.example.com"
		}
	},
	"apple_pay": null,
	"pixel": null,
	"email": null,
	"transfer": null,
	"owned_by": null,
	"issues": [
		{
			"capability": "verification",
			"code": "verification_required",
			"message": "Publish this TXT record to prove you own the domain. Verification runs automatically.",
			"dns_records": [
				{
					"type": "TXT",
					"name": "_whop.shop.example.com",
					"value": "whop-domain-verification=2a76d0f9f48c3e215b9ed6011a4e7328da5f70936bcaf874c2195088dedd9201"
				}
			],
			"code_block": null
		},
		{
			"capability": "website",
			"code": "dns_required",
			"message": "Point DNS to Whop using these records. Disable other CDN proxies while connecting.",
			"dns_records": [
				{
					"type": "CNAME",
					"name": "shop.example.com",
					"value": "customers.example.net"
				}
			],
			"code_block": null
		}
	],
	"stats": {
		"unique_visitors_last_7_days": 1284
	},
	"metadata": {
		"project": "storefront"
	},
	"created_at": "2026-09-09T12:00:00.000Z",
	"updated_at": "2026-09-09T12:00:00.000Z",
	"availability": null,
	"public_record": null
}
```


## Related topics

- [API Versions](https://docs.whop.com/developer/api/versioning.md)
- [Whop API Overview](https://docs.whop.com/api-reference/overview.md)
- [Migrate from Previous Versions](https://docs.whop.com/developer/api/migrate-from-previous-versions.md)
