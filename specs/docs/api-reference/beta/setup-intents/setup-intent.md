> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Setup Intent

A Setup Intent saves a buyer's payment method for later without taking money now. Create one from a confirmation token the payment elements collected in setup mode, or from a payment method already on file to re-verify it. It runs the same collection flow a payment does, so the buyer may still owe a step: 3D Secure on a card, a hosted enrollment, or linking a bank account.

The create response is the setup intent as created, not its outcome. Hand its `client_secret` to the elements' `handleNextAction`, or poll [Retrieve status](/api-reference/beta/setup-intents/retrieve-setup-status) for how far the setup has gone and what is outstanding. Once it reaches `succeeded`, `payment_method_id` names the saved method and Create Payment charges it.

## Endpoints

| Endpoint                                                                             | Request                                                                                            |
| ------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------- |
| [List Setup Intents](/api-reference/beta/setup-intents/list-setup-intents)           | <Badge color="blue" size="sm" stroke>GET</Badge> `/setup_intents`                                  |
| [Retrieve Setup Intent](/api-reference/beta/setup-intents/retrieve-setup-intent)     | <Badge color="blue" size="sm" stroke>GET</Badge> `/setup_intents/{id}`                             |
| [Retrieve setup status](/api-reference/beta/setup-intents/retrieve-setup-status)     | <Badge color="blue" size="sm" stroke>GET</Badge> `/setup_intents/{setup_intent_id}/status`         |
| [Create Setup Intent](/api-reference/beta/setup-intents/create-setup-intent)         | <Badge color="green" size="sm" stroke>POST</Badge> `/setup_intents`                                |
| [Update setup return URL](/api-reference/beta/setup-intents/update-setup-return-url) | <Badge color="orange" size="sm" stroke>PATCH</Badge> `/setup_intents/{setup_intent_id}/return_url` |

## Attributes

<Columns cols={2}>
  <Column>
    <ResponseField name="id" type="string" required>
      Setup intent ID, prefixed `sint_`.
    </ResponseField>

    <ResponseField name="account_id" type="string | null" required>
      The account the payment method is saved for, prefixed `biz_`.
    </ResponseField>

    <ResponseField name="checkout_configuration_id" type="string | null" required>
      The checkout configuration this setup was created through, prefixed `ch_`.
      Null for a setup created through this API rather than a hosted checkout.
    </ResponseField>

    <ResponseField name="client_secret" type="string | null" required>
      The credential a buyer's surface presents to poll this setup and set its
      return URL — hand it to the elements' `handleNextAction`. Only on setups
      created through this API, and always null in list responses — retrieve the
      setup intent for it.
    </ResponseField>

    <ResponseField name="created_at" type="string" required>
      When the setup intent was created, as an ISO 8601 timestamp.
    </ResponseField>

    <ResponseField name="last_setup_error" type="object | null" required>
      Why the setup ended where it did, or `null` when nothing has failed. Present on `canceled` — a buyer who abandoned carries no code, one refused by the provider does. Dropped once the setup succeeds.

      <Accordion title="Properties" defaultOpen={true}>
        <ResponseField name="code" type="string | null" required>
          A machine-readable classification of the failure, e.g. `enrollment_declined`.
          Absent when the buyer simply abandoned the setup.
        </ResponseField>

        <ResponseField name="message" type="string | null" required>
          A human-readable explanation of the failure.
        </ResponseField>
      </Accordion>
    </ResponseField>

    <ResponseField name="member_id" type="string | null" required>
      The buyer's member record on the account, prefixed `mber_`. Null without the
      member:basic:read permission, unless the caller is the buyer.
    </ResponseField>

    <ResponseField name="metadata" type="object | null" required>
      Your own key-value data attached when the setup intent was created.
    </ResponseField>

    <ResponseField name="payment_instrument" type="object | null" required>
      The method behind this setup shaped for display: a buyer-facing name, the standard icon set, and the card's brand, last four, issuer identification number, and expiry when it was a card. Null until a method was collected.

      <Accordion title="Properties" defaultOpen={true}>
        <ResponseField name="card" type="object | null" required>
          Card payments only: the card's network, last four, and issuer identification number.

          <Accordion title="Properties" defaultOpen={true}>
            <ResponseField name="brand" type="string | null" required>
              The network identifier (`visa`, `amex`, …), matching `card.networks` entries
              and saved card payment methods. Null when the vault did not record the
              network.
            </ResponseField>

            <ResponseField name="exp_month" type="number | null" required>
              The card's expiry month, 1 to 12. Null when the vault did not record it.
            </ResponseField>

            <ResponseField name="exp_year" type="number | null" required>
              The card's four-digit expiry year. Null when the vault did not record it.
            </ResponseField>

            <ResponseField name="issuer_identification_number" type="string | null" required>
              The issuer identification number, also called the BIN: the card's leading six
              or eight digits, which identify the issuing bank. Null when the processor did
              not report it.
            </ResponseField>

            <ResponseField name="last4" type="string | null" required>
              The card's last four digits, when captured.
            </ResponseField>
          </Accordion>
        </ResponseField>

        <ResponseField name="display_name" type="string" required>
          Buyer-facing instrument name — "Visa •••• 4242" when the card surfaced, else
          the method's own name ("Klarna").
        </ResponseField>

        <ResponseField name="icons" type="object" required>
          The standard icon set: square and card shapes, each in light and dark colorways.

          <Accordion title="Properties" defaultOpen={true}>
            <ResponseField name="card" type="object" required>
              The credit-card-proportioned tile (48x30).

              <Accordion title="Properties" defaultOpen={true}>
                <ResponseField name="dark" type="object" required>
                  The colorway for dark surfaces.

                  <Accordion title="Properties" defaultOpen={true}>
                    <ResponseField name="png_1x" type="string" required>
                      Raster fallback at the shape's native size.
                    </ResponseField>

                    <ResponseField name="png_2x" type="string" required>
                      Raster fallback at double density.
                    </ResponseField>

                    <ResponseField name="png_4x" type="string" required>
                      Raster fallback at quadruple density.
                    </ResponseField>

                    <ResponseField name="svg" type="string" required>
                      The vector file. Prefer this everywhere SVG renders.
                    </ResponseField>
                  </Accordion>
                </ResponseField>

                <ResponseField name="light" type="object" required>
                  The colorway for light surfaces.

                  <Accordion title="Properties" defaultOpen={true}>
                    <ResponseField name="png_1x" type="string" required>
                      Raster fallback at the shape's native size.
                    </ResponseField>

                    <ResponseField name="png_2x" type="string" required>
                      Raster fallback at double density.
                    </ResponseField>

                    <ResponseField name="png_4x" type="string" required>
                      Raster fallback at quadruple density.
                    </ResponseField>

                    <ResponseField name="svg" type="string" required>
                      The vector file. Prefer this everywhere SVG renders.
                    </ResponseField>
                  </Accordion>
                </ResponseField>
              </Accordion>
            </ResponseField>

            <ResponseField name="square" type="object" required>
              The square tile (32x32).

              <Accordion title="Properties" defaultOpen={true}>
                <ResponseField name="dark" type="object" required>
                  The colorway for dark surfaces.

                  <Accordion title="Properties" defaultOpen={true}>
                    <ResponseField name="png_1x" type="string" required>
                      Raster fallback at the shape's native size.
                    </ResponseField>

                    <ResponseField name="png_2x" type="string" required>
                      Raster fallback at double density.
                    </ResponseField>

                    <ResponseField name="png_4x" type="string" required>
                      Raster fallback at quadruple density.
                    </ResponseField>

                    <ResponseField name="svg" type="string" required>
                      The vector file. Prefer this everywhere SVG renders.
                    </ResponseField>
                  </Accordion>
                </ResponseField>

                <ResponseField name="light" type="object" required>
                  The colorway for light surfaces.

                  <Accordion title="Properties" defaultOpen={true}>
                    <ResponseField name="png_1x" type="string" required>
                      Raster fallback at the shape's native size.
                    </ResponseField>

                    <ResponseField name="png_2x" type="string" required>
                      Raster fallback at double density.
                    </ResponseField>

                    <ResponseField name="png_4x" type="string" required>
                      Raster fallback at quadruple density.
                    </ResponseField>

                    <ResponseField name="svg" type="string" required>
                      The vector file. Prefer this everywhere SVG renders.
                    </ResponseField>
                  </Accordion>
                </ResponseField>
              </Accordion>
            </ResponseField>
          </Accordion>
        </ResponseField>

        <ResponseField name="installment_count" type="number | null" required>
          Installment methods only: how many payments the charge splits into. Data, not
          copy — compose and translate the label client-side.
        </ResponseField>

        <ResponseField name="payment_method_type" type="string" required>
          The payment method type identifier, e.g. `card`, `klarna`, `apple_pay`.
        </ResponseField>
      </Accordion>
    </ResponseField>

    <ResponseField name="payment_method_id" type="string | null" required>
      The saved payment method, prefixed `payt_`, ready to charge with Create
      Payment. Null until the setup has `succeeded`.
    </ResponseField>

    <ResponseField name="payment_method_type" type="string | null" required>
      The kind of instrument being saved, for example `card` or `us_bank_account`.
    </ResponseField>

    <ResponseField name="return_url" type="string | null" required>
      Where the buyer lands after completing an off-site step, or `null` to leave
      them where they are.
    </ResponseField>

    <ResponseField name="status" type="string" required>
      How far the setup has got. **A 201 or 200 means we answered, not that the method was saved — always branch on this.** `requires_action` — the buyer has a step outstanding; hand `client_secret` to the elements or poll Retrieve setup status. `processing` — the processor is deciding. `succeeded` — the method is saved, and only this one means saved. `canceled` — abandoned or refused; see `last_setup_error`.

      Available options: `processing`, `succeeded`, `canceled`, `requires_action`
    </ResponseField>

    <ResponseField name="three_ds_verified" type="boolean" required>
      True when the buyer completed 3D Secure while saving this payment method.
    </ResponseField>

    <ResponseField name="updated_at" type="string" required>
      When the setup intent was last updated, as an ISO 8601 timestamp.
    </ResponseField>

    <ResponseField name="user" type="object | null" required>
      The user saving the payment method. Null when the buyer is a company rather than a user.

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
  </Column>

  <Column>
    <div className="api-resource-sticky-example">
      ```json SetupIntent theme={null}
      {
      	"id": "sint_xxxxxxxxxxxxxx",
      	"account_id": "biz_xxxxxxxxxxxxxx",
      	"checkout_configuration_id": null,
      	"client_secret": null,
      	"created_at": "2026-09-22T12:00:00.000Z",
      	"last_setup_error": null,
      	"member_id": "mber_xxxxxxxxxxxxxx",
      	"metadata": {
      		"customer_id": "cus_4417"
      	},
      	"payment_method_id": "payt_xxxxxxxxxxxxxx",
      	"payment_method_type": "card",
      	"return_url": "https://shinetime.example/billing/saved",
      	"status": "succeeded",
      	"three_ds_verified": false,
      	"updated_at": "2026-09-22T12:00:00.000Z",
      	"user": {
      		"id": "user_xxxxxxxxxxxxxx",
      		"name": "Dana Whitfield",
      		"profile_picture": {
      			"url": "https://ui-avatars.com/api/"
      		},
      		"username": "danawhitfield"
      	},
      	"payment_instrument": {
      		"card": {
      			"brand": "visa",
      			"issuer_identification_number": "41111111",
      			"last4": "4242",
      			"exp_month": 11,
      			"exp_year": 2030
      		},
      		"display_name": "Visa •••• 4242",
      		"icons": {
      			"card": {
      				"dark": {
      					"png_1x": "https://content.whop.com/payment_methods/visa/icons/card_dark_30.png",
      					"png_2x": "https://content.whop.com/payment_methods/visa/icons/card_dark_60.png",
      					"png_4x": "https://content.whop.com/payment_methods/visa/icons/card_dark_120.png",
      					"svg": "https://content.whop.com/payment_methods/visa/icons/card_dark.svg"
      				},
      				"light": {
      					"png_1x": "https://content.whop.com/payment_methods/visa/icons/card_dark_30.png",
      					"png_2x": "https://content.whop.com/payment_methods/visa/icons/card_dark_60.png",
      					"png_4x": "https://content.whop.com/payment_methods/visa/icons/card_dark_120.png",
      					"svg": "https://content.whop.com/payment_methods/visa/icons/card_dark.svg"
      				}
      			},
      			"square": {
      				"dark": {
      					"png_1x": "https://content.whop.com/payment_methods/visa/icons/card_dark_30.png",
      					"png_2x": "https://content.whop.com/payment_methods/visa/icons/card_dark_60.png",
      					"png_4x": "https://content.whop.com/payment_methods/visa/icons/card_dark_120.png",
      					"svg": "https://content.whop.com/payment_methods/visa/icons/card_dark.svg"
      				},
      				"light": {
      					"png_1x": "https://content.whop.com/payment_methods/visa/icons/card_dark_30.png",
      					"png_2x": "https://content.whop.com/payment_methods/visa/icons/card_dark_60.png",
      					"png_4x": "https://content.whop.com/payment_methods/visa/icons/card_dark_120.png",
      					"svg": "https://content.whop.com/payment_methods/visa/icons/card_dark.svg"
      				}
      			}
      		},
      		"installment_count": null,
      		"payment_method_type": "card"
      	}
      }
      ```
    </div>
  </Column>
</Columns>
