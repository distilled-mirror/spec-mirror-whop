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

# Notifications API

> A Notification is a message delivered to a user, such as a new post, a payment, or a mention.

Each one comes from an experience the user belongs to or a team they are on, and falls under a topic, like new sales, that users set preferences on.

Use the Notifications API to list a user's feed, read and clear unread badges, send notifications from your app to an experience's users or an account's team, and list the topic catalog that notification preferences reference.

## Endpoints

| Endpoint | Request |
| - | - |
| [List Notifications](https://docs.whop.com/api-reference/notifications/list-notifications.md) | GET `/notifications` |
| [Retrieve Notification](https://docs.whop.com/api-reference/notifications/retrieve-notification.md) | GET `/notifications/{id}` |
| [List Notification Badges](https://docs.whop.com/api-reference/notifications/list-notification-badges.md) | GET `/notifications/badges` |
| [List Notification Topics](https://docs.whop.com/api-reference/notifications/list-notification-topics.md) | GET `/notifications/topics` |
| [Send Notification](https://docs.whop.com/api-reference/notifications/send-notification.md) | POST `/notifications` |
| [Mark Notifications Read](https://docs.whop.com/api-reference/notifications/mark-notifications-read.md) | POST `/notifications/mark_read` |

## Attributes

**id** `string` *required*

Notification ID. Feed rows carry a composite id that doubles as the list
cursor.

**account** `object | null` *required*

Account the notification belongs to. `null` when the notification is not associated with an account.

> [accordion] Properties
>
> **id** `string` *required*
>
> Account ID, prefixed `biz_`.
>
> **logo\_url** `string | null` *required*
>
> Account logo image URL. `null` when the account has not uploaded one.
>
> **route** `string | null` *required*
>
> URL slug of the account's store page on whop.com.
>
> **title** `string` *required*
>
> Account display name.

**attachment\_url** `string` *required*

Image displayed alongside the notification — the sender's avatar, or the
default notification image.

**content** `string` *required*

The body text of the notification displayed to the user.

**created\_at** `string` *required*

When the notification was created, as an ISO 8601 timestamp.

**experience** `object | null` *required*

Experience the notification is related to. `null` when not tied to a specific experience.

> [accordion] Properties
>
> **id** `string` *required*
>
> Experience ID, prefixed `exp_`.
>
> **app** `object | null` *required*
>
> App the experience belongs to.
>
> > [accordion] Properties
> >
> > **id** `string` *required*
> >
> > App ID, prefixed `app_`.
> >
> > **icon\_url** `string` *required*
> >
> > Icon image URL. Always present — the default app icon when none is uploaded.
>
> **name** `string | null` *required*
>
> Display name of the experience.

**iframe\_link** `string | null` *required*

The same destination on the app's own domain, which the Whop web and mobile
clients embed instead of navigating to it. Only relevant if you render Whop
apps yourself. `null` when the notification carries its own `link`.

**link** `string | null` *required*

The whop.com page the notification opens, as a normal top-level navigation.
This is the link to use unless you host Whop apps yourself. `null` when the
notification has no click-through destination.

**mentions\_me** `boolean` *required*

Whether the authenticated user was directly mentioned in this notification.

**rest\_path** `string | null` *required*

Additional path information appended to the notification's deep link, if any.

**sender** `object | null` *required*

User who triggered the notification. `null` when it was system-generated.

> [accordion] Properties
>
> **id** `string` *required*
>
> User ID, prefixed `user_`.
>
> **name** `string | null` *required*
>
> Display name.
>
> **username** `string` *required*
>
> Public username.

**subject** `string` *required*

The title line of the notification displayed to the user.

**topic** `object | null` *required*

Topic category the notification belongs to, used for grouping and preference management. `null` when uncategorized.

> [accordion] Properties
>
> **id** `string` *required*
>
> Notification topic ID, prefixed `topic_`.
>
> **default\_preference\_value** `boolean` *required*
>
> Whether notifications for this topic are enabled by default when the user has
> not set a preference.
>
> **is\_mention** `boolean` *required*
>
> Whether this topic exclusively handles mention-based notifications.
>
> **topic\_type** `string` *required*
>
> Scope of the topic: whether it applies to an account, a user, or an account's team.
>
> Available options: `account`, `user`, `account_team`

```json Notification theme={null}
{
	"id": "ntfr_xxxxxxxxxxxxxx:xxxxxxxxxxxxx",
	"account": {
		"id": "biz_xxxxxxxxxxxxxx",
		"logo_url": "https://whop-assets-example.s3.amazonaws.com/uploads/image/2026-01-01/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
		"route": "shine-time-auto-detailing",
		"title": "Shine Time Auto Detailing"
	},
	"content": "Tuesday 9:00 AM, Bay 2",
	"created_at": "2026-09-15T14:30:00.000Z",
	"experience": {
		"id": "exp_xxxxxxxxxxxxxx",
		"app": {
			"id": "app_xxxxxxxxxxxxxx",
			"icon_url": "https://pub-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx.r2.dev/custom-perk-icon.svg"
		},
		"name": "Bookings"
	},
	"iframe_link": "https://whopappdomain0000001.apps.whop.com/experiences/exp_xxxxxxxxxxxxxx/bookings/upcoming",
	"link": "https://whop.com/joined/shine-time-auto-detailing/bookings-xxxxxxxxxxxxxx/app/bookings/upcoming/",
	"mentions_me": false,
	"rest_path": "/bookings/upcoming",
	"sender": {
		"id": "user_xxxxxxxxxxxxxx",
		"name": "Tanya Cole",
		"username": "tanyacole"
	},
	"subject": "Your ceramic coating is booked",
	"topic": {
		"id": "topic_xxxxxxxxxxxxxx",
		"default_preference_value": true,
		"is_mention": false,
		"topic_type": "user"
	},
	"attachment_url": "https://ui-avatars.com/api/?name=Tanya+Cole"
}
```


## Related topics

- [Create notification](https://docs.whop.com/api-reference/legacy/notifications/create-notification.md)
- [List Notifications](https://docs.whop.com/api-reference/notifications/list-notifications.md)
- [Send Notification](https://docs.whop.com/api-reference/notifications/send-notification.md)
