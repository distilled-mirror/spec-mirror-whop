> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Install the Whop Pixel on WebinarJam

> Add the Whop Pixel to a WebinarJam webinar so every step is tracked.

<Steps>
  <Step title="Get your Whop Account ID" iconType="regular" titleSize="h2">
    Find it from your [Whop Dashboard](https://whop.com/dashboard/) - the part of the URL after `dashboard/`, starting with `biz_`. Looks like `biz_xxxxxxxxxxx`.

    <img src="https://mintcdn.com/whop/i81Det37UaKELkrh/images/dashboard-url.png?fit=max&auto=format&n=i81Det37UaKELkrh&q=85&s=f8dce6ea378318d4c89f7bce9239605d" width="1270" height="244" data-path="images/dashboard-url.png" />
  </Step>

  <Step title="Open your webinar's tracking settings" iconType="regular" titleSize="h2">
    In WebinarJam, open your webinar, go to **Integrations > Integrate a 3rd Party Tracking System**, then select **Embed Your Custom Tracking Code**.

    WebinarJam gives you a separate tracking field for each step of the funnel: the registration page, the registration form, the post-registration thank-you page, the live room, and the replay page.
  </Step>

  <Step title="Paste the tracking code into each field" iconType="regular" titleSize="h2">
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

    <Note>
      Running registration on your own site instead of WebinarJam's hosted registration page? Install the snippet on that page directly — see the main [Whop Pixel guide](/developer/ads/pixel) — since WebinarJam's tracking fields only cover its own hosted pages.
    </Note>
  </Step>

  <Step title="Confirm it's tracking" iconType="regular" titleSize="h2">
    Visit a live registration page, then check the [Websites page](https://whop.com/dashboard/websites) in your Whop dashboard. Your domain appears there once a page view comes through. Then register with a test email and watch for a `lead` event to land the moment you complete the form.
  </Step>
</Steps>

<Note>
  **Don't track purchases, subscriptions, or trials.** Whop records every checkout view, purchase, subscription, and trial start server-side with zero configuration. Only send events Whop can't see, such as registrations and attendance.
</Note>
