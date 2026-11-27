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

# App Views

> Configure where your app appears on Whop: an experience view for members, a dashboard view for creators, or both.

App views determine where and how Whop displays your app. You can configure one or more views depending on your app's functionality and target audience.

Each view serves a different purpose and appears in a different context on the platform.

## Experience view

The experience view is where members interact with your app's core features. Apps with this view appear directly in the Whop sidebar alongside native features like chat, forums, and courses.

When creators install your app, they can create one or more experiences powered by your app. Each experience maps to a single item in the sidebar and can render unique custom content.

Each instance of your experience view has a unique experience id which looks like `exp_xxxxxxxx`.

![Experience View](https://mintcdn.com/whop/10U_4RMUCDhk64co/images/app-experience-view.jpeg?fit=max\&auto=format\&n=10U_4RMUCDhk64co\&q=85\&s=695a5e126942d5b709b641edd3f3a513)

### When to use experience view

Experience view is ideal for consumer-focused apps that members interact with:

* Live quizzes and polls that run directly within communities
* Custom course delivery systems with interactive content
* Gated content libraries with videos, files, or downloads
* Community games and interactive experiences
* Real-time collaboration tools for members

### Configure experience view

**1. Go to your app's hosting settings**

1. Go to the [developer dashboard](https://whop.com/dashboard/developer)
2. Create a new app or select an existing one
3. Scroll down to the **Hosting** section

![App Settings](https://mintcdn.com/whop/10U_4RMUCDhk64co/images/app-settings-hosting.png?fit=max\&auto=format\&n=10U_4RMUCDhk64co\&q=85\&s=428a4fa02ead35e8f4e3de4c368d744b)

**2. Enter your path**

Enter your path for the experience view. The recommended default path is `/experiences/[experienceId]`.

![Experience View Path](https://mintcdn.com/whop/10U_4RMUCDhk64co/images/experience-view-path.png?fit=max\&auto=format\&n=10U_4RMUCDhk64co\&q=85\&s=09b247eb50e14ee3f0dc4bf5e903d099)

* `[experienceId]` is used to provide the accessed experience ID: `/experiences/[experienceId] -> /experiences/exp_***`
* `[restPath]` is used for deep linking to specific sections of your app: `/experiences/[experienceId]/[restPath] -> /experiences/exp_***/posts/1`

### Preview experience view

**1. Install your app**

Select the install button, or copy the installation link and visit it in your browser. You'll be prompted to install your app into your whop.

![Install App Button](https://mintcdn.com/whop/10U_4RMUCDhk64co/images/app-settings-install-app-button.png?fit=max\&auto=format\&n=10U_4RMUCDhk64co\&q=85\&s=9a5bbb5ab8d08cfb419aaa64b0754765)

If you've already installed your app, you can access it from your whop.

![Whop Sidebar Apps Section](https://mintcdn.com/whop/YCN8CE8TO9oEZrci/images/whop-experiences.png?fit=max\&auto=format\&n=YCN8CE8TO9oEZrci\&q=85\&s=3b441cbe9f4a93a4c58d9ab83a6f59f0)

**2. Set the environment**

1. Open the developer tools by selecting the **cog** button
2. Set the environment to **localhost**

<video controls src="https://mintcdn.com/whop/wmug03bhIwuWRAcH/how-to-videos/experience-app-preview-set-localhost.mp4?fit=max&auto=format&n=wmug03bhIwuWRAcH&q=85&s=707023002211309d87f552fab95a7b5d" />

### Validate experience access

> [!WARNING]
>
> `verifyUserToken` is not available in the current `@whop/sdk`. The validation snippets on this page are kept for reference and will not run as written.

Check if a user has access to an experience using the `checkAccess` method. Whop passes the experience ID as a path parameter when your app loads.

```typescript theme={null}
import { headers } from "next/headers";
import { whopsdk } from "@/lib/whop-sdk";

export default async function ExperiencePage({
    params,
}: {
    params: Promise<{ experienceId: string }>;
}) {
    const { experienceId } = await params;
    const { userId } = await whopsdk.verifyUserToken(await headers());

    const access = await whopsdk.users.checkAccess({
        id: userId,
        resource_id: experienceId,
    });

    if (!access.has_access) {
        return <div>Access denied</div>;
    }

    // access.access_level can be:
    // "customer" - User has a valid membership
    // "admin" - User is a team member

    return <div>Welcome to the experience!</div>;
}
```

See [Authentication - Customer app](https://docs.whop.com/developer/guides/authentication.md#customer-app-experience-view) for more information.

### Examples

* [Next.js](https://github.com/whopio/whop-nextjs-app-template/blob/main/app/experiences/%5BexperienceId%5D/page.tsx)
* [React Native](https://github.com/whopio/whop-sdk-ts/blob/main/packages/create-react-native/template/src/views/experience-view.tsx)

## Dashboard view

The dashboard view appears directly in the creator's business dashboard. Use this view for apps that help businesses grow and manage their operations.

Apps with dashboard views appear in the dashboard sidebar under the apps section, making them easy to find when creators need to manage business operations.

![Dashboard View](https://mintcdn.com/whop/10U_4RMUCDhk64co/images/app-dashboard-view.jpeg?fit=max\&auto=format\&n=10U_4RMUCDhk64co\&q=85\&s=b8c203cd338932683757b8843226da6b)

### When to use dashboard view

Dashboard view is ideal for B2B apps that help creators run their business:

* Analytics dashboards showing revenue, member growth, and engagement metrics
* Customer upsell tools that send targeted offers to loyal members
* Member management interfaces for organizing and segmenting customers
* Automated marketing campaigns and email builders
* Custom admin panels for managing app-specific settings

### Configure dashboard view

**1. Go to your app's hosting settings**

1. Go to the [developer dashboard](https://whop.com/dashboard/developer)
2. Create a new app or select an existing one
3. Scroll down to the **Hosting** section

![App Settings](https://mintcdn.com/whop/10U_4RMUCDhk64co/images/app-settings-hosting.png?fit=max\&auto=format\&n=10U_4RMUCDhk64co\&q=85\&s=428a4fa02ead35e8f4e3de4c368d744b)

**2. Enter your path**

Enter your path for the dashboard view. The recommended default path is `/dashboard/[companyId]`.

![Dashboard View Path](https://mintcdn.com/whop/10U_4RMUCDhk64co/images/dashboard-view-path.png?fit=max\&auto=format\&n=10U_4RMUCDhk64co\&q=85\&s=24abea4609726b03e150679e9081138e)

* `[companyId]` is used to provide the accessed account ID: `/dashboard/[companyId] -> /dashboard/biz_***`
* `[restPath]` is used for deep linking to specific sections of your app: `/dashboard/[companyId]/[restPath] -> /dashboard/biz_***/posts/1`

### Preview dashboard view

**1. Install your app**

Select the preview button next to the field to open your app's dashboard view. You'll be prompted to install your app if you haven't already.

![Preview App Button](https://mintcdn.com/whop/YCN8CE8TO9oEZrci/images/preview-dashboard-app-button.png?fit=max\&auto=format\&n=YCN8CE8TO9oEZrci\&q=85\&s=16043cb9c3553600f4d81942bde76904)

If you've already installed your app, you can access it from your dashboard under the **apps** section.

![Dashboard Sidebar Apps Section](https://mintcdn.com/whop/10U_4RMUCDhk64co/images/dashboard-apps-sidebar-section.png?fit=max\&auto=format\&n=10U_4RMUCDhk64co\&q=85\&s=b0ab34c95536596c42f59b51b7dff586)

**2. Set the environment**

1. Open the developer tools by selecting the **cog** button
2. Set the environment to **localhost**

<video controls src="https://mintcdn.com/whop/wmug03bhIwuWRAcH/how-to-videos/dashboard-app-preview-set-localhost.mp4?fit=max&auto=format&n=wmug03bhIwuWRAcH&q=85&s=6f0eb2a0a7c60799a3bae0054697c208" />

### Validate account access

Only account admins should have access to dashboard apps. Check access using the `checkAccess` method with the account ID.

```typescript theme={null}
import { headers } from "next/headers";
import { whopsdk } from "@/lib/whop-sdk";

export default async function DashboardPage({
    params,
}: {
    params: Promise<{ companyId: string }>;
}) {
    const { companyId } = await params;
    const { userId } = await whopsdk.verifyUserToken(await headers());

    const access = await whopsdk.users.checkAccess({
        id: userId,
        resource_id: companyId,
    });

    if (access.access_level !== "admin") {
        return <div>Admin access required</div>;
    }

    return <div>Welcome to the dashboard!</div>;
}
```

See [Authentication - Dashboard app](https://docs.whop.com/developer/guides/authentication.md#dashboard-app-dashboard-view) for more information.

### Examples

* [Next.js](https://github.com/whopio/whop-nextjs-app-template/blob/main/app/dashboard/%5BcompanyId%5D/page.tsx)
* [React Native](https://github.com/whopio/whop-sdk-ts/blob/main/packages/create-react-native/template/src/views/dashboard-view.tsx)

## Choosing the right views

You can configure multiple views for your app depending on its functionality:

* **Consumer apps**: Use Experience View to serve members with interactive features
* **Business apps**: Use Dashboard View to help creators manage operations
* **Hybrid apps**: Use both views to serve both members and creators

Configure your views in the Hosting section of your app dashboard to get started.


## Related topics

- [Request App Permissions](https://docs.whop.com/developer/guides/permissions.md)
- [Authenticate Users in Whop Apps](https://docs.whop.com/developer/guides/authentication.md)
- [Build Whop Apps](https://docs.whop.com/developer/apps/overview.md)
