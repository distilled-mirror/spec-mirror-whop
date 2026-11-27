> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

> ## Agent Instructions
> Treat these docs and the Whop MCP server as the source of truth for Whop API behavior. Don't infer endpoint behavior from marketing pages.
> MCP server: connect https://mcp.whop.com/mcp (streamable HTTP, browser sign-in, no API key in the client config) to search these docs and read or change live Whop data. Setup: https://docs.whop.com/developer/guides/ai_and_mcp.md
> Whop CLI: install with `curl -fsSL https://whop.com/install.sh | sh` to scaffold, run, and deploy Whop apps and websites. It acts on production, not the sandbox. Overview: https://docs.whop.com/cli/overview.md Agent mode: https://docs.whop.com/cli/agent-mode.md
> Hosting: Whop serves a site or app from the build `whop apps deploy` uploads, with rollbacks, secrets, and server logs. Guide: https://docs.whop.com/developer/websites/hosting.md
> Plugins: in Claude Code, run `/plugin marketplace add whopio/plugins`, then `/plugin install whop@whop`. Guide: https://docs.whop.com/developer/guides/plugins/claude-code.md Every agent tool: https://docs.whop.com/developer/ai/overview.md
> Environments: build and test in the sandbox (API `https://sandbox-api.whop.com/api/v1`, keys at https://sandbox.whop.com/dashboard/developer, SDK environment `Sandbox`; no real money). Production (API `https://api.whop.com/api/v1`, keys at https://whop.com/dashboard/developer, SDK environment `Production`, the SDK default) moves real money, so use it only at launch. The two share no accounts, keys, products, or webhooks, and their keys look identical: never mix them. Sandbox guide: https://docs.whop.com/developer/guides/sandbox.md Comparison: https://docs.whop.com/developer/quickstart.md
> API: send `Authorization: Bearer <API_KEY>` from your server only, and pin a dated version with the `Api-Version-Date` header. OpenAPI spec: https://api.whop.com/api/v1/openapi.json
> Which API: build on the Current API, whose pages live under https://docs.whop.com/api-reference/ (start at https://docs.whop.com/api-reference/overview.md). Pages under /api-reference/legacy/ are the Legacy API. Use one only when https://docs.whop.com/api-reference/stability.md lists the resource as Legacy-only.

# Agencies

> Set up your agency on Whop to attract prospects with a free whop, then take client payments and run projects in private spaces.

You can manage lead generation, take sales calls, showcase case studies, run private client portals, handle payments, and coordinate your team - all in Whop.

## How it works

**For prospects:** Create a free whop with your case studies, resources, and a call booking link.

**For clients:** Send private checkout links, collect payments, and give access to private project spaces where you can chat, share project materials, and manage timelines.

## Launch your agency on Whop in 9 steps

### 1. Create your whop

Head over to [**whop.com/sell**](https://whop.com/dashboard/start/) and get your community up and running:

* Choose a memorable name for your agency
* Select your custom URL
* Complete the basic setup

Your whop is now live and ready to customize.

![New agency whop](https://mintcdn.com/whop/gkU9N_oxv5Gb4qnv/images/createwhopagencies.png?fit=max\&auto=format\&n=gkU9N_oxv5Gb4qnv\&q=85\&s=965a67dbb61d4b467779306c62dfad75)

### 2. Add the Calendar Bookings app

Like Calendly, this app lets you configure a calendar where prospects can schedule discovery sales calls.

1. Add the [**Calendar Bookings**](https://whop.com/apps/app_dC2dkRARmXE4qf/install/) app from the Whop App Store, then select **Configure Calendar Bookings** to set up your availability
2. Select **Add availability** or **+** at the top right
3. Add the: 
   * **Title** 
   * **Duration** 
   * **Meeting link/Location**
   * You can optionally add a **Description** and a **Price per booking**
4. Select **Continue**
5. Select the **Days** and **Time** you’ll be available
6. Select **Create**

   ![Add Calendar Bookings app](https://mintcdn.com/whop/XkQkLHsY-Hk8KVyG/how-to-videos/calendarbookingstoagency.gif?s=d3b0e54894bb55501e1c2b2ec1b626c7)

### 3. Add the Files app

Add high value materials and documents using the [**Files**](https://whop.com/apps/app_5UWRrs7haZZeiQ/install/) app to turn your whop into a lead magnet.

* **Case studies:** Showcase client work, before/after results, and success stories
* **Free resources:** Upload templates, audits, checklists, or anything you'd normally offer as a lead magnet

Leads join your whop to access the resources. You capture their details, and they can browse your whop and book a call.

![Add Files app](https://mintcdn.com/whop/XkQkLHsY-Hk8KVyG/images/addfilestoagency.png?fit=max\&auto=format\&n=XkQkLHsY-Hk8KVyG\&q=85\&s=f7b77f7435c2281eb24e88752a2d3326)

### 4. Add the Content app

The [**Content**](https://whop.com/apps/app_mmjPLOxIgL1qxe/install/) app gives you unlimited flexibility to share text-based resources with your prospects.

* Welcome guide explaining how prospects can get the most value from your whop
* FAQ section answering common questions about your services
* Your methodology or approach to solving client problems

  ![Add Content app](https://mintcdn.com/whop/XkQkLHsY-Hk8KVyG/images/addingcontenttoagencies.png?fit=max\&auto=format\&n=XkQkLHsY-Hk8KVyG\&q=85\&s=f0fc8aa509fde6c62106a93074166158)

### 5. Add the Chat app (optional)

Consider adding a community element to your whop where prospects can ask questions, engage with your content, and stay connected with your agency. Add the [**Chat**](https://whop.com/apps/app_xml5hbizmZPgUT/install/) app from the Whop App Store and rename it to **Ask questions** or **General chat**. This creates a space where prospects can:

* Ask questions about your area of expertise
* Get quick answers and build trust with your agency
* Connect with other prospects

> [!TIP]
>
> Stay active in the chat to build relationships and demonstrate your expertise. Give prompt answers and share valuable insights. This positions you as helpful and knowledgeable before prospects even book a call.

![Add Chat app](https://mintcdn.com/whop/XkQkLHsY-Hk8KVyG/images/addingchattoagencies.png?fit=max\&auto=format\&n=XkQkLHsY-Hk8KVyG\&q=85\&s=bd588e32c4876b1aba849e8dbcd5333b)

### 6. Set up a free pricing model

Now that you've added your apps, create a free product that gives prospects access to everything.

* Go to your whop > select **Products** > select **Add product**

Configure the settings in the product to make it free to access.

[Learn more about setting up pricing and products here](https://docs.whop.com/manage-your-business/payment-processing/set-up-pricing.md).

### 7. Design your store page

Your store page is where prospects decide whether to join your agency's free whop. Select your whop name in the top left, then select **Design store page** to open the editor. Select **Edit details** to customize:

* **Choose a clear name and headline:** Make it immediately obvious what your agency offers and who it's for
* **Write a compelling description:** Focus on the results you deliver for clients
* **Upload a clean logo:** Even a simple logo builds credibility and makes your agency look legit
* **Add gallery images or video:** Include case study screenshots, client testimonials, or a brief welcome video that builds trust and showcases your expertise

  ![Agency store page](https://mintcdn.com/whop/gkU9N_oxv5Gb4qnv/images/storepageagencies.png?fit=max\&auto=format\&n=gkU9N_oxv5Gb4qnv\&q=85\&s=884d10aae8d0a4e03bd011c981455d34)

### 8. Convert leads into paying clients

When someone books a call through your **Calendar Bookings** app and decides to work with you, you can create a paid product (and checkout link) for them. When you close a client, you have two options:

### Option A: Use Whop for payment processing only

Choose this option if you just want to handle the payment through Whop and manage everything else with your current tools.

* Create a new product specifically for that client
* [Choose your pricing](https://docs.whop.com/manage-your-business/payment-processing/set-up-pricing.md) (one-time project fee or recurring retainer)
* Don't toggle on any apps for this product
* Send them the private checkout link to complete payment
* Manage project delivery through your existing tools

### Option B: Create a complete client portal

Give your client a dedicated space inside your whop where you can communicate and share project materials.

* Create a new paid product for the client
* [Choose your pricing structure](https://docs.whop.com/manage-your-business/payment-processing/set-up-pricing.md)
* Add apps in your whop that are only accessible to this client:
  * **Chat**: Private 1:1 communication channel
  * **Files**: Upload project files, contracts, and assets
  * **Events**: Schedule review sessions, deadlines, and milestones
  * **Content**: Share project briefs, strategy documents, or progress updates
  * **Calendar**: Let clients book additional calls or check-ins
* Hide this product from your public store page
* Send the private checkout link after your sales call

### 9. Manage your team

As your agency grows, you can add team members.

* [Learn more about managing your team, roles and permissions here](https://docs.whop.com/manage-your-business/team-management/manage-team-roles.md)

You can even pay your team through whop.

* [Learn how to pay your team on Whop here](https://docs.whop.com/manage-your-business/manage-payouts/pay-your-team.md)

Add internal apps that only your team members can access to keep your operations organized.

* **Files**: Store SOPs, templates, and internal documentation
* **Chat**: Create team communication channels
* **Events**: Schedule team meetings and deadlines
* **Calendar**: Let team members book 1-on-1s with you

## Next steps

> [card] [Grow your agency on Whop](https://docs.whop.com/manage-your-business.md)
>
> Learn how to use Whop's tools and features to get more clients.


## Related topics

- [Get Started with Whop](https://docs.whop.com/get-started.md)
- [Team Management](https://docs.whop.com/manage-your-business/team-management/manage-team-roles.md)
- [Create a Product and Choose a Pricing Strategy](https://docs.whop.com/manage-your-business/products/create-product.md)
