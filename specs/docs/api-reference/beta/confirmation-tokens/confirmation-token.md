> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Confirmation Token

A Confirmation Token is a single-use, short-lived reference to a payment method and billing details collected from a buyer. Its response contains only a display-safe preview and never returns the underlying payment credential.

Create a confirmation token in a buyer-facing collection flow, then send its `ctok_` ID to the Payments API from your server. Retrieve a token to display its payment method and billing preview or check whether it is still usable.

## Endpoints

| Endpoint                                                                                           | Request                                                                      |
| -------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| [Create Confirmation Token](/api-reference/beta/confirmation-tokens/create-confirmation-token)     | <Badge color="green" size="sm" stroke>POST</Badge> `/confirmation_tokens`    |
| [Retrieve Confirmation Token](/api-reference/beta/confirmation-tokens/retrieve-confirmation-token) | <Badge color="blue" size="sm" stroke>GET</Badge> `/confirmation_tokens/{id}` |

## Attributes

<Columns cols={2}>
  <Column>
    <ResponseField name="id" type="string" required />

    <ResponseField name="billing_details" type="object | null" required>
      Enough of the billing details to raise a customer record and recognise the method — email, name, country and postal code. The street address is collected for the charge but never returned; this endpoint is a display-safe preview.

      <Accordion title="Properties" defaultOpen={true}>
        <ResponseField name="country" type="string | null" required>
          ISO 3166-1 alpha-2 country code.
        </ResponseField>

        <ResponseField name="email" type="string | null" required>
          Email supplied when the method was collected.
        </ResponseField>

        <ResponseField name="name" type="string | null" required>
          Name on the payment method.
        </ResponseField>

        <ResponseField name="postal_code" type="string | null" required>
          Postal or ZIP code.
        </ResponseField>
      </Accordion>
    </ResponseField>

    <ResponseField name="created_at" type="string" required>
      When the token was created, as an ISO 8601 timestamp.
    </ResponseField>

    <ResponseField name="expires_at" type="string" required>
      When the token expires, as an ISO 8601 timestamp. Tokens are single-use and
      short-lived.
    </ResponseField>

    <ResponseField name="object" type="string" required>
      Always `confirmation_token`.
    </ResponseField>

    <ResponseField name="payment_method_preview" type="object" required>
      Display-only preview of the collected method — never the underlying token.

      <Accordion title="Properties" defaultOpen={true}>
        <ResponseField name="id" type="string | null" required>
          The saved payment method this preview came from, or `null` when the buyer
          supplied a new one.
        </ResponseField>

        <ResponseField name="bank_debit" type="object">
          Present when the category is `bank_debit`. Empty until the account is charged.

          <Accordion title="Properties" defaultOpen={true}>
            <ResponseField name="brand" type="string">
              Lowercase card brand, such as `visa` or `mastercard`.
            </ResponseField>

            <ResponseField name="fingerprint" type="string">
              Uniquely identifies this particular card number. Matches the `fingerprint` on
              any payment method saved from this token, so you can recognize a card across
              attempts. For a wallet, this identifies the network token rather than the
              underlying card.
            </ResponseField>

            <ResponseField name="last4" type="string">
              The last four digits of the card.
            </ResponseField>
          </Accordion>
        </ResponseField>

        <ResponseField name="card" type="object">
          Details of the card, when the category is `card`.

          <Accordion title="Properties" defaultOpen={true}>
            <ResponseField name="brand" type="string">
              Lowercase card brand, such as `visa` or `mastercard`.
            </ResponseField>

            <ResponseField name="fingerprint" type="string">
              Uniquely identifies this particular card number. Matches the `fingerprint` on
              any payment method saved from this token, so you can recognize a card across
              attempts. For a wallet, this identifies the network token rather than the
              underlying card.
            </ResponseField>

            <ResponseField name="last4" type="string">
              The last four digits of the card.
            </ResponseField>
          </Accordion>
        </ResponseField>

        <ResponseField name="category" type="string" required>
          The family the type belongs to.

          Available options: `card`, `wallet`, `bank_debit`, `bank_transfer`, `voucher`, `redirect`, `crypto`, `balance`, `in_app_purchase`, `saved`
        </ResponseField>

        <ResponseField name="display_name" type="string" required>
          Human-readable label for the method, e.g. `Visa •••• 4242`.
        </ResponseField>

        <ResponseField name="saved" type="object">
          Details of the stored card, when the category is `saved`. Absent for a balance.

          <Accordion title="Properties" defaultOpen={true}>
            <ResponseField name="brand" type="string">
              Lowercase card brand, such as `visa` or `mastercard`.
            </ResponseField>

            <ResponseField name="fingerprint" type="string">
              Uniquely identifies this particular card number. Matches the `fingerprint` on
              any payment method saved from this token, so you can recognize a card across
              attempts. For a wallet, this identifies the network token rather than the
              underlying card.
            </ResponseField>

            <ResponseField name="last4" type="string">
              The last four digits of the card.
            </ResponseField>
          </Accordion>
        </ResponseField>

        <ResponseField name="type" type="string" required>
          The payment method type, e.g. `card`, `apple_pay`, `klarna`.
        </ResponseField>

        <ResponseField name="wallet" type="object">
          Details of the network token the wallet supplied, when the category is `wallet`.

          <Accordion title="Properties" defaultOpen={true}>
            <ResponseField name="brand" type="string">
              Lowercase card brand, such as `visa` or `mastercard`.
            </ResponseField>

            <ResponseField name="fingerprint" type="string">
              Uniquely identifies this particular card number. Matches the `fingerprint` on
              any payment method saved from this token, so you can recognize a card across
              attempts. For a wallet, this identifies the network token rather than the
              underlying card.
            </ResponseField>

            <ResponseField name="last4" type="string">
              The last four digits of the card.
            </ResponseField>
          </Accordion>
        </ResponseField>
      </Accordion>
    </ResponseField>

    <ResponseField name="setup_future_usage" type="string | null" required>
      Save-consent state the element displayed at collection: `off_session`,
      `on_session`, or `null`. Confirm may vault only if attested here.
    </ResponseField>

    <ResponseField name="status" type="string" required>
      `pending` until it is used, then `consumed`; `expired` once its short lifetime elapses. Only a `pending` token can be charged.

      Available options: `pending`, `consumed`, `expired`
    </ResponseField>
  </Column>

  <Column>
    <div className="api-resource-sticky-example">
      ```json ConfirmationToken theme={null}
      {
      	"id": "ctok_xxxxxxxxxxxxxx",
      	"object": "confirmation_token",
      	"status": "pending",
      	"payment_method_preview": {
      		"id": null,
      		"type": "card",
      		"category": "card",
      		"display_name": "Visa •••• 4242",
      		"card": {
      			"brand": "visa",
      			"fingerprint": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
      			"last4": "4242"
      		}
      	},
      	"setup_future_usage": "off_session",
      	"billing_details": {
      		"email": "buyer@example.com",
      		"name": "Buyer Name",
      		"country": "US",
      		"postal_code": "78701"
      	},
      	"created_at": "2026-08-28T12:00:00.000Z",
      	"expires_at": "2026-08-28T12:15:00.000Z"
      }
      ```
    </div>
  </Column>
</Columns>
