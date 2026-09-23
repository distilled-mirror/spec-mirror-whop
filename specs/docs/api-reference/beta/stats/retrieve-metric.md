> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Retrieve Metric

> Retrieves a metric as a time series of points for an account or user over a time range. The `market_prices` metric is public and requires no authentication. The `funnel` metric measures 2 to 10 ordered events per person. Its first matching event inside from/to anchors the cohort, breakdown and conversion window; later entries do not restart it. Intervening events are allowed, and conversions may occur after to. Funnel values are final conversion percentages; steps include counts and cumulative conversion percentages. Experiment funnels use experiment.exposure as step 1 and breakdown_by=variant. See funnel step properties for current availability. Pass steps using bracket parameters such as steps[1][event]=pixel.page&steps[1][page]=/pricing*&steps[2][event]=payment.completed.



## OpenAPI

<!-- OpenAPI source: `get /stats/{metric}` in specs/api-v1-native.json (inlined by docs.whop.com; stripped on download) -->