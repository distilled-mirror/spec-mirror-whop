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

# Sync Users to Embedded Chat

> Enroll your platform users as subaccounts before minting chat tokens, then set an account-specific name and profile picture with Update user.

Sync users before you mint account-scoped user tokens for embedded chat. The user you pass as `user_id` must exist on Whop, and you can provide an account-specific name and profile picture that appears for that user in chat.

> [!NOTE]
>
> This **isn't needed for OAuth**. If you use OAuth, the user signs in to Whop and the OAuth token identifies their Whop account.

## 1. Enroll users as subaccounts

Create a subaccount for each user on your platform. This is the same flow used by the [payouts SDK](https://docs.whop.com/developer/platforms/render-payout-portal.md) — see the [Enroll subaccounts](https://docs.whop.com/developer/platforms/enroll-subaccounts.md) guide for the full walkthrough.

```typescript theme={null}
import { WhopClient } from "@whop/sdk";

const client = new WhopClient({ token: process.env.WHOP_API_KEY });

const account = await client.accounts.create({
  email: "user@example.com",
  title: "Dana Whitfield",
  external_id: "user_12345", // your platform's user ID
});
```

Save the resulting user ID on your side. You'll pass it when minting account-scoped chat tokens.

## 2. Set an account-specific name and profile picture

Call the [Update user](https://docs.whop.com/api-reference/users/update-user.md) endpoint with your account ID to define overrides shown for that user in the context of your account. Without an account ID, the fields update the user's global profile instead.

Upload avatars through the [Create file](https://docs.whop.com/api-reference/files/create-file.md) endpoint, then pass the returned file ID as the profile picture.

Call this whenever a user's profile changes on your platform so Whop stays in sync.

> [!NOTE]
>
> The Update user endpoint requires the `user:profile:update` permission on the API key making the request. Use an account API key with **owner-level** access, or a key that includes the `user:profile:update` scope.

## Next steps

> [card] [Authentication](https://docs.whop.com/developer/guides/chat/authentication.md)
>
> Mint account-scoped user tokens for embedded chat.

> [card] [OAuth](https://docs.whop.com/developer/guides/chat/oauth.md)
>
> Use Whop sign-in instead of syncing users yourself.

> [card] [Chat element](https://docs.whop.com/developer/guides/chat/chat-element.md)
>
> Render a live chat UI once your users are authenticated.

> [card] [Enroll subaccounts](https://docs.whop.com/developer/platforms/enroll-subaccounts.md)
>
> Learn the full subaccount enrollment flow.


## Related topics

- [Embedded Chat Direct Messages](https://docs.whop.com/developer/guides/chat/direct-messages.md)
- [Chat Quickstart](https://docs.whop.com/developer/guides/chat/quickstart.md)
- [Embedded Chat Channels](https://docs.whop.com/developer/guides/chat/channels.md)
