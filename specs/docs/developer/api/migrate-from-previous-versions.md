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

# Migrate from Previous Versions

> Move off API v2, public GraphQL, or the payment method domains API. Find the Current API replacement for each route and what behaves differently.

Whop is retiring older APIs and endpoints. Move your integration to the Current API at `https://api.whop.com/api/v1`, with the same API keys.

| If your integration calls | It stops working | Guide |
| - | - | - |
| `https://api.whop.com/api/v2` or `https://api.whop.com/v2` | November 13, 2026 | [API v2](#v2) |
| `https://api.whop.com/public-graphql`, or uses the `@whop/api` package | October 26, 2026 | [Public GraphQL](#public-graphql) |
| `https://api.whop.com/api/v1/payment_method_domains` | After December 23, 2026 | [Payment Method Domains](#payment-method-domains) |

## Switch to the Current API

**1. Keep your API key**

The Current API uses the same account and app API keys. If a request returns `403`, the key is missing the permission that endpoint needs. Add it in **Developer → API keys**, or request it in your app's permissions.

**2. Install the SDK and pin a version**

The official SDKs call the Current API and send the API version they were built against. If you use `@whop/api`, replace it with `@whop/sdk`. Its `withUser` and `withCompany` helpers have no equivalent: pass the user or account ID on each request instead.

```typescript TypeScript theme={null}
import { WhopClient } from "@whop/sdk";

const client = new WhopClient({ token: process.env.WHOP_API_KEY });

const access = await client.users.checkAccess({
  id: "user_xxxxxxxxxxxxx",
  resource_id: "exp_xxxxxxxxxxxxx",
});
```

```python Python theme={null}
import os

from whop_sdk import Whop

client = Whop(token=os.environ["WHOP_API_KEY"])

membership = client.memberships.retrieve("mem_xxxxxxxxxxxxx")
```

```bash cURL theme={null}
curl https://api.whop.com/api/v1/memberships/mem_xxxxxxxxxxxxx \
  -H "Authorization: Bearer $WHOP_API_KEY" \
  -H "Api-Version-Date: 2026-08-21-1"
```

If you call the API without an SDK, send `Api-Version-Date` on every request. [API versions](https://docs.whop.com/developer/api/versioning.md) explains pinning.

**3. Replace each call**

Use the tab for the API you're leaving, then remove the old base URL, endpoints, or package from your code.

> [tab] API v2
>
> ### Find your v2 calls
>
> Every v2 response carries three headers:
>
> | Header | Example | Meaning |
> | - | - | - |
> | `Deprecation` | `@1791417600` | Whop deprecated this endpoint. The value is the deprecation date as Unix seconds. |
> | `Sunset` | `Fri, 13 Nov 2026 00:00:00 GMT` | API v2 stops working on November 13, 2026. |
> | `Link` | `<https://api.whop.com/api/v1/memberships/mem_xxxxxxxxxxxxx>; rel="successor-version", <https://docs.whop.com/developer/api/migrate-from-previous-versions#v2>; rel="deprecation"` | The `successor-version` URL is the Current API request that replaces this one. The `deprecation` URL is this guide. |
>
> Log responses that include `Deprecation` to find every v2 call your integration still makes.
>
> ### Route map
>
> #### Memberships and licenses
>
> | v2 route | Current API | Notes |
> | - | - | - |
> | `GET /memberships` | `GET /memberships` | Cursor pagination. See [Pagination](#pagination). Replace `discord_id=<id>` with `external_account=discord_<id>`. |
> | `GET /memberships/{id}` | `GET /memberships/{id}` | Also accepts a license key in place of the `mem_` ID. |
> | `POST /memberships/{id}` | `PATCH /memberships/{id}` | Send `metadata` to merge keys. Send `{}` to clear it. |
> | `POST /memberships/{id}/cancel` | `POST /memberships/{id}/cancel` | Pass `cancel_at_period_end: true` to keep access until the renewal date. |
> | `POST /memberships/{id}/terminate` | `POST /memberships/{id}/cancel` | Omit `cancel_at_period_end` to revoke access immediately. |
> | `POST /memberships/{id}/add_free_days` | `POST /memberships/{id}/extend` | |
> | `POST /memberships/{id}/validate_license` | `GET /memberships/{id}` and `PATCH /memberships/{id}` | Check the key and bind a machine from your server. See [License keys](#license-keys). |
> | `GET /licenses` | `GET /memberships` | Each membership has a `license_key` when its product includes the Software app. |
>
> #### Members and customers
>
> | v2 route | Current API | Notes |
> | - | - | - |
> | `GET /members` | `GET /members` | |
> | `GET /customers` | `GET /members` | Customers and members are the same resource. |
> | `GET /members/{search}` | `GET /members` | Filter with `user_ids` or `query`. `GET /members/{id}` takes only a `mber_` ID. To find a member by Discord ID, call `GET /memberships?external_account=discord_<id>` and read each membership's `member_id`. |
> | `GET /customers/{search}` | `GET /members` | Same as above. |
>
> #### Payments and checkout
>
> | v2 route | Current API | Notes |
> | - | - | - |
> | `GET /payments` | `GET /payments` | Filters are singular, such as `status` and `membership_id`. |
> | `GET /payments/{id}` | `GET /payments/{id}` | |
> | `POST /payments/{id}/refund` | `POST /payments/{id}/refund` | |
> | `GET /payment_fees?payment_id=` | `GET /payments/{id}/fees` | |
> | `GET /payment_methods` | `GET /payment_methods` | |
> | `POST /checkout_requests` | `POST /payments` | Pass `member_id` (`mber_`) and `payment_method_id`. The response is the payment. |
> | `GET /checkout_requests/{id}` | `GET /payments/{id}` | `POST /payments` returns the payment directly, so you don't need to poll. |
> | `GET /checkout_sessions` | `GET /checkout_configurations` | Checkout sessions are now called checkout configurations. |
> | `GET /checkout_sessions/{id}` | `GET /checkout_configurations/{id}` | |
> | `POST /checkout_sessions` | `POST /checkout_configurations` | The response includes `purchase_url`. |
> | `DELETE /checkout_sessions/{id}` | `DELETE /checkout_configurations/{id}` | |
>
> #### Products and plans
>
> Plans are now called variants. Variant IDs keep the `plan_` prefix, so stored IDs still work.
>
> | v2 route | Current API | Notes |
> | - | - | - |
> | `GET /products`, `GET /access_passes` | `GET /products` | Access passes are now called products. |
> | `GET /products/{id}`, `GET /access_passes/{id}` | `GET /products/{id}` | |
> | `POST /products/{id}`, `POST /access_passes/{id}` | `PATCH /products/{id}` | |
> | `GET /plans` | `GET /variants` | |
> | `GET /plans/{id}` | `GET /variants/{id}` | |
> | `POST /plans` | `POST /variants` | |
> | `POST /plans/{id}` | `PATCH /variants/{id}` | |
> | `POST /plans/{id}/create_quick_link` | `POST /variants` | Create a variant with `visibility: "quick_link"`, `stock`, and `short_link`. The Current API doesn't copy an existing plan, so send its price fields too. |
> | `GET /promo_codes` | `GET /promo_codes` | |
> | `GET /promo_codes/{id}` | `GET /promo_codes/{id}` | |
> | `POST /promo_codes` | `POST /promo_codes` | |
> | `POST /promo_codes/{id}` | `POST /promo_codes/{id}/activate`, `POST /promo_codes/{id}/deactivate` | Covers `status` only. Create a new promo code to change its variants, stock, or metadata. Then deactivate the old one. |
> | `DELETE /promo_codes/{id}` | `DELETE /promo_codes/{id}` | |
>
> #### Account, experiences, reviews, and webhooks
>
> | v2 route | Current API | Notes |
> | - | - | - |
> | `GET /company` | `GET /accounts/me` | |
> | `GET /experiences` | `GET /experiences` | |
> | `GET /experiences/{id}` | `GET /experiences/{id}` | |
> | `POST /experiences/{id}` | `PATCH /experiences/{id}` | |
> | `DELETE /experiences/{id}` | `DELETE /experiences/{id}` | |
> | `GET /reviews` | `GET /reviews` | |
> | `GET /reviews/{id}` | `GET /reviews/{id}` | |
> | `GET /webhooks` | `GET /webhooks` | |
> | `GET /webhooks/{id}` | `GET /webhooks/{id}` | |
> | `POST /webhooks` | `POST /webhooks` | See [Webhooks](#webhooks). |
> | `POST /webhooks/{id}` | `PATCH /webhooks/{id}` | |
> | `DELETE /webhooks/{id}` | `DELETE /webhooks/{id}` | |
>
> ### What changed from v2
>
> #### Partial updates
>
> v2 updated a resource with `POST /resource/{id}`. The Current API uses `PATCH /resource/{id}` and changes only the fields you send.
>
> #### License keys
>
> v2 `validate_license` checked a key and bound it to a machine in one call. In the Current API, your server retrieves the membership by license key, checks its `status`, and writes a hardware ID to `metadata` the first time someone uses the key. The [SaaS guide](https://docs.whop.com/supported-business-models/saas.md#license-key-integration) has the full example.
>
> Keep these calls on your server. Your account API key can change every membership in your business, so never ship it inside downloadable software.
>
> #### Webhooks in the v2 format
>
> Webhooks in the v2 and v5 formats stop receiving events at the end of December 6, 2026, Pacific time. To receive the current format, create a new webhook from the dashboard or with `POST /webhooks`, move your handler to it, then delete the v2 webhook. [Webhooks](https://docs.whop.com/developer/guides/webhooks.md) covers signature checks and event types.

> [tab] Public GraphQL
>
> ### Find your GraphQL calls
>
> If your app imports `WhopServerSdk` or `makeWhopServerSdk` from `@whop/api`, it calls the GraphQL API. Every method on that client, such as `whopSdk.access.checkIfUserHasAccessToExperience`, sends a GraphQL operation with the same name.
>
> Every GraphQL response also carries these headers:
>
> | Header | Example | Meaning |
> | - | - | - |
> | `Deprecation` | `@1789948800` | Whop deprecated this API. The value is the deprecation date as Unix seconds. |
> | `Sunset` | `Mon, 26 Oct 2026 00:00:00 GMT` | The GraphQL API stops working on October 26, 2026. |
> | `Link` | `<https://docs.whop.com/developer/api/migrate-from-previous-versions#public-graphql>; rel="deprecation"` | This guide. |
>
> Search your code for `@whop/api`, or log responses that include `Deprecation`, to find every call your app still makes.
>
> > [!NOTE]
> >
> > `verifyUserToken` from `@whop/api` checks the `x-whop-user-token` header locally and never calls the GraphQL API, so it keeps working after the sunset. [Authentication](https://docs.whop.com/developer/guides/authentication.md) covers verifying the token in other languages.
>
> ### Operation map
>
> #### Access checks
>
> Most apps call one of these on every page load. All three map to one endpoint that takes the user ID and the experience, product, or account ID.
>
> | GraphQL operation | Current API | Notes |
> | - | - | - |
> | `checkIfUserHasAccessToExperience` | `GET /users/{id}/access/{resource_id}` | Pass an `exp_` ID as `resource_id`. |
> | `checkIfUserHasAccessToAccessPass` | `GET /users/{id}/access/{resource_id}` | Pass a `prod_` ID. Access passes are now called products. |
> | `checkIfUserHasAccessToCompany` | `GET /users/{id}/access/{resource_id}` | Pass a `biz_` ID. |
>
> The response has `has_access` and `access_level`, which is `customer`, `admin`, or `no_access`, the same values as `hasAccess` and `accessLevel` in GraphQL.
>
> #### Chat and messages
>
> | GraphQL operation | Current API | Notes |
> | - | - | - |
> | `listMessagesFromChat` | `GET /messages` | Pass the chat experience ID as `channel_id`. See [Stop polling chat](#stop-polling-chat). |
> | `sendMessageToChat` | `POST /messages` | Pass the chat experience ID as `channel_id` and the text as `content`. |
> | `sendDirectMessageToUser` | `POST /channels`, then `POST /messages` | Create the conversation with `kind` set to `private` and the recipient in `participant_user_ids`, then send to its ID as `channel_id`. |
> | `listDirectMessageConversations` | `GET /channels` | Pass `kind=private`. |
>
> #### Users
>
> | GraphQL operation | Current API | Notes |
> | - | - | - |
> | `getUser` | `GET /users/{id}` | |
> | `getCurrentUser` | `GET /users/me` | Returns the user the token belongs to. With an app API key, read the user ID from `verifyUserToken` and call `GET /users/{id}`. |
> | `listUsersForExperience` | `GET /members` | Filter with `product_ids`. |
>
> #### Experiences and products
>
> | GraphQL operation | Current API | Notes |
> | - | - | - |
> | `getExperience` | `GET /experiences/{id}` | |
> | `listExperiences` | `GET /experiences` | Filter with `product_id` or `app_id`. |
> | `getAccessPass` | `GET /products/{id}` | |
> | `listAccessPasses` | `GET /products` | |
> | `listAccessPassesForExperience` | `GET /products` | |
> | `createAccessPass` | `POST /products` | |
> | `updateAccessPass` | `PATCH /products/{id}` | |
> | `deleteAccessPass` | `DELETE /products/{id}` | |
> | `listPlans` | `GET /variants` | Plans are now called variants. Variant IDs keep the `plan_` prefix. |
>
> #### Your account
>
> | GraphQL operation | Current API | Notes |
> | - | - | - |
> | `getCompany` | `GET /accounts/me` | Companies are now called accounts. |
> | `listMemberships` | `GET /memberships` | |
> | `listMembers`, `getMember` | `GET /members`, `GET /members/{id}` | |
> | `listAuthorizedUsers` | `GET /authorized_users` | |
> | `listWaitlistEntries` | `GET /waitlist_entries` | |
> | `listReceiptsForCompany` | `GET /payments` | |
>
> #### Payments
>
> | GraphQL operation | Current API | Notes |
> | - | - | - |
> | `chargeUser` | `POST /payments` | |
> | `createCheckoutSession` | `POST /checkout_configurations` | Checkout sessions are now called checkout configurations. The response includes `purchase_url`. |
> | `payUser` | `POST /transfers` | |
> | `listInvoices`, `getInvoice`, `createInvoice` | `GET /invoices`, `GET /invoices/{id}`, `POST /invoices` | |
> | `listPromoCodes`, `createPromoCode` | `GET /promo_codes`, `POST /promo_codes` | |
> | `listAffiliates`, `createAffiliate` | `GET /affiliates`, `POST /affiliates` | |
>
> #### Files and notifications
>
> | GraphQL operation | Current API | Notes |
> | - | - | - |
> | `uploadMedia`, `processAttachment` | `POST /files`, then `POST /files/{id}/complete` | |
> | `getAttachment` | `GET /files/{id}` | |
> | `sendPushNotification` | `POST /notifications` | Target an `experience_id` or a list of `user_ids`. |
>
> #### Courses and forums
>
> | GraphQL operation | Current API | Notes |
> | - | - | - |
> | `listCoursesForExperience` | `GET /courses` | Filter with `experience_id`. |
> | `listCoursesForCompany` | `GET /courses` | Filter with `account_id`. |
> | `getCourse` | `GET /courses/{id}` | |
> | `getLesson` | `GET /course_lessons/{id}` | |
> | `getUserLessonInteractions` | `GET /course_lesson_interactions` | Filter with `user_id`, `lesson_id`, or `course_id`. |
> | `markLessonAsCompleted` | `POST /course_lessons/{lesson_id}/mark_as_completed` | |
> | `listForumPostsFromForum` | `GET /forum_posts` | Filter with `experience_id`. |
> | `createForumPost` | `POST /forum_posts` | |
>
> #### Webhook operations
>
> | GraphQL operation | Current API | Notes |
> | - | - | - |
> | `listWebhooks`, `getWebhook` | `GET /webhooks`, `GET /webhooks/{id}` | |
> | `createWebhook` | `POST /webhooks` | |
> | `updateWebhook` | `PATCH /webhooks/{id}` | |
> | `deleteWebhook` | `DELETE /webhooks/{id}` | |
> | `testWebhook` | `POST /webhooks/{id}/test` | |
>
> If an operation your app uses isn't listed, search the [API reference](https://docs.whop.com/api-reference.md) for the resource, or contact support with the operation name.
>
> ### Behavior changes
>
> #### Stop polling chat
>
> Apps that react to chat messages often called `listMessagesFromChat` on a timer. Subscribe to the `chat.message.created` webhook instead: Whop sends each new message to your server as it's posted, so you don't need to poll. Use `GET /messages` only to load history.
>
> #### Field names
>
> Current API fields use `snake_case` (`has_access`, `access_level`) where GraphQL used `camelCase` (`hasAccess`, `accessLevel`). Resource IDs keep their prefixes, so IDs you stored still work.

> [tab] Payment Method Domains
>
> The payment method domains API (`/api/v1/payment_method_domains`) is deprecated and stops working after December 23, 2026. The [domains API](https://docs.whop.com/api-reference/domains/domain) replaces it: every domain has an `apple_pay` capability that you add, check, and remove on the domain itself. If your integration registers checkout domains for Apple Pay and Google Pay, switch it to the domains API.
>
> > [!NOTE]
> >
> > Your domains have already moved. Each payment method domain is a domain with the same ID, so you don't need to copy anything. The IDs you already stored work with `/domains/{id}`.
>
> ### Update your API key permissions
>
> The payment method domains API needed `company:manage_checkout`. The domains API needs `domain:read` to list and retrieve domains, and `domain:manage` to add, check, update, and remove them. Add both in **Developer → API keys**, or request them in your app's permissions. Without them, listing, adding, checking, and updating domains return `403`. Retrieving a domain still returns it, but with `apple_pay: null`, which looks the same as a domain without Apple Pay.
>
> ### Find your payment method domains calls
>
> Every response from `/payment_method_domains` carries a `Deprecation` header, a `Sunset` header with the shutdown date, and a `Link` header with `rel="successor-version"`. The `Link` URL is the domains API request that replaces it. Log responses with `Deprecation` to find every call your integration still makes.
>
> ### Request map
>
> | Payment method domains | Domains API | Notes |
> | - | - | - |
> | `GET /payment_method_domains` | [`GET /domains?apple_pay=any&order=created_at`](https://docs.whop.com/api-reference/domains/list-domains) | Filter by hostname with `domain` instead of `hostname`. `account_id` works the same way. See [List filters](#list-filters) for `status` and the date filters. |
> | `GET /payment_method_domains/{id}` | [`GET /domains/{id}`](https://docs.whop.com/api-reference/domains/retrieve-domain) | Same ID. |
> | `POST /payment_method_domains` | [`POST /domains`](https://docs.whop.com/api-reference/domains/create-domain) with `apple_pay: {}` | Send the hostname as `domain`, plus `account_id` for a subaccount. If the account already has the domain, call [`PATCH /domains/{id}`](https://docs.whop.com/api-reference/domains/update-domain) with `apple_pay: {}` instead. |
> | `POST /payment_method_domains/{id}/verify` | [`POST /domains/{id}/check`](https://docs.whop.com/api-reference/domains/check-domain) | |
> | `DELETE /payment_method_domains/{id}` | [`PATCH /domains/{id}`](https://docs.whop.com/api-reference/domains/update-domain) with `apple_pay: null` | Removes Apple Pay and keeps the domain. |
>
> > [!WARNING]
> >
> > Don't replace `DELETE /payment_method_domains/{id}` with `DELETE /domains/{id}`. Deleting a domain removes the whole domain, including any website on it, not only Apple Pay.
>
> ### Examples
>
> #### Add Apple Pay to a domain
>
> Host the [Apple Pay verification file](https://docs.whop.com/payments/apple-pay.md#self-hosted-verification) on the domain first, then add the domain with an empty `apple_pay` object.
>
> ```typescript TypeScript theme={null}
> import { WhopClient } from "@whop/sdk";
>
> const client = new WhopClient({ token: process.env.WHOP_API_KEY });
>
> const domain = await client.domains.create({
>   domain: "checkout.example.com",
>   account_id: "biz_xxxxxxxxxxxxx",
>   apple_pay: {},
> });
>
> console.log(domain.apple_pay?.state); // "pending" while Whop registers the domain with Apple
> ```
>
> ```python Python theme={null}
> import os
>
> from whop_sdk import Whop
>
> client = Whop(token=os.environ["WHOP_API_KEY"])
>
> domain = client.domains.create(
>     domain="checkout.example.com",
>     account_id="biz_xxxxxxxxxxxxx",
>     apple_pay={},
> )
>
> print(domain.apple_pay.state)  # "pending" while Whop registers the domain with Apple
> ```
>
> If the call returns `409` because the account already has `checkout.example.com`, find it with `GET /domains?domain=checkout.example.com` and add Apple Pay with `PATCH /domains/{id}` and `apple_pay: {}`.
>
> #### List your Apple Pay domains
>
> ```typescript TypeScript theme={null}
> for await (const domain of client.domains.list({ apple_pay: "any" })) {
>   console.log(domain.domain, domain.apple_pay?.state);
> }
> ```
>
> ```python Python theme={null}
> for domain in client.domains.list(apple_pay="any"):
>     print(domain.domain, domain.apple_pay.state)
> ```
>
> Without `account_id`, the list covers your account and its subaccounts, like `GET /payment_method_domains` did.
>
> #### Check a domain again
>
> After you fix the verification file, register the domain with Apple again:
>
> ```typescript TypeScript theme={null}
> await client.domains.check({ id: "dom_xxxxxxxxxxxxx" });
> ```
>
> ```python Python theme={null}
> client.domains.check(id="dom_xxxxxxxxxxxxx")
> ```
>
> #### Remove Apple Pay from a domain
>
> ```typescript TypeScript theme={null}
> await client.domains.update({ id: "dom_xxxxxxxxxxxxx", apple_pay: null });
> ```
>
> ```bash cURL theme={null}
> curl -X PATCH https://api.whop.com/api/v1/domains/dom_xxxxxxxxxxxxx \
>   -H "Authorization: Bearer $WHOP_API_KEY" \
>   -H "Content-Type: application/json" \
>   -d '{"apple_pay": null}'
> ```
>
> ### What changed from payment method domains
>
> #### List filters
>
> `GET /domains` filters Apple Pay with `apple_pay`, which takes one state at a time.
>
> | Payment method domains | Domains API |
> | - | - |
> | `status=verified` | `apple_pay=ready` |
> | `status=pending` | Two requests, `apple_pay=pending` and `apple_pay=action_required`. Or list with `apple_pay=any` and keep the domains whose `apple_pay.state` is `pending` or `action_required`. |
> | `provider=apple` | Drop it. Every Apple Pay domain is Apple's. |
> | `hostname` | `domain` |
> | `created_before`, `created_after` | No equivalent. List with `order=created_at` and stop paging once `created_at` passes your cutoff. |
> | `order=created_at` (the default) | `order=created_at`. Without it, the domains API sorts by `unique_visitors_last_7_days`. |
>
> > [!WARNING]
> >
> > Don't carry `status` over. `GET /domains` also has a `status` filter, but it filters the domain's own status, not Apple Pay, so `status=pending` returns different domains without an error.
>
> #### Status is `apple_pay.state`
>
> A payment method domain had `status` and `provider`. A domain has `apple_pay.state` instead, and `apple_pay` is `null` on a domain without Apple Pay.
>
> | Payment method domain `status` | Domain `apple_pay.state` | Meaning |
> | - | - | - |
> | `verified` | `ready` | Apple verified the domain. Apple Pay and Google Pay show on its checkouts. |
> | `pending` | `pending` | Whop is registering the domain with Apple. |
> | `pending` | `action_required` | Apple couldn't fetch the verification file. The domain's `issues` say how to fix it. Fix it, then check the domain. |
> | — | `releasing` | Whop is removing the domain from Apple after you removed Apple Pay. |
>
> `provider` is gone. Apple is the only provider, and a domain with Apple Pay ready also clears Google Pay.
>
> #### Registration runs in the background
>
> Creating a domain or checking it returns right away with `apple_pay.state: "pending"`. Retrieve the domain until `apple_pay.state` is `ready` or `action_required`.
>
> #### Domain field names
>
> | Payment method domain | Domain |
> | - | - |
> | `hostname` | `domain` |
> | `account_id` | `account_id` |
> | `status` | `apple_pay.state` |
> | `created_at`, `updated_at` | `created_at`, `updated_at` |
>
> A domain also has fields the payment method domain didn't, such as `issues`, `website`, and `metadata`. The [domain object](https://docs.whop.com/api-reference/domains/domain) lists them all.

## Pagination

v2 paged with `page` and `per` and returned `pagination.current_page` and `pagination.total_page`. Current API lists return a `data` array and a `page_info` object. Pass `first` to set the page size and `after` with the previous `page_info.end_cursor` to get the next page. Stop when `page_info.has_next_page` is `false`. The SDKs page for you.

```typescript TypeScript theme={null}
for await (const membership of client.memberships.list({ product_id: "prod_xxxxxxxxxxxxx" })) {
  console.log(membership.id);
}
```

```python Python theme={null}
for membership in client.memberships.list(product_id="prod_xxxxxxxxxxxxx"):
    print(membership.id)
```

## Next steps

> [card] [API versions](https://docs.whop.com/developer/api/versioning.md)
>
> Pin a version so later changes never break your integration.

> [card] [Webhooks](https://docs.whop.com/developer/guides/webhooks.md)
>
> Verify signatures, handle the current event format, and receive chat messages instead of polling.

> [card] [Manage memberships](https://docs.whop.com/developer/guides/memberships.md)
>
> Cancel, pause, extend, and update memberships.

> [card] [App views](https://docs.whop.com/developer/guides/app-views.md)
>
> Check member access in experience and dashboard views.

> [card] [Enable Apple Pay and Google Pay](https://docs.whop.com/payments/apple-pay.md)
>
> Host the verification file and register your domain.

> [card] [Domains API](https://docs.whop.com/api-reference/domains/domain)
>
> Every field and request on a domain.


## Related topics

- [Whop API Overview](https://docs.whop.com/api-reference/overview.md)
- [API Versions](https://docs.whop.com/developer/api/versioning.md)
- [Migrate from Legacy Invoices](https://docs.whop.com/developer/guides/migrate-from-legacy-invoices.md)
