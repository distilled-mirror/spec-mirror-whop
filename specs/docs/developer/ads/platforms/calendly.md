> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Install the Whop Pixel with Calendly

> Track Calendly bookings with the Whop Pixel.

Calendly's booking page can't tell your website when someone books a call, so you can't track bookings the normal way. The fix: have Calendly send bookers to a "Thank You" page on your own site after they book. Put the pixel there instead, and it picks up the booker's name and email automatically.

<Steps>
  <Step title="Get your Whop Account ID" iconType="regular" titleSize="h2">
    Find it from your [Whop Dashboard](https://whop.com/dashboard/) - the part of the URL after `dashboard/`, starting with `biz_`. Looks like `biz_xxxxxxxxxxx`.

    <img src="https://mintcdn.com/whop/i81Det37UaKELkrh/images/dashboard-url.png?fit=max&auto=format&n=i81Det37UaKELkrh&q=85&s=f8dce6ea378318d4c89f7bce9239605d" width="1270" height="244" data-path="images/dashboard-url.png" />
  </Step>

  <Step title="Create a Thank You page" iconType="regular" titleSize="h2">
    Build a simple "Thank You" or "Call Confirmed" page wherever you already host your site. This is the page bookers land on right after they book a call.
  </Step>

  <Step title="Install the pixel on your booking page too" iconType="regular" titleSize="h2">
    This guide's other steps cover the Thank You page — but the pixel also needs to be on the page where people actually book the call (your landing page, or wherever the Calendly scheduler is embedded or linked from). Without it there, you won't see page views or ad clicks for that page, only the final booking. Paste this into the `<head>` of that page. Replace `biz_xxxxxxxxxxxxx` with your own account ID.

    ```html Pixel snippet theme={null}
    <script>
    !function(w,d,s,u,n,a,b){if(w[n])return;a=w[n]={q:[],t:+new Date,s:[],o:u,track:function(){a.q.push([+new Date].concat([].slice.call(arguments)))},setScope:function(){a.s=[].slice.call(arguments).filter(function(x){return typeof x==="string"});a.q.push([+new Date,"setScope"].concat(a.s))},scope:function(){var c=[].slice.call(arguments);return{track:function(){a.q.push([+new Date].concat([].slice.call(arguments)).concat([{__scope:c}]))}}}};b=d.createElement(s);b.async=1;b.src=u+"/s.js";d.getElementsByTagName(s)[0].parentNode.insertBefore(b,d.getElementsByTagName(s)[0])}(window,document,"script","https://t.whop.tw","whop");
    whop.setScope("biz_xxxxxxxxxxxxx");
    whop.track("page");
    </script>
    ```

    If your site builder doesn't let you edit the `<head>`, check its dedicated guide in this section — or, if you already use Google Tag Manager, add this same code there as a Custom HTML tag that fires on all pages.
  </Step>

  <Step title="Tell Calendly to send bookers to your page" iconType="regular" titleSize="h2">
    Open your event type, then go to **Edit > More options > Confirmation page**. Under **After booking**, choose **Redirect to an external site**, and enter your Thank You page's URL. Turn on **Pass event details to your redirected page** — this is what makes Calendly send the booker's name and email along with the redirect.
  </Step>

  <Step title="Paste the pixel snippet onto your Thank You page" iconType="regular" titleSize="h2">
    Paste the same pixel snippet from step 3 into the `<head>` of your Thank You page too — every page in your funnel needs its own copy, not just the booking page.
  </Step>

  <Step title="Add one more snippet to record the booking" iconType="regular" titleSize="h2">
    Right under the pixel snippet on your Thank You page, paste this too. Calendly already includes the booker's name and email in the page's web address, and this picks them up automatically — you don't need to type anything in:

    ```html Track a booking theme={null}
    <script>
    var params = new URLSearchParams(window.location.search);
    whop.track("schedule", {
    	email: params.get("invitee_email") || undefined,
    	name: params.get("invitee_full_name") || undefined,
    });
    </script>
    ```
  </Step>

  <Step title="Confirm it's tracking" iconType="regular" titleSize="h2">
    Book a call through the flow, then check the [Websites page](https://whop.com/dashboard/websites) in your Whop dashboard. Your domain appears there once a page view comes through.
  </Step>
</Steps>

## Embedding Calendly on your own page instead?

If you embed Calendly's scheduler directly on a page (**Share > Add to Website**) rather than linking out to `calendly.com`, you can skip the redirect and listen for the booking to complete right there — the pixel snippet from step 3 still needs to be on that same page:

```html Track a booking on an embedded calendar theme={null}
<script>
window.addEventListener("message", function (e) {
	const isCalendlyEvent = e.origin === "https://calendly.com" && e.data.event && e.data.event.indexOf("calendly.") === 0;
	if (!isCalendlyEvent || e.data.event !== "calendly.event_scheduled") return;

	whop.track("schedule");
});
</script>
```

<Note>
  This message confirms a booking happened, but doesn't include the invitee's name or email — Calendly keeps those out of the client-side event for privacy. If you already collected the visitor's details earlier in your funnel, pass them into the same `track` call, or use the redirect method above instead.
</Note>

<Note>
  **Don't track purchases, subscriptions, or trials.** Whop records every checkout view, purchase, subscription, and trial start server-side with zero configuration. Only send events Whop can't see, such as bookings.
</Note>
