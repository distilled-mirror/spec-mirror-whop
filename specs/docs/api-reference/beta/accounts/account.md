> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Account

An Account represents a person or business on Whop that can have its own profile, wallet, and account-scoped settings. Use accounts for customers, creators, merchants, sellers, or connected businesses your integration supports.

Use the Accounts API to create accounts, list accounts visible to your credentials, retrieve or update an account, suspend a connected account managed by your platform, and retrieve the account associated with the current API key.

The `business_type`, `industry_group`, and `industry_type` fields classify accounts. See the [business types and industries glossary](#business-types-and-industries-glossary) for every valid value.

## Endpoints

| Endpoint                                                                                  | Request                                                                                   |
| ----------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| [List Accounts](/api-reference/beta/accounts/list-accounts)                               | <Badge color="blue" size="sm" stroke>GET</Badge> `/accounts`                              |
| [Create Account](/api-reference/beta/accounts/create-account)                             | <Badge color="green" size="sm" stroke>POST</Badge> `/accounts`                            |
| [Retrieve Account](/api-reference/beta/accounts/retrieve-account)                         | <Badge color="blue" size="sm" stroke>GET</Badge> `/accounts/{id}`                         |
| [Update Account](/api-reference/beta/accounts/update-account)                             | <Badge color="orange" size="sm" stroke>PATCH</Badge> `/accounts/{id}`                     |
| [Form Company](/api-reference/beta/accounts/form-company)                                 | <Badge color="green" size="sm" stroke>POST</Badge> `/accounts/{id}/form_company`          |
| [Retrieve Account Preferences](/api-reference/beta/accounts/retrieve-account-preferences) | <Badge color="blue" size="sm" stroke>GET</Badge> `/accounts/{account_id}/preferences`     |
| [Update Account Preferences](/api-reference/beta/accounts/update-account-preferences)     | <Badge color="orange" size="sm" stroke>PATCH</Badge> `/accounts/{account_id}/preferences` |
| [List Account Reserves](/api-reference/beta/accounts/list-account-reserves)               | <Badge color="blue" size="sm" stroke>GET</Badge> `/accounts/{account_id}/reserves`        |
| [Suspend a Connected Account](/api-reference/beta/accounts/suspend-a-connected-account)   | <Badge color="green" size="sm" stroke>POST</Badge> `/accounts/{id}/suspend`               |
| [Transfer Account Ownership](/api-reference/beta/accounts/transfer-account-ownership)     | <Badge color="green" size="sm" stroke>POST</Badge> `/accounts/{id}/transfer_ownership`    |

## Attributes

<Columns cols={2}>
  <Column>
    <ResponseField name="id" type="string" required>
      Account ID, prefixed `biz_`.
    </ResponseField>

    <ResponseField name="balances" type="object[]" required>
      Account holdings, each with USD value. Empty when `total_usd` is `null`.

      <Accordion title="Properties" defaultOpen={true}>
        <ResponseField name="balance" type="string" required>
          Total amount held in native units, as a decimal string.
        </ResponseField>

        <ResponseField name="breakdown" type="object" required>
          Balance split into available, pending, and reserve amounts, as native-unit decimal strings, with the days the pending amount is expected to settle. On-chain crypto is entirely available; good\_funds and fiat cash can have pending or reserve portions.

          <Accordion title="Properties" defaultOpen={true}>
            <ResponseField name="available" type="string" required>
              Amount you can spend, send, or withdraw now, in native units, as a decimal
              string.
            </ResponseField>

            <ResponseField name="in_transit" type="string" required>
              Amount moving between the account's own destinations, such as a treasury sweep
              to its crypto wallet or a card top-up. In native units, as a decimal string.
            </ResponseField>

            <ResponseField name="pending" type="string" required>
              Amount from recent payments still settling, in native units, as a decimal
              string.
            </ResponseField>

            <ResponseField name="pending_settlements" type="object[]" required>
              When the pending amount is expected to settle, one entry per day, earliest first. Money with no scheduled settlement day, such as a transfer in flight, is left out — so these can sum to less than `pending`, never more.

              <Accordion title="Properties" defaultOpen={true}>
                <ResponseField name="amount" type="string" required>
                  Amount expected that day, in native units, as a decimal string.
                </ResponseField>

                <ResponseField name="date" type="string" required>
                  The day this money is expected to finish settling, as an ISO 8601 date.
                </ResponseField>
              </Accordion>
            </ResponseField>

            <ResponseField name="reserve" type="string" required>
              Amount held back, in native units, as a decimal string. Retrieve the account's reserves for why it is held and when it unlocks.
            </ResponseField>
          </Accordion>
        </ResponseField>

        <ResponseField name="icon_url" type="string | null" required>
          Holding icon URL.
        </ResponseField>

        <ResponseField name="name" type="string" required>
          The holding's display name
        </ResponseField>

        <ResponseField name="price_usd" type="number | null" required>
          USD price per unit, or `null` when no exchange rate is available.
        </ResponseField>

        <ResponseField name="symbol" type="string" required>
          Holding display symbol, such as `USDT`, `cbBTC`, or `EUR`.
        </ResponseField>

        <ResponseField name="value_usd" type="string | null" required>
          Holding USD value, or `null` when no exchange rate is available.
        </ResponseField>
      </Accordion>
    </ResponseField>

    <ResponseField name="banner_image_url" type="string | null" required>
      Account banner image URL.
    </ResponseField>

    <ResponseField name="business_address" type="object | null" required>
      Account business address used to calculate tax, with `line1`, `line2`, `city`,
      `state`, `postal_code`, and `country`. `null` when no address is set.
    </ResponseField>

    <ResponseField name="business_name" type="string | null" required>
      The account's legal business name used with its tax address.
    </ResponseField>

    <ResponseField name="business_type" type="string | null" required>
      High-level business category for the account. See the [business types and
      industries
      glossary](/api-reference/beta/accounts/account#business-types-and-industries-glossary)
      for valid values.
    </ResponseField>

    <ResponseField name="can_transfer_pending_balance_to_children" type="boolean" required>
      Whether pending funds may be transferred from this platform account to its
      connected accounts.
    </ResponseField>

    <ResponseField name="capabilities" type="object | null" required>
      Payment rails enabled for this account, each `active`, `inactive`, or `pending` (onboarding or review in progress). Computed only on `retrieve` and `me` for callers with `company:balance:read` scope; `null` otherwise.

      <Accordion title="Properties" defaultOpen={true}>
        <ResponseField name="accept_bank_payments" type="string" required>
          Bank payins: debits, transfers, and local bank rails

          Available options: `active`, `inactive`, `pending`
        </ResponseField>

        <ResponseField name="accept_bnpl_payments" type="string" required>
          Buy-now-pay-later payins; requires approval

          Available options: `active`, `inactive`, `pending`
        </ResponseField>

        <ResponseField name="accept_card_payments" type="string" required>
          Card payins, including Apple Pay and Google Pay

          Available options: `active`, `inactive`, `pending`
        </ResponseField>

        <ResponseField name="bank_deposit" type="string" required>
          Deposits by bank wire or ACH to the account's virtual bank account

          Available options: `active`, `inactive`, `pending`
        </ResponseField>

        <ResponseField name="card_deposit" type="string" required>
          Balance top-ups by charging a stored payment method

          Available options: `active`, `inactive`, `pending`
        </ResponseField>

        <ResponseField name="card_issuing" type="string" required>
          Issuing Whop cards; requires card application approval

          Available options: `active`, `inactive`, `pending`
        </ResponseField>

        <ResponseField name="crypto_deposit" type="string" required>
          On-chain deposits to the account's crypto wallet

          Available options: `active`, `inactive`, `pending`
        </ResponseField>

        <ResponseField name="crypto_payout" type="string" required>
          On-chain payouts to a crypto wallet

          Available options: `active`, `inactive`, `pending`
        </ResponseField>

        <ResponseField name="instant_payout" type="string" required>
          Instant payouts to an eligible payout destination

          Available options: `active`, `inactive`, `pending`
        </ResponseField>

        <ResponseField name="run_ads" type="string" required>
          Launching ad campaigns through Whop Ads. `inactive` while a requested ads services agreement is awaiting the account's signature.

          Available options: `active`, `inactive`, `pending`
        </ResponseField>

        <ResponseField name="standard_payout" type="string" required>
          Standard payouts to an external payout destination

          Available options: `active`, `inactive`, `pending`
        </ResponseField>

        <ResponseField name="transfer" type="string" required>
          Transfers to other accounts

          Available options: `active`, `inactive`, `pending`
        </ResponseField>
      </Accordion>
    </ResponseField>

    <ResponseField name="cards" type="object | null" required>
      Whop Cards application details for the account. Computed only on `retrieve` and `me` for callers with `company:balance:read` scope; `null` otherwise, or when the account has no card application.

      <Accordion title="Properties" defaultOpen={true}>
        <ResponseField name="kind" type="string | null" required>
          Whether the card application verifies a business (`business`, KYB) or a person (`individual`, consumer identity). `null` when the application is not yet linked to a verification.

          Available options: `individual`, `business`
        </ResponseField>

        <ResponseField name="status" type="string" required>
          Where the card application stands. `approved` means cards can be issued. `needs_verification` means the applicant has not completed identity verification yet; `needs_information` means they did, but the documents were rejected for a fixable reason and must be resubmitted. `pending` and `manual_review` are in flight. `denied`, `locked`, and `canceled` are terminal.

          Available options: `approved`, `pending`, `manual_review`, `denied`, `locked`, `canceled`, `needs_verification`, `needs_information`
        </ResponseField>
      </Accordion>
    </ResponseField>

    <ResponseField name="collect_vat_id" type="boolean" required>
      Whether checkout shows a VAT/tax ID field for buyers to optionally enter. Does
      not require a VAT ID to purchase.
    </ResponseField>

    <ResponseField name="company_formation" type="object" required>
      Company formation state for the account, managed through [Form Company](/api-reference/beta/accounts/form-company). A `draft` `status` until the formation checkout is paid, then filing progress with downloadable documents and signatures awaiting action. Empty when the formation state is temporarily unavailable.

      <Accordion title="Properties" defaultOpen={true}>
        <ResponseField name="documents" type="object[]">
          Formation documents available for download, such as the Articles of Organization and the EIN confirmation letter. Present once `status` leaves `draft`.

          <Accordion title="Properties" defaultOpen={true}>
            <ResponseField name="id" type="string" required>
              Document ID, prefixed `file_`.
            </ResponseField>

            <ResponseField name="name" type="string" required>
              Human-readable document name, such as `Articles of Organization`.
            </ResponseField>

            <ResponseField name="type" type="string" required>
              Document category: `articles_of_organization`, `operating_agreement`,
              `ein_letter`, `signed_ss4`, `signed_form8821`, or `mail` for postal
              correspondence received on the company's behalf.
            </ResponseField>

            <ResponseField name="url" type="string" required>
              CDN URL for downloading the document.
            </ResponseField>
          </Accordion>
        </ResponseField>

        <ResponseField name="ein_registered" type="boolean">
          Whether the company's EIN has been issued by the IRS. Present once `status`
          leaves `draft`.
        </ResponseField>

        <ResponseField name="legal_name" type="string | null">
          Registered company name including the entity ending, for example `Acme, LLC`.
          Present once `status` leaves `draft`.
        </ResponseField>

        <ResponseField name="signatures" type="object">
          IRS forms still awaiting a founder's signature, each with a hosted signing URL. Present once `status` leaves `draft`; empty when nothing needs signing.

          <Accordion title="Properties" defaultOpen={true}>
            <ResponseField name="form8821" type="object">
              Signature state for IRS Form 8821, the tax information authorization. Present only while the form still needs the founder's action.

              <Accordion title="Properties" defaultOpen={true}>
                <ResponseField name="expires_at" type="string">
                  When the signing URL expires, as an ISO 8601 timestamp. Present while `status`
                  is `pending`.
                </ResponseField>

                <ResponseField name="status" type="string" required>
                  `pending` when a signing session is ready for the founder; `unknown` when the signature state could not be determined.

                  Available options: `pending`, `unknown`
                </ResponseField>

                <ResponseField name="url" type="string">
                  Hosted signing URL where the founder completes the form. Present while `status` is `pending`.
                </ResponseField>
              </Accordion>
            </ResponseField>

            <ResponseField name="ss4" type="object">
              Signature state for IRS Form SS-4, the EIN application. Present only while the form still needs the founder's action.

              <Accordion title="Properties" defaultOpen={true}>
                <ResponseField name="expires_at" type="string">
                  When the signing URL expires, as an ISO 8601 timestamp. Present while `status`
                  is `pending`.
                </ResponseField>

                <ResponseField name="status" type="string" required>
                  `pending` when a signing session is ready for the founder; `unknown` when the signature state could not be determined.

                  Available options: `pending`, `unknown`
                </ResponseField>

                <ResponseField name="url" type="string">
                  Hosted signing URL where the founder completes the form. Present while `status` is `pending`.
                </ResponseField>
              </Accordion>
            </ResponseField>
          </Accordion>
        </ResponseField>

        <ResponseField name="state_registered" type="boolean">
          Whether the state formation filing is complete. Present once `status` leaves
          `draft`.
        </ResponseField>

        <ResponseField name="status" type="string">
          Available options: `draft`, `processing`, `filed`, `rejected`, `completed`
        </ResponseField>
      </Accordion>
    </ResponseField>

    <ResponseField name="country" type="string | null" required>
      Country where the account is located.
    </ResponseField>

    <ResponseField name="created_at" type="string" required>
      When the account was created, as an ISO 8601 timestamp.
    </ResponseField>

    <ResponseField name="description" type="string | null" required>
      Account promotional description.
    </ResponseField>

    <ResponseField name="email" type="string | null" required>
      Account owner email address.
    </ResponseField>

    <ResponseField name="eula" type="object | null" required>
      The account's end-user license agreement document, or `null` if they have not published one.

      <Accordion title="Properties" defaultOpen={true}>
        <ResponseField name="id" type="string" required>
          The file's ID, prefixed `file_`.
        </ResponseField>

        <ResponseField name="content_type" type="string | null" required>
          The file's MIME type, e.g. `application/pdf`.
        </ResponseField>

        <ResponseField name="created_at" type="string" required>
          When the file was created, as an ISO 8601 timestamp.
        </ResponseField>

        <ResponseField name="filename" type="string | null" required>
          The original filename, including its extension.
        </ResponseField>

        <ResponseField name="multipart_chunk_size" type="integer | null">
          The byte size each part (except the last) must be. Present only on create, and
          only for multipart uploads.
        </ResponseField>

        <ResponseField name="multipart_upload_id" type="string | null">
          The ID of the multipart upload, passed back to `complete`. Present only on
          create, and only for multipart uploads.
        </ResponseField>

        <ResponseField name="multipart_upload_urls" type="object[] | null">
          The presigned URL for each part. Present only on create, and only for multipart uploads.

          <Accordion title="Properties" defaultOpen={true}>
            <ResponseField name="part_number" type="integer" required>
              The 1-based index of this part within the multipart upload.
            </ResponseField>

            <ResponseField name="url" type="string" required>
              The presigned URL to PUT this part's bytes to.
            </ResponseField>
          </Accordion>
        </ResponseField>

        <ResponseField name="object" type="string" required>
          The type of this object, always `file`.
        </ResponseField>

        <ResponseField name="size" type="integer | null" required>
          The file size in bytes. `null` until the upload has finished.
        </ResponseField>

        <ResponseField name="upload_headers" type="object">
          Headers to send with the upload PUT. Present only on create.
        </ResponseField>

        <ResponseField name="upload_status" type="string" required>
          Where the file is in its upload lifecycle.

          Available options: `pending`, `processing`, `ready`, `failed`
        </ResponseField>

        <ResponseField name="upload_url" type="string | null">
          Presigned URL to PUT the file's bytes to. Present only on create, and only for
          single-part uploads.
        </ResponseField>

        <ResponseField name="url" type="string | null" required>
          A URL to download the file: a permanent CDN URL for public files, a signed
          expiring URL for private ones. `null` until the upload has finished.
        </ResponseField>

        <ResponseField name="visibility" type="string" required>
          `public` files are served via an unsigned CDN URL; `private` files via a signed, expiring URL.

          Available options: `public`, `private`
        </ResponseField>
      </Accordion>
    </ResponseField>

    <ResponseField name="home_preferences" type="string[]" required>
      Public account home page preferences.

      Available options: `hide_member_count`, `hide_members_card`
    </ResponseField>

    <ResponseField name="industry_group" type="string | null" required>
      Account industry group. See the [business types and industries
      glossary](/api-reference/beta/accounts/account#business-types-and-industries-glossary)
      for valid values.
    </ResponseField>

    <ResponseField name="industry_type" type="string | null" required>
      Specific industry vertical for the account. See the [business types and
      industries
      glossary](/api-reference/beta/accounts/account#business-types-and-industries-glossary)
      for valid values.
    </ResponseField>

    <ResponseField name="invoice_prefix" type="string | null" required>
      Prefix used for account invoices.
    </ResponseField>

    <ResponseField name="logo_url" type="string | null" required>
      Account logo image URL.
    </ResponseField>

    <ResponseField name="metadata" type="object" required>
      Arbitrary key/value metadata supplied at account creation.
    </ResponseField>

    <ResponseField name="onboarding_type" type="string | null" required>
      Type of onboarding the account has completed.

      Available options: `platform`, `seller`
    </ResponseField>

    <ResponseField name="opengraph_image_url" type="string | null" required>
      Account Open Graph image URL.
    </ResponseField>

    <ResponseField name="opengraph_image_variant" type="string | null" required>
      Account Open Graph image variant.

      Available options: `white`, `black`, `orange`
    </ResponseField>

    <ResponseField name="other_business_description" type="string | null" required>
      Business type details when business\_type is `other`.
    </ResponseField>

    <ResponseField name="other_industry_description" type="string | null" required>
      Industry details when industry\_type is `other`.
    </ResponseField>

    <ResponseField name="owner" type="object" required>
      The single user who owns the account, whose email is the `email` above. Distinct from the `owner` role on team members, which any number of them can hold.

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

    <ResponseField name="parent_account" type="object | null" required>
      Parent account for connected accounts, or `null` for standalone accounts.

      <Accordion title="Properties" defaultOpen={true}>
        <ResponseField name="id" type="string" required>
          Account ID, prefixed `biz_`.
        </ResponseField>

        <ResponseField name="logo_url" type="string | null" required>
          Account logo image URL.
        </ResponseField>

        <ResponseField name="route" type="string" required>
          Account public route identifier.
        </ResponseField>

        <ResponseField name="title" type="string" required>
          Account display name.
        </ResponseField>
      </Accordion>
    </ResponseField>

    <ResponseField name="payment_controls" type="object | null" required>
      Payment health controls currently applied to the account. Computed only on `retrieve` and `me` for callers with `company:balance:read` scope; `null` otherwise.

      <Accordion title="Properties" defaultOpen={true}>
        <ResponseField name="dispute_alert_auto_refund" type="object" required>
          Automatic refund settings for pre-chargeback dispute alerts.

          <Accordion title="Properties" defaultOpen={true}>
            <ResponseField name="locked" type="boolean" required>
              Whether the account owner is prevented from changing this threshold.
            </ResponseField>

            <ResponseField name="threshold_usd" type="number | null" required>
              Maximum dispute alert amount automatically refunded in USD. `null` when automatic refunds are disabled.
            </ResponseField>
          </Accordion>
        </ResponseField>

        <ResponseField name="dispute_alert_fee_usd" type="number | null" required>
          Fee charged for each dispute alert in USD. `null` when unavailable.
        </ResponseField>

        <ResponseField name="enforce_3ds" type="boolean" required>
          Whether 3-D Secure is forced on every card payment at checkout. The account
          cannot bypass it while set.
        </ResponseField>

        <ResponseField name="financing_disabled" type="boolean" required>
          Whether payment health controls explicitly disable financing. This is
          independent of financing approval in `capabilities.accept_bnpl_payments`.
        </ResponseField>

        <ResponseField name="high_risk_processing_fee_percentage" type="number" required>
          Additional processing fee percentage for high-risk processing.
        </ResponseField>

        <ResponseField name="pending_auto_topup_fee_percentage" type="number" required>
          Percentage fee charged when pending, not-yet-settled balance is advanced to
          fund the account's cards balance, where `2` means 2%. `0` when the account is
          exempt.
        </ResponseField>

        <ResponseField name="pending_balance_delay_days" type="integer" required>
          Additional days payments remain pending before becoming available.
        </ResponseField>

        <ResponseField name="reserve" type="object" required>
          Reserve currently applied to incoming payment volume.

          <Accordion title="Properties" defaultOpen={true}>
            <ResponseField name="hold_period_days" type="integer" required>
              Number of days reserved funds are held before release.
            </ResponseField>

            <ResponseField name="percentage" type="number | null" required>
              Percentage of incoming payment volume held in reserve. `null` when no reserve is applied.
            </ResponseField>
          </Accordion>
        </ResponseField>

        <ResponseField name="resolution_center_auto_refund" type="object" required>
          Automatic refund settings for resolution center cases.

          <Accordion title="Properties" defaultOpen={true}>
            <ResponseField name="card_threshold_usd" type="number | null" required>
              Maximum card-funded resolution center case amount automatically refunded in
              USD. `null` when automatic refunds are disabled for cards.
            </ResponseField>

            <ResponseField name="financing_threshold_usd" type="number | null" required>
              Maximum financing-funded resolution center case amount automatically refunded
              in USD. `null` when automatic refunds are disabled for financing.
            </ResponseField>

            <ResponseField name="locked" type="boolean" required>
              Whether the account owner is prevented from changing these thresholds.
            </ResponseField>

            <ResponseField name="paypal_threshold_usd" type="number | null" required>
              Maximum PayPal-funded resolution center case amount automatically refunded in USD. `null` when automatic refunds are disabled for PayPal.
            </ResponseField>
          </Accordion>
        </ResponseField>

        <ResponseField name="restricted_payment_methods" type="string[]" required>
          Card brands blocked at checkout for this account. Empty when none are blocked. The account cannot re-enable them itself.

          Available options: `card_visa`, `card_mastercard`, `card_american_express`, `card_discover_global_network`
        </ResponseField>

        <ResponseField name="undated_pending_reason" type="string | null" required>
          Why pending funds without a settlement date aren't moving yet, when it's something the merchant can act on. `null` when there's no reason to show (still clearing, or the account is held for a reason that isn't merchant-actionable).

          Available options: `kyc_incomplete`, `pending_information_request`
        </ResponseField>

        <ResponseField name="withdrawal_schedule" type="object" required>
          How the account's balance automatically withdraws.

          <Accordion title="Properties" defaultOpen={true}>
            <ResponseField name="day" type="integer | null" required>
              Day the automatic withdrawal runs on: 0-6 (Sunday-Saturday) for `weekly`, 1-31
              for `monthly`. `null` for `manual` and `daily`.
            </ResponseField>

            <ResponseField name="frequency" type="string" required>
              How often the account's balance automatically withdraws.

              Available options: `manual`, `daily`, `weekly`, `monthly`
            </ResponseField>

            <ResponseField name="next_payout_date" type="string | null" required>
              Next date the automatic withdrawal is scheduled to run, as an ISO 8601 date. `null` for `manual` and `daily`, where no single next date applies.
            </ResponseField>
          </Accordion>
        </ResponseField>
      </Accordion>
    </ResponseField>

    <ResponseField name="privacy_policy" type="object | null" required>
      The account's privacy policy document, or `null` if they have not published one.

      <Accordion title="Properties" defaultOpen={true}>
        <ResponseField name="id" type="string" required>
          The file's ID, prefixed `file_`.
        </ResponseField>

        <ResponseField name="content_type" type="string | null" required>
          The file's MIME type, e.g. `application/pdf`.
        </ResponseField>

        <ResponseField name="created_at" type="string" required>
          When the file was created, as an ISO 8601 timestamp.
        </ResponseField>

        <ResponseField name="filename" type="string | null" required>
          The original filename, including its extension.
        </ResponseField>

        <ResponseField name="multipart_chunk_size" type="integer | null">
          The byte size each part (except the last) must be. Present only on create, and
          only for multipart uploads.
        </ResponseField>

        <ResponseField name="multipart_upload_id" type="string | null">
          The ID of the multipart upload, passed back to `complete`. Present only on
          create, and only for multipart uploads.
        </ResponseField>

        <ResponseField name="multipart_upload_urls" type="object[] | null">
          The presigned URL for each part. Present only on create, and only for multipart uploads.

          <Accordion title="Properties" defaultOpen={true}>
            <ResponseField name="part_number" type="integer" required>
              The 1-based index of this part within the multipart upload.
            </ResponseField>

            <ResponseField name="url" type="string" required>
              The presigned URL to PUT this part's bytes to.
            </ResponseField>
          </Accordion>
        </ResponseField>

        <ResponseField name="object" type="string" required>
          The type of this object, always `file`.
        </ResponseField>

        <ResponseField name="size" type="integer | null" required>
          The file size in bytes. `null` until the upload has finished.
        </ResponseField>

        <ResponseField name="upload_headers" type="object">
          Headers to send with the upload PUT. Present only on create.
        </ResponseField>

        <ResponseField name="upload_status" type="string" required>
          Where the file is in its upload lifecycle.

          Available options: `pending`, `processing`, `ready`, `failed`
        </ResponseField>

        <ResponseField name="upload_url" type="string | null">
          Presigned URL to PUT the file's bytes to. Present only on create, and only for
          single-part uploads.
        </ResponseField>

        <ResponseField name="url" type="string | null" required>
          A URL to download the file: a permanent CDN URL for public files, a signed
          expiring URL for private ones. `null` until the upload has finished.
        </ResponseField>

        <ResponseField name="visibility" type="string" required>
          `public` files are served via an unsigned CDN URL; `private` files via a signed, expiring URL.

          Available options: `public`, `private`
        </ResponseField>
      </Accordion>
    </ResponseField>

    <ResponseField name="product_tax_code" type="object | null" required>
      Tax classification code applied by default to the account's products, with
      `id`, `name`, and `product_type`. `null` when no default is set.
    </ResponseField>

    <ResponseField name="recommended_actions" type="object[] | null" required>
      DEPRECATED: Use the `GET /recommended_actions?account_id=\{account_id}` endpoint instead.

      <Accordion title="Properties" defaultOpen={true}>
        <ResponseField name="action" type="string" required>
          The recommendation; new values may be added, so handle unknown actions gracefully

          Available options: `theme_business`, `create_product`, `create_plan`, `verify_identity`, `connect_affiliate_program`, `create_promotion`, `migrate_from_stripe`, `accept_first_payment`, `launch_first_ad`, `launch_draft_campaign`, `increase_ad_budget`, `refresh_ad_creatives`, `fix_ad_billing`, `exclude_customers_from_ads`, `retarget_abandoned_checkouts`, `fix_funnel_dropoff`, `invite_team_member`, `enable_tax_collection`, `create_card`, `apply_for_financing`
        </ResponseField>

        <ResponseField name="blocked_capabilities" type="string[]" required>
          Capabilities this would unlock, or empty
        </ResponseField>

        <ResponseField name="cta" type="string" required>
          The URL the call-to-action links to
        </ResponseField>

        <ResponseField name="cta_label" type="string" required>
          Button label
        </ResponseField>

        <ResponseField name="description" type="string" required>
          Supporting copy, or empty
        </ResponseField>

        <ResponseField name="icon_url" type="string | null" required>
          Illustration icon URL, or `null`
        </ResponseField>

        <ResponseField name="impact_score" type="integer | null" required>
          Estimated impact from 0-100, or `null` when not ranked
        </ResponseField>

        <ResponseField name="reasoning" type="string | null" required>
          Why this action was recommended, or `null`
        </ResponseField>

        <ResponseField name="status" type="string" required>
          Always optional — never blocking

          Available options: `optional`
        </ResponseField>

        <ResponseField name="title" type="string" required>
          Headline for the recommendation
        </ResponseField>
      </Accordion>
    </ResponseField>

    <ResponseField name="require_2fa" type="boolean" required>
      Whether authorized users must enable two-factor authentication.
    </ResponseField>

    <ResponseField name="required_actions" type="object[] | null" required>
      Actions the account owner must take to unblock capabilities like payouts and card spend, ordered by display priority. Computed only on `retrieve` and `me` for callers with `company:balance:read` scope; `null` otherwise.

      <Accordion title="Properties" defaultOpen={true}>
        <ResponseField name="action" type="string" required>
          What the holder must do; new values may be added, so handle unknown actions gracefully

          Available options: `deposit_funds`, `submit_information_request`, `update_automatic_withdrawal_method`, `reauthorize_payout_methods`, `update_payout_profile`, `card_usage_review`, `verify_identity`, `sign_formation_documents`, `connect_fulfillment_tracker`, `setup_apple_pay_domains`, `configure_tax_remitter`, `add_vat_registration`
        </ResponseField>

        <ResponseField name="blocked_capabilities" type="string[]" required>
          Capabilities gated until this is resolved
        </ResponseField>

        <ResponseField name="cta" type="string | null" required>
          The URL the call-to-action links to, or null when there is no button
        </ResponseField>

        <ResponseField name="cta_label" type="string" required>
          Button label, or empty when there is no button
        </ResponseField>

        <ResponseField name="description" type="string" required>
          Supporting copy, or empty
        </ResponseField>

        <ResponseField name="icon_url" type="string | null" required>
          The URL of the action's illustration icon, or null if it has none
        </ResponseField>

        <ResponseField name="status" type="string" required>
          required (act now) or pending (under review)

          Available options: `required`, `pending`
        </ResponseField>

        <ResponseField name="title" type="string" required>
          Headline for the action
        </ResponseField>
      </Accordion>
    </ResponseField>

    <ResponseField name="return_policy" type="object | null" required>
      The account's return policy document, or `null` if they have not published one.

      <Accordion title="Properties" defaultOpen={true}>
        <ResponseField name="id" type="string" required>
          The file's ID, prefixed `file_`.
        </ResponseField>

        <ResponseField name="content_type" type="string | null" required>
          The file's MIME type, e.g. `application/pdf`.
        </ResponseField>

        <ResponseField name="created_at" type="string" required>
          When the file was created, as an ISO 8601 timestamp.
        </ResponseField>

        <ResponseField name="filename" type="string | null" required>
          The original filename, including its extension.
        </ResponseField>

        <ResponseField name="multipart_chunk_size" type="integer | null">
          The byte size each part (except the last) must be. Present only on create, and
          only for multipart uploads.
        </ResponseField>

        <ResponseField name="multipart_upload_id" type="string | null">
          The ID of the multipart upload, passed back to `complete`. Present only on
          create, and only for multipart uploads.
        </ResponseField>

        <ResponseField name="multipart_upload_urls" type="object[] | null">
          The presigned URL for each part. Present only on create, and only for multipart uploads.

          <Accordion title="Properties" defaultOpen={true}>
            <ResponseField name="part_number" type="integer" required>
              The 1-based index of this part within the multipart upload.
            </ResponseField>

            <ResponseField name="url" type="string" required>
              The presigned URL to PUT this part's bytes to.
            </ResponseField>
          </Accordion>
        </ResponseField>

        <ResponseField name="object" type="string" required>
          The type of this object, always `file`.
        </ResponseField>

        <ResponseField name="size" type="integer | null" required>
          The file size in bytes. `null` until the upload has finished.
        </ResponseField>

        <ResponseField name="upload_headers" type="object">
          Headers to send with the upload PUT. Present only on create.
        </ResponseField>

        <ResponseField name="upload_status" type="string" required>
          Where the file is in its upload lifecycle.

          Available options: `pending`, `processing`, `ready`, `failed`
        </ResponseField>

        <ResponseField name="upload_url" type="string | null">
          Presigned URL to PUT the file's bytes to. Present only on create, and only for
          single-part uploads.
        </ResponseField>

        <ResponseField name="url" type="string | null" required>
          A URL to download the file: a permanent CDN URL for public files, a signed
          expiring URL for private ones. `null` until the upload has finished.
        </ResponseField>

        <ResponseField name="visibility" type="string" required>
          `public` files are served via an unsigned CDN URL; `private` files via a signed, expiring URL.

          Available options: `public`, `private`
        </ResponseField>
      </Accordion>
    </ResponseField>

    <ResponseField name="route" type="string" required>
      Account public route identifier.
    </ResponseField>

    <ResponseField name="send_customer_emails" type="boolean" required>
      Whether Whop sends transactional emails to customers on behalf of this
      account.
    </ResponseField>

    <ResponseField name="show_joined_whops" type="boolean" required>
      Whether the account appears in joined whops on other accounts.
    </ResponseField>

    <ResponseField name="show_reviews_dtc" type="boolean" required>
      Whether reviews are displayed on direct-to-consumer product pages.
    </ResponseField>

    <ResponseField name="show_user_directory" type="boolean" required>
      Whether the account shows users in the user directory.
    </ResponseField>

    <ResponseField name="social_links" type="object[]" required>
      Account social links.

      <Accordion title="Properties" defaultOpen={true}>
        <ResponseField name="id" type="string" required>
          The ID of the social link
        </ResponseField>

        <ResponseField name="title" type="string | null" required>
          The optional display title for the social link
        </ResponseField>

        <ResponseField name="url" type="string" required>
          The social link URL
        </ResponseField>

        <ResponseField name="website" type="string" required>
          The social platform for this link

          Available options: `x`, `instagram`, `facebook`, `tiktok`, `youtube`, `linkedin`, `twitch`, `website`, `custom`
        </ResponseField>
      </Accordion>
    </ResponseField>

    <ResponseField name="stablecoin_rails" type="boolean" required>
      Whether the account settles on stablecoin rails — its balance is held on-chain
      as USDT and paid out over crypto, rather than as fiat cash.
    </ResponseField>

    <ResponseField name="status" type="string | null" required>
      Whether the account can operate on Whop: `active` or `suspended`. Computed on
      `list`, `retrieve`, `me`, and `suspend`; `null` otherwise.
    </ResponseField>

    <ResponseField name="status_reason" type="string | null" required>
      Why the account was suspended, in language safe to show the account owner.
      Computed on `retrieve`, `me`, and `suspend`; `null` otherwise, when `status`
      is not `suspended`, and when the suspension was recorded without a reason.
    </ResponseField>

    <ResponseField name="store_page_config" type="object" required>
      Account store page display configuration.

      <Accordion title="Properties" defaultOpen={true}>
        <ResponseField name="accent_color" type="string | null" required>
          Accent color used on the account store page.

          Available options: `ruby`, `tomato`, `red`, `crimson`, `pink`, `plum`, `purple`, `violet`, `iris`, `cyan`, `teal`, `jade`, `green`, `grass`, `brown`, `blue`, `orange`, `indigo`, `sky`, `mint`, `yellow`, `amber`, `lime`, `lemon`, `magenta`, `gold`, `bronze`, `gray`
        </ResponseField>

        <ResponseField name="layout" type="string | null" required>
          Layout used on the account store page.

          Available options: `featured`, `compact`
        </ResponseField>

        <ResponseField name="profile_variant" type="string | null" required>
          Profile presentation used on the account store page.

          Available options: `personal`, `business`
        </ResponseField>

        <ResponseField name="whop_affiliate_link" type="boolean" required>
          Whether the account store page shows a Whop affiliate link.
        </ResponseField>
      </Accordion>
    </ResponseField>

    <ResponseField name="target_audience" type="string | null" required>
      Target audience for this account.
    </ResponseField>

    <ResponseField name="tax_collection_enabled_states" type="string[]" required>
      US state codes (of the 50 states plus `DC`) where the account collects tax:
      the full set when Whop remits (`tax_remitted_by` `whop`), the configured
      subset when the account self-remits (`self`), and empty when neither. On
      update, send the complete list to replace it (only allowed when `self`).
    </ResponseField>

    <ResponseField name="tax_identifiers" type="object[]" required>
      Account tax/VAT registrations. Empty when none are set.

      <Accordion title="Properties" defaultOpen={true}>
        <ResponseField name="id" type="string" required>
          Tax identifier ID.
        </ResponseField>

        <ResponseField name="tax_id_type" type="string" required>
          Tax ID type.

          Available options: `ad_nrt`, `ao_tin`, `ar_cuit`, `al_tin`, `am_tin`, `aw_tin`, `au_abn`, `au_arn`, `eu_vat`, `az_tin`, `bs_tin`, `bh_vat`, `bd_bin`, `bb_tin`, `by_tin`, `bj_ifu`, `bo_tin`, `ba_tin`, `br_cnpj`, `br_cpf`, `bg_uic`, `bf_ifu`, `kh_tin`, `cm_niu`, `ca_bn`, `ca_gst_hst`, `ca_pst_bc`, `ca_pst_mb`, `ca_pst_sk`, `ca_qst`, `cv_nif`, `cl_tin`, `cn_tin`, `co_nit`, `cd_nif`, `cr_tin`, `hr_oib`, `do_rcn`, `ec_ruc`, `eg_tin`, `sv_nit`, `et_tin`, `eu_oss_vat`, `ge_vat`, `gh_tin`, `de_stn`, `gb_vat`, `gn_nif`, `hk_br`, `hu_tin`, `is_vat`, `in_gst`, `id_npwp`, `il_vat`, `jp_cn`, `jp_rn`, `jp_trn`, `kz_bin`, `ke_pin`, `kg_tin`, `la_tin`, `li_uid`, `li_vat`, `my_frp`, `my_itn`, `my_sst`, `mr_nif`, `mx_rfc`, `md_vat`, `me_pib`, `ma_vat`, `np_pan`, `nz_gst`, `ng_tin`, `mk_vat`, `no_vat`, `no_voec`, `om_vat`, `pe_ruc`, `ph_tin`, `pl_nip`, `ro_tin`, `ru_inn`, `ru_kpp`, `sa_vat`, `sn_ninea`, `rs_pib`, `sg_gst`, `sg_uen`, `si_tin`, `za_vat`, `kr_brn`, `es_cif`, `ch_uid`, `ch_vat`, `tw_vat`, `tj_tin`, `tz_vat`, `th_vat`, `tr_tin`, `ug_tin`, `ua_vat`, `ae_trn`, `us_ein`, `uy_ruc`, `uz_tin`, `uz_vat`, `ve_rif`, `vn_tin`, `zm_tin`, `zw_tin`, `sr_fin`, `xi_vat`
        </ResponseField>

        <ResponseField name="tax_id_value" type="string" required>
          Tax ID value.
        </ResponseField>
      </Accordion>
    </ResponseField>

    <ResponseField name="tax_remitted_by" type="string | null" required>
      Who calculates and remits tax for the account: `whop` (Whop calculates and remits), `self` (Whop calculates; the account collects and remits), or `none` (neither; the account is responsible). `null` until the account enrolls in the Whop tax service.

      Available options: `whop`, `self`, `none`
    </ResponseField>

    <ResponseField name="tax_type" type="string | null" required>
      How tax is applied to the account's prices: `inclusive` (tax included in the listed price) or `exclusive` (tax added on top). Defaults to `exclusive` when unset; `null` only when the account has no payment connection.

      Available options: `inclusive`, `exclusive`
    </ResponseField>

    <ResponseField name="terms_of_service" type="object | null" required>
      The account's terms of service document, or `null` if they have not published one.

      <Accordion title="Properties" defaultOpen={true}>
        <ResponseField name="id" type="string" required>
          The file's ID, prefixed `file_`.
        </ResponseField>

        <ResponseField name="content_type" type="string | null" required>
          The file's MIME type, e.g. `application/pdf`.
        </ResponseField>

        <ResponseField name="created_at" type="string" required>
          When the file was created, as an ISO 8601 timestamp.
        </ResponseField>

        <ResponseField name="filename" type="string | null" required>
          The original filename, including its extension.
        </ResponseField>

        <ResponseField name="multipart_chunk_size" type="integer | null">
          The byte size each part (except the last) must be. Present only on create, and
          only for multipart uploads.
        </ResponseField>

        <ResponseField name="multipart_upload_id" type="string | null">
          The ID of the multipart upload, passed back to `complete`. Present only on
          create, and only for multipart uploads.
        </ResponseField>

        <ResponseField name="multipart_upload_urls" type="object[] | null">
          The presigned URL for each part. Present only on create, and only for multipart uploads.

          <Accordion title="Properties" defaultOpen={true}>
            <ResponseField name="part_number" type="integer" required>
              The 1-based index of this part within the multipart upload.
            </ResponseField>

            <ResponseField name="url" type="string" required>
              The presigned URL to PUT this part's bytes to.
            </ResponseField>
          </Accordion>
        </ResponseField>

        <ResponseField name="object" type="string" required>
          The type of this object, always `file`.
        </ResponseField>

        <ResponseField name="size" type="integer | null" required>
          The file size in bytes. `null` until the upload has finished.
        </ResponseField>

        <ResponseField name="upload_headers" type="object">
          Headers to send with the upload PUT. Present only on create.
        </ResponseField>

        <ResponseField name="upload_status" type="string" required>
          Where the file is in its upload lifecycle.

          Available options: `pending`, `processing`, `ready`, `failed`
        </ResponseField>

        <ResponseField name="upload_url" type="string | null">
          Presigned URL to PUT the file's bytes to. Present only on create, and only for
          single-part uploads.
        </ResponseField>

        <ResponseField name="url" type="string | null" required>
          A URL to download the file: a permanent CDN URL for public files, a signed
          expiring URL for private ones. `null` until the upload has finished.
        </ResponseField>

        <ResponseField name="visibility" type="string" required>
          `public` files are served via an unsigned CDN URL; `private` files via a signed, expiring URL.

          Available options: `public`, `private`
        </ResponseField>
      </Accordion>
    </ResponseField>

    <ResponseField name="three_ds_level" type="string | null" required>
      Account-level 3D Secure behavior. `mandate_challenge` requires cardholder verification on supported card payments; `null` uses the standard checkout flow.

      Available options: `mandate_challenge`
    </ResponseField>

    <ResponseField name="title" type="string" required>
      Account display name.
    </ResponseField>

    <ResponseField name="total_earned_usd" type="number | null" required>
      Account lifetime sales, normalized to USD. Computed only on `retrieve` and
      `me` for callers with `stats:read` scope; `null` otherwise.
    </ResponseField>

    <ResponseField name="total_usd" type="string | null" required>
      Total USD value across balances with known exchange rates. Computed only on
      single-account reads (`retrieve` and `me`); `null` on list responses, writes,
      missing balance-read permission, or unavailable balance source.
    </ResponseField>

    <ResponseField name="use_logo_as_opengraph_image_fallback" type="boolean" required>
      Whether the account uses its logo as the fallback Open Graph image.
    </ResponseField>

    <ResponseField name="verification" type="object" required>
      Account identity verification status for the `individual` (KYC) and `business`
      (KYB) profiles. Each is `null` until created, otherwise a `status` of
      `not_started`, `pending`, `manual_review`, `approved`, or `rejected`.
    </ResponseField>

    <ResponseField name="volume_usd" type="number | null" required>
      Lifetime volume through the account — sales plus transfers received —
      normalized to USD. Computed only on `list` for callers with `stats:read` on
      the account; `null` otherwise.
    </ResponseField>

    <ResponseField name="wallet" type="object | null" required>
      Account primary crypto wallet, or `null` if none has been provisioned.

      <Accordion title="Properties" defaultOpen={true}>
        <ResponseField name="id" type="string" required>
          Wallet ID, prefixed `wallet_`.
        </ResponseField>

        <ResponseField name="address" type="string" required>
          The on-chain address of the wallet
        </ResponseField>

        <ResponseField name="network" type="string" required>
          The blockchain network the wallet lives on

          Available options: `solana`, `ethereum`, `bitcoin`
        </ResponseField>
      </Accordion>
    </ResponseField>
  </Column>

  <Column>
    <div className="api-resource-sticky-example">
      ```json Account theme={null}
      {
      	"id": "biz_xxxxxxxxxxxxx",
      	"balances": [
      		{
      			"balance": "1250.5",
      			"breakdown": {
      				"available": "1200.50",
      				"in_transit": "0",
      				"pending": "50.00",
      				"reserve": "0",
      				"pending_settlements": [
      					{
      						"amount": "50.00",
      						"date": "2026-08-01"
      					}
      				]
      			},
      			"icon_url": "https://cdn.whop.com/tokens/usdt.png",
      			"name": "Tether USD",
      			"price_usd": 1,
      			"symbol": "USDT",
      			"value_usd": "1250.50"
      		},
      		{
      			"balance": "45.00",
      			"breakdown": {
      				"available": "40.00",
      				"in_transit": "0",
      				"pending": "5.00",
      				"reserve": "0",
      				"pending_settlements": [
      					{
      						"amount": "3.00",
      						"date": "2026-08-01"
      					},
      					{
      						"amount": "2.00",
      						"date": "2026-08-04"
      					}
      				]
      			},
      			"icon_url": null,
      			"name": "EUR",
      			"price_usd": 1.11,
      			"symbol": "EUR",
      			"value_usd": "50.00"
      		}
      	],
      	"banner_image_url": "https://cdn.whop.com/banner.png",
      	"business_type": "physical_products",
      	"can_transfer_pending_balance_to_children": false,
      	"country": "US",
      	"created_at": "2026-06-01T12:00:00Z",
      	"description": "Petal Post delivers fresh bouquets.",
      	"email": "hello@petalpost.example",
      	"eula": null,
      	"home_preferences": ["hide_member_count"],
      	"industry_group": "retail",
      	"industry_type": "flower_delivery_gig",
      	"invoice_prefix": "PETAL",
      	"company_formation": {
      		"status": "filed",
      		"legal_name": "Petal Post, LLC",
      		"state_registered": true,
      		"ein_registered": false,
      		"signatures": {
      			"ss4": {
      				"status": "pending",
      				"url": "https://esign.doola.com/session/sess_1a2b3c",
      				"expires_at": "2026-08-01T00:00:00Z"
      			}
      		},
      		"documents": [
      			{
      				"id": "file_petalpost123",
      				"name": "Articles of Organization",
      				"type": "articles_of_organization",
      				"url": "https://img.whop.com/documents/articles-of-organization.pdf"
      			}
      		]
      	},
      	"logo_url": "https://cdn.whop.com/logo.png",
      	"metadata": {
      		"external_merchant_id": "merchant_123"
      	},
      	"onboarding_type": "seller",
      	"opengraph_image_url": "https://cdn.whop.com/og.png",
      	"opengraph_image_variant": "black",
      	"other_business_description": "Local flower delivery",
      	"other_industry_description": "Same-day floral gifts",
      	"owner": {
      		"id": "user_xxxxxxxxxxxx",
      		"username": "petalpost",
      		"name": "Ada Flores",
      		"profile_picture": {
      			"url": "https://cdn.whop.com/user.png"
      		}
      	},
      	"parent_account": {
      		"id": "biz_platform123",
      		"title": "Petal Platform",
      		"route": "petal-platform",
      		"logo_url": "https://cdn.whop.com/petal-platform-logo.png"
      	},
      	"privacy_policy": null,
      	"require_2fa": true,
      	"collect_vat_id": false,
      	"capabilities": {
      		"accept_card_payments": "active",
      		"accept_bank_payments": "active",
      		"accept_bnpl_payments": "inactive",
      		"standard_payout": "inactive",
      		"instant_payout": "inactive",
      		"crypto_payout": "inactive",
      		"transfer": "inactive",
      		"bank_deposit": "active",
      		"crypto_deposit": "active",
      		"card_deposit": "active",
      		"card_issuing": "inactive",
      		"run_ads": "active"
      	},
      	"payment_controls": {
      		"dispute_alert_auto_refund": {
      			"threshold_usd": 250,
      			"locked": false
      		},
      		"resolution_center_auto_refund": {
      			"card_threshold_usd": null,
      			"financing_threshold_usd": null,
      			"paypal_threshold_usd": null,
      			"locked": false
      		},
      		"dispute_alert_fee_usd": 29,
      		"high_risk_processing_fee_percentage": 0,
      		"pending_auto_topup_fee_percentage": 2,
      		"pending_balance_delay_days": 0,
      		"financing_disabled": false,
      		"enforce_3ds": false,
      		"restricted_payment_methods": [],
      		"reserve": {
      			"percentage": null,
      			"hold_period_days": 90
      		},
      		"withdrawal_schedule": {
      			"frequency": "monthly",
      			"day": 1,
      			"next_payout_date": "2026-09-01"
      		},
      		"undated_pending_reason": null
      	},
      	"cards": {
      		"kind": "business",
      		"status": "approved"
      	},
      	"required_actions": [
      		{
      			"action": "verify_identity",
      			"status": "required",
      			"title": "Identity verification required",
      			"description": "Complete verification before your total earnings exceed $5k to continue accepting payments.",
      			"cta_label": "Verify now",
      			"cta": "https://whop.com/dashboard/biz_xxxxxxxxxxxxx/balance/?verify=true",
      			"icon_url": null,
      			"blocked_capabilities": [
      				"accept_card_payments",
      				"accept_bank_payments",
      				"standard_payout",
      				"instant_payout",
      				"crypto_payout",
      				"transfer",
      				"bank_deposit",
      				"card_issuing"
      			]
      		}
      	],
      	"recommended_actions": [
      		{
      			"action": "migrate_from_stripe",
      			"status": "optional",
      			"title": "Migrate your business from Stripe.",
      			"description": "",
      			"cta_label": "Get started",
      			"cta": "https://whop.com/dashboard/biz_xxxxxxxxxxxxx/settings/stripe-migrations/",
      			"icon_url": "https://whop.com/illustrations/orange/crane.svg",
      			"blocked_capabilities": [],
      			"reasoning": null,
      			"impact_score": null
      		},
      		{
      			"action": "accept_first_payment",
      			"status": "optional",
      			"title": "Accept your first payment.",
      			"description": "",
      			"cta_label": "Create payment link",
      			"cta": "https://whop.com/dashboard/biz_xxxxxxxxxxxxx/links/checkout/create/",
      			"icon_url": "https://whop.com/illustrations/orange/card.svg",
      			"blocked_capabilities": [],
      			"reasoning": null,
      			"impact_score": null
      		},
      		{
      			"action": "apply_for_financing",
      			"status": "optional",
      			"title": "Offer financing at checkout",
      			"description": "Let customers pay over time with buy now, pay later.",
      			"cta_label": "Apply",
      			"cta": "https://whop.com/dashboard/biz_xxxxxxxxxxxxx/settings/payments/",
      			"icon_url": "https://whop.com/illustrations/orange/piggy-bank.svg",
      			"blocked_capabilities": ["accept_bnpl_payments"],
      			"reasoning": null,
      			"impact_score": null
      		}
      	],
      	"return_policy": null,
      	"route": "petal-post",
      	"send_customer_emails": true,
      	"show_joined_whops": false,
      	"show_reviews_dtc": true,
      	"show_user_directory": false,
      	"stablecoin_rails": false,
      	"status": "active",
      	"status_reason": null,
      	"terms_of_service": null,
      	"three_ds_level": "mandate_challenge",
      	"total_earned_usd": 3450,
      	"volume_usd": 4250.5,
      	"social_links": [
      		{
      			"id": "social_petalpost123",
      			"title": "Petal Post",
      			"url": "https://petalpost.example",
      			"website": "website"
      		}
      	],
      	"store_page_config": {
      		"accent_color": "red",
      		"layout": "compact",
      		"profile_variant": "business",
      		"whop_affiliate_link": true
      	},
      	"target_audience": "Customers sending flowers locally",
      	"tax_collection_enabled_states": ["CA", "NY"],
      	"tax_remitted_by": "whop",
      	"tax_type": "exclusive",
      	"product_tax_code": {
      		"id": "ptc_CzLNn2Z058xEC1",
      		"name": "Digital Group Chat",
      		"product_type": "digital"
      	},
      	"business_address": {
      		"line1": "123 Garden Way",
      		"line2": null,
      		"city": "Austin",
      		"state": "TX",
      		"postal_code": "78701",
      		"country": "US"
      	},
      	"business_name": "Petal Post, LLC",
      	"tax_identifiers": [
      		{
      			"id": "taxid_petalpost123",
      			"tax_id_type": "us_ein",
      			"tax_id_value": "12-3456789"
      		}
      	],
      	"title": "Petal Post",
      	"total_usd": "1300.50",
      	"use_logo_as_opengraph_image_fallback": true,
      	"verification": {
      		"business": null,
      		"individual": {
      			"status": "approved"
      		}
      	},
      	"wallet": {
      		"id": "wallet_petalpost123",
      		"address": "So11111111111111111111111111111111111111112",
      		"network": "solana"
      	}
      }
      ```
    </div>
  </Column>
</Columns>

## Business types and industries

Three fields classify accounts: `business_type`, `industry_group`, and `industry_type`. The full taxonomy lives on its own page:

<Card title="Business types and industries" icon="tags" horizontal arrow href="/api-reference/beta/accounts/business-types" />
