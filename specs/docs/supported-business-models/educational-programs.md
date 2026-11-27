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

# Educational Programs

> Sell a course on Whop: host lessons in the Courses app, add content, forums, and chat for your community, design your store page, and set up pricing.

If you want to launch an online business selling a course, Whop puts everything in one place: host your course, create a community around it, and accept payments seamlessly.

## Launch your course business

### 1. Create your whop

Head over to [whop.com/sell](https://whop.com/dashboard/start/):

* Choose your name
* Select your custom URL
* Complete the basic setup

Your whop is now live and ready to customize. For every option in this flow, see [Create a whop](https://docs.whop.com/payments/create-a-whop.md).

![Create course on Whop](https://mintcdn.com/whop/gkU9N_oxv5Gb4qnv/images/createwhopcourse.png?fit=max\&auto=format\&n=gkU9N_oxv5Gb4qnv\&q=85\&s=d14fed36f8763747d43746fa87f87579)

### 2. Add the Courses app

The [**Courses**](https://whop.com/apps/app_0vPZThfBpAwLo/install/) app is where your students will access your educational videos and learning materials.

1. Add the **Courses** app from the [Whop App Store](https://private-docs.mintlify.app/whop-apps/whop-app-store)
2. Select **Begin setup**
3. Select **Add module**
4. Enter the title and description for this module - this helps students understand what they'll learn
5. Start organizing your content into chapters and lessons within each module

You'll find a more detailed breakdown of how to use the **Courses** app [below](#setting-up-your-courses-app).

![Add Courses app](https://mintcdn.com/whop/XkQkLHsY-Hk8KVyG/how-to-videos/addingcoursestocoursewhop.gif?s=048906527c219c7686aad686735d70a6)

### 3. Add the Content app

Use the [**Content**](https://whop.com/apps/app_mmjPLOxIgL1qxe/install/) app to create a "Start here" guide that helps new students get oriented and know exactly what to do first. Create a welcome guide that covers:

* How to navigate your whop and find everything
* Where to begin with your course content
* How to ask questions and get support
* Any introductions or first steps they should take

  ![Add Content app](https://mintcdn.com/whop/XkQkLHsY-Hk8KVyG/images/addingcontenttocourses.png?fit=max\&auto=format\&n=XkQkLHsY-Hk8KVyG\&q=85\&s=6f9018d0da1549c787e1166a54673846)

### 4. Add the Forums app

The [**Forums**](https://whop.com/apps/app_dYfm2IdXhDMquv/install/) app is where you’ll share important updates, announcements, and valuable content that all members should see. Popular forum uses include:

* **Announcements**: Share course updates, new lesson releases, and important information
* **Weekly insights**: Post regular tips, industry updates, or motivational content
* **Student wins**: Highlight student success stories and achievements to inspire others
* **Q\&A discussions**: Answer student questions and encourage peer-to-peer learning

> [!TIP]
>
> You can add the Forums app multiple times to keep different types of discussions organized.

![Add Forums app](https://mintcdn.com/whop/XkQkLHsY-Hk8KVyG/images/addingforumsapptocourse.png?fit=max\&auto=format\&n=XkQkLHsY-Hk8KVyG\&q=85\&s=13750d1770c6c7a55957ea661cebe02b)

### 5. Add the Chat app

Give your students a place to connect and ask questions. Popular uses of the [**Chat**](https://whop.com/apps/app_xml5hbizmZPgUT/install/) app include:

* **General chat**: Main conversation space for students to hang out and ask questions
* **Introductions**: Help new students connect and share their learning goals
* **Assignment submissions**: Create spaces for students to share their work and get feedback

> [!TIP]
>
> Add multiple Chat apps to create separate spaces for different topics or student cohorts.

![Chat app added to a course](https://mintcdn.com/whop/XkQkLHsY-Hk8KVyG/images/addingchatapptocourse.png?fit=max\&auto=format\&n=XkQkLHsY-Hk8KVyG\&q=85\&s=19e0b4fcd54364559afd4e0687b6bb87)

### 6. Design your store page

Your store page is where people can learn about your offer. Select your whop name in the top left, then select **Design store page** to open the editor. Select **Edit details** to customize:

* **Choose a clear name and headline**: Make it immediately obvious what students will learn and how it will benefit them. Examples: "Master Facebook Ads in 30 Days" or "Complete Python Programming Boot Camp"
* **Write a compelling description**: Focus on the transformation students will experience and specific skills they'll gain. Highlight outcomes like "land your first coding job," "double your ad ROI," or "launch your consulting business"
* **Upload a logo**: Add a clean, simple logo to your store page
* **Add gallery images or video**: Viewers see this content at the top of your store page. Add a short video about you, your expertise, and what students will learn in the course

  ![Course store page](https://mintcdn.com/whop/gkU9N_oxv5Gb4qnv/images/storepagecourse.png?fit=max\&auto=format\&n=gkU9N_oxv5Gb4qnv\&q=85\&s=c7a985fb2a22bc3408294863c2f29803)

### 7. Set up pricing

Set up your pricing to choose how much people have to pay to get access to your course.

* Go to your whop > select **Products** > select **Add product**

You can structure your pricing as free access, a one-time payment, or a recurring subscription:

* **Free**: Make it free to access your course (then upsell a higher ticket offer or done-for-you services)
* **One-time payment**: For example, \$297 for lifetime access to your complete course
* **Subscription model**: For example, \$47/month for ongoing access with a continuing stream of new content

[Learn more about setting up pricing and products here](https://docs.whop.com/manage-your-business/payment-processing/set-up-pricing.md).

> [!TIP]
>
> You can also create multiple pricing tiers (for example, a basic course, premium with bonuses, and VIP with 1-on-1 coaching). Start simple - you can always add more options as your business grows.

## Setting up your Courses app

Now that you've set up the basic structure of your course business, learn how to use the **Courses** app in more detail.

### Understanding the course structure

Organize your course content with a three-level hierarchy:

1. **Modules**: These are your main course topics. Think "Beginner basics," "Advanced techniques," "Bonus resources," or "Live call recordings."
2. **Chapters**: These break each module into smaller, bite-sized sections that are easier to follow. Each chapter should focus on a specific subtopic or learning objective that builds toward the module's overall goal.
3. **Lessons:** This is where you actually teach - each lesson is one video, PDF, quiz, or piece of content (more info on this below).

### Types of lessons you can create

The **Courses** app gives you flexibility in how you deliver your content:

> [card] Multimedia
>
> Combine text, video, and downloads in one comprehensive lesson.

> [card] Video
>
> Upload or embed videos that students can watch at their own pace.

> [card] Text
>
> Written guides and step-by-step instructions with rich formatting.

> [card] PDF
>
> Share downloadable worksheets, templates, and reference materials.

> [card] Knowledge check
>
> Ungraded questions that help students self-assess their progress.

> [card] Quiz
>
> Graded tests to certify student mastery before they advance.

### Course settings

In the **Settings** panel for each module, you can customize how students experience your content:

* **Description**: Add context about what students will learn in this specific module.
* **Require lesson completion**: Toggle this on to create a structured learning path where students must complete lessons in order. This ensures they don't skip ahead and miss important foundational concepts.
* **Completion certificates**: Generate professional certificates when students finish your course.
* **Language settings**: Choose the language for closed captions on your videos.

## Next steps

> [card] [Grow your course business on Whop](https://docs.whop.com/manage-your-business.md)
>
> Learn how to use Whop's tools and features to get more students.


## Related topics

- [Get Started with Whop](https://docs.whop.com/get-started.md)
- [Affiliate Program](https://docs.whop.com/manage-your-business/growth-marketing/affiliate-program.md)
- [Dispute Monitoring Programs](https://docs.whop.com/trust-and-safety/account-health/dispute-monitoring-programs.md)
