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

# Bounties

> Post paid tasks with an escrowed reward, collect submitted work from workers, and pay out on approval.

A bounty is a paid task. You describe the work, fund a reward pool up front, and workers submit their work for review. Whop escrows the money when the bounty is created and releases it when a submission is approved, so a worker can see the reward is real before starting.

## What you can build

* **Post tasks** with a funded reward, one-off or on a repeating schedule.
* **Restrict who can work them** by country.
* **Collect submissions** as links, uploaded files, or both.
* **Run a clipping or UGC program** by declaring what the work is meant to achieve.

## Core objects

Bounty, bounty submission. Each is defined in [Core Concepts](https://docs.whop.com/developer/concepts.md).

**bounty** `bnty_`

The task and its funded reward pool, created by the poster. See [Bounties](https://docs.whop.com/api-reference/bounties/bounty).

**bounty submission** `btys_`

One worker's attempt, carrying the submitted work, and a review status. See [Bounty Submissions](https://docs.whop.com/api-reference/bounty-submissions/retrieve-bounty-submission.md).

## Where to go next

> [card] [Run a bounty program](https://docs.whop.com/developer/bounties/run-a-bounty-program.md)
>
> Post, fund, review, and cancel, end to end.

> [card] [Submit to a bounty](https://docs.whop.com/api-reference/bounty-submissions/create-bounty-submission.md)
>
> The worker side, in one call.

> [card] [Upload files](https://docs.whop.com/developer/guides/upload-files.md)
>
> Get the file IDs a submission references.

> [card] [Bounties reference](https://docs.whop.com/api-reference/bounties/bounty)
>
> Every bounty endpoint, with a playground.


## Related topics

- [Bounties API](https://docs.whop.com/api-reference/bounties/bounty.md)
- [List bounties](https://docs.whop.com/api-reference/legacy/bounties/list-bounties.md)
- [Create bounty](https://docs.whop.com/api-reference/legacy/bounties/create-bounty.md)
