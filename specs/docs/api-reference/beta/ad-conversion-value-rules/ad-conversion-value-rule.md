> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Conversion Value Rule

A conversion value rule allows you to report accurate conversion values to Whop while modifying how those values are sent to ad networks. Rules belong to an account and can apply to an account, an ad campaign, an ad group, or an ad.

Each rule contains targets and events that share one value adjustment. Every selected event applies to every selected target. Create, retrieve, edit, delete, pause, or resume one rule by its ID. Create may set an initial active or paused status. Edits keep that status. Filter the list with resource\_id to find rules overlapping a campaign, ad group, or ad. Every target must support every selected event; Google does not support named custom events. Create and edit accept replace\_rule\_ids to replace only the overlapping selections in the same transaction. Other selections keep their values. Remaining selections may split into separate rules so every event still applies to every target. Broader rules remain as fallbacks for other items; the most specific rule applies. Rules with no remaining selections are paused. Unpause automatically replaces overlapping selections using the same behavior as create and edit: other selections keep their values, and broader rules remain as defaults. Resuming an already-active rule makes no changes. Each write succeeds or fails as one transaction. Use Idempotency-Key to safely retry POST requests. Each conversion send attempt uses the rules saved at that time, including retries.

## Endpoints

| Endpoint                                                                                                           | Request                                                                                      |
| ------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------- |
| [List conversion value rules](/api-reference/beta/ad-conversion-value-rules/list-conversion-value-rules)           | <Badge color="blue" size="sm" stroke>GET</Badge> `/ad_conversion_value_rules`                |
| [Retrieve a conversion value rule](/api-reference/beta/ad-conversion-value-rules/retrieve-a-conversion-value-rule) | <Badge color="blue" size="sm" stroke>GET</Badge> `/ad_conversion_value_rules/{id}`           |
| [Create a conversion value rule](/api-reference/beta/ad-conversion-value-rules/create-a-conversion-value-rule)     | <Badge color="green" size="sm" stroke>POST</Badge> `/ad_conversion_value_rules`              |
| [Pause a conversion value rule](/api-reference/beta/ad-conversion-value-rules/pause-a-conversion-value-rule)       | <Badge color="green" size="sm" stroke>POST</Badge> `/ad_conversion_value_rules/{id}/pause`   |
| [Unpause a conversion value rule](/api-reference/beta/ad-conversion-value-rules/unpause-a-conversion-value-rule)   | <Badge color="green" size="sm" stroke>POST</Badge> `/ad_conversion_value_rules/{id}/unpause` |
| [Update a conversion value rule](/api-reference/beta/ad-conversion-value-rules/update-a-conversion-value-rule)     | <Badge color="orange" size="sm" stroke>PATCH</Badge> `/ad_conversion_value_rules/{id}`       |
| [Delete a conversion value rule](/api-reference/beta/ad-conversion-value-rules/delete-a-conversion-value-rule)     | <Badge color="red" size="sm" stroke>DELETE</Badge> `/ad_conversion_value_rules/{id}`         |

## Attributes

<Columns cols={2}>
  <Column>
    <ResponseField name="id" type="string" required>
      Conversion value rule ID, prefixed adcvr\_.
    </ResponseField>

    <ResponseField name="account_id" type="string" required>
      Business that owns this shared, editable rule.
    </ResponseField>

    <ResponseField name="adjustment_type" type="string" required>
      Set a fixed amount or change the original value by a signed percentage.

      Available options: `fixed`, `percentage`
    </ResponseField>

    <ResponseField name="created_at" type="string" required>
      When the rule was created, as an ISO 8601 timestamp.
    </ResponseField>

    <ResponseField name="events" type="object[]" required>
      Events adjusted on every selected target.

      <Accordion title="Properties" defaultOpen={true}>
        <ResponseField name="custom_name" type="string | null" required>
          Exact custom event name. Null for standard events.
        </ResponseField>

        <ResponseField name="event_name" type="string" required>
          Event to adjust. Purchase includes Whop purchases and external purchase events.

          Available options: `purchase`, `subscribe`, `start_trial`, `lead`, `complete_registration`, `submit_application`, `schedule`, `contact`, `view_content`, `add_to_cart`, `custom`
        </ResponseField>
      </Accordion>
    </ResponseField>

    <ResponseField name="fixed_value" type="object | null" required>
      Amount sent for a fixed rule. Null for a percentage rule.

      <Accordion title="Properties" defaultOpen={true}>
        <ResponseField name="amount" type="string" required>
          The amount in major units, as an exact decimal string — `"10.00"` is ten
          dollars. A string so no float rounds it in transit.
        </ResponseField>

        <ResponseField name="currency" type="string" required>
          Three-letter ISO 4217 currency code, lowercase.
        </ResponseField>

        <ResponseField name="decimals" type="integer" required>
          How many decimal places the amount CARRIES — the precision the charge itself
          runs at.
        </ResponseField>

        <ResponseField name="display_decimals" type="integer" required>
          How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
        </ResponseField>
      </Accordion>
    </ResponseField>

    <ResponseField name="metadata" type="object" required>
      Free-form string keys and values for the caller.
    </ResponseField>

    <ResponseField name="percentage_change" type="number | null" required>
      Signed percent change: 20 increases by 20%, negative 20 decreases by 20%. The
      sent value cannot go below zero. Null for a fixed rule.
    </ResponseField>

    <ResponseField name="status" type="string" required>
      Whether this rule is active or paused.

      Available options: `active`, `paused`
    </ResponseField>

    <ResponseField name="targets" type="object[]" required>
      Targets covered by this rule. Every selected event applies to every target.

      <Accordion title="Properties" defaultOpen={true}>
        <ResponseField name="platform" type="string" required>
          Ad platform that receives the adjusted value.

          Available options: `tiktok`, `meta`, `google`
        </ResponseField>

        <ResponseField name="resource_id" type="string | null" required>
          Selected campaign, ad group, or ad ID. Null for a business target.
        </ResponseField>

        <ResponseField name="resource_title" type="string | null" required>
          Name of the selected item. Null when it is no longer available.
        </ResponseField>

        <ResponseField name="scope" type="string" required>
          Business, campaign, ad group, or ad covered by this target.

          Available options: `business`, `campaign`, `ad_group`, `ad`
        </ResponseField>
      </Accordion>
    </ResponseField>

    <ResponseField name="updated_at" type="string" required>
      When the rule was edited, as an ISO 8601 timestamp.
    </ResponseField>
  </Column>

  <Column>
    <div className="api-resource-sticky-example">
      ```json AdConversionValueRule theme={null}
      {
      	"id": "adcvr_3jT8kR2pN6sQ9x",
      	"account_id": "biz_4dG8nR2qP9sT6v",
      	"fixed_value": {
      		"amount": "60.00",
      		"currency": "usd",
      		"decimals": 2,
      		"display_decimals": 2
      	},
      	"metadata": {},
      	"created_at": "2026-09-20T00:00:00.000Z",
      	"updated_at": "2026-09-20T00:00:00.000Z",
      	"adjustment_type": "fixed",
      	"percentage_change": null,
      	"targets": [
      		{
      			"scope": "business",
      			"resource_id": null,
      			"resource_title": "Torii",
      			"platform": "meta"
      		}
      	],
      	"events": [
      		{
      			"event_name": "purchase",
      			"custom_name": null
      		}
      	],
      	"status": "active"
      }
      ```
    </div>
  </Column>
</Columns>
