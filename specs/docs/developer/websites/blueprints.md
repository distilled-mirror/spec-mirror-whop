> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Blueprints

> Deploy a working Whop business — products, pricing, and a live website — in one step, or clone its code with the CLI.

A blueprint is a whole Whop business: products with their pricing, and a website that sells them. The one in the gallery is a real business that's running, and deploying it gives you your own copy. Browse them at [whop.com/blueprints](https://whop.com/blueprints).

Deploying a blueprint sets up:

* **Your products**, copied from the blueprint with their pricing plans, images, and store page styling
* **Your website**, live at your own `whop.site` address
* **The wiring between them**, so the site can take payments from the moment it's up

|                       | Curated                         | Community blueprints            |
| --------------------- | ------------------------------- | ------------------------------- |
| **What's in it**      | Blueprints Whop has verified    | Every other published blueprint |
| **How it gets there** | Whop verifies it                | Published by its owner          |
| **Sorting**           | Most businesses created from it | Trending or newest              |

The rows partition the set, so a verified blueprint appears in the curated row only. Each card shows how many businesses have been created from it.

## Deploy a blueprint

<Tabs>
  <Tab title="From the gallery">
    1. Open [whop.com/blueprints](https://whop.com/blueprints) and select a blueprint
    2. Select the deploy button on its detail page

    Whop creates your business if you don't have one, copies the products and the site over, and serves it at your route. Nothing builds on your machine.
  </Tab>

  <Tab title="From your dashboard">
    1. Open **Websites** in your [dashboard](https://whop.com/dashboard)
    2. Select **New website**, then **Use a blueprint**
    3. Pick one from the gallery

    The new site appears in the **Websites** list, where the **Edit**, **Settings**, and **Versions** tabs let you change it, rename its address, and roll builds back.
  </Tab>
</Tabs>

## The 10% affiliate cut

Blueprints are a revenue stream for the people who publish them. When a business deploys your blueprint, you earn **10% of Whop's profit** on the sales its site makes — attributed automatically, the same model as [referring a business to Whop](/refer-businesses-to-whop).

It comes out of Whop's share, not the seller's: if you deploy another creator's blueprint, their 10% doesn't change your pricing or your payouts. The cut is on sales only, and it's tied to the deployed `whop.site` site — the sales that run through the copy you shipped are the ones that count.

## Clone the source

Deploying gives you a running business. To change how the site works, get the code:

```bash theme={null}
whop apps init --template app_xxxxxxxx --name "Shine Time" --route shine-time
```

This registers a new app and downloads the source archive from the blueprint's production build. The directory links to *your* app, so the first `whop apps deploy` ships to your route. Cloning a website blueprint carries the `website` type over, and `--app_type` overrides it.

From there it's an ordinary project — see the [quickstart](/developer/websites/quickstart).

## Next steps

<CardGroup cols={2}>
  <Card title="Build a website with the CLI" href="/developer/websites/quickstart">
    Start from scratch instead of from an existing site.
  </Card>

  <Card title="How hosting works" href="/developer/websites/hosting">
    Secrets, rollbacks, logs, and the injected API key.
  </Card>

  <Card title="Accept payments" href="/developer/guides/accept-payments">
    Turn the site you deployed into something that sells.
  </Card>

  <Card title="Track visitors" href="/developer/websites/tracking">
    The pixel is already installed — add your own events.
  </Card>
</CardGroup>
