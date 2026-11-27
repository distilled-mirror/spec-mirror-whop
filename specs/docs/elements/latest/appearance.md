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

# Appearance

> Theme, tokens, and per-part styling for every element.

> [!NOTE]
>
> This page documents `@whop/elements@1.10.0` and `@whop/elements-react@1.10.0`.

One `appearance` object styles every element. Pass it globally at construction, per group at `create()` (or as React props), and change it live at any point with `update({ appearance })`. Mounted elements restyle in place.

```ts theme={null}
const payments = whop.payments.create({
  appearance: {
    theme: { appearance: 'dark', accentColor: 'blue' },
    variables: { '--radius': '8px' },
    classes: { /* per-part declarations, see below */ }
  }
});
```

## Theme

The high-level design tokens. Every field is optional and fully typed. The values below are enumerated from the source types:

**appearance** `"light" | "dark"`

The color scheme to use. - `"light"` - Light mode with dark text on light backgrounds - `"dark"` - Dark mode with light text on dark backgrounds

**accentColor** `AccentColor`

The primary accent color used for interactive elements. One of `ruby`, `blue`, `red`, `yellow`, `green`, `gray`, `tomato`, `crimson`, `pink`, `plum`, `purple`, `violet`, `iris`, `cyan`, `teal`, `jade`, `grass`, `brown`, `orange`, `indigo`, `sky`, `mint`, `amber`, `lime`, `lemon`, `magenta`, `gold`, `bronze`.

**grayColor** `string`

The gray color palette to use for neutral elements. Use `"auto"` to automatically match the accent color.

One of `"gray"`, `"mauve"`, `"slate"`, `"sage"`, `"olive"`, `"sand"`, or `"auto"`.

**dangerColor** `"ruby" | "red" | "tomato"`

The color used for error states and destructive actions.

**warningColor** `"yellow" | "amber"`

The color used for warning states.

**successColor** `"green" | "teal" | "jade" | "grass"`

The color used for success states.

**infoColor** `"sky" | "blue"`

The color used for informational states.

## Variables

CSS custom properties (variables) that can be used to customize element styles. Keys must start with `--` prefix.

```ts theme={null}
appearance: { variables: { "--radius": "8px" } }
```

## Classes

Style declarations for each `whop-*` class name. Each entry restyles one documented part of an element. See the Styling section on each element page for its class list. The framework checks each declaration, then injects the sanitized result inside the element's frame because page stylesheets cannot reach it.

A style declaration object that maps CSS properties to values, like React's `CSSProperties`. Property names can use camel case (`fontWeight`) or kebab case (`font-weight`). The framework normalizes each name and checks it against the list of safe properties before applying it.

```ts theme={null}
appearance: {
  classes: {
    "whop-SomePart": { borderRadius: "8px", fontWeight: "600" },
  },
}
```

Find each element's part list in the **Styling** section of its page.

One `appearance` object styles every element. Pass it on `<WhopElements>` and it applies to everything beneath, or per group on `<Payments>`. Change it at any time and mounted elements restyle in place.

```tsx theme={null}
<WhopElements
  appearance={{
    theme: { appearance: 'dark', accentColor: 'blue' },
    tokens: { radius: 10, spacing: 4 },
    parts: { 'whop-Input': { borderWidth: 1 } },
  }}
>
```

## Theme

Identical to the web object, field for field, so a theme written for your website types and behaves the same here:

```tsx theme={null}
theme: {
  appearance: 'light' | 'dark',
  accentColor: 'blue',   // any Radix accent scale
  grayColor: 'auto',
  dangerColor: 'ruby' | 'red' | 'tomato',
  warningColor: 'yellow' | 'amber',
  successColor: 'green' | 'teal' | 'jade' | 'grass',
  infoColor: 'sky' | 'blue',
}
```

Omitting `appearance` follows the device's light or dark setting. `grayColor: 'auto'` picks the gray scale that pairs with your accent.

## Tokens

The web's `variables` are CSS custom properties, which React Native has no analogue for. `tokens` is the closed, typed replacement, and its values are **numbers rather than strings**, because there are no units here:

```tsx theme={null}
tokens: {
  radius: 8,           // corner radius
  spacing: 4,          // the base unit gaps and padding multiply
  fontSize: 15,        // input and value text
  labelFontSize: 13,
  borderWidth: 1,
  controlHeight: 44,
  fontFamily: 'Inter', // undefined uses the platform font
}
```

Every key is optional and merges over the defaults.

## Parts

The web's `classes` are CSS declarations keyed by `whop-*` part name. `parts` keeps **the same part names**, so one styling vocabulary covers both surfaces, and swaps CSS for React Native style objects:

```tsx theme={null}
parts: {
  'whop-Input': { borderWidth: 1.5, borderRadius: 12 },
  'whop-MethodTileSelected': { borderColor: '#5B5BD6' },
  'whop-Label': { fontWeight: '600' },
}
```

Flat per-part overrides applied after the theme. There is no cascade, no pseudo-classes and no combinators, so a state a stylesheet would reach with `:focus` has its own part instead (`whop-InputFocused`, `whop-InputInvalid`).

Find each element's part list in the **Styling** section of its page.

> [!NOTE]
>
> `variables` and `classes` are typed `never` here rather than ignored, so an appearance object copied from the web fails to compile at the two keys that cannot travel instead of silently doing nothing.

Reading the theme yourself: `useTheme()` returns the resolved scales and tokens, which is how you match your own surrounding UI to the elements. See [Getting started](https://docs.whop.com/elements/latest/getting-started.md).


## Related topics

- [PaymentDetailElement](https://docs.whop.com/elements/latest/dashboard/paymentDetail.md)
- [PaymentsTableElement](https://docs.whop.com/elements/latest/dashboard/paymentsTable.md)
- [VerificationElement](https://docs.whop.com/elements/latest/dashboard/verification.md)
