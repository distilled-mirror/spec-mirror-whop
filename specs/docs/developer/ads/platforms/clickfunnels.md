> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Install the Whop Pixel on ClickFunnels

> Add the Whop Pixel to a ClickFunnels funnel so every step is tracked.

<Steps>
  <Step title="Get your Whop Account ID" iconType="regular" titleSize="h2">
    Find it from your [Whop Dashboard](https://whop.com/dashboard/) - the part of the URL after `dashboard/`, starting with `biz_`. Looks like `biz_xxxxxxxxxxx`.

    <img src="https://mintcdn.com/whop/i81Det37UaKELkrh/images/dashboard-url.png?fit=max&auto=format&n=i81Det37UaKELkrh&q=85&s=f8dce6ea378318d4c89f7bce9239605d" width="1270" height="244" data-path="images/dashboard-url.png" />
  </Step>

  <Step title="Open your funnel's custom code settings" iconType="regular" titleSize="h2">
    In ClickFunnels, open your funnel, go to **Funnel Settings > Custom Code**, then open the **Head Code** field.

    Setting this at the funnel level applies the pixel to every step in that funnel. To apply it across every funnel in your account instead, add it under your Site's global **Header Code** setting in the Sites app.
  </Step>

  <Step title="Paste the snippet" iconType="regular" titleSize="h2">
    Paste this into the **Head Code** field. Replace `biz_xxxxxxxxxxxxx` with your own account ID.

    ```html Pixel snippet theme={null}
    <script>
    !function(w,d,s,u,n,a,b){if(w[n])return;a=w[n]={q:[],t:+new Date,s:[],o:u,track:function(){a.q.push([+new Date].concat([].slice.call(arguments)))},setScope:function(){a.s=[].slice.call(arguments).filter(function(x){return typeof x==="string"});a.q.push([+new Date,"setScope"].concat(a.s))},scope:function(){var c=[].slice.call(arguments);return{track:function(){a.q.push([+new Date].concat([].slice.call(arguments)).concat([{__scope:c}]))}}}};b=d.createElement(s);b.async=1;b.src=u+"/s.js";d.getElementsByTagName(s)[0].parentNode.insertBefore(b,d.getElementsByTagName(s)[0])}(window,document,"script","https://t.whop.tw","whop");
    whop.setScope("biz_xxxxxxxxxxxxx");
    whop.track("page");
    </script>
    ```

    Save the funnel. ClickFunnels adds this code to the `<head>` of every step automatically.
  </Step>

  <Step title="Confirm it's tracking" iconType="regular" titleSize="h2">
    Visit a live funnel step, then check the [Websites page](https://whop.com/dashboard/websites) in your Whop dashboard. Your funnel's domain appears there once a page view comes through.
  </Step>
</Steps>

## Track leads and other funnel events

If your funnel has a form, application, or booking step, add a `whop.track` call for it — see [Track events](/developer/ads/pixel#track-events) in the main pixel guide.

<Note>
  **Don't track purchases, subscriptions, or trials.** Whop records every checkout view, purchase, subscription, and trial start server-side with zero configuration. Only send events Whop can't see, such as leads and bookings.
</Note>
