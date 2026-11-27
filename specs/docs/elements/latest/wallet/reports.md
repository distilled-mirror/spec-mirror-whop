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

# Reports

> Balance history and the financial activity behind it, including CSV exports. Account statements are available in the Whop dashboard.

> [!NOTE]
>
> This page documents `@whop/elements@1.10.0` and `@whop/elements-react@1.10.0`.

*Since `v1.0.0`.*

Mounts inside [`Wallet`](https://docs.whop.com/elements/latest/wallet/overview). Create it to get a handle, then mount its elements on that handle. Call `destroy()` to remove the sub-controller. Create it again to get a fresh handle.

## Preview

A live, interactive demo of this sub-controller's default arrangement with example data:

```tsx React theme={null}
import { WhopElements, Wallet, Reports, BalanceReportElement, ReportActivityElement } from "@whop/elements-react";
import { loadWhop } from "@whop/elements";

function Example() {
  return (
    <WhopElements elements={loadWhop()}>
      <Wallet /* options */>
        <Reports>
          <BalanceReportElement />
          <ReportActivityElement />
        </Reports>
      </Wallet>
    </WhopElements>
  );
}
```

```html JavaScript theme={null}
<script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>
<script type="module">
  const wallet = window.WhopElements().wallet.create({ /* options */ });
  const reports = wallet.create('reports', { /* options */ });
  reports.create('balanceReport').mount('#wallet-reports-balanceReport');
  reports.create('reportActivity').mount('#wallet-reports-reportActivity');
</script>
```

## Options

Pass these to `wallet.create('reports', { … })`, or as props on `<Reports>` in React. Parent-injected props never appear here.

*Reports takes no options.*

## Methods

Call these on the sub handle from `wallet.create('reports', { … })`.

### `update`

Merges new props and callbacks into the sub-controller.

**Parameters**

**options** `Partial\<ReportsSubOptions\>` *required*

### `destroy`

Destroys the sub-controller and its elements, then frees its exclusive slot. A later `create("reports")` starts fresh.

## Elements

The elements this sub-controller mounts. Each has its own page:

> [card] [BalanceReportElement](https://docs.whop.com/elements/latest/wallet/reports-balanceReport.md)
>
> Balance history with date, timezone, and currency controls, starting and ending balances, and money-in and money-out breakdowns. Drill into activity and export it without leaving the element.

> [card] [ReportActivityElement](https://docs.whop.com/elements/latest/wallet/reports-reportActivity.md)
>
> Financial activity with date, currency, direction, and movement filters, pagination, and CSV export. Mount independently or open it from the balance report.


## Related topics

- [List Reports](https://docs.whop.com/api-reference/stats/list-reports.md)
- [Review Reports API](https://docs.whop.com/api-reference/review-reports/review-report.md)
- [List Review Reports](https://docs.whop.com/api-reference/review-reports/list-review-reports.md)
