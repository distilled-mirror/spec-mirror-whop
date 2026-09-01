> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Install the Whop Pixel on Shopify

> Add the Whop Pixel to a Shopify storefront, including checkout, with a custom pixel.

Shopify's checkout runs in a sandboxed environment that ignores scripts pasted into `theme.liquid`, so a normal pixel install never sees it. Shopify's own **Custom Pixel** feature is the way around that — it runs in every part of your store, checkout included, and reports standard events like `checkout_completed` and `payment_info_submitted` directly. The script below listens for those events and forwards each one to Whop.

<Steps>
  <Step title="Get your Whop Account ID" iconType="regular" titleSize="h2">
    Find it from your [Whop Dashboard](https://whop.com/dashboard/) - the part of the URL after `dashboard/`, starting with `biz_`. Looks like `biz_xxxxxxxxxxx`.

    <img src="https://mintcdn.com/whop/i81Det37UaKELkrh/images/dashboard-url.png?fit=max&auto=format&n=i81Det37UaKELkrh&q=85&s=f8dce6ea378318d4c89f7bce9239605d" width="1270" height="244" data-path="images/dashboard-url.png" />
  </Step>

  <Step title="Add a custom pixel" iconType="regular" titleSize="h2">
    In Shopify, go to **Settings > Customer events**, then click **Add custom pixel**. Name it `Whop`.
  </Step>

  <Step title="Paste the script" iconType="regular" titleSize="h2">
    Paste this into the pixel's code editor. Replace `biz_xxxxxxxxxxxxx` with your own account ID.

    ```javascript Custom pixel theme={null}
    const WHOP_ACCOUNT_ID = "biz_xxxxxxxxxxxxx";

    const WHOP_ENDPOINT = "https://t.whop.tw/conversions";

    const TRACKED_EVENTS = new Set([
      "product_viewed",
      "collection_viewed",
      "search_submitted",
      "cart_viewed",
      "product_added_to_cart",
      "product_removed_from_cart",
      "checkout_started",
      "checkout_contact_info_submitted",
      "checkout_address_info_submitted",
      "checkout_shipping_info_submitted",
      "payment_info_submitted",
      "checkout_completed",
    ]);

    const MAX_EVENT_NAME_LENGTH = 34;

    const WHOP_STANDARD_EVENTS = new Set([
      "page",
      "lead",
      "contact",
      "schedule",
      "submit_application",
      "complete_registration",
      "view_content",
      "add_to_cart",
    ]);

    // Map onto Whop's own standard event names wherever Shopify's event has a real
    // equivalent — a standard name skips the character-limit problem entirely,
    // since only custom names get prefixed with your company tag. Everything else
    // falls through to a shopify_-prefixed custom name below; Shopify's own names
    // for the three checkout steps are too long once prefixed, so they're
    // shortened here too.
    const EVENT_NAME_OVERRIDES = {
      page_viewed: "page",
      product_viewed: "view_content",
      product_added_to_cart: "add_to_cart",
      checkout_completed: "purchase",
      checkout_contact_info_submitted: "shopify_checkout_contact",
      checkout_address_info_submitted: "shopify_checkout_address",
      checkout_shipping_info_submitted: "shopify_checkout_shipping",
    };

    const TRACKED_QUERY_PARAMS = [
      "utm_id",
      "utm_source",
      "utm_medium",
      "utm_campaign",
      "utm_term",
      "utm_content",
      "fbclid",
      "gclid",
      "ttclid",
      "li_fat_id",
      "msclkid",
      "twclid",
      "rdt_cid",
      "sccid",
      "wbraid",
      "gbraid",
      "ig_sid",
    ];

    function compact(object) {
      return Object.fromEntries(
        Object.entries(object).filter(
          ([, value]) =>
            value !== null &&
            value !== undefined &&
            value !== "",
        ),
      );
    }

    async function readCookie(name) {
      try {
        return (await browser.cookie.get(name)) || null;
      } catch {
        return null;
      }
    }

    async function readCookies() {
      const [fbp, fbc, ga, ttp] = await Promise.all([
        readCookie("_fbp"),
        readCookie("_fbc"),
        readCookie("_ga"),
        readCookie("_ttp"),
      ]);

      return { fbp, fbc, ga, ttp };
    }

    function readQueryParameters(href) {
      if (!href) return {};

      try {
        const url = new URL(href);

        return compact(
          Object.fromEntries(
            TRACKED_QUERY_PARAMS.map((name) => [
              name,
              url.searchParams.get(name),
            ]),
          ),
        );
      } catch {
        return {};
      }
    }

    function readTimezone() {
      try {
        return Intl.DateTimeFormat()
          .resolvedOptions()
          .timeZone;
      } catch {
        return null;
      }
    }

    function buildContext(event, cookies) {
      const documentContext = event.context?.document;
      const navigatorContext = event.context?.navigator;
      const screen = event.context?.window?.screen;
      const href = documentContext?.location?.href;

      return compact({
        user_agent: navigatorContext?.userAgent,
        language: navigatorContext?.language,
        timezone: readTimezone(),
        screen_resolution: screen ? `${screen.width}x${screen.height}` : null,
        fbp: cookies.fbp,
        fbc: cookies.fbc,
        ga: cookies.ga,
        ttp: cookies.ttp,
        ...readQueryParameters(href),
      });
    }

    function getAnonymousId(clientId) {
      if (!clientId) return null;

      const anonymousId = `shopify_${clientId}`;

      if (anonymousId.length > 64) {
        console.error("Shopify client ID is too long");
        return null;
      }

      return anonymousId;
    }

    function getWhopEventName(shopifyEventName) {
      const name =
        EVENT_NAME_OVERRIDES[shopifyEventName] ||
        `shopify_${shopifyEventName}`;

      if (
        !WHOP_STANDARD_EVENTS.has(name) &&
        name.length > MAX_EVENT_NAME_LENGTH
      ) {
        console.error(
          "Whop event name too long, Meta will drop it",
          name,
          name.length,
        );
      }

      return name;
    }

    function getEventId(event, checkout) {
      if (event.name === "checkout_completed") {
        const orderOrCheckoutId =
          checkout?.order?.id ||
          checkout?.token ||
          event.id;

        return `shopify:purchase:${orderOrCheckoutId}`;
      }

      return `shopify:${event.name}:${event.id}`;
    }

    function buildUser(event, checkout) {
      const anonymousId = getAnonymousId(event.clientId);

      if (!anonymousId) return null;

      const address =
        checkout?.shippingAddress ||
        checkout?.billingAddress ||
        {};

      const firstName = address.firstName || null;
      const lastName = address.lastName || null;

      return compact({
        anonymous_id: anonymousId,
        external_id:
          checkout?.order?.customer?.id || null,
        email: checkout?.email || null,
        phone:
          checkout?.phone ||
          address.phone ||
          null,
        first_name: firstName,
        last_name: lastName,
        name:
          [firstName, lastName]
            .filter(Boolean)
            .join(" ") || null,
        city: address.city || null,
        state:
          address.provinceCode ||
          address.province ||
          null,
        postal_code: address.zip || null,
        country:
          address.countryCode ||
          address.country ||
          null,
      });
    }

    async function sendWhopEvent(payload) {
      try {
        const response = await fetch(WHOP_ENDPOINT, {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify(payload),
          keepalive: true,
          credentials: "omit",
        });

        const responseBody = await response.text();

        if (!response.ok) {
          console.error("Whop event failed", {
            status: response.status,
            response: responseBody,
            payload,
          });

          return;
        }

        console.log(
          "Whop event sent",
          payload.event_name,
          responseBody,
        );
      } catch (error) {
        console.error(
          "Whop request failed",
          payload.event_name,
          error,
        );
      }
    }

    async function handleShopifyEvent(event) {
      const checkout = event.data?.checkout;
      const user = buildUser(event, checkout);

      if (!user) {
        console.warn(
          "Shopify event missing client ID",
          event.name,
        );
        return;
      }

      const cookies = await readCookies();
      const isPurchase =
        event.name === "checkout_completed";

      const payload = compact({
        account_id: WHOP_ACCOUNT_ID,
        event_name: getWhopEventName(event.name),
        event_id: getEventId(event, checkout),
        event_time: event.timestamp,
        action_source:
          event.name === "page_viewed" ? null : "website",
        url:
          event.context?.document?.location?.href ||
          null,
        referrer_url:
          event.context?.document?.referrer ||
          null,
        title:
          event.context?.document?.title ||
          null,
        user,
        context: buildContext(event, cookies),
      });

      if (
        isPurchase &&
        checkout?.totalPrice?.amount != null
      ) {
        payload.value = Number(
          checkout.totalPrice.amount,
        );

        const currency =
          checkout.totalPrice.currencyCode ||
          checkout.currencyCode;

        if (currency) {
          payload.currency = currency.toLowerCase();
        }
      }

      await sendWhopEvent(payload);
    }

    analytics.subscribe("page_viewed", handleShopifyEvent);

    analytics.subscribe(
      "all_standard_events",
      async (event) => {
        if (
          event.name === "page_viewed" ||
          !TRACKED_EVENTS.has(event.name)
        ) {
          return;
        }

        await handleShopifyEvent(event);
      },
    );
    ```
  </Step>

  <Step title="Save and connect the pixel" iconType="regular" titleSize="h2">
    Click **Save**, then **Connect**. Shopify won't send any events to an unconnected pixel.
  </Step>

  <Step title="Confirm it's tracking" iconType="regular" titleSize="h2">
    Visit a live storefront page, then check the [Websites page](https://whop.com/dashboard/websites) in your Whop dashboard. Your domain appears there once a page view comes through. Then place a test order and confirm a purchase shows up too.
  </Step>
</Steps>

<Note>
  Whop's automatic server-side purchase tracking only covers checkouts that happen on Whop itself. A Shopify order is processed entirely inside Shopify, so Whop never sees it unless something reports it — that's what this script's `checkout_completed` (sent as `purchase`) is for. Don't remove it thinking it's a duplicate of Whop's own tracking.
</Note>
