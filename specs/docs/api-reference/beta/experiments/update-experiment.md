> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Update Experiment

> Updates the targeting rules, treatment allocation, metrics, or hypothesis of an existing experiment or feature flag. Weights and metrics can only grow, so enrolled users never change arms and an existing metric is never dropped. Lifecycle moves through the transition endpoints (`activate`, `pause`, `end`), never through this update. Requires the corresponding experiment permission on the owning account, or Whop internal access for internal experiments.



## OpenAPI

<!-- OpenAPI source: `patch /experiments/{id}` in specs/api-v1-native.json (inlined by docs.whop.com; stripped on download) -->