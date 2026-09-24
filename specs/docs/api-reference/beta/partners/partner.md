> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Partners

Get started at [whop.com/network](https://whop.com/network). A Partner is a user who refers people and businesses to Whop. The partner profile includes enrollment, active direct business referral counts, and default payout terms.

Retrieve your profile with `/partners/{id}`. Use `/partner_referral_requests` to create and manage referral links and their rewards. You can also enroll in the partner program, review referred users and businesses, track earnings, and see the partner leaderboard.

## Endpoints

| Endpoint                                                                                              | Request                                                                               |
| ----------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| [Retrieve a partner](/api-reference/beta/partners/retrieve-a-partner)                                 | <Badge color="blue" size="sm" stroke>GET</Badge> `/partners/{id}`                     |
| [List referred businesses](/api-reference/beta/partners/list-referred-businesses)                     | <Badge color="blue" size="sm" stroke>GET</Badge> `/partners/businesses`               |
| [Retrieve a referred business](/api-reference/beta/partners/retrieve-a-referred-business)             | <Badge color="blue" size="sm" stroke>GET</Badge> `/partners/businesses/{id}`          |
| [List referred business earnings](/api-reference/beta/partners/list-referred-business-earnings)       | <Badge color="blue" size="sm" stroke>GET</Badge> `/partners/businesses/{id}/earnings` |
| [Retrieve the leaderboard](/api-reference/beta/partners/retrieve-the-leaderboard)                     | <Badge color="blue" size="sm" stroke>GET</Badge> `/partners/leaderboard`              |
| [List the users the caller referred](/api-reference/beta/partners/list-the-users-the-caller-referred) | <Badge color="blue" size="sm" stroke>GET</Badge> `/partners/referred_users`           |
| [Enroll as a Whop partner](/api-reference/beta/partners/enroll-as-a-whop-partner)                     | <Badge color="green" size="sm" stroke>POST</Badge> `/partners`                        |

## Attributes

<Columns cols={2}>
  <Column>
    <ResponseField name="certification_complete" type="boolean" required>
      Whether the user finished the partner certification course: every visible quiz
      and knowledge check has a passing result, or, when the course has none, every
      visible lesson is marked completed.
    </ResponseField>

    <ResponseField name="joined_at" type="string | null" required>
      When the user joined the partner program, as an ISO 8601 timestamp. Null when
      they have not joined.
    </ResponseField>

    <ResponseField name="payout_rates" type="object[]" required>
      Default payout percentages grouped by referral tier, with one earning duration per tier. Existing businesses can have individual terms.

      <Accordion title="Properties" defaultOpen={true}>
        <ResponseField name="duration" type="object" required>
          Default period during which a new referred business can generate commissions, measured from its attribution start. This is not a payout delay. Individual business terms can differ.

          <Accordion title="Properties" defaultOpen={true}>
            <ResponseField name="unit" type="string" required>
              Unit of the earning window. Month means a calendar month; day means a day.

              Available options: `day`, `month`
            </ResponseField>

            <ResponseField name="value" type="integer" required>
              Number of units in the earning window.
            </ResponseField>
          </Accordion>
        </ResponseField>

        <ResponseField name="rates" type="object[]" required>
          Default payout percentages for each income source in this referral tier.

          <Accordion title="Properties" defaultOpen={true}>
            <ResponseField name="income_source" type="string" required>
              Income source that generates this percentage payout.

              Available options: `sales`, `transfer`, `card_interchange`, `ad_spend`
            </ResponseField>

            <ResponseField name="percentage" type="number" required>
              Partner's default percentage for this tier and income source. For example, 30 means 30%.
            </ResponseField>
          </Accordion>
        </ResponseField>

        <ResponseField name="tier" type="string" required>
          Referral tier: first for a directly referred business, or second for a business brought by a referred partner.

          Available options: `first`, `second`
        </ResponseField>
      </Accordion>
    </ResponseField>

    <ResponseField name="referred_businesses_count" type="integer" required>
      Number of active first-tier business referrals attributed to the partner,
      excluding deleted businesses.
    </ResponseField>

    <ResponseField name="user" type="object" required>
      The authenticated partner's public profile.

      <Accordion title="Properties" defaultOpen={true}>
        <ResponseField name="id" type="string" required>
          User ID, prefixed `user_`.
        </ResponseField>

        <ResponseField name="name" type="string | null" required>
          Display name.
        </ResponseField>

        <ResponseField name="profile_picture" type="object" required>
          Avatar wrapper; its `url` is always present, using a generated placeholder when the user set no picture.

          <Accordion title="Properties" defaultOpen={true}>
            <ResponseField name="url" type="string" required>
              Avatar image URL. Always present — a generated placeholder when the user set no picture.
            </ResponseField>
          </Accordion>
        </ResponseField>

        <ResponseField name="username" type="string" required>
          Public username.
        </ResponseField>
      </Accordion>
    </ResponseField>

    <ResponseField name="verification_waitlist_joined" type="boolean" required>
      Whether the user has a pending or approved personal entry on the Verified
      Partner waitlist.
    </ResponseField>

    <ResponseField name="whop_partner_verified_at" type="string | null" required>
      When the user became a verified Whop Partner, as an ISO 8601 timestamp. `null`
      if not verified.
    </ResponseField>
  </Column>

  <Column>
    <div className="api-resource-sticky-example">
      ```json Partner theme={null}
      {
      	"user": {
      		"id": "user_9mHw2b4TkL8pQ",
      		"username": "jane",
      		"name": "Jane Chen",
      		"profile_picture": {
      			"url": "https://example.com/jane-avatar.png"
      		}
      	},
      	"joined_at": "2026-09-01T12:00:00.000Z",
      	"whop_partner_verified_at": null,
      	"verification_waitlist_joined": false,
      	"certification_complete": false,
      	"referred_businesses_count": 12,
      	"payout_rates": [
      		{
      			"tier": "first",
      			"duration": {
      				"unit": "month",
      				"value": 600
      			},
      			"rates": [
      				{
      					"income_source": "sales",
      					"percentage": 30
      				},
      				{
      					"income_source": "transfer",
      					"percentage": 30
      				},
      				{
      					"income_source": "card_interchange",
      					"percentage": 30
      				},
      				{
      					"income_source": "ad_spend",
      					"percentage": 1
      				}
      			]
      		},
      		{
      			"tier": "second",
      			"duration": {
      				"unit": "month",
      				"value": 600
      			},
      			"rates": [
      				{
      					"income_source": "sales",
      					"percentage": 10
      				},
      				{
      					"income_source": "transfer",
      					"percentage": 10
      				},
      				{
      					"income_source": "card_interchange",
      					"percentage": 10
      				},
      				{
      					"income_source": "ad_spend",
      					"percentage": 0.5
      				}
      			]
      		}
      	]
      }
      ```
    </div>
  </Column>
</Columns>
