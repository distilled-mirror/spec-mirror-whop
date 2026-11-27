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

# Run a Bounty Program

> Post paid bounties through the API, fund the escrow from an account balance, schedule repeats, take and review submissions, and cancel bounties.

Post paid tasks from your own product, fund them from an account's balance, and pull the submission queue into your own interface. This guide covers the full loop: creating a bounty, taking work, reviewing it, and cancelling.

> [!NOTE]
>
> You need an API key with `bounty:create` and `payout:transfer_funds`, because posting a bounty moves money. Reading submissions needs `bounty:basic:read`.

## Post a bounty

[`POST /bounties`](https://docs.whop.com/api-reference/bounties/create-bounty) needs a `title`, a `description` holding the instructions a worker follows, and a `gross_reward_amount`.

```typescript TypeScript theme={null}
import { WhopClient } from "@whop/sdk";

const client = new WhopClient({ token: process.env.WHOP_API_KEY });

const bounty = await client.bounties.create({
	title: "Clip our launch stream",
	description: "Cut a 30 to 60 second vertical clip and post it to TikTok. Include the product name in the caption.",
	gross_reward_amount: 25,
	accepted_submissions_limit: 4,
	business_goal_type: "clipping",
	account_id: "biz_XXXXXXXX",
	allowed_country_codes: ["US", "CA"],
});

console.log(bounty.id);
```

```python Python theme={null}
import os
from whop_sdk import Whop

client = Whop(token=os.environ["WHOP_API_KEY"])

bounty = client.bounties.create(
    title="Clip our launch stream",
    description="Cut a 30 to 60 second vertical clip and post it to TikTok. Include the product name in the caption.",
    gross_reward_amount=25,
    accepted_submissions_limit=4,
    business_goal_type="clipping",
    account_id="biz_XXXXXXXX",
    allowed_country_codes=["US", "CA"],
)

print(bounty.id)
```

### What the reward costs you

`gross_reward_amount` is whole US dollars **per accepted submission**, not per bounty. `accepted_submissions_limit` sets how many submissions can win, and defaults to `1`. Whop escrows the two multiplied together, and that total must be at least $5. The example above escrows $100.

Platform fees and affiliate shares come out of the gross amount rather than being added on top, so a $25 reward costs the funder $25.

### Where the money comes from

The pool draws on the caller's personal balance by default. Pass `account_id` to fund it from an account's balance instead, which needs permission to move that account's funds.

### Where the bounty lives

`experience_id` places a bounty in a specific experience, public for an open bounty or private for an invited one, and **it's required unless you set `account_id`**. When you set `account_id` and omit the experience, the bounty anchors in that account's public forum.

### Declaring the goal

`business_goal_type` says what the work is meant to achieve, and you declare it once when you create the bounty. It takes `clipping`, `post_engagement`, `owned_account_growth`, `ugc_content`, `local_activation`, `data_capture`, or `other`.

`capture_spec` is only accepted when the goal is `data_capture`. Sending it with any other goal fails.

## Schedule and repeat

A bounty publishes as soon as you create it, unless you set `publish_at`. Whop creates it as a hidden draft, then funds and publishes it at the time you named. `publish_at_timezone` is required whenever `publish_at` is set.

`frequency` turns a scheduled bounty into a recurring one. It defaults to `once`, takes `hourly`, `daily`, `weekly`, or `monthly`, and **only applies alongside `publish_at`**. Each occurrence is a separate bounty with its own submissions and its own escrow.

```typescript TypeScript theme={null}
const weekly = await client.bounties.create({
	title: "Weekly product clip",
	description: "One vertical clip per week featuring a different product from the catalog.",
	gross_reward_amount: 50,
	account_id: "biz_XXXXXXXX",
	business_goal_type: "clipping",
	publish_at: "2026-09-01T15:00:00Z",
	publish_at_timezone: "America/New_York",
	frequency: "weekly",
});
```

```python Python theme={null}
weekly = client.bounties.create(
    title="Weekly product clip",
    description="One vertical clip per week featuring a different product from the catalog.",
    gross_reward_amount=50,
    account_id="biz_XXXXXXXX",
    business_goal_type="clipping",
    publish_at="2026-09-01T15:00:00Z",
    publish_at_timezone="America/New_York",
    frequency="weekly",
)
```

## Edit a bounty

What [`PATCH /bounties/{id}`](https://docs.whop.com/api-reference/bounties/update-bounty) accepts depends on whether the bounty has published. A **scheduled draft** accepts everything: the reward, the winner slots, the schedule, and the content fields.

A **published bounty** accepts content only, meaning `title`, `description`, `allowed_country_codes`, and `business_goal_type`. Even those are refused once the bounty stops accepting attempts, or once any submission reaches `submitted` or `approved`. The reward, the winner slots, and the schedule are all fixed once a bounty publishes.

`accepted_submissions_per_user_limit` caps how many slots one worker can win. It defaults to `1`, can't exceed `accepted_submissions_limit`, and a worker runs one attempt at a time.

## Take submissions

[`POST /bounty_submissions`](https://docs.whop.com/api-reference/bounty-submissions/create-bounty-submission) takes the `bounty_id` and a `deliverable`. Combine `urls`, `file_ids`, and a `caption`, with **at least one link, or one file**. Whop rejects an empty deliverable. On a standard bounty this single call is the whole worker flow, and the submission goes straight to review. Submissions need a user credential, so an Account API key can't author one.

```typescript TypeScript theme={null}
const submission = await client.bountySubmissions.create({
	bounty_id: "bnty_XXXXXXXX",
	deliverable: {
		urls: ["https://www.tiktok.com/@creator/video/1234567890"],
		file_ids: ["file_XXXXXXXX"],
		caption: "Vertical cut, 42 seconds.",
	},
});
```

```python Python theme={null}
submission = client.bounty_submissions.create(
    bounty_id="bnty_XXXXXXXX",
    deliverable={
        "urls": ["https://www.tiktok.com/@creator/video/1234567890"],
        "file_ids": ["file_XXXXXXXX"],
        "caption": "Vertical cut, 42 seconds.",
    },
)
```

`allowed_country_codes` restricts who can work a bounty. It takes ISO 3166 alpha-2 codes. An empty list means worldwide.

A worker can withdraw their own attempt with [`DELETE /bounty_submissions/{id}`](https://docs.whop.com/api-reference/bounty-submissions/cancel-bounty-submission). Only the worker who started it can, and an Account API key can't do it for them.

### Goals your integration can't complete

Two goal types accumulate their proof outside the API. On a `data_capture` bounty, create the submission **without** a deliverable: that starts a claimed attempt whose clips accumulate server-side. Proof for a livestream bounty runs through a surface the public API doesn't expose. Your integration can post these bounties and read their queues, but a worker finishes the attempt in Whop.

## Review the queue

Read submissions with [`GET /bounty_submissions`](https://docs.whop.com/api-reference/bounty-submissions/list-bounty-submissions), filtered by `bounty_id` and optionally by `status`.

```typescript TypeScript theme={null}
const queue = await client.bountySubmissions.list({
	bounty_id: "bnty_XXXXXXXX",
	status: "submitted",
});

for (const submission of queue.data) {
	console.log(submission.id, submission.status);
}
```

```python Python theme={null}
queue = client.bounty_submissions.list(
    bounty_id="bnty_XXXXXXXX",
    status="submitted",
)

for submission in queue.data:
    print(submission.id, submission.status)
```

A submission moves through four states:

| `status` | Meaning |
| - | - |
| `in_progress` | An active attempt that hasn't submitted proof |
| `submitted` | Awaiting a decision |
| `approved` | Accepted, and the escrowed reward pays out |
| `denied` | Rejected. `denial_reason` carries the reason when a presentable one exists, and is `null` otherwise |

> [!WARNING]
>
> Approving and denying happen in the Whop dashboard. The API has no approve or deny operation, so your product can show the queue and its outcomes, but a person decides in Whop.

Bounties emit no webhooks. To reflect outcomes in your own product, poll `GET /bounty_submissions` filtered to the bounty. Once a minute suits a queue a person reviews. Back off to every few minutes when a bounty has no recent activity.

## Cancel a bounty

[`POST /bounties/{id}/cancel`](https://docs.whop.com/api-reference/bounties/cancel) refunds the funder immediately when no work is in flight. When work is under review, it stops new submissions and cancels once that work resolves and pays out.

Repeating the request does nothing. A bounty that already paid out every slot returns a `400`.

## Requests that pass validation and still fail

Four rules the request schema can't express. Each returns an error even though the body is well formed:

* `experience_id` omitted with no `account_id` set.
* `frequency` sent without `publish_at`.
* `capture_spec` sent when `business_goal_type` is anything other than `data_capture`.
* A `deliverable` carrying neither a link nor a file.

## Next steps

> [card] [Bounties overview](https://docs.whop.com/developer/bounties/overview.md)
>
> The objects and how they fit together.

> [card] [Upload files](https://docs.whop.com/developer/guides/upload-files.md)
>
> Get the file IDs a deliverable references.

> [card] [Bounties reference](https://docs.whop.com/api-reference/bounties/bounty)
>
> Request and response shapes for every endpoint.

> [card] [Bounty submissions reference](https://docs.whop.com/api-reference/bounty-submissions/retrieve-bounty-submission.md)
>
> The submission object and its filters.


## Related topics

- [Bounties](https://docs.whop.com/developer/bounties/overview.md)
- [Update Bounty](https://docs.whop.com/api-reference/bounties/update-bounty.md)
- [Create Bounty](https://docs.whop.com/api-reference/bounties/create-bounty.md)
