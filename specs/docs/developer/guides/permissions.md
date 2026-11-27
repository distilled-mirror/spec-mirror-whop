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

# Request App Permissions

> Configure the permissions your app needs and request approval from the creators who install it.

Apps use permissions to access account data and act on behalf of users. Every API call requires the matching permission. You declare the ones you need in your app's settings, creators approve them at install, and your SDK calls succeed only after creators grant those permissions.

> [!TIP]
>
> Find the required permissions for any endpoint in the API reference. Each operation page lists them under "Required permissions."

## Required and optional permissions

You can mark each permission as required or optional. The choice changes the install experience for creators.

| Type | Creator can disable at install | Use when |
| - | - | - |
| **Required** | No | The app can't function without this scope |
| **Optional** | Yes | The scope powers a side feature, not the core flow (e.g. analytics opt-in) |

Optional permissions are good for opt-in functionality. If a creator declines an optional permission, your app should still work, just without that feature.

## Set up permissions

> [!WARNING]
>
> The permissions flow is required even for testing on your own account. Same flow on install or self-test, so you catch missing scopes early.

**1. Open your app's permissions tab**

1. Go to the [Developer dashboard](https://whop.com/dashboard/developer).
2. Select or create an app.
3. Select the **Permissions** tab.

![Permissions Settings](https://mintcdn.com/whop/CTin6M1qeROeLXJs/images/app-permissions-settings.png?fit=max\&auto=format\&n=CTin6M1qeROeLXJs\&q=85\&s=514a98987e714d5a2542b3d7665020f7)

**2. Add the permissions you need**

Select **Add permissions**, select what your app needs, and confirm.

Cross-reference the API endpoints you plan to call and add every permission they require.

**3. Configure each permission**

For each one:

1. Write a short justification explaining why the app needs it. Creators see this at install, and each permission needs its own justification to meet the [App Store listing requirements](https://docs.whop.com/developer/apps/listing-requirements.md).
2. Mark it **required** or **optional**.

![Permissions Justification](https://mintcdn.com/whop/CTin6M1qeROeLXJs/images/app-permissions-settings-justification.png?fit=max\&auto=format\&n=CTin6M1qeROeLXJs\&q=85\&s=93ec46a13be088ad236a7fe69f6f6ad2)

**4. Save**

![Save Permissions Settings](https://mintcdn.com/whop/CTin6M1qeROeLXJs/images/app-permissions-settings-save.png?fit=max\&auto=format\&n=CTin6M1qeROeLXJs\&q=85\&s=683b42f4dcfed0eeee535d82dcce3e5f)

**5. Install on a test account**

Visit your direct install link: `https://whop.com/apps/app_xxxxxxxxx/install`.

Pick an account, review the permission prompt, and approve.

![Permissions Prompt](https://mintcdn.com/whop/CTin6M1qeROeLXJs/images/app-permissions-oauth.png?fit=max\&auto=format\&n=CTin6M1qeROeLXJs\&q=85\&s=4b427b52f269ee6b92fcf7434069ad43)

## Update permissions later

Permissions can change as the app evolves. When you add a new one:

* Existing installs see a **Re-approve** button next to your app.
* API calls that need the new permission fail until each creator re-approves.

> [!NOTE]
>
> When you add a permission, re-approve on your own test account too. New scopes don't carry over until you accept them in [Authorized apps](https://whop.com/dashboard/settings/authorized-apps).

Creators can manage granted permissions any time at `Dashboard → Settings → Authorized apps`.

<video controls src="https://mintcdn.com/whop/_aQsA39rzB5Zq8ZK/how-to-videos/configure-app-permissions.mp4?fit=max&auto=format&n=_aQsA39rzB5Zq8ZK&q=85&s=dff99f10fb43e8914fdcd1b0eb4bd044" />

## Frequently asked questions

> [accordion] How many permissions can an app request?
>
> Up to 100 per app.

> [accordion] Where are the required permissions for an endpoint listed?
>
> Each endpoint in the [API reference](https://docs.whop.com/api-reference/payments/list-payments.md) lists its required permissions inline.
>
> ![SDK Reference Permissions](https://mintcdn.com/whop/purcney6SuEUQzS5/images/sdk-reference-required-permissions.png?fit=max\&auto=format\&n=purcney6SuEUQzS5\&q=85\&s=141d5a09a00942f216c98d233824dcf9)

> [accordion] Can an app request additional permissions afterward?
>
> Yes. You can request additional permissions and the creator will be asked to re-approve them.
>
> > [!NOTE]
> >
> > Until the permissions are re-approved, API requests requiring the **newly requested** permissions will fail. Make sure your code handles these errors and provides a useful fallback.
>
> > [!WARNING]
> >
> > When developing your app, make sure you re-approve the permissions yourself in your [Authorized apps](https://whop.com/dashboard/settings/authorized-apps) settings.
> >
> > See [Configure your permissions](https://docs.whop.com/developer/guides/permissions.md#update-permissions-later) for more information.

## Next steps

> [card] [Authentication](https://docs.whop.com/developer/guides/authentication.md)
>
> Verify the user behind a request and check their access level.

> [card] [Listen to webhooks](https://docs.whop.com/developer/guides/webhooks.md)
>
> Receive payment, membership, and entry events. Webhooks need their own `webhook_receive:*` scopes.

> [card] [Track installations](https://docs.whop.com/developer/guides/app-installations.md)
>
> Know when an account installs or removes your app, and link it to your own records.

> [card] [Build app views](https://docs.whop.com/developer/guides/app-views.md)
>
> Set up dashboard views, experiences, and discover listings.

> [card] [Run a local development proxy](https://docs.whop.com/developer/guides/dev-proxy.md)
>
> Match the production iframe + cookie setup on localhost.


## Related topics

- [Update App Permissions](https://docs.whop.com/api-reference/apps/update-app-permissions.md)
- [Update requested permissions](https://docs.whop.com/api-reference/legacy/apps/update-requested-permissions.md)
- [App Installations](https://docs.whop.com/developer/guides/app-installations.md)
