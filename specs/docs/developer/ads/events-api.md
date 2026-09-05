> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Send Server-Side Events

> Send conversion events from your server and tie them to the visitor the Whop Pixel already tracks.

The Events API is the server-side counterpart to the [Whop Pixel](/developer/ads/pixel). [`POST /events`](/api-reference/beta/events/create-event) accepts the same events the pixel sends from the browser, so you can report conversions that happen where no pixel runs. That covers a sale closed in your CRM, a payment on your own backend, a webhook from a form or booking tool, or a call a rep marks as booked.

<Note>
  **Before you start**, you need three things:

  * The [Whop Pixel](/developer/ads/pixel) installed on every page of your funnel. It creates the visitor ID this guide passes along.
  * An API key with the `event:create` permission. [Create one](https://whop.com/dashboard/developer) under **Account API Keys**, and keep it on your server. Never put it in browser code.
  * Your account ID, the part of your dashboard URL after `dashboard/` that starts with `biz_`.
</Note>

## When to use the Events API

Most integrations don't need this. The pixel already reports everything that happens in the browser, and it fills in the visitor ID, IP address, and user agent on its own. Use the Events API only when your server is the only place that knows a conversion happened. If a `whop.track` call can fire where the action happens, use that instead.

## Send a server event

<Steps>
  <Step title="Read the visitor ID in the browser" iconType="regular" titleSize="h3">
    The pixel stores each visitor's ID in a first-party cookie named `_wuid` on your domain, and mirrors it to `localStorage` under the same key. The value looks like `wuid_k3j9x2m1p8q4r7s0t5u6`. Read it at the moment the visitor takes the action you'll later report, not at page load. The cookie exists only after the pixel snippet has run on the page.

    ```html Read the visitor ID theme={null}
    <script>
      function whopVisitorId() {
        const match = document.cookie.match(/(?:^|;\s*)_wuid=([^;]*)/);
        if (match) return match[1];
        try {
          return localStorage.getItem("_wuid") || undefined;
        } catch {
          return undefined;
        }
      }
    </script>
    ```

    Each subdomain gets its own `_wuid`. The pixel links them to one person, so read whichever cookie is on the page where the handoff happens.
  </Step>

  <Step title="Send it to your server with the action" iconType="regular" titleSize="h3">
    Attach the visitor ID to whatever request already carries the action to your backend: a form post, a checkout call, a signup request. Capture the page URL at the same time. It carries the UTMs and click IDs that your server can't see otherwise.

    <CodeGroup>
      ```html Hidden form fields theme={null}
      <form id="quote-form" action="/api/quotes" method="post">
        <input type="email" name="email" required />
        <input type="hidden" name="whop_visitor_id" />
        <input type="hidden" name="whop_page_url" />
        <button type="submit">Get a quote</button>
      </form>

      <script>
        document.getElementById("quote-form").addEventListener("submit", (e) => {
          e.target.whop_visitor_id.value = whopVisitorId() || "";
          e.target.whop_page_url.value = location.href;
        });
      </script>
      ```

      ```html JSON request theme={null}
      <script>
        async function submitQuote(email) {
          await fetch("/api/quotes", {
            method: "POST",
            headers: { "Content-Type": "application/json" },
            body: JSON.stringify({
              email,
              whop_visitor_id: whopVisitorId(),
              whop_page_url: location.href,
            }),
          });
        }
      </script>
      ```

      ```javascript Same-host backend (Node) theme={null}
      app.post("/api/quotes", (req, res) => {
        const lead = {
          email: req.body.email,
          whop_visitor_id: req.cookies._wuid,
          whop_page_url: req.body.whop_page_url,
        };
        // Store the lead, then respond.
      });
      ```
    </CodeGroup>

    If your backend answers on the same host as the page, the `_wuid` cookie arrives on the request by itself. Read it there instead of sending it from the browser. The cookie is set for that host only, so a request from `example.com` to `api.example.com` doesn't carry it.

    Your server already sees the visitor's IP address and user agent on that request. Keep both.
  </Step>

  <Step title="Store it on the customer record" iconType="regular" titleSize="h3">
    Save the visitor ID, page URL, IP address, and user agent on the lead, customer, or order. The conversion you report might happen days after the visit. A deal your sales team closes next week still needs the visitor ID from the original visit to attribute. Whop accepts events up to 28 days old.
  </Step>

  <Step title="Send the event" iconType="regular" titleSize="h3">
    Post the event from your server with your API key. The example reports a lead from a visitor who arrived from an ad, filled out a quote form, and was then qualified in a CRM.

    <CodeGroup>
      ```bash cURL theme={null}
      curl -X POST https://api.whop.com/api/v1/events \
        -H "Authorization: Bearer $WHOP_API_KEY" \
        -H "Content-Type: application/json" \
        -d '{
          "account_id": "biz_xxxxxxxxxxxxx",
          "event_name": "lead",
          "event_id": "lead_8f21c0",
          "event_time": "2026-09-03T14:32:11Z",
          "action_source": "website",
          "url": "https://example.com/quote?utm_meta_ad_id=120215678901234567&utm_meta_adset_id=120215678901234566&utm_meta_campaign_id=120215678901234565&utm_source=fb&utm_placement=Facebook_Mobile_Feed&utm_medium=paid_social&utm_content=Ceramic%20coating%20reel&utm_adset=Austin%20SUV%20owners&utm_whop=true&wacid=adcamp_xxxxxxxxxx&wasid=adgrp_xxxxxxxxxxx&waid=ad_xxxxxxxxxxxxx&fbclid=IwAR0example",
          "user": {
            "anonymous_id": "wuid_k3j9x2m1p8q4r7s0t5u6",
            "email": "amara@example.com",
            "phone": "+15125550137",
            "first_name": "Amara",
            "last_name": "Okafor",
            "external_id": "crm_8842"
          },
          "context": {
            "ip_address": "203.0.113.42",
            "user_agent": "Mozilla/5.0 (iPhone; CPU iPhone OS 17_5 like Mac OS X) AppleWebKit/605.1.15",
            "fbclid": "IwAR0example",
            "utm_source": "fb"
          }
        }'
      ```

      ```typescript TypeScript theme={null}
      import { WhopClient } from "@whop/sdk";

      const client = new WhopClient({ token: process.env.WHOP_API_KEY });

      await client.events.create({
        account_id: "biz_xxxxxxxxxxxxx",
        event_name: "lead",
        event_id: "lead_8f21c0",
        event_time: "2026-09-03T14:32:11Z",
        action_source: "website",
        url: "https://example.com/quote?utm_meta_ad_id=120215678901234567&utm_meta_adset_id=120215678901234566&utm_meta_campaign_id=120215678901234565&utm_source=fb&utm_placement=Facebook_Mobile_Feed&utm_medium=paid_social&utm_content=Ceramic%20coating%20reel&utm_adset=Austin%20SUV%20owners&utm_whop=true&wacid=adcamp_xxxxxxxxxx&wasid=adgrp_xxxxxxxxxxx&waid=ad_xxxxxxxxxxxxx&fbclid=IwAR0example",
        user: {
          anonymous_id: "wuid_k3j9x2m1p8q4r7s0t5u6",
          email: "amara@example.com",
          phone: "+15125550137",
          first_name: "Amara",
          last_name: "Okafor",
          external_id: "crm_8842",
        },
        context: {
          ip_address: "203.0.113.42",
          user_agent: "Mozilla/5.0 (iPhone; CPU iPhone OS 17_5 like Mac OS X) AppleWebKit/605.1.15",
          fbclid: "IwAR0example",
          utm_source: "fb",
        },
      });
      ```

      ```python Python theme={null}
      import os
      from whop_sdk import Whop

      client = Whop(token=os.environ["WHOP_API_KEY"])

      client.events.create(
          account_id="biz_xxxxxxxxxxxxx",
          event_name="lead",
          event_id="lead_8f21c0",
          event_time="2026-09-03T14:32:11Z",
          action_source="website",
          url="https://example.com/quote?utm_meta_ad_id=120215678901234567&utm_meta_adset_id=120215678901234566&utm_meta_campaign_id=120215678901234565&utm_source=fb&utm_placement=Facebook_Mobile_Feed&utm_medium=paid_social&utm_content=Ceramic%20coating%20reel&utm_adset=Austin%20SUV%20owners&utm_whop=true&wacid=adcamp_xxxxxxxxxx&wasid=adgrp_xxxxxxxxxxx&waid=ad_xxxxxxxxxxxxx&fbclid=IwAR0example",
          user={
              "anonymous_id": "wuid_k3j9x2m1p8q4r7s0t5u6",
              "email": "amara@example.com",
              "phone": "+15125550137",
              "first_name": "Amara",
              "last_name": "Okafor",
              "external_id": "crm_8842",
          },
          context={
              "ip_address": "203.0.113.42",
              "user_agent": "Mozilla/5.0 (iPhone; CPU iPhone OS 17_5 like Mac OS X) AppleWebKit/605.1.15",
              "fbclid": "IwAR0example",
              "utm_source": "fb",
          },
      )
      ```
    </CodeGroup>

    The `url` in the example is what a visitor lands on after tapping a Whop ad on Meta. `utm_meta_ad_id`, `utm_meta_adset_id`, and `utm_meta_campaign_id` are the network's IDs. `wacid`, `wasid`, and `waid` are the Whop campaign, ad group, and ad IDs. `utm_source` names the surface (`fb`, `ig`, `msg`, or `an`), `utm_placement` the placement, `utm_content` and `utm_adset` the ad and ad group names, and `fbclid` is the tag the network appends itself. Whop resolves which ad drove the visit from these, so store the URL whole rather than picking parameters out of it.

    The response returns the stored event ID. Whop prefixes your `event_id` with the account so IDs from different accounts never collide:

    ```json theme={null}
    { "id": "biz_xxxxxxxxxxxxx:lead_8f21c0" }
    ```
  </Step>

  <Step title="Confirm it landed on the right person" iconType="regular" titleSize="h3">
    [List Events](/api-reference/beta/events/list-events) accepts a visitor ID as the `identifier`, so you can read the whole journey the event joined. The request needs a key with a read permission such as `company:basic:read`.

    ```bash cURL theme={null}
    curl "https://api.whop.com/api/v1/events?identifier=wuid_k3j9x2m1p8q4r7s0t5u6" \
      -H "Authorization: Bearer $WHOP_API_KEY"
    ```

    Your server event appears next to the pixel's page views with the same `person_id`. If the page views are there but your event isn't, the visitor ID didn't match. See [Troubleshooting](#troubleshooting). The event also shows up on the [Events page](https://whop.com/dashboard/events) in your dashboard.
  </Step>
</Steps>

## Parameters that decide attribution

Only `account_id` and `event_name` are required. The rest of the payload is what makes the event attributable, so treat these as required too.

| Field                                                                | What to send                                                                                                                                                                                                                                                                    |
| -------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `user.anonymous_id`                                                  | The visitor ID from the `_wuid` cookie. This is the one field that ties a server event to the browser session that came from the ad. Without it, Whop can only match on email or phone.                                                                                         |
| `user.email`, `user.phone`                                           | Plain text, exactly as the customer entered them. Whop matches on these when the pixel captured them earlier. A hashed email is dropped as invalid.                                                                                                                             |
| `user.first_name`, `user.last_name`, `user.external_id`              | Whatever else you know. `external_id` is your own customer ID.                                                                                                                                                                                                                  |
| `event_id`                                                           | A stable ID for this one action: your order number, deal ID, or form submission ID. Whop stores each `event_name` and `event_id` pair once, so a retry or a second copy of the same conversion collapses into one event. See [Avoid duplicate events](#avoid-duplicate-events). |
| `event_time`                                                         | When the action happened, as an ISO 8601 timestamp. Defaults to now. Whop rejects anything older than 28 days and clamps future timestamps to now. Send the real time, not the time your job ran.                                                                               |
| `value`, `currency`                                                  | The monetary value and its lowercase ISO 4217 code, such as `usd`. Optional on most events. Required and greater than zero on `purchase`.                                                                                                                                       |
| `url`                                                                | The page the visitor was on when the action started, including its query string. On a visit from a Whop ad, that query string carries the IDs Whop resolves the campaign, ad group, and ad from.                                                                                |
| `context.ip_address`, `context.user_agent`                           | The visitor's, copied from their original browser request. Whop reads these from the connection only for unauthenticated browser calls. A server call gets nothing inferred, so an event without them has no device or location signals.                                        |
| `context.fbclid`, `context.gclid`, `context.ttclid`, `context.utm_*` | The ad network IDs and `utm_*` parameters from the landing page URL, if you stored them.                                                                                                                                                                                        |
| `context.fbp`, `context.fbc`                                         | The Meta `_fbp` and `_fbc` cookie values from the visitor's browser, if you captured them.                                                                                                                                                                                      |
| `action_source`                                                      | Where the action happened: `website`, `app`, `email`, `phone_call`, `chat`, `physical_store`, `system_generated`, `business_messaging`, or `other`.                                                                                                                             |

The full request schema is on the [Create Event](/api-reference/beta/events/create-event) reference page.

## Name the event

Use the same names as the pixel. A `lead` you report from your server lands as the same event as a `lead` the pixel reports, so it counts toward the results of a campaign judged on leads.

| Event                   | When to send it                                            |
| ----------------------- | ---------------------------------------------------------- |
| `lead`                  | A visitor submits contact info or an opt-in form           |
| `schedule`              | A visitor books a call or appointment                      |
| `submit_application`    | A visitor submits an application                           |
| `contact`               | A visitor starts a conversation or contact request         |
| `complete_registration` | A signup or registration finishes                          |
| `view_content`          | A visitor views a key page or piece of content             |
| `add_to_cart`           | A visitor adds an item to a cart                           |
| `purchase`              | A sale completes on your own checkout. `value` is required |

Any other `event_name` becomes a custom event under that name. Keep names short and stable, and reuse a small set.

```json Custom event theme={null}
{ "account_id": "biz_xxxxxxxxxxxxx", "event_name": "deal_closed", "value": 2400, "currency": "usd" }
```

<Warning>
  Never send `purchase` for a sale that Whop processed. Whop records every checkout, purchase, subscription, and trial on its own checkout automatically and reports it to the ad network. Only send events Whop can't see.
</Warning>

## Avoid duplicate events

If the same conversion can reach Whop from both the browser and your server, send it from both with the same `event_name` and the same `event_id`. Whop keeps one copy. This is the safest setup when a form fires `whop.track("lead", { event_id })` on submit and your backend reports the same lead again after validating it.

The `event_id` identifies one action, not the type of action. Every lead needs its own. A value made up at send time is new every time, so it stops nothing. Use an ID your system already has for that one action.

## Troubleshooting

**The event is stored but not attributed to an ad.** The event carried no `user.anonymous_id`, or a value the pixel never used. The usual causes are reading the cookie before the pixel snippet ran, reading it on a domain without the pixel, or storing the visitor ID on the wrong record. Confirm the fix by listing events for that visitor ID. The pixel's page views must show up under the same `identifier` as your event.

**`400` with a message about `event_time`.** The timestamp is more than 28 days old. Send the event closer to when it happens, or drop it.

**`400` on a `purchase` event.** `value` is missing or not greater than zero. Ad networks can't optimize toward a sale with no amount, so Whop rejects it.

**`401` or `403`.** The key is missing, lacks `event:create`, or belongs to a different account than `account_id`. Production keys use `https://api.whop.com/api/v1`.
