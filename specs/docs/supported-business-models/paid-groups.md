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

# Paid Groups

> Set up a paid group on Whop step by step, from adding community apps to designing your page and setting a price.

Anyone can start a paid group. Pick a niche that interests you, or use a [niche idea generator](https://originality.ai/blog/niche-idea-generator). Then create a paid group around it. Paid groups often have a group chat, an announcements board, and a monthly membership fee.

## Launch a paid group

### 1. Create your whop

Head over to [**whop.com/sell**](https://whop.com/dashboard/start/):

* Choose a memorable name for your community
* Select your custom URL
* Complete the basic setup

Your whop is now live and ready to customize. For every option in this flow, see [Create a whop](https://docs.whop.com/payments/create-a-whop.md).

![Create a community](https://mintcdn.com/whop/gkU9N_oxv5Gb4qnv/images/createawhopcommunity.png?fit=max\&auto=format\&n=gkU9N_oxv5Gb4qnv\&q=85\&s=36ae05914ab535e5b0fed4ae9643d0bd)

### 2. Add the Forums app to share updates and announcements

The [**Forums**](https://whop.com/apps/app_dYfm2IdXhDMquv/install/) app is your primary channel for communicating with your entire community. This is where you'll share important updates, announcements, and valuable content that all members should see. Popular forum uses include:

* **Announcements:** Share important community updates, new features, and policy changes
* **Daily updates:** Post regular insights, tips, or industry news relevant to your community
* **Success stories:** Highlight member wins and achievements to inspire and motivate others
* **Weekly roundups:** Summarize key discussions, share upcoming events, or recap important content
* **Special offers:** Announce exclusive deals, promotions, or limited-time opportunities for members
* **New resources:** Alert members when you've added new courses, content, or tools
* **Event recaps:** Share highlights and key takeaways from live sessions or workshops

> [!TIP]
>
> You can add the Forums app multiple times (and rename them to whatever you want) to keep your communications organized and easy to follow.

![Add forums](https://mintcdn.com/whop/XkQkLHsY-Hk8KVyG/how-to-videos/community-forums.gif?s=cfda7065a4ea0c9478675d5384a4a664)

### 3. Add the Chat app

Give your members a place to talk, share tips, and hype each other up. Add the [**Chat**](https://whop.com/apps/app_xml5hbizmZPgUT/install/) app and customize it for your community's needs:

* **General chat:** Main conversation space for casual discussions
* **Introductions:** Help new members connect and introduce themselves
* **Topic-specific chats:** Focus conversations around specific interests or goals

> [!TIP]
>
> Add multiple Chat apps to create separate spaces for different topics or member groups.

![Add Chat app](https://mintcdn.com/whop/XkQkLHsY-Hk8KVyG/images/Chatappcommunity.png?fit=max\&auto=format\&n=XkQkLHsY-Hk8KVyG\&q=85\&s=ec6439ae899c3ed0fc0ba4b0db6204fd)

### 4. Provide learning resources and materials

The most successful communities provide resources and learning materials that help their members achieve their goals. Here's how:

1. **Add the Courses app**

Create structured learning experiences with the [**Course**](https://whop.com/apps/app_xml5hbizmZPgUT/install/) app. After adding the Course app, select **Configure Courses**, then **Create your course now** to begin building your educational content. Popular course topics include:

* **Course for new members**: Walk new members through your methodology and approach
* **Skill-building series**: Teach core concepts and techniques in your area of expertise
* **Call recordings**: Store all your live session recordings so members can access past content anytime

2. **Add the Content app**

The [**Content**](https://whop.com/apps/app_mmjPLOxIgL1qxe/install/) app gives you unlimited flexibility to share text-based resources with your community:

* **Welcome guide:** Help new members understand how to get the most value from your community from day one
* **Community guidelines:** Set expectations for behavior and participation
* **Resource library:** Share templates, checklists, and reference materials
* **FAQ section:** Answer common questions to support members and reduce repetitive inquiries

  ![Add resources](https://mintcdn.com/whop/gkU9N_oxv5Gb4qnv/images/resourcescommunity.png?fit=max\&auto=format\&n=gkU9N_oxv5Gb4qnv\&q=85\&s=4a37e0f544d59c0cb447a49ec5aafc03)

### 5. Add the Events app to schedule live sessions

The **Events** app makes it easy to schedule and manage all your live sessions. Whether you're meeting in person, on Zoom, Google Meet, or using Whop's built-in livestream, your members can:

* See all upcoming events
* Add them to their calendar
* Get notified before the event starts

Use the [**Events**](https://whop.com/apps/app_RnVNENx5vtP8Zp/install/) app to schedule:

* **Weekly live sessions:** Regular training, Q\&A, or discussion sessions
* **Monthly workshops:** Deep-dive training on specific topics
* **Guest expert interviews:** Bring in outside experts to share knowledge
* **Community meetups:** Social events to strengthen member relationships

To schedule events, add the **Events** app from the Whop App Store, select **Configure Events**, then **Create event**.

> [!TIP]
>
> Pro tip: Use Whop's built-in livestream feature to broadcast directly to your community. Members get notified when you go live, and streams are automatically saved for anyone who missed them.

![Add Events app](https://mintcdn.com/whop/XkQkLHsY-Hk8KVyG/images/Eventsappcommunity.png?fit=max\&auto=format\&n=XkQkLHsY-Hk8KVyG\&q=85\&s=2a64e1d9945a956daccede04b5891b2c)

### 6. Design your product page

Your product page is where people can learn about your offer. Select **Products** in your dashboard, then select **Create product** to open the editor. Use the fields and sections to customize your page:

* **Choose a product name and headline:** Make it clear what your product offers and who it’s for. Examples: “AI Tools for Marketers” or “Coding Boot Camp for Beginners.”
* **Write a compelling description:** Focus on the value customers receive and how your product helps them achieve results. Highlight benefits like expert guidance, exclusive resources, or community access.
* **Set pricing:** Choose between free, one-time, or recurring payments. Add multiple pricing options if you offer tiers or different levels of access.
* **Add features:** List what members will gain (e.g., “Weekly live workshops,” “Private chat access,” or “Course library”).

### 7. Set up pricing

Set up your pricing to choose how much your members pay to get access to your community.

* Go to your whop > select **Products** > select **Add product**

Many communities choose a recurring fee (i.e. a subscription model) - for example:

* **\$30/month** for full access
* **\$200/year** for a yearly pass

To do this, select **Recurring** when creating your pricing and select weekly, monthly, or yearly. You can also add multiple pricing options (for example, a weekly plan, a monthly plan, and a lifetime membership).

[Learn more about setting up pricing and products here](https://docs.whop.com/set-up-store-page).

> [!TIP]
>
> Pro tip: Most of the largest communities on Whop use a free + premium model. This means setting up two 'Products' in your whop - one that's free to join, and another with paid access to premium apps (like chats, courses and resources).

## Next steps

> [card] [Grow your community on Whop](https://docs.whop.com/manage-your-business.md)
>
> Learn how to use Whop's tools and features to get more members in your
> community.


## Related topics

- [Get Started with Whop](https://docs.whop.com/get-started.md)
- [Create a Product and Choose a Pricing Strategy](https://docs.whop.com/manage-your-business/products/create-product.md)
- [Invoice Customers from Your Dashboard](https://docs.whop.com/payments/send-an-invoice.md)
