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

# Install the Whop Pixel on WebinarJam

> Paste the Whop Pixel into WebinarJam's custom tracking fields, then fire a lead event when someone registers.

## 1. Get your Whop Account ID

Find it from your [Whop Dashboard](https://whop.com/dashboard/) - the part of the URL after `dashboard/`, starting with `biz_`. Looks like `biz_xxxxxxxxxxx`.

![Whop dashboard URL in the browser address bar, with the account ID after dashboard underlined](https://mintcdn.com/whop/i81Det37UaKELkrh/images/dashboard-url.png?fit=max\&auto=format\&n=i81Det37UaKELkrh\&q=85\&s=f8dce6ea378318d4c89f7bce9239605d)

## 2. Open your webinar's tracking settings

In WebinarJam, open your webinar, go to **Integrations > Integrate a 3rd Party Tracking System**, then select **Embed Your Custom Tracking Code**.

WebinarJam gives you a separate tracking field for each step of the funnel: the registration page, the registration form, the post-registration thank-you page, the live room, and the replay page.

## 3. Paste the tracking code into each field

Replace `biz_xxxxxxxxxxxxx` with your own account ID in each snippet below, then paste it into the matching field.

**Registration Form Tracking** gets the plain pixel snippet. The registration form is a popup with its own context, separate from the page around it — the visitor's email, name, and phone fields live here, so this is what lets the pixel's automatic field capture actually see them:

```html Registration Form Tracking theme={null}
<script>
!function(w,d,s,u,n,a,b){if(w[n])return;a=w[n]={q:[],t:+new Date,s:[],o:u,track:function(){a.q.push([+new Date].concat([].slice.call(arguments)))},setScope:function(){a.s=[].slice.call(arguments).filter(function(x){return typeof x==="string"});a.q.push([+new Date,"setScope"].concat(a.s))},scope:function(){var c=[].slice.call(arguments);return{track:function(){a.q.push([+new Date].concat([].slice.call(arguments)).concat([{__scope:c}]))}}}};b=d.createElement(s);b.async=1;b.src=u+"/s.js";d.getElementsByTagName(s)[0].parentNode.insertBefore(b,d.getElementsByTagName(s)[0])}(window,document,"script","https://t.whop.tw","whop");
whop.setScope("biz_xxxxxxxxxxxxx");
whop.track("page");
</script>
```

**Registration Page Tracking** gets the pixel plus a listener that fires the `lead` event the instant registration completes — before the redirect to the thank-you page even happens:

```html Registration Page Tracking theme={null}
<script>
!function(w,d,s,u,n,a,b){if(w[n])return;a=w[n]={q:[],t:+new Date,s:[],o:u,track:function(){a.q.push([+new Date].concat([].slice.call(arguments)))},setScope:function(){a.s=[].slice.call(arguments).filter(function(x){return typeof x==="string"});a.q.push([+new Date,"setScope"].concat(a.s))},scope:function(){var c=[].slice.call(arguments);return{track:function(){a.q.push([+new Date].concat([].slice.call(arguments)).concat([{__scope:c}]))}}}};b=d.createElement(s);b.async=1;b.src=u+"/s.js";d.getElementsByTagName(s)[0].parentNode.insertBefore(b,d.getElementsByTagName(s)[0])}(window,document,"script","https://t.whop.tw","whop");
whop.setScope("biz_xxxxxxxxxxxxx");
whop.track("page");

window.addEventListener("message", function (e) {
	if (e.origin !== "https://event.webinarjam.com") return;

	let data;
	try {
		data = typeof e.data === "string" ? JSON.parse(e.data) : e.data;
	} catch {
		return;
	}

	if (data.func === "redirect" && typeof data.url === "string" && data.url.indexOf("/thank-you") !== -1) {
		whop.track("lead");
	}
});
</script>
```

**Post-Registration Thank You Page Tracking** gets the plain pixel snippet, as a backup page-view record:

```html Post-Registration Thank You Page Tracking theme={null}
<script>
!function(w,d,s,u,n,a,b){if(w[n])return;a=w[n]={q:[],t:+new Date,s:[],o:u,track:function(){a.q.push([+new Date].concat([].slice.call(arguments)))},setScope:function(){a.s=[].slice.call(arguments).filter(function(x){return typeof x==="string"});a.q.push([+new Date,"setScope"].concat(a.s))},scope:function(){var c=[].slice.call(arguments);return{track:function(){a.q.push([+new Date].concat([].slice.call(arguments)).concat([{__scope:c}]))}}}};b=d.createElement(s);b.async=1;b.src=u+"/s.js";d.getElementsByTagName(s)[0].parentNode.insertBefore(b,d.getElementsByTagName(s)[0])}(window,document,"script","https://t.whop.tw","whop");
whop.setScope("biz_xxxxxxxxxxxxx");
whop.track("page");
</script>
```

Add the plain pixel snippet to the live room and replay page fields too if you want visits to those steps tracked. Save your changes.

> [!NOTE]
>
> Running registration on your own site instead of WebinarJam's hosted registration page? Install the snippet on that page directly — see the main [Whop Pixel guide](https://docs.whop.com/developer/ads/pixel.md) — since WebinarJam's tracking fields only cover its own hosted pages.

## 4. Confirm it's tracking

Visit a live registration page, then check the [Websites page](https://whop.com/dashboard/websites) in your Whop dashboard. Your domain appears there once a page view comes through. Then register with a test email and watch for a `lead` event to land the moment you complete the form.

> [!NOTE]
>
> **Don't track purchases, subscriptions, or trials.** Whop records every checkout view, purchase, subscription, and trial start server-side with zero configuration. Only send events Whop can't see, such as registrations and attendance.


## Related topics

- [Install the Whop Pixel](https://docs.whop.com/developer/ads/pixel.md)
- [Install the Whop Pixel on Shopify](https://docs.whop.com/developer/ads/platforms/shopify.md)
- [Install the Whop Pixel with Calendly](https://docs.whop.com/developer/ads/platforms/calendly.md)
