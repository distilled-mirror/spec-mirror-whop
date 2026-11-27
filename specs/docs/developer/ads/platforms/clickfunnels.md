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

# Install the Whop Pixel on ClickFunnels

> Add the Whop Pixel to the Head Code field in your ClickFunnels Funnel Settings so every step is tracked, then confirm page views and add lead events.

## 1. Get your Whop Account ID

Find it from your [Whop Dashboard](https://whop.com/dashboard/) - the part of the URL after `dashboard/`, starting with `biz_`. Looks like `biz_xxxxxxxxxxx`.

![Whop dashboard URL in the browser address bar, with the account ID after dashboard underlined](https://mintcdn.com/whop/i81Det37UaKELkrh/images/dashboard-url.png?fit=max\&auto=format\&n=i81Det37UaKELkrh\&q=85\&s=f8dce6ea378318d4c89f7bce9239605d)

## 2. Open your funnel's custom code settings

In ClickFunnels, open your funnel, go to **Funnel Settings > Custom Code**, then open the **Head Code** field.

Setting this at the funnel level applies the pixel to every step in that funnel. To apply it across every funnel in your account instead, add it under your Site's global **Header Code** setting in the Sites app.

## 3. Paste the snippet

Paste this into the **Head Code** field. Replace `biz_xxxxxxxxxxxxx` with your own account ID.

```html Pixel snippet theme={null}
<script>
!function(w,d,s,u,n,a,b){if(w[n])return;a=w[n]={q:[],t:+new Date,s:[],o:u,track:function(){a.q.push([+new Date].concat([].slice.call(arguments)))},setScope:function(){a.s=[].slice.call(arguments).filter(function(x){return typeof x==="string"});a.q.push([+new Date,"setScope"].concat(a.s))},scope:function(){var c=[].slice.call(arguments);return{track:function(){a.q.push([+new Date].concat([].slice.call(arguments)).concat([{__scope:c}]))}}}};b=d.createElement(s);b.async=1;b.src=u+"/s.js";d.getElementsByTagName(s)[0].parentNode.insertBefore(b,d.getElementsByTagName(s)[0])}(window,document,"script","https://t.whop.tw","whop");
whop.setScope("biz_xxxxxxxxxxxxx");
whop.track("page");
</script>
```

Save the funnel. ClickFunnels adds this code to the `<head>` of every step automatically.

## 4. Confirm it's tracking

Visit a live funnel step, then check the [Websites page](https://whop.com/dashboard/websites) in your Whop dashboard. Your funnel's domain appears there once a page view comes through.

## 5. Track leads and other funnel events

If your funnel has a form, application, or booking step, add a `whop.track` call for it — see [Track events](https://docs.whop.com/developer/ads/pixel.md#track-events) in the main pixel guide.

> [!NOTE]
>
> **Don't track purchases, subscriptions, or trials.** Whop records every checkout view, purchase, subscription, and trial start server-side with zero configuration. Only send events Whop can't see: leads and bookings.


## Related topics

- [Install the Whop Pixel](https://docs.whop.com/developer/ads/pixel.md)
- [Install the Whop Pixel with Calendly](https://docs.whop.com/developer/ads/platforms/calendly.md)
- [Install the Whop Pixel on Kajabi](https://docs.whop.com/developer/ads/platforms/kajabi.md)
