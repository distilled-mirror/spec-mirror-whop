> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Install the Whop Pixel

> How to use the Whop Pixel to capture the full customer journey.

The Whop Pixel is a JavaScript snippet that you add to your website. It measures page views, links visitors to their purchases, and attributes conversion events back to your ads. Once installed, you'll see your website in the [Websites page](https://whop.com/dashboard/websites).

## Install the Whop Pixel

<Steps>
  <Step title="Get your Whop Account ID" iconType="regular" titleSize="h2">
    Find it from your [Whop Dashboard](https://whop.com/dashboard/) - the part of the URL after `dashboard/`, starting with `biz_`. Looks like `biz_xxxxxxxxxxxxx`.

    <img src="https://mintcdn.com/whop/i81Det37UaKELkrh/images/dashboard-url.png?fit=max&auto=format&n=i81Det37UaKELkrh&q=85&s=f8dce6ea378318d4c89f7bce9239605d" width="1270" height="244" data-path="images/dashboard-url.png" />
  </Step>

  <Step title="Install the snippet" iconType="regular" titleSize="h2">
    Paste this snippet inside the `<head>` of **every page** in your funnel — landing pages, advertorials, checkouts, and thank-you pages, not just your homepage. Replace `biz_xxxxxxxxxxxxx` with your own account ID.

    ```html Pixel snippet theme={null}
    <script>
    !function(w,d,s,u,n,a,b){if(w[n])return;a=w[n]={q:[],t:+new Date,s:[],o:u,track:function(){a.q.push([+new Date].concat([].slice.call(arguments)))},setScope:function(){a.s=[].slice.call(arguments).filter(function(x){return typeof x==="string"});a.q.push([+new Date,"setScope"].concat(a.s))},scope:function(){var c=[].slice.call(arguments);return{track:function(){a.q.push([+new Date].concat([].slice.call(arguments)).concat([{__scope:c}]))}}}};b=d.createElement(s);b.async=1;b.src=u+"/s.js";d.getElementsByTagName(s)[0].parentNode.insertBefore(b,d.getElementsByTagName(s)[0])}(window,document,"script","https://t.whop.tw","whop");
    whop.setScope("biz_xxxxxxxxxxxxx");
    whop.track("page");
    </script>
    ```

    Building your funnel on a platform instead of raw HTML? Use the dedicated install guide instead of pasting the snippet into a page builder text block.

    | If you use   | Follow                                                                          |
    | ------------ | ------------------------------------------------------------------------------- |
    | ClickFunnels | [Install the Whop Pixel on ClickFunnels](/developer/ads/platforms/clickfunnels) |
    | GoHighLevel  | [Install the Whop Pixel on GoHighLevel](/developer/ads/platforms/gohighlevel)   |
    | Kajabi       | [Install the Whop Pixel on Kajabi](/developer/ads/platforms/kajabi)             |
    | WordPress    | [Install the Whop Pixel on WordPress](/developer/ads/platforms/wordpress)       |
    | WooCommerce  | [Install the Whop Pixel on WooCommerce](/developer/ads/platforms/woocommerce)   |
    | Shopify      | [Install the Whop Pixel on Shopify](/developer/ads/platforms/shopify)           |
    | WebinarJam   | [Install the Whop Pixel on WebinarJam](/developer/ads/platforms/webinarjam)     |
    | iClosed      | [Install the Whop Pixel with iClosed](/developer/ads/platforms/iclosed)         |
    | Calendly     | [Install the Whop Pixel with Calendly](/developer/ads/platforms/calendly)       |
  </Step>

  <Step title="Track events" iconType="regular" titleSize="h2">
    If your funnel has important steps outside Whop checkout, like a lead form, call booking, or application, add `whop.track` calls to track those events.

    The most common setup is to fire events either:

    * when a form submits successfully
    * on the thank-you page or confirmation page the visitor sees after submitting

    ```javascript Tracking events theme={null}
    whop.track("lead");                                      // a standard event
    whop.track("schedule", { value: 50, currency: "USD" });  // optionally with a value
    whop.track("quiz_completed");                            // or your own event name
    ```

    **It's important to include any information you have about the customer at every stage of the funnel.** If you accept a lead with a name, email and phone number, include those in the event fired to Whop.

    ```javascript theme={null}
    whop.track("lead", { name: "John Doe", email: "johndoe@example.com", phone: "555-555-1234" });
    ```
  </Step>
</Steps>

## Standard events

The Whop Pixel includes a set of standard event names for common stages of a funnel. Each one optionally accepts a `value` (number) and `currency` ISO 4217 code).

| Event                   | When to fire it                                    |
| ----------------------- | -------------------------------------------------- |
| `lead`                  | A visitor submits contact info or an opt-in form   |
| `schedule`              | A visitor books a call or appointment              |
| `submit_application`    | A visitor submits an application                   |
| `contact`               | A visitor starts a conversation or contact request |
| `complete_registration` | A signup or registration finishes                  |
| `view_content`          | A visitor views a key page or piece of content     |
| `add_to_cart`           | A visitor adds an item to a cart                   |

## Custom events

If Whop's standard events don't cover your funnel's needs, or you want to track specific stages of the customer journey, you can fire custom events.

```javascript Custom event theme={null}
whop.track("watched_vsl");
whop.track("quiz_completed", { value: 25, currency: "USD" });
```

Keep names short and stable, and reuse a small set. Whop stores names up to 250 characters.

### Where to put the tracking call depends on what happens after the action

* **The action doesn't redirect** (e.g. a form that submits in place and shows an inline success message). Fire the event in the action's success handler, such as the form's `onSubmit` callback, after it succeeds:

```javascript Track on submit (no redirect) theme={null}
form.addEventListener("submit", async (e) => {
	e.preventDefault();
	await submitForm();
	whop.track("watched_vsl");
});
```

* **The action redirects** (e.g. a form that sends the visitor to a new page on submit). Fire the event on the page they land on, such as the thank-you or confirmation page. Make sure the pixel snippet is installed on that page too.

## Give each event an ID

Pass an `event_id` with any event that could reach Whop more than once. For example, if you fire an event when landing on a thank you page, ensure that event is fired once per real conversion, not once per page view. Whop counts each `event_name` and `event_id` pair once, so a retry, a page refresh, or the same conversion sent from two places collapses into a single event.

```javascript Event with an ID theme={null}
whop.track("lead", { event_id: "lead_8f21c0" }); // one visitor's form submission
whop.track("lead", { event_id: "lead_2b94de" }); // a different visitor's
```

**The ID identifies one single event, not the type of event.** Every lead needs its own. Send `"lead"` as the ID for every lead and Whop treats them all as one event, so hundreds of leads show up as one.

Use a value your own system already has for that one action — a lead ID, order number, or form submission ID. Reuse that value only when you're sending that same action again. A retry counts as the same action. So does the same conversion mirrored from your server. A value made up at fire time is new every time, so it won't stop duplicates.

If the same conversion can come from both your website and your server, send the same `event_id` from both so it lands as one event.

## Attach customer information

Always send as much customer information as you have available. This helps Whop match events to customers and improve conversion tracking.

These fields can be attached to any event type and are optional. Send only the fields you have.

```javascript Lead with customer fields theme={null}
whop.track("lead", {
  email: "visitor@example.com",
  first_name: "Jane",
  last_name: "Doe",
  name: "Jane Doe",
  phone: "+15551234567",
  external_id: "customer_123",
  city: "New York",
  state: "NY",
  postal_code: "10001",
  country: "US",
});
```

| Field         | Description                  |
| ------------- | ---------------------------- |
| `email`       | Visitor's email address      |
| `first_name`  | Visitor's first name         |
| `last_name`   | Visitor's last name          |
| `name`        | Visitor's full display name  |
| `phone`       | Visitor's phone number       |
| `external_id` | Your own user/customer ID    |
| `city`        | Visitor's city               |
| `state`       | Visitor's state or region    |
| `postal_code` | Visitor's postal or ZIP code |
| `country`     | Visitor's country            |

<Warning>
  Send these as plain text. A hashed email gets dropped because it isn't a valid address. A hashed phone number gets stored as meaningless digits and matches nobody.
</Warning>

<Note>
  **Don't track purchases, subscriptions, or trials.** Whop records every checkout view, purchase, subscription, and trial start server-side with zero configuration. The pixel won't accept duplicates. Only send events Whop can't see. Examples include leads and bookings on your own infrastructure.
</Note>
