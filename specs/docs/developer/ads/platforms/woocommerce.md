> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Install the Whop Pixel on WooCommerce

> Add the Whop Pixel to a WooCommerce store so every page is tracked.

WooCommerce is a WordPress plugin, so it uses the same `<head>` mechanism as any WordPress site — WordPress doesn't expose the `<head>` tag by default, so you need a plugin that adds custom code there.

<Steps>
  <Step title="Get your Whop Account ID" iconType="regular" titleSize="h2">
    Find it from your [Whop Dashboard](https://whop.com/dashboard/) - the part of the URL after `dashboard/`, starting with `biz_`. Looks like `biz_xxxxxxxxxxx`.

    <img src="https://mintcdn.com/whop/i81Det37UaKELkrh/images/dashboard-url.png?fit=max&auto=format&n=i81Det37UaKELkrh&q=85&s=f8dce6ea378318d4c89f7bce9239605d" width="1270" height="244" data-path="images/dashboard-url.png" />
  </Step>

  <Step title="Install a header/footer script plugin" iconType="regular" titleSize="h2">
    Install a plugin that lets you add custom code to the site header, such as **WPCode** or **Insert Headers and Footers**, from **Plugins > Add New**.
  </Step>

  <Step title="Paste the snippet" iconType="regular" titleSize="h2">
    Open the plugin's **Header** field. Paste this in, then replace `biz_xxxxxxxxxxxxx` with your own account ID.

    ```html Pixel snippet theme={null}
    <script>
    !function(w,d,s,u,n,a,b){if(w[n])return;a=w[n]={q:[],t:+new Date,s:[],o:u,track:function(){a.q.push([+new Date].concat([].slice.call(arguments)))},setScope:function(){a.s=[].slice.call(arguments).filter(function(x){return typeof x==="string"});a.q.push([+new Date,"setScope"].concat(a.s))},scope:function(){var c=[].slice.call(arguments);return{track:function(){a.q.push([+new Date].concat([].slice.call(arguments)).concat([{__scope:c}]))}}}};b=d.createElement(s);b.async=1;b.src=u+"/s.js";d.getElementsByTagName(s)[0].parentNode.insertBefore(b,d.getElementsByTagName(s)[0])}(window,document,"script","https://t.whop.tw","whop");
    whop.setScope("biz_xxxxxxxxxxxxx");
    whop.track("page");
    </script>
    ```

    Save the changes. The plugin adds this code to the `<head>` of every page on the site, including the shop, product, and cart pages.
  </Step>

  <Step title="Confirm it's tracking" iconType="regular" titleSize="h2">
    Visit a live page, then check the [Websites page](https://whop.com/dashboard/websites) in your Whop dashboard. Your domain appears there once a page view comes through.
  </Step>

  <Step title="Track leads and other funnel events" iconType="regular" titleSize="h2">
    If your store has a form, application, or booking step outside of checkout, add a `whop.track` call for it. See [Track events](/developer/ads/pixel#track-events) in the main pixel guide.

    <Note>
      **Don't track purchases, subscriptions, or trials.** Whop records every checkout view, purchase, subscription, and trial start server-side with zero configuration. Only send events Whop can't see: leads and bookings.
    </Note>
  </Step>
</Steps>
