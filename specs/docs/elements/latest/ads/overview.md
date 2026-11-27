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

# Ads

> An advertising account. Scope it to a Whop account with `accountId` — the ad account underneath is assigned server-side and never surfaces here — then mount `reporting` for what the account spent and what came back, `campaign-creator` to build a campaign, or `ads-agreement` to sign the Whop Ads agreement a launch requires.

> [!NOTE]
>
> This page documents `@whop/elements@1.10.0` and `@whop/elements-react@1.10.0`.

*Since `v1.0.0`.*

## Playground

Assemble the elements with example data. Drive the controls, add and arrange elements, and watch events fire live:

```tsx React theme={null}
import { WhopElements, Ads } from "@whop/elements-react";
import { loadWhop } from "@whop/elements";

function Example() {
  return (
    <WhopElements elements={loadWhop()}>
      <Ads /* options */>
        {/* mount elements here */}
      </Ads>
    </WhopElements>
  );
}
```

```html JavaScript theme={null}
<script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>
<script type="module">
  const ads = window.WhopElements().ads.create({ /* options */ });
</script>
```

## Options

Pass these to `whop.ads.create({ … })`, or as props on `<Ads>` in React.

**accountId** `string`

Whop account ID, prefixed `biz_`, whose advertising this reads. Required; this is not an ad account ID. Defaults to `""`.

**accessToken** `string`

A scoped token every surface under this handle reads and writes with. **Mint one token for the whole handle, not one per element.** Work out the scopes of every element you are going to mount, request them on a single token, and set it here — the handle projects that one credential into each element's frame, so a second token per surface buys nothing and leaves you several expiration dates to track instead of one. Mint it on your server with `POST /api/v1/access_tokens` and set a fresh one with `update({ accessToken })` before it expires. Mounting everything needs `ad_campaign:read`/`:create`, `ad_group:read`, `ad:read`, `stats:read`, the `:update`/`:delete` scopes the table's row actions use, `company:basic:read` for the pixel checks `campaign-creator` runs, and `payment:charge` plus `user:email:read` to add an ad-spend card; the billing slots list stored cards and spendable wallets with `member:payment_methods:read`. Mounting fewer surfaces can ask for fewer scopes — still on the one token. Omitted, the calls carry the viewer's own session, which only answers same-origin.

**links** `CrossLinksConfig`

Where the reporting `table`’s click and result counts link on your site — typically the pages where you mounted the `tracking` namespace’s `people` and `events` elements. `people` and `events` are absolute base URLs (`https://…`); the table appends the same query params those elements take as props (`?source=whop:adcamp_…:*&event=…&from=…&to=…`), so the target page can hydrate its element straight from its URL. A relative URL would resolve against the element frame, so one is treated as unset; unset links render the counts as plain figures. Defaults to `{}`.

**properties**

**people** `string`

**events** `string`

**person** `string`

**appearance** `Appearance`

Visual customization for this group's elements. Overrides the global `WhopElements({ appearance })`. Change it live with `update({ appearance })`.

**locale** `WhopElementsLocale`

Locale for this group's element UI text. Set it to one of the app's built locales to override the global configuration. Any other value falls back to the default locale.

## Events

Pass callbacks in the create options or React props.

### `onBillingChanged`

The account's ad-spend payment methods changed and the change is saved. Slot values are the method IDs themselves — `payt_…` for a card, `ldgr_…` for a balance; an empty `backup` means none is set.

**payload** `object`

**properties**

**primary** `string` *required*

**backup** `string` *required*

### `onLoadingChange`

Runs when the grouped loading state changes. The value is `true` while any mounted element is still loading.

**loading** `boolean`

## Methods

Call these on the Ads handle from `whop.ads.create({ … })` or `useAds()`.

### `update`

Merges new handle options into every mounted element. In React, change the namespace props instead.

**Parameters**

**options** `Partial\<AdsOptions\>` *required*

### `destroy`

Destroys every element and sub-controller this handle created, removes the controller frame, and releases its subscriptions. You can call it more than once, but a destroyed handle refuses any other call — create a new handle to start over. React removes the group automatically when the provider unmounts.

## Elements

The elements this group mounts. Each has its own page:

> [card] [Reporting](https://docs.whop.com/elements/latest/ads/reporting)
>
> An advertising account's reporting: what it spent and what came back, over a window, at whatever level you narrow to. Mount `chart`, `table`, or both — they read the same window, the same credit rule, and the same selection, so two surfaces side by side can never disagree about what they are showing. The `ads` handle's `links` configuration wires the table's click and result counts to your own pages — the ones where you mounted the `tracking` namespace's `people` and `events`. *(sub-controller, 3 elements)*

> [card] [BillingSetupElement](https://docs.whop.com/elements/latest/ads/billing-setup.md)
>
> Which payment methods ad spend bills against: a primary used first, and an optional backup for when it fails. Both are chosen from the methods already saved on the account, and picking one saves it immediately — there is no separate submit. Adding a card opens a card form over your page and returns to the slot that asked for it. The account's own balance appears alongside its cards wherever the account has one.

> [card] [CampaignCreatorElement](https://docs.whop.com/elements/latest/ads/campaign-creator.md)
>
> The advertising campaign builder: objective and budget, ad groups with their targeting and schedule, and the ads themselves with their creative — the whole flow through to launch. Opens empty to build a new campaign, or on an existing one when you pass `campaignId`. It runs the pixel checker too: a launch is gated on the ad group’s conversion event being live on the destination URL, and the `pixel-setup` wizard opens inside this element to fix an install that is missing. A launch is also gated on the account having signed the Whop Ads agreement: when it has not, the handle opens `ads-agreement` over the builder and the launch carries on once it is signed. Budgets are entered and shown in the account’s `ads_reporting_currency` — the currency its reports already use — and converted to and billed in USD at the current exchange rate. The one token you set on the `ads` handle must carry the pixel scopes alongside the campaign scopes — `ad_campaign:create` and `company:basic:read` — rather than using a separate token per element.

> [card] [AdsAgreementElement](https://docs.whop.com/elements/latest/ads/ads-agreement.md)
>
> Signs the Whop Ads Services and Payment Authorization Agreement for an account. Saving the account's ad-spend billing opens the agreement; until an authorized representative signs it, no campaign can launch. The signer types their full legal name and, optionally, their title, accepts the terms, and the typed signature is filed with the agreement. Once it is signed this shows who signed it and when; before billing is set up there is nothing to sign and it says so. `campaign-creator` opens this over the builder by itself when a launch needs a signature, so mount it yourself only to offer signing ahead of time — on a settings or onboarding page, say.


## Related topics

- [Ads](https://docs.whop.com/api-reference/legacy/ads/ad.md)
- [Ads API](https://docs.whop.com/api-reference/ads/ad.md)
- [List Ads](https://docs.whop.com/api-reference/ads/list-ads.md)
