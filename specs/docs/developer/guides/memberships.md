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

# Memberships

> Read memberships and change their billing with the API, such as pausing a subscription or adding free days.

A Membership is the active relationship between a user and a product. It tracks access, billing status, and renewal schedule. You don't create memberships directly because checkout does that for you. Once a membership exists, you can read it, pause or cancel billing, comp time, or update metadata.

> [!TIP]
>
> Most apps don't need to manage memberships at all. Reach for these methods when you're building admin tools, customer support flows, or self-serve dashboards that let users pause or cancel.

## Lifecycle at a glance

| Action | Method | What it does |
| - | - | - |
| Pause billing | [`memberships.pause`](https://docs.whop.com/api-reference/memberships/pause-membership) | Stops future renewals until resumed. Existing access stays. |
| Resume billing | [`memberships.resume`](https://docs.whop.com/api-reference/memberships/resume-membership) | Reverses a pause. |
| Cancel | [`memberships.cancel`](https://docs.whop.com/api-reference/memberships/cancel-membership) | At period end, or immediate (default). |
| Restore cancellation | [`memberships.update`](https://docs.whop.com/api-reference/memberships/update-membership) | Send `cancel_at_period_end: false` to reverse a pending cancellation. |
| Comp time | [`memberships.extend`](https://docs.whop.com/api-reference/memberships/extend-membership) | Extends the next renewal date by N days. |
| Change the billing day | [`memberships.update`](https://docs.whop.com/api-reference/memberships/update-membership) | Send `current_period_end` to move the next renewal. Memberships billed every 30, 60, 90 … 360 or 365 days keep renewing on that day of the month. |
| Update metadata | [`memberships.update`](https://docs.whop.com/api-reference/memberships/update-membership) | Patch metadata or other writable fields. |

## Retrieve and list

```typescript TypeScript theme={null}
const membership = await client.memberships.retrieve({ id: "mem_xxxxxxxxxxxxx" });

// List with auto-pagination
const memberships = await client.memberships.list({
  account_id: "biz_xxxxxxxxxxxxx",
});

for await (const item of memberships) {
  console.log(item);
}
```

```python Python theme={null}
membership = client.memberships.retrieve("mem_xxxxxxxxxxxxx")

for item in client.memberships.list(account_id="biz_xxxxxxxxxxxxx"):
    print(item)
```

```ruby Ruby theme={null}
membership = client.memberships.retrieve(id: "mem_xxxxxxxxxxxxx")

# list returns an Enumerable that pages for you and yields one membership at a time
client.memberships.list(account_id: "biz_xxxxxxxxxxxxx").each do |item|
  puts item
end
```

```rust Rust theme={null}
let membership = client
    .memberships
    .retrieve(&"mem_xxxxxxxxxxxxx".to_string(), None)
    .await?;

// List one page of memberships for an account
let memberships = client
    .memberships
    .list(
        &MembershipsListQueryRequest {
            account_id: Some("biz_xxxxxxxxxxxxx".to_string()),
            ..Default::default()
        },
        None,
    )
    .await?;

for item in memberships.data {
    println!("{item:?}");
}
```

```go Go theme={null}
membership, err := client.Memberships.Retrieve(context.TODO(), &whopsdk.RetrieveMembershipsRequest{
    ID: "mem_xxxxxxxxxxxxx",
})
if err != nil {
    log.Fatal(err)
}
fmt.Println(membership)

// List with auto-pagination
page, err := client.Memberships.List(context.TODO(), &whopsdk.ListMembershipsRequest{
    AccountID: whopsdk.String("biz_xxxxxxxxxxxxx"),
})
if err != nil {
    log.Fatal(err)
}

iter := page.Iterator()
for iter.Next(context.TODO()) {
    fmt.Println(iter.Current())
}
if err := iter.Err(); err != nil {
    log.Fatal(err)
}
```

## Pause and resume

Pausing stops the next billing cycle. The user keeps their existing access until the current period ends, but the renewal won't fire. Pass `until` to schedule an automatic resume.

```typescript TypeScript theme={null}
await client.memberships.pause({
  id: "mem_xxxxxxxxxxxxx",
  until: "2026-09-01T00:00:00Z", // optional, resumes automatically
});

// Later
await client.memberships.resume({ id: "mem_xxxxxxxxxxxxx" });
```

```python Python theme={null}
client.memberships.pause(
    "mem_xxxxxxxxxxxxx",
    until="2026-09-01T00:00:00Z",  # optional, resumes automatically
)

client.memberships.resume("mem_xxxxxxxxxxxxx")
```

```ruby Ruby theme={null}
client.memberships.pause(
  id: "mem_xxxxxxxxxxxxx",
  until: "2026-09-01T00:00:00Z", # optional, resumes automatically
)

# Later
client.memberships.resume(id: "mem_xxxxxxxxxxxxx")
```

```rust Rust theme={null}
client
    .memberships
    .pause(
        &"mem_xxxxxxxxxxxxx".to_string(),
        &PauseMembershipsRequest {
            until: Some("2026-09-01T00:00:00Z".to_string()), // optional, resumes automatically
        },
        None,
    )
    .await?;

// Later
client.memberships.resume(&"mem_xxxxxxxxxxxxx".to_string(), None).await?;
```

```go Go theme={null}
_, err := client.Memberships.Pause(context.TODO(), &whopsdk.PauseMembershipsRequest{
    ID:    "mem_xxxxxxxxxxxxx",
    Until: whopsdk.String("2026-09-01T00:00:00Z"), // optional, resumes automatically
})
if err != nil {
    log.Fatal(err)
}

// Later
if _, err := client.Memberships.Resume(context.TODO(), &whopsdk.ResumeMembershipsRequest{
    ID: "mem_xxxxxxxxxxxxx",
}); err != nil {
    log.Fatal(err)
}
```

## Cancel

Two cancellation modes. `cancel_at_period_end: true` keeps access until the current renewal date and then deactivates. Omit it (or pass `false`) to revoke access immediately.

```typescript TypeScript theme={null}
// Cancel at the end of the current billing period
await client.memberships.cancel({
  id: "mem_xxxxxxxxxxxxx",
  cancel_at_period_end: true,
});

// Cancel immediately. Access ends now.
await client.memberships.cancel({
  id: "mem_xxxxxxxxxxxxx",
  cancel_at_period_end: false,
  reason: "Requested by customer",
});
```

```python Python theme={null}
# Cancel at the end of the current billing period
client.memberships.cancel(
    "mem_xxxxxxxxxxxxx",
    cancel_at_period_end=True,
)

# Cancel immediately. Access ends now.
client.memberships.cancel(
    "mem_xxxxxxxxxxxxx",
    cancel_at_period_end=False,
    reason="Requested by customer",
)
```

```ruby Ruby theme={null}
# Cancel at the end of the current billing period
client.memberships.cancel(
  id: "mem_xxxxxxxxxxxxx",
  cancel_at_period_end: true,
)

# Cancel immediately. Access ends now.
client.memberships.cancel(
  id: "mem_xxxxxxxxxxxxx",
  cancel_at_period_end: false,
  reason: "Requested by customer",
)
```

```rust Rust theme={null}
// Cancel at the end of the current billing period
client
    .memberships
    .cancel(
        &"mem_xxxxxxxxxxxxx".to_string(),
        &CancelMembershipsRequest {
            cancel_at_period_end: Some(true),
            ..Default::default()
        },
        None,
    )
    .await?;

// Cancel immediately. Access ends now.
client
    .memberships
    .cancel(
        &"mem_xxxxxxxxxxxxx".to_string(),
        &CancelMembershipsRequest {
            cancel_at_period_end: Some(false),
            reason: Some("Requested by customer".to_string()),
        },
        None,
    )
    .await?;
```

```go Go theme={null}
// Cancel at the end of the current billing period
_, err := client.Memberships.Cancel(context.TODO(), &whopsdk.CancelMembershipsRequest{
    ID:                "mem_xxxxxxxxxxxxx",
    CancelAtPeriodEnd: whopsdk.Bool(true),
})
if err != nil {
    log.Fatal(err)
}

// Cancel immediately. Access ends now.
_, err = client.Memberships.Cancel(context.TODO(), &whopsdk.CancelMembershipsRequest{
    ID:                "mem_xxxxxxxxxxxxx",
    CancelAtPeriodEnd: whopsdk.Bool(false),
    Reason:            whopsdk.String("Requested by customer"),
})
if err != nil {
    log.Fatal(err)
}
```

> [!NOTE]
>
> `cancel_at_period_end: true` flips `cancel_at_period_end` to `true` on the membership. The user keeps access until renewal, then the membership deactivates and `membership.updated` fires with `previous_attributes.status` set to the status it left. Omitting the field revokes access immediately, so send it explicitly whenever you mean to cancel at period end.

## Restore a cancellation

If the user changes their mind before the period ends, clear the pending period-end cancellation with an update. Has no effect if the membership wasn't scheduled to cancel.

```typescript TypeScript theme={null}
await client.memberships.update({
  id: "mem_xxxxxxxxxxxxx",
  cancel_at_period_end: false,
});
```

```python Python theme={null}
client.memberships.update("mem_xxxxxxxxxxxxx", cancel_at_period_end=False)
```

```ruby Ruby theme={null}
client.memberships.update(id: "mem_xxxxxxxxxxxxx", cancel_at_period_end: false)
```

```rust Rust theme={null}
client
    .memberships
    .update(
        &"mem_xxxxxxxxxxxxx".to_string(),
        &UpdateMembershipsRequest {
            cancel_at_period_end: Some(false),
            ..Default::default()
        },
        None,
    )
    .await?;
```

```go Go theme={null}
if _, err := client.Memberships.Update(context.TODO(), &whopsdk.UpdateMembershipsRequest{
    ID:                "mem_xxxxxxxxxxxxx",
    CancelAtPeriodEnd: whopsdk.Bool(false),
}); err != nil {
    log.Fatal(err)
}
```

## Add free days

Comp the user with extra time on their current period. The next renewal date moves forward by `days`. Useful for service interruptions, support gestures, or referral rewards.

```typescript TypeScript theme={null}
await client.memberships.extend({
  id: "mem_xxxxxxxxxxxxx",
  days: 7,
});
```

```python Python theme={null}
client.memberships.extend("mem_xxxxxxxxxxxxx", days=7)
```

```ruby Ruby theme={null}
client.memberships.extend(id: "mem_xxxxxxxxxxxxx", days: 7)
```

```rust Rust theme={null}
client
    .memberships
    .extend(
        &"mem_xxxxxxxxxxxxx".to_string(),
        &ExtendMembershipsRequest { days: 7 },
        None,
    )
    .await?;
```

```go Go theme={null}
if _, err := client.Memberships.Extend(context.TODO(), &whopsdk.ExtendMembershipsRequest{
    ID:   "mem_xxxxxxxxxxxxx",
    Days: 7,
}); err != nil {
    log.Fatal(err)
}
```

`days` accepts 1 through 1095 (3 years).

## Update metadata

Patch arbitrary metadata or other writable fields on the membership.

```typescript TypeScript theme={null}
await client.memberships.update({
  id: "mem_xxxxxxxxxxxxx",
  metadata: { internal_user_id: "user_12345", tier: "gold" },
});
```

```python Python theme={null}
client.memberships.update(
    "mem_xxxxxxxxxxxxx",
    metadata={"internal_user_id": "user_12345", "tier": "gold"},
)
```

```ruby Ruby theme={null}
client.memberships.update(
  id: "mem_xxxxxxxxxxxxx",
  metadata: { internal_user_id: "user_12345", tier: "gold" },
)
```

```rust Rust theme={null}
client
    .memberships
    .update(
        &"mem_xxxxxxxxxxxxx".to_string(),
        &UpdateMembershipsRequest {
            metadata: Some(HashMap::from([
                ("internal_user_id".to_string(), json!("user_12345")),
                ("tier".to_string(), json!("gold")),
            ])),
            ..Default::default()
        },
        None,
    )
    .await?;
```

```go Go theme={null}
if _, err := client.Memberships.Update(context.TODO(), &whopsdk.UpdateMembershipsRequest{
    ID: "mem_xxxxxxxxxxxxx",
    Metadata: map[string]any{
        "internal_user_id": "user_12345",
        "tier":             "gold",
    },
}); err != nil {
    log.Fatal(err)
}
```

## Listen for lifecycle events

Subscribe via [webhooks](https://docs.whop.com/developer/guides/webhooks.md). These are the events that fire across the lifecycle:

| Event | When it fires |
| - | - |
| [`membership.activated`](https://docs.whop.com/api-reference/memberships/membership-activated) | Membership becomes valid (initial purchase or renewal payment succeeds). |
| [`membership.updated`](https://docs.whop.com/api-reference/memberships/membership-updated) | Something on the membership changed. It went invalid (failed payment, immediate cancel, period-end cancel landing, or the user left). The user toggled cancellation on or off. The seller edited the billing period, price, or renewal date. `previous_attributes` carries the values that changed from. Activations are delivered as `membership.updated` too. |

For `pause` / `resume` / `extend`, the membership status doesn't flip, so no activation/deactivation event fires. If you need to confirm the mutation succeeded, retrieve the membership after the call or poll on your own schedule.

## Next steps

> [card] [Accept payments](https://docs.whop.com/developer/guides/accept-payments.md)
>
> Where memberships come from. One-time and recurring checkouts.

> [card] [Save payment methods](https://docs.whop.com/developer/guides/save-payment-methods.md)
>
> On-file cards for future renewals and off-session billing.

> [card] [Listen to webhooks](https://docs.whop.com/developer/guides/webhooks.md)
>
> React to activation, deactivation, and cancellation toggles.

> [card] [Memberships API reference](https://docs.whop.com/api-reference/memberships/membership)
>
> Full resource: fields, statuses, and every endpoint.


## Related topics

- [Membership](https://docs.whop.com/api-reference/legacy/memberships/membership.md)
- [Memberships API](https://docs.whop.com/api-reference/memberships/membership.md)
- [Transfer Membership](https://docs.whop.com/api-reference/memberships/transfer-membership.md)
