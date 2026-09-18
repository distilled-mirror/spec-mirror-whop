> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Audience

An Audience is a reusable group of people to include or exclude when targeting ads. Build custom audiences from customer lists, Whop People data, or social engagement, and create lookalikes to reach people similar to an existing audience.

Use the Audiences API to create, list, and delete audiences and monitor asynchronous processing. Meta engagement sources include videos, lead forms, Instagram profiles, and Facebook pages. Engagement membership updates on Meta; Whop People audiences can refresh automatically or keep a snapshot.

## Endpoints

| Endpoint                                                         | Request                                                                         |
| ---------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| [List Audiences](/api-reference/beta/audiences/list-audiences)   | <Badge color="blue" size="sm" stroke>GET</Badge> `/audiences`                   |
| [Create Audience](/api-reference/beta/audiences/create-audience) | <Badge color="green" size="sm" stroke>POST</Badge> `/audiences`                 |
| [Add People](/api-reference/beta/audiences/add-people)           | <Badge color="green" size="sm" stroke>POST</Badge> `/audiences/{id}/add_people` |
| [Update Audience](/api-reference/beta/audiences/update-audience) | <Badge color="orange" size="sm" stroke>PATCH</Badge> `/audiences/{id}`          |
| [Delete Audience](/api-reference/beta/audiences/delete-audience) | <Badge color="red" size="sm" stroke>DELETE</Badge> `/audiences/{id}`            |

## Attributes

<Columns cols={2}>
  <Column>
    <ResponseField name="id" type="string" required>
      Audience ID, prefixed `adaud_`.
    </ResponseField>

    <ResponseField name="audience_type" type="string" required>
      Whether the audience targets a defined group of people or people similar to an existing audience.

      Available options: `custom`, `lookalike`
    </ResponseField>

    <ResponseField name="auto_refresh" type="boolean" required>
      Whether Whop rebuilds membership from saved People filters twice a day. When
      `false`, People audiences keep the members matched at creation. Always `false`
      for uploaded lists, lookalikes, and engagement audiences. Engagement
      membership is maintained by Meta.
    </ResponseField>

    <ResponseField name="created_at" type="string" required>
      When the audience was created, as an ISO 8601 timestamp.
    </ResponseField>

    <ResponseField name="engagement" type="object | null" required>
      Social engagement rules maintained by the ad platform. `null` for other audience sources.

      <Accordion title="Properties" defaultOpen={true}>
        <ResponseField name="exclude" type="video or lead_form or instagram_profile or facebook_page[]" required>
          Exclude anyone matching any exclusion rule. Supply 0–10 rules. Video audiences
          do not support exclusions; use a separate audience in ad-group exclusions.
        </ResponseField>

        <ResponseField name="include" type="video or lead_form or instagram_profile or facebook_page[]" required>
          Match any inclusion rule. Supply 1–10 rules. Video rules must share a
          retention window and cannot be combined with other sources.
        </ResponseField>

        <ResponseField name="platform" type="string" required>
          Ad platform that maintains membership.

          Available options: `meta`
        </ResponseField>
      </Accordion>
    </ResponseField>

    <ResponseField name="error_message" type="string | null" required>
      Processing error message. `null` unless processing is partial or failed.
    </ResponseField>

    <ResponseField name="filters" type="object | null" required>
      Saved Whop People filters that define membership, using the same keys as `GET
            	/people`. `null` for uploaded lists, engagement audiences, and lookalikes.
    </ResponseField>

    <ResponseField name="last_refreshed_at" type="string | null" required>
      When the audience membership was last rebuilt, as an ISO 8601 timestamp.
      `null` until the first build completes.
    </ResponseField>

    <ResponseField name="lookalike_ratio" type="number | null" required>
      For lookalikes: the upper bound of the similarity band as a fraction (0.02 =
      top 2%). `null` for custom audiences.
    </ResponseField>

    <ResponseField name="lookalike_starting_ratio" type="number | null" required>
      For lookalikes: the lower bound of the similarity band as a fraction. `null`
      for custom audiences and first-tier lookalikes.
    </ResponseField>

    <ResponseField name="match_rates" type="object[]" required>
      Estimated match rates by ad platform. Empty for engagement audiences and audiences not sent to a supported platform.

      <Accordion title="Properties" defaultOpen={true}>
        <ResponseField name="lower_bound" type="number | null" required>
          Lower bound of the estimated match rate percentage. `null` until available.
        </ResponseField>

        <ResponseField name="platform" type="string" required>
          The ad platform that provided the match-rate estimate.

          Available options: `meta`
        </ResponseField>

        <ResponseField name="status" type="string | null" required>
          Availability of the estimated match rate.

          Available options: `calculating`, `available`, `unavailable`
        </ResponseField>

        <ResponseField name="upper_bound" type="number | null" required>
          Upper bound of the estimated match rate percentage. `null` until available.
        </ResponseField>
      </Accordion>
    </ResponseField>

    <ResponseField name="matched_rows" type="number" required>
      Members successfully uploaded to connected ad accounts. Always 0 for
      lookalikes and engagement audiences.
    </ResponseField>

    <ResponseField name="name" type="string" required>
      Audience display name.
    </ResponseField>

    <ResponseField name="platform_audience_ids" type="string[]" required>
      External audience IDs created on connected ad platforms, such as Meta.
    </ResponseField>

    <ResponseField name="processed_rows" type="number" required>
      Members processed from the source so far. Always 0 for lookalikes and
      engagement audiences.
    </ResponseField>

    <ResponseField name="progress_percent" type="number" required>
      Processing progress from 0 to 100.
    </ResponseField>

    <ResponseField name="source_audience_id" type="string | null" required>
      For lookalikes: the audience this lookalike was built from. `null` for custom
      audiences.
    </ResponseField>

    <ResponseField name="source_type" type="string" required>
      Membership source: an uploaded CSV, Whop People filters, or social engagement.

      Available options: `csv_upload`, `people_filter`, `engagement`
    </ResponseField>

    <ResponseField name="status" type="string" required>
      Current state of audience creation. For engagement audiences, `ready` means the rules were created on Meta; membership may still be populating. `syncing` means Whop is sending matched rows to connected ad accounts. When status is `partial` or `failed`, `error_message` explains what went wrong.

      Available options: `pending`, `processing`, `syncing`, `ready`, `partial`, `failed`
    </ResponseField>

    <ResponseField name="total_rows" type="number" required>
      Total members detected in the source — CSV rows for uploaded lists, matching
      people for automatic audiences. Always 0 for lookalikes and engagement
      audiences.
    </ResponseField>

    <ResponseField name="updated_at" type="string" required>
      When the audience was last updated, as an ISO 8601 timestamp.
    </ResponseField>
  </Column>

  <Column>
    <div className="api-resource-sticky-example">
      ```json Audience theme={null}
      {
      	"id": "adaud_PastPurchasers123",
      	"name": "Past purchasers",
      	"status": "ready",
      	"audience_type": "custom",
      	"source_type": "people_filter",
      	"source_audience_id": null,
      	"lookalike_ratio": null,
      	"lookalike_starting_ratio": null,
      	"total_rows": 2500,
      	"processed_rows": 2500,
      	"matched_rows": 1830,
      	"progress_percent": 100,
      	"error_message": null,
      	"platform_audience_ids": ["120246230799130686"],
      	"engagement": null,
      	"filters": {
      		"has_purchased": true,
      		"country": "US"
      	},
      	"auto_refresh": true,
      	"last_refreshed_at": "2025-12-01T01:00:00Z",
      	"match_rates": [
      		{
      			"platform": "meta",
      			"status": "available",
      			"lower_bound": 68.2,
      			"upper_bound": 74.5
      		}
      	],
      	"created_at": "2025-12-01T00:00:00Z",
      	"updated_at": "2025-12-01T01:00:00Z"
      }
      ```
    </div>
  </Column>
</Columns>
