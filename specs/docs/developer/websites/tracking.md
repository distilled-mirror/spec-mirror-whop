> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Track Visitors

> Every site on whop.site ships the Whop pixel already installed. Page views and checkout events are automatic; add whop.track calls for the funnel steps Whop can't see.

Whop injects the pixel into every HTML page your site serves, scoped to the app's business, and fires a page view. There is no snippet to paste and no company ID to configure.

| Tracked automatically            | You add a `whop.track` call                          |
| -------------------------------- | ---------------------------------------------------- |
| Page views                       | Lead forms, call bookings, applications              |
| Whop checkout views              | Quizzes, VSL completions, anything on your own pages |
| Purchases, subscriptions, trials | Steps Whop's payment data can't see                  |

## Track your own events

`window.whop` exists before your app code runs, so call it directly:

```javascript theme={null}
whop.track("lead");
whop.track("schedule", { value: 50, currency: "USD" });
whop.track("watched_vsl");
```

Standard event names — `lead`, `schedule`, `submit_application`, `contact`, `complete_registration`, `view_content`, `add_to_cart` — each optionally take a `value` and a `currency`. Any other name works too, and shows up under that name in reporting.

Fire the event where the action completes: in the success handler if the page stays put, or on the confirmation page if it redirects.

<Tip>
  Pass an `event_id` on anything that could arrive twice — a retry, a refresh, or the same conversion mirrored from your server. Whop counts each name and ID pair once. Use an ID your system already has for that one action, never a constant like `"lead"`.
</Tip>

## Where the data lands

Open **Websites** in your [dashboard](https://whop.com/dashboard) for visitors, page views, and attributed revenue per domain, or the [pixel dashboard](https://whop.com/dashboard) for the full event breakdown. First events appear about a minute after the visit.

<Note>
  Installing the pixel snippet yourself is still supported — the platform one checks for an existing `window.whop` and no-ops, so nothing double-counts. Do that when your site needs a scope other than its own business.
</Note>

For a site on your own domain rather than `whop.site`, install the snippet by hand: [Install the Whop pixel](/developer/ads/pixel).

## Next steps

<CardGroup cols={2}>
  <Card title="Pixel reference" href="/developer/ads/pixel">
    Every event field, customer matching, and server-side sends.
  </Card>

  <Card title="Accept payments" href="/developer/guides/accept-payments">
    Give the pixel conversions to attribute.
  </Card>
</CardGroup>
