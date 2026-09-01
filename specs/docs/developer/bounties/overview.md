> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Bounties

> Post paid tasks with an escrowed reward, collect submitted work from workers, and pay out on approval.

A bounty is a paid task. You describe the work, fund a reward pool up front, and workers submit their work for review. Whop escrows the money when the bounty is created and releases it when a submission is approved, so a worker can see the reward is real before starting.

## What you can build

* **Post tasks** with a funded reward, one-off or on a repeating schedule.
* **Restrict who can work them** by country.
* **Collect submissions** as links, uploaded files, or both.
* **Run a clipping or UGC program** by declaring what the work is meant to achieve.

## Core objects

Bounty, bounty submission. Each is defined in [Core Concepts](/developer/concepts).

<ResponseField name="bounty" type="bnty_">
  The task and its funded reward pool, created by the poster. See [Bounties](/api-reference/beta/bounties/bounty).
</ResponseField>

<ResponseField name="bounty submission" type="btys_">
  One worker's attempt, carrying the submitted work, and a review status. See [Bounty Submissions](/api-reference/beta/bounty-submissions/retrieve-bounty-submission).
</ResponseField>

## Where to go next

<Columns cols={2}>
  <Card title="Run a bounty program" icon="trophy" href="/developer/bounties/run-a-bounty-program">
    Post, fund, review, and cancel, end to end.
  </Card>

  <Card title="Submit to a bounty" icon="paper-plane" href="/api-reference/beta/bounty-submissions/create-bounty-submission">
    The worker side, in one call.
  </Card>

  <Card title="Upload files" icon="arrow-up-from-bracket" href="/developer/guides/upload-files">
    Get the file IDs a submission references.
  </Card>

  <Card title="Bounties reference" icon="code" href="/api-reference/beta/bounties/bounty">
    Every bounty endpoint, with a playground.
  </Card>
</Columns>
