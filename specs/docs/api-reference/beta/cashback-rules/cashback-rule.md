> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Cashback Rule

Cashback rules designate a funding platform, optional merchant name and category filters, a rate, and an eligibility window. Every supplied merchant filter must match. An account ID limits the rule to one of the platform's direct connected accounts and is required when both merchant filters are omitted or null.

Use the Cashback Rules API to create future-dated rules, update their merchant name, MCC, description, or expiration, and list every rule funded by the authenticated platform, including expired and discarded rules. Discarded rules cannot be updated. Creating or updating a rule does not transfer funds.

## Endpoints

| Endpoint                                                                        | Request                                                                     |
| ------------------------------------------------------------------------------- | --------------------------------------------------------------------------- |
| [List Cashback Rules](/api-reference/beta/cashback-rules/list-cashback-rules)   | <Badge color="blue" size="sm" stroke>GET</Badge> `/cashback_rules`          |
| [Create Cashback Rule](/api-reference/beta/cashback-rules/create-cashback-rule) | <Badge color="green" size="sm" stroke>POST</Badge> `/cashback_rule`         |
| [Update Cashback Rule](/api-reference/beta/cashback-rules/update-cashback-rule) | <Badge color="orange" size="sm" stroke>PATCH</Badge> `/cashback_rules/{id}` |

## Attributes

<Columns cols={2}>
  <Column>
    <ResponseField name="id" type="string" required>
      Cashback rule ID, prefixed `cicbr_`.
    </ResponseField>

    <ResponseField name="created_at" type="string" required>
      When the rule was created, as an ISO 8601 timestamp.
    </ResponseField>

    <ResponseField name="description" type="string | null" required>
      Optional description of the cashback rule.
    </ResponseField>

    <ResponseField name="discarded_at" type="string | null" required>
      When the rule was discarded, as an ISO 8601 timestamp. Null means it has not
      been discarded.
    </ResponseField>

    <ResponseField name="expires_at" type="string | null" required>
      Exclusive end of the eligibility window, as an ISO 8601 timestamp. Null means
      no expiration.
    </ResponseField>

    <ResponseField name="funding_account_id" type="string" required>
      Platform account designated to fund cashback, prefixed `biz_`. Derived from
      the authenticated credential.
    </ResponseField>

    <ResponseField name="merchant_category_code" type="string | null" required>
      Four-digit merchant category code. Null matches any MCC. When both merchant
      filters are null, scoped\_account\_id is required.
    </ResponseField>

    <ResponseField name="merchant_name" type="string | null" required>
      Raw merchant name reported by the card provider. Null matches any merchant
      name. When set, matches together with any MCC filter; not a substring or
      enriched display-name match.
    </ResponseField>

    <ResponseField name="rate_bps" type="integer" required>
      Cashback rate in basis points. 100 means 1%, and 10000 means 100%.
    </ResponseField>

    <ResponseField name="scoped_account_id" type="string | null" required>
      Connected account ID, prefixed `biz_`. Null designates all direct connected
      accounts of the funding platform.
    </ResponseField>

    <ResponseField name="starts_at" type="string" required>
      Inclusive start of the rule's eligibility window, as an ISO 8601 timestamp.
    </ResponseField>

    <ResponseField name="updated_at" type="string" required>
      When the rule was last updated, as an ISO 8601 timestamp.
    </ResponseField>
  </Column>

  <Column>
    <div className="api-resource-sticky-example">
      ```json CashbackRule theme={null}
      {
      	"id": "cicbr_a1B2c3D4e5F6g",
      	"merchant_name": "ACME SOFTWARE",
      	"merchant_category_code": "5734",
      	"rate_bps": 500,
      	"description": "Software cashback",
      	"funding_account_id": "biz_a1B2c3D4e5F6g",
      	"scoped_account_id": "biz_h7I8j9K0l1M2n",
      	"starts_at": "2026-10-01T00:00:00.000Z",
      	"expires_at": "2026-11-01T00:00:00.000Z",
      	"discarded_at": null,
      	"created_at": "2026-09-10T08:00:00.000Z",
      	"updated_at": "2026-09-10T08:00:00.000Z"
      }
      ```
    </div>
  </Column>
</Columns>
