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

# Verifications

> Collects and verifies identity information for an account. Mount `kyc` for a complete KYC or KYB flow backed by the Verifications API.

> [!NOTE]
>
> This page documents `@whop/elements@1.10.0` and `@whop/elements-react@1.10.0`.

*Since `v1.0.0`.*

## Playground

Assemble the elements with example data. Drive the controls, add and arrange elements, and watch events fire live:

```tsx React theme={null}
import { WhopElements, Verifications } from "@whop/elements-react";
import { loadWhop } from "@whop/elements";

function Example() {
  return (
    <WhopElements elements={loadWhop()}>
      <Verifications /* options */>
        {/* mount elements here */}
      </Verifications>
    </WhopElements>
  );
}
```

```html JavaScript theme={null}
<script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>
<script type="module">
  const verifications = window.WhopElements().verifications.create({ /* options */ });
</script>
```

## Options

Pass these to `whop.verifications.create({ … })`, or as props on `<Verifications>` in React.

**kind** `"individual" | "business"`

Use `individual` for KYC or `business` for KYB. The mounted element owns the matching intake form. Defaults to `"individual"`.

**accountId** `string` *required*

The account being verified: a company `biz_…` tag or the viewer's own `user_…` tag.

**getToken** `() =\> Promise\<string\>`

Called on your page before the element mounts. Return a fresh bearer credential with `identity:read` and `identity:write`; omit it only for a same-origin Whop session.

**appearance** `Appearance`

Visual customization for this group's elements. Overrides the global `WhopElements({ appearance })`. Change it live with `update({ appearance })`.

**locale** `WhopElementsLocale`

Locale for this group's element UI text. Set it to one of the app's built locales to override the global configuration. Any other value falls back to the default locale.

## Events

Pass callbacks in the create options or React props.

### `onLoadingChange`

Runs when the grouped loading state changes. The value is `true` while any mounted element is still loading.

**loading** `boolean`

## Methods

Call these on the Verifications handle from `whop.verifications.create({ … })` or `useVerifications()`.

### `update`

Merges new handle options into every mounted element. In React, change the namespace props instead.

**Parameters**

**options** `Partial\<VerificationsOptions\>` *required*

### `destroy`

Destroys every element and sub-controller this handle created, removes the controller frame, and releases its subscriptions. You can call it more than once, but a destroyed handle refuses any other call — create a new handle to start over. React removes the group automatically when the provider unmounts.

## Elements

The elements this group mounts. Each has its own page:

> [card] [KycElement](https://docs.whop.com/elements/latest/verifications/kyc.md)
>
> A complete identity-verification flow. It collects individual KYC or business KYB details, starts or resumes the hosted provider session, handles follow-up information and document requests, polls status, and renders the final result without requiring the host to build verification UI.

> [card] [CapabilitiesElement](https://docs.whop.com/elements/latest/verifications/capabilities.md)
>
> The account's verification standing: whether individual and business verification are done, and which capabilities that unlocks. Read-only — pressing a verify button reports `verificationRequested` and stays put, so the host mounts its own flow, such as this namespace's `kyc` element. Both halves can be hidden, so a host that only wants the capability list, or only the two verification rows, can drop the other.

> [card] [RfiElement](https://docs.whop.com/elements/latest/verifications/rfi.md)
>
> Everything the account still owes compliance, and the forms to answer it. Each row is one group of requirements — grouped by the system that asked, because each relays to its provider once its own items are answered — and pressing it opens that group’s form in place. Rows already with a reviewer are shown but not answerable. Renders an all-clear once nothing is outstanding, so it can sit permanently in a settings page.


## Related topics

- [Verification](https://docs.whop.com/api-reference/legacy/verifications/verification.md)
- [Verifications API](https://docs.whop.com/api-reference/verifications/verification.md)
- [Create Verification](https://docs.whop.com/api-reference/verifications/create-verification.md)
