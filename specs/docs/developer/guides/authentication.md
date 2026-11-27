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

# Authenticate Users in Whop Apps

> Verify the current user and check their access level inside your Whop app.

Whop apps render inside an iframe on `whop.com`. To figure out *who* is making a request to your app, Whop passes a short-lived JSON Web Token (JWT) in the `x-whop-user-token` header on every same-origin request. You verify this token with the SDK to get a user ID, then check what that user can see.

## Verify the iframe user token

> [!WARNING]
>
> `verifyUserToken` isn't available in the current `@whop/sdk`, so the TypeScript snippets on this page are kept for reference and won't run as written. Python is different: `whop-sdk` ships the helper as `whop_sdk.lib.verify_user_token`, which needs PyJWT installed alongside it (`pip install "pyjwt[crypto]"`). The `whop_sdk` gem ships it too, but as a module function under a different root namespace from the client: `WhopSDK::Helpers::VerifyUserToken.verify_user_token!`, where the client is `Whop_sdk::Client`. It's not a method on the client, and the gem already depends on `jwt`.

Call `verifyUserToken` on each request. It reads the `x-whop-user-token` header, validates the JWT signature and expiry, and returns the user ID. Invalid tokens throw by default. TypeScript and Python also offer a non-throwing variant:

| Language | Non-throwing variant |
| - | - |
| TypeScript | Pass `{ dontThrow: true }` as the second argument |
| Python | Call `try_verify_user_token`, which returns `None` instead of raising |
| Ruby | None. `verify_user_token!` is the only entry point, so rescue `StandardError`. |

```typescript JS/TS + NextJS theme={null}
import { headers } from "next/headers";
import { whopsdk } from "@/lib/whop-sdk";

export default async function MyServerRenderedPage() {
	const { userId } = await whopsdk.verifyUserToken(await headers());

    // ... the rest of your component / api route etc...
}
```

```javascript JS/TS + ExpressJS theme={null}
import express from "express";
import { whopsdk } from "./lib/whop-sdk";

const app = express();

app.get("/my-route", async (req, res) => {
    const { userId } = await whopsdk.verifyUserToken(req.headers);

    // ... the rest of your route handler
});
```

```python Python + FastAPI theme={null}
from fastapi import FastAPI, Request
from whop_sdk.lib.verify_user_token import verify_user_token

app = FastAPI()

@app.get("/my-route")
async def my_route(request: Request):
    # Synchronous, and app_id is required: it is checked against the token's audience.
    result = verify_user_token(request.headers, app_id="app_xxxxxxxxxxxxx")
    user_id = result.user_id

    # ... the rest of your route handler
```

```ruby Ruby on Rails theme={null}
class MyController < ApplicationController
  def my_action
    # Give the helper a token string or a plain Hash, not an ActionDispatch headers object.
    # app_id is optional in Ruby. Pass it to check the token's audience.
    auth_result = WhopSDK::Helpers::VerifyUserToken.verify_user_token!(
      request.headers["x-whop-user-token"],
      app_id: "app_xxxxxxxxxxxxx",
    )
    user_id = auth_result.user_id

    # ... the rest of your controller action
  end
end
```

The token is only attached to requests hitting the `window.location.origin` of your iframe. Examples:

* Relative page navigation: `<a href="/sub_page">...</a>`
* Fetches without a domain: `await fetch("/api/quizzes")`

These resolve to your `App.base_url` domain.

> [accordion] The API is on a different domain
>
> If your frontend runs on `example.com` but your API runs on `api.example.com`, reverse-proxy API requests through `example.com/api` so they carry the `x-whop-user-token` header.
>
> * **Cloudflare**: create an ["origin rule"](https://developers.cloudflare.com/rules/origin-rules/features/#dns-record) from `/api` on `example.com` to rewrite to `api.example.com/api`.
>
> * **Next.js**: add a [rewrite](https://nextjs.org/docs/app/api-reference/config/next-config-js/rewrites) in `next.config.mjs`.
>
> * **nginx / Caddy**: use the rewrite primitive in your server config.
>
> > [!NOTE]
> >
> > This setup is required because of strict browser cross-origin cookie policies.

## Local setup

Whop ships a local reverse proxy that matches the production iframe and cookie behavior, so the code you write works identically on `localhost` and in production. See the [development proxy guide](https://docs.whop.com/developer/guides/dev-proxy.md) for setup.

## Check access

Now that you know who's making the request, check what they're allowed to see. Use the [`checkAccess`](https://docs.whop.com/api-reference/users/check-user-access) method to verify access to an Experience, Account, or Product.

```typescript JS/TS theme={null}
import { whopsdk } from "@/lib/whop-sdk";

const response = await whopsdk.users.checkAccess({
    id: "user_xxxxxxxxxxxxx",
    resource_id: "resource_id",
});

// Response:
// {
//   "has_access": true,
//   "access_level": "customer"
// }
```

```python Python theme={null}
response = whopsdk.users.check_access(
    resource_id="resource_id",
    id="user_xxxxxxxxxxxxx",
)

# Response:
# {
#   "has_access": True,
#   "access_level": "customer"
# }
```

```ruby Ruby theme={null}
response = whopsdk.users.check_access(
  resource_id: "resource_id",
  id: "user_xxxxxxxxxxxxx",
)

# Response:
# {
#   "has_access": true,
#   "access_level": "customer"
# }
```

```rust Rust theme={null}
let response = client
    .users
    .check_access(&"user_xxxxxxxxxxxxx".to_string(), &"resource_id".to_string(), None)
    .await?;

println!("{} {}", response.has_access, response.access_level);
// has_access => true, access_level => "customer"
```

```go Go theme={null}
response, err := client.Users.CheckAccess(context.TODO(), &whopsdk.CheckAccessUsersRequest{
    ID:         "user_xxxxxxxxxxxxx",
    ResourceID: "resource_id",
})
if err != nil {
    log.Fatal(err)
}

fmt.Println(response.HasAccess, response.AccessLevel)
// has_access => true, access_level => "customer"
```

```bash cURL theme={null}
curl --request GET \
  --url https://api.whop.com/api/v1/users/{id}/access/{resource_id} \
  --header 'Authorization: Bearer <token>'
```

### Resource IDs and access levels

| `resource_id` prefix | Checks access to |
| - | - |
| `biz_xxxx` | Account |
| `prod_xxxx` | Product |
| `exp_xxxx` | Experience |

The response's `access_level` is one of:

| Level | Meaning |
| - | - |
| `customer` | User has a valid membership. For an experience, they have a membership to any product connected to it. For a product, to that specific product. For an account, to any product on it. |
| `admin` | User is a team member of the account (any role, including moderator). |
| `no_access` | User has no access (`has_access` is `false`). |

### Customer app (experience view)

Customer apps should gate on the `experienceId` passed in path params. Return `no_access` or redirect if the viewer doesn't have a valid membership.

```typescript theme={null}
import { headers } from "next/headers";
import { whopsdk } from "@/lib/whop-sdk";

export default async function ExperiencePage({
    params,
}: {
    params: Promise<{ experienceId: string }>;
}) {
    const { experienceId } = await params;
    const { userId } = await whopsdk.verifyUserToken(await headers());

    const access = await whopsdk.users.checkAccess({
        id: userId,
        resource_id: experienceId,
    });

    if (!access.has_access) {
        return <div>Access denied</div>;
    }

    return <div>Welcome to the experience!</div>;
}
```

The same pattern works in an API route. Swap `headers()` for `request.headers` and return a `403 Response` instead of JSX.

### Dashboard app (dashboard view)

Dashboard apps should gate on `access_level === "admin"` so only account team members can load the view.

```typescript theme={null}
import { headers } from "next/headers";
import { whopsdk } from "@/lib/whop-sdk";

export default async function DashboardPage({
    params,
}: {
    params: Promise<{ companyId: string }>;
}) {
    const { companyId } = await params;
    const { userId } = await whopsdk.verifyUserToken(await headers());

    const access = await whopsdk.users.checkAccess({
        id: userId,
        resource_id: companyId,
    });

    if (access.access_level !== "admin") {
        return <div>Admin access required</div>;
    }

    return <div>Welcome to the dashboard!</div>;
}
```

Same pattern in an API route. Swap `headers()` for `request.headers` and return a `403 Response` instead of JSX.

## Next steps

> [card] [Run a local development proxy](https://docs.whop.com/developer/guides/dev-proxy.md)
>
> Match the production iframe + cookie setup on localhost.

> [card] [Request permissions](https://docs.whop.com/developer/guides/permissions.md)
>
> Add the scopes your app needs before publishing.

> [card] [Listen to webhooks](https://docs.whop.com/developer/guides/webhooks.md)
>
> Receive payment, membership, and entry events on your server.

> [card] [Build an app view](https://docs.whop.com/developer/guides/app-views.md)
>
> Set up dashboard views, experiences, and discover listings.


## Related topics

- [Build Whop Apps](https://docs.whop.com/developer/apps/overview.md)
- [Chat Authentication](https://docs.whop.com/developer/guides/chat/authentication.md)
- [Chat OAuth](https://docs.whop.com/developer/guides/chat/oauth.md)
