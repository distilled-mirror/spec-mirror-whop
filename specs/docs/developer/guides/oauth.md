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

# OAuth

> Add "Sign in with Whop" to your site with OAuth 2.1 and Proof Key for Code Exchange (PKCE).

Use OAuth 2.1 + PKCE with OpenID Connect (OIDC) to let users sign in with Whop on your website or app. The returned access tokens let you access data and act on behalf of those users.

> [!NOTE]
>
> OAuth endpoints live at `https://api.whop.com/oauth/`.

## Step 1: Create your OAuth app

> [!WARNING]
>
> The `App` object returned by `apps.create` in the current `@whop/sdk` does not expose a `client_secret` field. Copy the client secret from the Dashboard tab instead.

> [tab] API
>
> Use the [Create App](https://docs.whop.com/api-reference/apps/create-app.md) endpoint to create an OAuth app programmatically:
>
> ```typescript theme={null}
> import { WhopClient } from "@whop/sdk";
>
> const client = new WhopClient({ token: process.env.WHOP_API_KEY });
>
> const app = await client.apps.create({
>   name: "My App",
>   redirect_uris: [
>     "http://localhost:3000/api/auth/callback/whop",
>     "https://myapp.com/api/auth/callback/whop"
>   ],
> });
>
> console.log(app.id);            // NEXT_PUBLIC_WHOP_APP_ID
> console.log(app.client_secret); // WHOP_CLIENT_SECRET
> ```
>
> Update redirect URIs later with the [Update App](https://docs.whop.com/api-reference/apps/update-app.md) endpoint.

> [tab] Dashboard
>
> **1. Create or select your app**
>
> Go to the [Developer Dashboard](https://whop.com/dashboard/developer), then create a new app or select an existing one.
>
> **2. Add redirect URIs**
>
> In the OAuth section, add every redirect URI you plan to use (exact match required).
>
> **3. Copy credentials**
>
> Copy your `client_id` (looks like `app_xxxxx`) and `client_secret`.
>
> **4. Select your scopes**
>
> Select your available OAuth scopes from the "View available scopes" button and select only the ones you need.

## Step 2: Send users to authorize

In your web or mobile client, use PKCE to securely redirect users to Whop's OAuth flow.

```typescript expandable theme={null}
const STORAGE_KEY = "whop_oauth_pkce";

function base64url(bytes: Uint8Array) {
	return btoa(String.fromCharCode(...bytes)).replace(
		/[+/=]/g,
		(c) => ({ "+": "-", "/": "_", "=": "" })[c]!,
	);
}

function randomString(len: number) {
	return base64url(crypto.getRandomValues(new Uint8Array(len)));
}

async function sha256(str: string) {
	return base64url(
		new Uint8Array(
			await crypto.subtle.digest("SHA-256", new TextEncoder().encode(str)),
		),
	);
}

async function startWhopOAuth(
	clientId: string,
	redirectUri: string,
	scope = "openid profile email",
	accountId?: string,
) {
	const pkce = {
		codeVerifier: randomString(32),
		state: randomString(16),
		nonce: randomString(16),
	};
	sessionStorage.setItem(STORAGE_KEY, JSON.stringify(pkce));

	const params = new URLSearchParams({
		response_type: "code",
		client_id: clientId,
		redirect_uri: redirectUri,
		scope,
		state: pkce.state,
		nonce: pkce.nonce,
		code_challenge: await sha256(pkce.codeVerifier),
		code_challenge_method: "S256",
		...(accountId && { company_id: accountId }),
	});

	window.location.href = `https://api.whop.com/oauth/authorize?${params}`;
}
```

Call `startWhopOAuth` to redirect the user:

```typescript theme={null}
await startWhopOAuth(
	"app_xxxxxxxxx",
	"https://yourapp.com/oauth/callback",
	"openid profile email", // optionally specify more custom scopes here
	"biz_xxxxx", // optionally specify a scoped account id here
);
```

If the user isn't logged in, Whop will prompt for login, then show the consent screen.
If the user has already approved your application for the requested scopes,
they will be automatically redirected back without needing to confirm twice.

> [!NOTE]
>
> If you pass an account ID as `company_id`, tokens are scoped to that account
> for the user. You can access only the resources that the user can control on
> that Whop account.

## Step 3: Handle the callback and exchange the code

Whop redirects back to your `redirect_uri` with `code` and `state`. Use this function to verify the state, exchange the code for tokens, and retrieve credentials:

```typescript expandable theme={null}
const STORAGE_KEY = "whop_oauth_pkce";

interface WhopTokens {
	access_token: string;
	refresh_token: string;
	id_token?: string; // only present if "openid" scope was requested
	token_type: string;
	expires_in: number;
	obtained_at: number; // we add this client-side for refresh logic
}

async function handleWhopCallback(
	clientId: string,
	redirectUri: string,
): Promise<WhopTokens> {
	const params = new URLSearchParams(window.location.search);
	const [code, returnedState, error] = [
		params.get("code"),
		params.get("state"),
		params.get("error"),
	];
	if (error)
		throw new Error(
			`OAuth error: ${error} - ${params.get("error_description") || ""}`,
		);

	const stored = JSON.parse(sessionStorage.getItem(STORAGE_KEY) || "null");
	sessionStorage.removeItem(STORAGE_KEY);
	if (!stored || returnedState !== stored.state)
		throw new Error("Invalid state - possible CSRF");

	const res = await fetch("https://api.whop.com/oauth/token", {
		method: "POST",
		headers: { "Content-Type": "application/json" },
		body: JSON.stringify({
			grant_type: "authorization_code",
			code,
			redirect_uri: redirectUri,
			client_id: clientId,
			code_verifier: stored.codeVerifier,
		}),
	});

	if (!res.ok) {
		const err = await res.json().catch(() => ({}));
		throw new Error(
			`Token exchange failed: ${err.error_description || res.status}`,
		);
	}

	const tokens = await res.json();
	return { ...tokens, obtained_at: Date.now() };
}

function storeTokens(tokens: WhopTokens) {
	document.cookie = `whop_tokens=${encodeURIComponent(JSON.stringify(tokens))}; path=/; max-age=${60 * 60 * 24 * 30}; secure; samesite=strict`;
}

function getTokens(): WhopTokens | null {
	const match = document.cookie.match(/whop_tokens=([^;]+)/);
	return match ? JSON.parse(decodeURIComponent(match[1])) : null;
}

function clearTokens() {
	document.cookie = "whop_tokens=; path=/; max-age=0";
}
```

On your callback page:

```typescript theme={null}
const tokens = await handleWhopCallback(
	"app_xxxxxxxxx",
	"https://yourapp.com/oauth/callback",
);
storeTokens(tokens);
```

## Step 4: Use the tokens

Initialize the Whop SDK with the user's access token:

```typescript theme={null}
import { WhopClient } from "@whop/sdk";

const tokens = getTokens();

const client = new WhopClient({
	token: tokens.access_token,
});
```

View the [API Reference](https://docs.whop.com/api-reference/overview.md) to see all available endpoints.

## Step 5: Refresh tokens

Access tokens expire after 1 hour. Use the refresh token to get new credentials:

```typescript expandable theme={null}
async function refreshTokens(
	clientId: string,
	accountId?: string,
): Promise<WhopTokens> {
	const tokens = getTokens();
	if (!tokens?.refresh_token) throw new Error("No refresh token available");

	const res = await fetch("https://api.whop.com/oauth/token", {
		method: "POST",
		headers: { "Content-Type": "application/json" },
		body: JSON.stringify({
			grant_type: "refresh_token",
			refresh_token: tokens.refresh_token,
			client_id: clientId,
			...(accountId && { company_id: accountId }),
		}),
	});

	if (!res.ok) {
		const error = await res.json().catch(() => ({}));
		if (res.status === 401 || error.error === "invalid_grant") {
			clearTokens();
			throw new Error("Session expired - please log in again");
		}
		throw new Error(
			`Token refresh failed: ${error.error_description || res.status}`,
		);
	}

	const newTokens = await res.json();
	const stored = { ...newTokens, obtained_at: Date.now() };
	storeTokens(stored);
	return stored;
}

async function getValidAccessToken(clientId: string): Promise<string> {
	const tokens = getTokens();
	if (!tokens) throw new Error("Not logged in");

	const expiresAt = tokens.obtained_at + tokens.expires_in * 1000;
	const needsRefresh = Date.now() > expiresAt - 5 * 60 * 1000; // 5 min buffer

	if (needsRefresh) {
		const refreshed = await refreshTokens(clientId);
		return refreshed.access_token;
	}

	return tokens.access_token;
}
```

Usage:

```typescript theme={null}
const accessToken = await getValidAccessToken("app_xxxxxxxxx");
```

> [!NOTE]
>
> Refresh tokens rotate on each use. Always store the new tokens returned from
> the refresh endpoint. If you provided `company_id` during authorization, you
> must provide the same `company_id` when refreshing.

## Step 6: Userinfo and revoke

### Get user info

Fetch the authenticated user's profile using the userinfo endpoint:

```typescript theme={null}
interface WhopUserInfo {
	sub: string; // user tag (e.g. "user_xxxxx")
	name?: string; // requires "profile" scope
	preferred_username?: string; // requires "profile" scope
	picture?: string; // requires "profile" scope
	email?: string; // requires "email" scope
	email_verified?: boolean; // requires "email" scope
}

async function getUserInfo(accessToken: string): Promise<WhopUserInfo> {
	const res = await fetch("https://api.whop.com/oauth/userinfo", {
		headers: { Authorization: `Bearer ${accessToken}` },
	});
	if (!res.ok) throw new Error(`Failed to fetch user info: ${res.status}`);
	return res.json();
}
```

> [!NOTE]
>
> The fields returned depend on the scopes granted. `openid` is required,
> `profile` adds name/username/picture, `email` adds email fields.

### Revoke tokens on logout

When a user logs out, revoke their refresh token. Access tokens expire after 1 hour and can't be server-revoked.

```typescript theme={null}
async function logout(clientId: string) {
	const tokens = getTokens();
	if (tokens?.refresh_token) {
		await fetch("https://api.whop.com/oauth/revoke", {
			method: "POST",
			headers: { "Content-Type": "application/json" },
			body: JSON.stringify({
				token: tokens.refresh_token,
				client_id: clientId,
			}),
		});
	}
	clearTokens();
}
```

> [!WARNING]
>
> Always revoke tokens when users log out. This invalidates the refresh token
> immediately, preventing unauthorized access even if the token was compromised.

## Error handling

OAuth errors follow the standard format:

```json theme={null}
{
	"error": "invalid_grant",
	"error_description": "Authorization code has expired"
}
```

Common error codes:

* `invalid_request`: missing or invalid parameter.
* `invalid_grant`: code or token expired or revoked.
* `invalid_client`: unknown `client_id`.
* `insufficient_scope`: token doesn't have the required scope.
* `rate_limit_exceeded`: too many requests (check the `Retry-After` header).

## Next steps

> [card] [API reference](https://docs.whop.com/api-reference.md)
>
> Every endpoint a granted access token can call.

> [card] [Accept payments](https://docs.whop.com/developer/guides/accept-payments.md)
>
> Pair sign-in with checkout for end-to-end purchase flows.

> [card] [Listen to webhooks](https://docs.whop.com/developer/guides/webhooks.md)
>
> React to payment, membership, and entry events server-side.

> [card] [Request permissions](https://docs.whop.com/developer/guides/permissions.md)
>
> Configure the OAuth scopes your app actually needs.


## Related topics

- [Chat Authentication](https://docs.whop.com/developer/guides/chat/authentication.md)
- [Chat OAuth](https://docs.whop.com/developer/guides/chat/oauth.md)
- [List OAuth Grants](https://docs.whop.com/api-reference/users/list-oauth-grants.md)
