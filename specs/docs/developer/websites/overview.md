> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Websites

> Ship a fully hosted website on Whop — served at your own whop.site address, versioned on every deploy, and wired to payments and analytics.

A Whop website is an app of type `website`: a site visitors browse at `<route>.whop.site`, hosted by Whop. No server to provision, no deploy pipeline to wire. Write a Vite app and run one command, or start from a [blueprint](/developer/websites/blueprints): a working business with products already set up.

<Warning>
  Choose the app type up front. A `website` can't be converted into a [Whop app](/developer/guides/app-views) (`b2c_app`) later.
</Warning>

## Four ways to start

|                 | Deploy a blueprint                          | Describe it to AI                         | Build it with the CLI       | Migrate an existing site       |
| --------------- | ------------------------------------------- | ----------------------------------------- | --------------------------- | ------------------------------ |
| **Effort**      | One step                                    | A few minutes of chat                     | A normal dev loop           | One step                       |
| **You get**     | A working business with products and a site | A site built from your prompt             | Your own code and framework | A copy of a site you run       |
| **Code access** | Via `whop apps init --template`             | Edit in the dashboard, or pull the source | It's your repo              | Yes                            |
| **Best for**    | Launching today                             | Non-developers, first drafts              | Anything custom             | A second brand, a staging copy |

<Note>
  Blueprints share revenue both ways. Deploy one and its creator earns 10% of Whop's profit on your site's sales. Publish one and you earn 10% from every business that deploys it. See [The 10% affiliate cut](/developer/websites/blueprints#the-10-affiliate-cut).
</Note>

This section covers everything except the AI path. For that one, open **Websites** in your [dashboard](https://whop.com/dashboard), select **New website**, then **Create your own**.

## What you get

* **Payments.** Use checkout links or embedded checkout — see [Accept payments](/developer/guides/accept-payments).
* **A live address** at `<route>.whop.site`, as soon as the first build is promoted.
* **Versioned deploys.** Promoting an older build is how you roll back, from the CLI or the **Versions** tab.
* **API calls with no key handling.** The runtime signs server-side requests to the Whop API. Your code never sees the key — see [Hosting](/developer/websites/hosting#call-the-whop-api).
* **The Whop pixel, pre-installed.** Page views and checkout conversions are tracked with no snippet to paste — see [Track visitors](/developer/websites/tracking).

## Next steps

<CardGroup cols={2}>
  <Card title="Blueprints" href="/developer/websites/blueprints">
    Deploy a working business — products, pricing, and a site — in one step.
  </Card>

  <Card title="Build a website with the CLI" href="/developer/websites/quickstart">
    Scaffold, run locally, and deploy your first site in four commands.
  </Card>

  <Card title="How hosting works" href="/developer/websites/hosting">
    Builds, secrets, logs, rollbacks, and the injected API key.
  </Card>

  <Card title="Accept payments" href="/developer/guides/accept-payments">
    Add checkout to the site you just shipped.
  </Card>
</CardGroup>
