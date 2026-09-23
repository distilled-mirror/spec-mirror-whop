> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Payments

A Payment is one charge against a buyer. Create an on-session payment with a `confirmation_token` for the method the buyer selected, or an off-session payment with an existing member's stored payment method.

Collection runs in the background, so the create response is not the outcome. Poll [Retrieve status](/api-reference/beta/payments/retrieve-payment-status) for how far the payment has gone and, while it is `requires_action`, what the buyer must do next — follow a redirect, complete 3D Secure, display transfer instructions, or link a bank account. Use the return\_url operation to change where they land afterwards, up until they come back.

## Endpoints

| Endpoint                                                                            | Request                                                                                  |
| ----------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| [List Payments](/api-reference/beta/payments/list-payments)                         | <Badge color="blue" size="sm" stroke>GET</Badge> `/payments`                             |
| [Retrieve Payment](/api-reference/beta/payments/retrieve-payment)                   | <Badge color="blue" size="sm" stroke>GET</Badge> `/payments/{id}`                        |
| [List Payment Fees](/api-reference/beta/payments/list-payment-fees)                 | <Badge color="blue" size="sm" stroke>GET</Badge> `/payments/{id}/fees`                   |
| [Retrieve payment status](/api-reference/beta/payments/retrieve-payment-status)     | <Badge color="blue" size="sm" stroke>GET</Badge> `/payments/{payment_id}/status`         |
| [Create Payment](/api-reference/beta/payments/create-payment)                       | <Badge color="green" size="sm" stroke>POST</Badge> `/payments`                           |
| [Capture payment](/api-reference/beta/payments/capture-payment)                     | <Badge color="green" size="sm" stroke>POST</Badge> `/payments/{id}/capture`              |
| [Refund Payment](/api-reference/beta/payments/refund-payment)                       | <Badge color="green" size="sm" stroke>POST</Badge> `/payments/{id}/refund`               |
| [Resume a parked renewal](/api-reference/beta/payments/resume-a-parked-renewal)     | <Badge color="green" size="sm" stroke>POST</Badge> `/payments/{payment_id}/resume`       |
| [Retry Payment](/api-reference/beta/payments/retry-payment)                         | <Badge color="green" size="sm" stroke>POST</Badge> `/payments/{id}/retry`                |
| [Void Payment](/api-reference/beta/payments/void-payment)                           | <Badge color="green" size="sm" stroke>POST</Badge> `/payments/{id}/void`                 |
| [Update payment return URL](/api-reference/beta/payments/update-payment-return-url) | <Badge color="orange" size="sm" stroke>PATCH</Badge> `/payments/{payment_id}/return_url` |

## Attributes

<Columns cols={2}>
  <Column>
    <ResponseField name="id" type="string" required>
      Payment ID, prefixed `pay_`.
    </ResponseField>

    <ResponseField name="account_id" type="string | null" required>
      The account that received the payment, prefixed `biz_`.
    </ResponseField>

    <ResponseField name="amount_after_fees" type="object" required>
      What the account keeps: the total less Whop's fees.

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

    <ResponseField name="auto_refunded" type="boolean" required>
      True when Whop refunded the payment automatically, for example on a dispute
      alert.
    </ResponseField>

    <ResponseField name="billing_address" type="object | null" required>
      The billing address the buyer entered, or null.

      <Accordion title="Properties" defaultOpen={true}>
        <ResponseField name="city" type="string | null" required>
          The city.
        </ResponseField>

        <ResponseField name="country" type="string | null" required>
          The ISO 3166-1 alpha-2 country code.
        </ResponseField>

        <ResponseField name="line1" type="string | null" required>
          The first street address line.
        </ResponseField>

        <ResponseField name="line2" type="string | null" required>
          The second street address line.
        </ResponseField>

        <ResponseField name="name" type="string | null" required>
          The name on the address.
        </ResponseField>

        <ResponseField name="postal_code" type="string | null" required>
          The postal or ZIP code.
        </ResponseField>

        <ResponseField name="state" type="string | null" required>
          The state, province or region.
        </ResponseField>
      </Accordion>
    </ResponseField>

    <ResponseField name="billing_reason" type="string | null" required>
      Why the charge was created: a first purchase, a renewal, a one-time payment,
      or a manual charge.
    </ResponseField>

    <ResponseField name="checkout_configuration_id" type="string | null" required>
      The checkout configuration the buyer paid through, prefixed `ch_`, or null.
    </ResponseField>

    <ResponseField name="client_secret" type="string | null" required>
      The credential a buyer's surface presents to poll this payment and set its
      return URL. Only on payments created from a confirmation token, and always
      null in list responses — retrieve the payment for it.
    </ResponseField>

    <ResponseField name="created_at" type="string" required>
      When the payment was created, as an ISO 8601 timestamp.
    </ResponseField>

    <ResponseField name="currency" type="string" required>
      The currency the payment settles in, lowercase ISO 4217. Every money field below is stated in it unless it says otherwise.

      Available options: `usd`, `sgd`, `inr`, `aud`, `brl`, `cad`, `dkk`, `eur`, `nok`, `gbp`, `sek`, `chf`, `hkd`, `huf`, `jpy`, `mxn`, `myr`, `pln`, `czk`, `nzd`, `aed`, `eth`, `ape`, `cop`, `ron`, `thb`, `bgn`, `idr`, `dop`, `php`, `try`, `krw`, `twd`, `vnd`, `pkr`, `clp`, `uyu`, `ars`, `zar`, `dzd`, `tnd`, `mad`, `kes`, `kwd`, `jod`, `all`, `xcd`, `amd`, `bsd`, `bhd`, `bob`, `bam`, `khr`, `crc`, `xof`, `egp`, `etb`, `gmd`, `ghs`, `gtq`, `gyd`, `ils`, `jmd`, `mop`, `mga`, `mur`, `mdl`, `mnt`, `nad`, `ngn`, `mkd`, `omr`, `pyg`, `pen`, `qar`, `rwf`, `sar`, `rsd`, `lkr`, `tzs`, `ttd`, `uzs`, `rub`, `btc`, `cny`, `usdt`, `kzt`, `awg`, `whop_usd`, `xau`
    </ResponseField>

    <ResponseField name="customer_email" type="string | null" required>
      The buyer's email address. Null without `member:email:read` on the account or
      when the buyer has no assigned email.
    </ResponseField>

    <ResponseField name="customer_phone" type="string | null" required>
      The phone number the buyer gave at checkout, when one was collected.
    </ResponseField>

    <ResponseField name="decline_code" type="string | null" required>
      The normalized decline reason of the most recent failed attempt, or null.
    </ResponseField>

    <ResponseField name="dispute_alerted_at" type="string | null" required>
      When an issuer warned that this payment will be disputed, or null.
    </ResponseField>

    <ResponseField name="failure_message" type="string | null" required>
      Why the most recent attempt failed, in plain words, or null.
    </ResponseField>

    <ResponseField name="financing_installments_count" type="number | null" required>
      For installment methods, how many payments the charge splits into.
    </ResponseField>

    <ResponseField name="holds" type="object[]" required>
      The active holds on this payment. Each hold has its own release date, independent of `settlement_time_at`. Empty when nothing is held; released holds are omitted.

      <Accordion title="Properties" defaultOpen={true}>
        <ResponseField name="amount" type="object" required>
          The amount currently held, in the hold's currency.

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

        <ResponseField name="percentage" type="number | null" required>
          The reserve percentage recorded when the hold was created, for example 3.5 for
          3.5%. Null for other hold types or when no percentage was recorded.
        </ResponseField>

        <ResponseField name="release_at" type="string | null" required>
          When the held funds are scheduled to become available, as an ISO 8601
          timestamp. Never earlier than the payment's settlement date. Null when release
          depends on an event, such as shipment resolution, rather than a date.
        </ResponseField>

        <ResponseField name="type" type="string" required>
          The reason funds are held: `reserve`, `bnpl`, `sequra`, `fraud_hold`, or `preshipment_hold`.

          Available options: `reserve`, `bnpl`, `sequra`, `fraud_hold`, `preshipment_hold`
        </ResponseField>
      </Accordion>
    </ResponseField>

    <ResponseField name="last_payment_attempt_at" type="string | null" required>
      When the most recent charge attempt ran, or null.
    </ResponseField>

    <ResponseField name="line_items" type="object[]" required>
      Everything this payment charged for, in purchase order, with quantities and subtotals in the purchase currency. Payments made before item snapshots were recorded return the single item implied by their plan. Empty when no items or plan can be resolved.

      <Accordion title="Properties" defaultOpen={true}>
        <ResponseField name="id" type="string | null" required>
          Line item ID, prefixed `li_`. Null when the payment predates item snapshots
          and the item is read from the payment's plan.
        </ResponseField>

        <ResponseField name="label" type="string | null" required>
          The item's name as shown at checkout — the product title, else the plan title.
        </ResponseField>

        <ResponseField name="plan_id" type="string | null" required>
          The plan bought, prefixed `plan_`. Null when the plan has since been deleted.
        </ResponseField>

        <ResponseField name="plan_title" type="string | null" required>
          The plan's current title, or `null` when the plan has been deleted or has no
          title.
        </ResponseField>

        <ResponseField name="product_id" type="string | null" required>
          The product the plan belongs to, prefixed `prod_`. On a payment that predates
          item snapshots this falls back to the plan's product, so it can be set where
          the parent's own `product_id` is null. Null for a plan with no product.
        </ResponseField>

        <ResponseField name="product_title" type="string | null" required>
          The product's current title, or `null` when the item has no product.
        </ResponseField>

        <ResponseField name="quantity" type="number" required>
          How many units were bought.
        </ResponseField>

        <ResponseField name="subtotal" type="object | null" required>
          The recorded amount for this item's full quantity, before discounts, tax, and fees, in its purchase currency. Returns `null` when no item amount was recorded.

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
      </Accordion>
    </ResponseField>

    <ResponseField name="member_id" type="string | null" required>
      The buyer's member record on the account, prefixed `mber_`. Null without the
      member:basic:read permission.
    </ResponseField>

    <ResponseField name="membership_id" type="string | null" required>
      The membership this payment is billed against, prefixed `mem_`. Null for
      one-off purchases or without the member:basic:read permission.
    </ResponseField>

    <ResponseField name="metadata" type="object | null" required>
      Your own key-value data attached when the payment was created.
    </ResponseField>

    <ResponseField name="needs_tracking" type="boolean | null" required>
      True when funds are held until the order ships and no tracking number has been
      added yet. Null without the shipment:basic:read permission.
    </ResponseField>

    <ResponseField name="next_payment_attempt_at" type="string | null" required>
      When the next automatic retry is scheduled, or null.
    </ResponseField>

    <ResponseField name="paid_at" type="string | null" required>
      When the money was collected, or null while it has not been.
    </ResponseField>

    <ResponseField name="payment_instrument" type="object | null" required>
      The instrument shaped for display: a buyer-facing name, the standard icon set, and the card's brand, last four and issuer identification number when it was a card.

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
      The stored payment method that was charged, prefixed `payt_`. Null when the
      method was not saved.
    </ResponseField>

    <ResponseField name="payment_method_type" type="string | null" required>
      The kind of instrument used, for example `card`, `apple_pay`, `klarna`, or
      `us_bank_account`.
    </ResponseField>

    <ResponseField name="payment_rule_matches" type="object[]" required>
      The account's own payment rules that decided this payment, recorded when they ran. Only one action is taken per payment, so a rule that matched but was skipped or outranked is not listed. Empty when none decided it, when the account had no rules, or when Whop blocked the payment before they ran.

      <Accordion title="Properties" defaultOpen={true}>
        <ResponseField name="id" type="string" required>
          Payment rule ID, prefixed `prule_`.
        </ResponseField>

        <ResponseField name="action" type="string" required>
          What the rule asked for.

          Available options: `allow`, `block`, `review`, `enforce_3ds`
        </ResponseField>

        <ResponseField name="name" type="string | null" required>
          The rule's name when it matched. Renaming the rule afterwards does not rewrite this.
        </ResponseField>
      </Accordion>
    </ResponseField>

    <ResponseField name="payments_failed" type="number" required>
      How many charge attempts have failed on this payment.
    </ResponseField>

    <ResponseField name="plan_id" type="string | null" required>
      The plan that was charged, prefixed `plan_`.
    </ResponseField>

    <ResponseField name="presentment_total" type="object | null" required>
      The account-facing total in the currency presented to the buyer, before conversion into the settlement currency. Excludes buyer fees.

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

    <ResponseField name="product_id" type="string | null" required>
      The product the plan belongs to, prefixed `prod_`. Null for a plan with no
      product.
    </ResponseField>

    <ResponseField name="promo_code_id" type="string | null" required>
      The promo code applied at checkout, prefixed `promo_`, or null.
    </ResponseField>

    <ResponseField name="recovery_url" type="string | null" required>
      Whop-hosted URL where the buyer can sign in and complete 3D Secure for an
      off-session charge the bank challenged — a subscription renewal or a
      saved-card payment. Null when recovery is unavailable, you lack
      `member:basic:read`, or in list responses. Retrieve the payment for it.
    </ResponseField>

    <ResponseField name="refundable" type="boolean" required>
      True when the payment is `paid`, not yet fully refunded, and its processor
      supports refunds.
    </ResponseField>

    <ResponseField name="refunded_amount" type="object | null" required>
      How much has been refunded so far, as it settled — refunds convert at the rate in force when each one was issued, not the payment's original rate.

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

    <ResponseField name="refunded_at" type="string | null" required>
      When the payment was refunded, or null.
    </ResponseField>

    <ResponseField name="retryable" type="boolean" required>
      True when the payment is `open` and Whop can attempt the charge again — see
      `POST /payments/\{id}/retry`.
    </ResponseField>

    <ResponseField name="risk_score" type="number | null" required>
      Whop's published risk index from 0 (lowest) to 100 (highest), including
      enforced decision floors. This is not a fraud probability. Null when no score
      is available.
    </ResponseField>

    <ResponseField name="risk_signals" type="object | null" required>
      Deprecated. Risk score explanations are no longer provided; always null.
      DEPRECATED: Risk score explanations are no longer provided. Always null.
    </ResponseField>

    <ResponseField name="settlement_time_at" type="string | null" required>
      When the portion not listed in `holds` posts to the account's available
      balance, at midnight UTC. The `financial_activity.funds_available` webhook's
      `posted_at` carries the same value when the settlement that clears it posts.
      Null until the payment is paid, and always null in list responses — retrieve
      the payment for it.
    </ResponseField>

    <ResponseField name="shipment_id" type="string | null" required>
      The shipment fulfilling this payment, prefixed `ship_`. Null when nothing
      ships or without the shipment:basic:read permission.
    </ResponseField>

    <ResponseField name="shipping_address" type="object | null" required>
      The shipping address for physical goods, or null.

      <Accordion title="Properties" defaultOpen={true}>
        <ResponseField name="city" type="string | null" required>
          The city.
        </ResponseField>

        <ResponseField name="country" type="string | null" required>
          The ISO 3166-1 alpha-2 country code.
        </ResponseField>

        <ResponseField name="line1" type="string | null" required>
          The first street address line.
        </ResponseField>

        <ResponseField name="line2" type="string | null" required>
          The second street address line.
        </ResponseField>

        <ResponseField name="name" type="string | null" required>
          The name on the address.
        </ResponseField>

        <ResponseField name="postal_code" type="string | null" required>
          The postal or ZIP code.
        </ResponseField>

        <ResponseField name="state" type="string | null" required>
          The state, province or region.
        </ResponseField>
      </Accordion>
    </ResponseField>

    <ResponseField name="status" type="string" required>
      The lifecycle state of the charge: `open` while collection is outstanding, `paid` once the money moved, `pending` while a settlement rail clears, `void`/`uncollectible` when it ended without collecting.

      Available options: `draft`, `open`, `authorized`, `paid`, `pending`, `uncollectible`, `unresolved`, `void`
    </ResponseField>

    <ResponseField name="substatus" type="string" required>
      The dashboard's finer-grained reading of the payment, folding in refunds, disputes and Resolution Center cases.

      Available options: `succeeded`, `requires_capture`, `pending`, `failed`, `blocked`, `past_due`, `canceled`, `price_too_low`, `uncollectible`, `refunded`, `auto_refunded`, `partially_refunded`, `dispute_warning`, `dispute_needs_response`, `dispute_warning_needs_response`, `resolution_needs_response`, `dispute_under_review`, `dispute_warning_under_review`, `resolution_under_review`, `dispute_won`, `dispute_warning_closed`, `resolution_won`, `dispute_lost`, `dispute_closed`, `resolution_lost`, `drafted`, `incomplete`, `unresolved`, `open_dispute`, `open_resolution`
    </ResponseField>

    <ResponseField name="subtotal" type="object | null" required>
      The price before discounts, tax and fees.

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

    <ResponseField name="tax_amount" type="object | null" required>
      The sales tax or VAT collected. Null when no tax applied.

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

    <ResponseField name="tax_behavior" type="string | null" required>
      Whether `tax_amount` was added on top of the price (`exclusive`) or was
      already inside it (`inclusive`).
    </ResponseField>

    <ResponseField name="tax_refunded_amount" type="object" required>
      How much of the collected tax has been returned to the buyer so far. Zero when the payment carried no tax, or when nothing has been refunded.

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

    <ResponseField name="three_ds_verified" type="boolean" required>
      True when the buyer completed 3D Secure for this payment.
    </ResponseField>

    <ResponseField name="total" type="object | null" required>
      The account-facing total: the price after discounts, plus any tax added on top. Excludes buyer fees, which the buyer pays above this amount — so this is not necessarily what the buyer's statement shows.

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

    <ResponseField name="updated_at" type="string" required>
      When the payment last changed, as an ISO 8601 timestamp.
    </ResponseField>

    <ResponseField name="usd_total" type="object | null" required>
      The total converted to USD at the time of the charge, for reporting across currencies. Excludes the adaptive pricing FX markup, which the account does not keep.

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

    <ResponseField name="user" type="object | null" required>
      The buyer. Null when the payment belongs to a company buyer rather than a user.

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

    <ResponseField name="verification_checks" type="object | null" required>
      The Address Verification Service (AVS), cardholder name, and Card Verification Value (CVV/CVC) results, or null when the processor returned none.

      <Accordion title="Properties" defaultOpen={true}>
        <ResponseField name="address_line1" type="string | null" required>
          The Address Verification Service (AVS) result for the billing street address.
        </ResponseField>

        <ResponseField name="authorization_code" type="string | null" required>
          The card issuer's authorization code for this charge, or null when the
          processor did not return one.
        </ResponseField>

        <ResponseField name="card_holder_name" type="string | null" required>
          Whether the cardholder name matched the issuer's records.
        </ResponseField>

        <ResponseField name="card_security_code" type="string | null" required>
          The Card Verification Value (CVV/CVC) result.
        </ResponseField>

        <ResponseField name="zip_code" type="string | null" required>
          The Address Verification Service (AVS) result for the billing postal code.
        </ResponseField>
      </Accordion>
    </ResponseField>

    <ResponseField name="voidable" type="boolean" required>
      True when the payment can be voided or canceled. The request is rejected if
      the payment is no longer eligible — see `POST /payments/\{id}/void`.
    </ResponseField>
  </Column>

  <Column>
    <div className="api-resource-sticky-example">
      ```json Payment theme={null}
      {
      	"id": "pay_xxxxxxxxxxxxxx",
      	"account_id": "biz_xxxxxxxxxxxxxx",
      	"amount_after_fees": {
      		"amount": "27.10",
      		"currency": "usd",
      		"decimals": 2,
      		"display_decimals": 2
      	},
      	"auto_refunded": false,
      	"billing_address": {
      		"city": "Austin",
      		"country": "US",
      		"line1": "1114 Bouldin Ave",
      		"line2": "Unit B",
      		"name": "Dana Whitfield",
      		"postal_code": "78704",
      		"state": "TX"
      	},
      	"billing_reason": "subscription_create",
      	"checkout_configuration_id": null,
      	"client_secret": null,
      	"created_at": "2026-01-01T12:00:00.000Z",
      	"currency": "usd",
      	"customer_email": "marcus@shinetime.example",
      	"customer_phone": "+15125550142",
      	"decline_code": null,
      	"dispute_alerted_at": null,
      	"failure_message": null,
      	"financing_installments_count": null,
      	"last_payment_attempt_at": "2026-01-01T12:00:00.000Z",
      	"line_items": [
      		{
      			"id": "li_xxxxxxxxxxxxxx",
      			"label": "Shine Time Detailing Club",
      			"plan_id": "plan_xxxxxxxxxxxxxx",
      			"plan_title": "Monthly",
      			"product_id": "prod_xxxxxxxxxxxxxx",
      			"product_title": "Shine Time Detailing Club",
      			"quantity": 1,
      			"subtotal": {
      				"amount": "27.99",
      				"currency": "usd",
      				"decimals": 2,
      				"display_decimals": 2
      			}
      		}
      	],
      	"member_id": "mber_xxxxxxxxxxxxxx",
      	"membership_id": "mem_xxxxxxxxxxxxxx",
      	"metadata": {
      		"order_ref": "SHINE-4417"
      	},
      	"needs_tracking": false,
      	"next_payment_attempt_at": null,
      	"paid_at": "2026-01-01T12:00:00.000Z",
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
      		"installment_count": 0,
      		"payment_method_type": "card"
      	},
      	"payment_method_id": "payt_xxxxxxxxxxxxxx",
      	"payment_method_type": "card",
      	"payment_rule_matches": [
      		{
      			"id": "prule_xxxxxxxxxxxxx",
      			"action": "allow",
      			"name": "Allow returning members"
      		}
      	],
      	"payments_failed": 0,
      	"plan_id": "plan_xxxxxxxxxxxxxx",
      	"presentment_total": {
      		"amount": "29.99",
      		"currency": "usd",
      		"decimals": 2,
      		"display_decimals": 2
      	},
      	"product_id": "prod_xxxxxxxxxxxxxx",
      	"promo_code_id": null,
      	"recovery_url": null,
      	"refundable": true,
      	"refunded_amount": {
      		"amount": "0.00",
      		"currency": "usd",
      		"decimals": 2,
      		"display_decimals": 2
      	},
      	"refunded_at": null,
      	"retryable": false,
      	"risk_score": 12,
      	"risk_signals": null,
      	"holds": [
      		{
      			"type": "reserve",
      			"amount": {
      				"amount": "1.00",
      				"currency": "usd",
      				"decimals": 2,
      				"display_decimals": 2
      			},
      			"percentage": 3.5,
      			"release_at": "2026-04-01T00:00:00.000Z"
      		}
      	],
      	"settlement_time_at": "2026-01-03T12:00:00.000Z",
      	"shipment_id": null,
      	"shipping_address": {
      		"city": "Austin",
      		"country": "US",
      		"line1": "1114 Bouldin Ave",
      		"line2": "Unit B",
      		"name": "Dana Whitfield",
      		"postal_code": "78704",
      		"state": "TX"
      	},
      	"status": "paid",
      	"substatus": "succeeded",
      	"subtotal": {
      		"amount": "27.99",
      		"currency": "usd",
      		"decimals": 2,
      		"display_decimals": 2
      	},
      	"tax_amount": {
      		"amount": "2.00",
      		"currency": "usd",
      		"decimals": 2,
      		"display_decimals": 2
      	},
      	"tax_behavior": "exclusive",
      	"tax_refunded_amount": {
      		"amount": "0.00",
      		"currency": "usd",
      		"decimals": 2,
      		"display_decimals": 2
      	},
      	"three_ds_verified": true,
      	"total": {
      		"amount": "29.99",
      		"currency": "usd",
      		"decimals": 2,
      		"display_decimals": 2
      	},
      	"updated_at": "2026-01-01T12:00:00.000Z",
      	"usd_total": {
      		"amount": "29.99",
      		"currency": "usd",
      		"decimals": 2,
      		"display_decimals": 2
      	},
      	"user": {
      		"id": "user_xxxxxxxxxxxxxx",
      		"name": "Dana Whitfield",
      		"profile_picture": {
      			"url": "https://ui-avatars.com/api/"
      		},
      		"username": "danawhitfield"
      	},
      	"verification_checks": {
      		"address_line1": "PASS",
      		"authorization_code": "A1B2C3",
      		"card_holder_name": "PASS",
      		"card_security_code": "PASS",
      		"zip_code": "PASS"
      	},
      	"voidable": false
      }
      ```
    </div>
  </Column>
</Columns>
