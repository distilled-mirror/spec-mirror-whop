> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Affiliates

> Create affiliate records, assign per-plan or rev-share commission overrides, and track referral earnings.

Affiliates earn commissions for referred sales. The API flow has two parts: create an affiliate record, then add overrides that define the commission.

## How attribution works

1. You create an affiliate record for a user, then add overrides. Each override returns `product_direct_link` and `checkout_direct_link` fields. These are referral URLs the affiliate shares (they include `?a=<username>`).
2. A buyer uses the link. Whop stores the affiliate cookie (30-day attribution window by default).
3. Buyer checks out within the window. The matching override calculates the commission: `standard` (per-plan, percentage or flat fee) or `rev_share` (percentage of revenue, product-specific or account-wide).
4. Whop attributes the commission to the affiliate and reflects it in the override's `total_referral_earnings_usd` field.

<Note>
  **Refunds reverse commissions.** If a buyer refunds within the window, the affiliate's earning on that sale is clawed back automatically.
</Note>

<Note>
  **Affiliates need a Whop account to receive payouts.** Earnings accrue on the record. Payouts go to their account balance via [transfers](/developer/platforms/collect-payments-for-connected-accounts#transfers). If they don't have an account yet, onboard them with [connected account enrollment](/developer/platforms/enroll-connected-accounts).
</Note>

## Create an affiliate

`user_identifier` resolves flexibly. Pass a username, email, user ID, or Discord ID. If an affiliate record already exists for the account and user pair, Whop returns the existing record (idempotent).

<CodeGroup>
  ```typescript TypeScript theme={null}
  import { WhopClient } from "@whop/sdk";

  const client = new WhopClient({ token: process.env.WHOP_API_KEY });

  const affiliate = await client.affiliates.create({
    company_id: "biz_xxxxxxxxxxxxx",
    user_identifier: "johndoe", // username, email, usr_xxx, or Discord ID
  });

  console.log(`Affiliate ${affiliate.id} created`);
  ```

  ```python Python theme={null}
  import os
  from whop_sdk import Whop

  client = Whop(token=os.environ["WHOP_API_KEY"])

  affiliate = client.affiliates.create(
      company_id="biz_xxxxxxxxxxxxx",
      user_identifier="johndoe",
  )

  print(f"Affiliate {affiliate.id} created")
  ```

  ```rust Rust theme={null}
  use whop_sdk::prelude::*;

  let config = ClientConfig {
      token: Some(std::env::var("WHOP_API_KEY").unwrap()),
      ..Default::default()
  };
  let client = Whop::new(config).expect("Failed to build client");

  let affiliate = client
      .affiliates
      .create(
          &CreateAffiliatesRequest {
              company_id: "biz_xxxxxxxxxxxxx".to_string(),
              user_identifier: "johndoe".to_string(), // username, email, usr_xxx, or Discord ID
          },
          None,
      )
      .await?;

  println!("Affiliate {} created", affiliate.id);
  ```

  ```go Go theme={null}
  import (
      "context"
      "fmt"
      "log"
      "os"

      whopsdk "github.com/whopio/whopsdk-go"
      "github.com/whopio/whopsdk-go/client"
      "github.com/whopio/whopsdk-go/option"
  )

  client := client.NewWhop(option.WithToken(os.Getenv("WHOP_API_KEY")))

  affiliate, err := client.Affiliates.Create(context.TODO(), &whopsdk.CreateAffiliatesRequest{
      CompanyID:      "biz_xxxxxxxxxxxxx",
      UserIdentifier: "johndoe", // username, email, usr_xxx, or Discord ID
  })
  if err != nil {
      log.Fatal(err)
  }

  fmt.Printf("Affiliate %s created\n", affiliate.ID)
  ```
</CodeGroup>

## Add commission overrides

An affiliate record doesn't define a commission by itself. Add **overrides** to decide what the affiliate gets paid.

| Override type   | What it does                                               | Required fields                                                         |
| --------------- | ---------------------------------------------------------- | ----------------------------------------------------------------------- |
| **`standard`**  | Per-plan commission, percentage or flat fee                | `plan_id`, `commission_type`, `commission_value`, `applies_to_payments` |
| **`rev_share`** | Percentage revenue share, product-specific or account-wide | `commission_value` (always percentage) and optional `product_id`        |

### Standard (per-plan)

<CodeGroup>
  ```typescript TypeScript theme={null}
  const override = await client.affiliates.overrides.create({
    id: affiliate.id,
    body: {
      id: affiliate.id,
      override_type: "standard",
      plan_id: "plan_xxxxxxxxxxxxx",
      commission_type: "percentage",       // "percentage" or "flat_fee"
      commission_value: 40,                // 40% (or $40 if flat_fee)
      applies_to_payments: "first_payment" // "first_payment" or "all_payments"
    },
  });

  // Share these with the affiliate
  console.log(override.product_direct_link);
  console.log(override.checkout_direct_link);
  ```

  ```python Python theme={null}
  override = client.affiliates.overrides.create(
      affiliate.id,
      request={
          "override_type": "standard",
          "plan_id": "plan_xxxxxxxxxxxxx",
          "commission_type": "percentage",   # "percentage" or "flat_fee"
          "commission_value": 40,            # 40% (or $40 if flat_fee)
          "applies_to_payments": "first_payment",
      },
  )

  print(override.product_direct_link)
  print(override.checkout_direct_link)
  ```

  ```rust Rust theme={null}
  let created = client
      .affiliates
      .overrides
      .create(
          &"aff_xxxxxxxxxxxxx".to_string(),
          // (applies_to_payments, commission_type, commission_value, affiliate_id, plan_id)
          &CreateOverridesRequestBody::standard_with_commission_type(
              Some(AffiliateAppliesToPayments::FirstPayment),
              AffiliatePayoutTypes::Percentage, // percentage or flat_fee
              40.0,                             // 40% (or $40 if flat_fee)
              "aff_xxxxxxxxxxxxx".to_string(),
              "plan_xxxxxxxxxxxxx".to_string(),
          ),
          None,
      )
      .await?;

  // Share these with the affiliate
  println!("{:?}", created.product_direct_link);
  println!("{:?}", created.checkout_direct_link);
  ```

  ```go Go theme={null}
  created, err := client.Affiliates.Overrides.Create(context.TODO(), &affiliates.CreateOverridesRequest{
      ID: "aff_xxxxxxxxxxxxx",
      Body: &affiliates.CreateOverridesRequestBody{
          Standard: &affiliates.CreateOverridesRequestBodyStandard{
              ID:                "aff_xxxxxxxxxxxxx",
              PlanID:            "plan_xxxxxxxxxxxxx",
              CommissionType:    whopsdk.AffiliatePayoutTypesPercentage.Ptr(), // percentage or flat_fee
              CommissionValue:   40,                                           // 40% (or $40 if flat_fee)
              AppliesToPayments: whopsdk.AffiliateAppliesToPaymentsFirstPayment.Ptr(),
          },
      },
  })
  if err != nil {
      log.Fatal(err)
  }

  // Share these with the affiliate
  fmt.Println(*created.ProductDirectLink)
  fmt.Println(*created.CheckoutDirectLink)
  ```
</CodeGroup>

`commission_value` rules:

* `"percentage"`: whole number 1–100 (`40` = 40%).
* `"flat_fee"`: dollar amount (`10` = \$10).

`applies_to_payments`:

* `"first_payment"`: affiliate earns only on the initial purchase.
* `"all_payments"`: affiliate earns on every recurring payment too.

### Rev-share (revenue percentage)

Rev-share overrides are always percentage-based. Don't pass `commission_type: "flat_fee"`.

<CodeGroup>
  ```typescript TypeScript theme={null}
  // Product-specific: affiliate earns 30% on sales of this product
  await client.affiliates.overrides.create({
    id: affiliate.id,
    body: {
      id: affiliate.id,
      override_type: "rev_share",
      product_id: "prod_xxxxxxxxxxxxx",
      commission_value: 30,
    },
  });

  // Account-wide: affiliate earns 15% on every product
  await client.affiliates.overrides.create({
    id: affiliate.id,
    body: {
      id: affiliate.id,
      override_type: "rev_share",
      commission_value: 15,
    },
  });
  ```

  ```python Python theme={null}
  # Product-specific: affiliate earns 30% on sales of this product
  client.affiliates.overrides.create(
      affiliate.id,
      request={
          "override_type": "rev_share",
          "product_id": "prod_xxxxxxxxxxxxx",
          "commission_value": 30,
      },
  )

  # Account-wide: affiliate earns 15% on every product
  client.affiliates.overrides.create(
      affiliate.id,
      request={
          "override_type": "rev_share",
          "commission_value": 15,
      },
  )
  ```

  ```rust Rust theme={null}
  // Product-specific: affiliate earns 30% on sales of this product
  client
      .affiliates
      .overrides
      .create(
          &"aff_xxxxxxxxxxxxx".to_string(),
          // (commission_type, commission_value, affiliate_id, product_id, revenue_basis)
          &CreateOverridesRequestBody::rev_share_with_product_id(
              None,
              30.0,
              "aff_xxxxxxxxxxxxx".to_string(),
              "prod_xxxxxxxxxxxxx".to_string(),
              None,
          ),
          None,
      )
      .await?;

  // Account-wide: affiliate earns 15% on every product
  client
      .affiliates
      .overrides
      .create(
          &"aff_xxxxxxxxxxxxx".to_string(),
          &CreateOverridesRequestBody::rev_share(15.0, "aff_xxxxxxxxxxxxx".to_string()),
          None,
      )
      .await?;
  ```

  ```go Go theme={null}
  // Product-specific: affiliate earns 30% on sales of this product
  _, err := client.Affiliates.Overrides.Create(context.TODO(), &affiliates.CreateOverridesRequest{
      ID: "aff_xxxxxxxxxxxxx",
      Body: &affiliates.CreateOverridesRequestBody{
          RevShare: &affiliates.CreateOverridesRequestBodyRevShare{
              ID:              "aff_xxxxxxxxxxxxx",
              ProductID:       whopsdk.String("prod_xxxxxxxxxxxxx"),
              CommissionValue: 30,
          },
      },
  })
  if err != nil {
      log.Fatal(err)
  }

  // Account-wide: affiliate earns 15% on every product
  _, err = client.Affiliates.Overrides.Create(context.TODO(), &affiliates.CreateOverridesRequest{
      ID: "aff_xxxxxxxxxxxxx",
      Body: &affiliates.CreateOverridesRequestBody{
          RevShare: &affiliates.CreateOverridesRequestBodyRevShare{
              ID:              "aff_xxxxxxxxxxxxx",
              CommissionValue: 15,
          },
      },
  })
  if err != nil {
      log.Fatal(err)
  }
  ```
</CodeGroup>

## Manage overrides

<CodeGroup>
  ```typescript TypeScript theme={null}
  // List all overrides for an affiliate
  const overrides = await client.affiliates.overrides.list({ id: "aff_xxxxxxxxxxxxx" });

  // Filter by type
  const standardOnly = await client.affiliates.overrides.list({
    id: "aff_xxxxxxxxxxxxx",
    override_type: "standard",
  });

  // Retrieve one
  const o = await client.affiliates.overrides.retrieve({
    id: "aff_xxxxxxxxxxxxx",
    override_id: "aovr_xxxxxxxxxxxxx",
  });

  // Update
  await client.affiliates.overrides.update({
    id: "aff_xxxxxxxxxxxxx",
    override_id: "aovr_xxxxxxxxxxxxx",
    commission_value: 50,
    applies_to_payments: "all_payments",
  });

  // Delete (for standard overrides, this also removes the affiliate from that plan)
  await client.affiliates.overrides.delete({
    id: "aff_xxxxxxxxxxxxx",
    override_id: "aovr_xxxxxxxxxxxxx",
  });
  ```

  ```python Python theme={null}
  # List all overrides for an affiliate
  overrides = client.affiliates.overrides.list("aff_xxxxxxxxxxxxx")

  # Filter by type
  standard_only = client.affiliates.overrides.list(
      "aff_xxxxxxxxxxxxx",
      override_type="standard",
  )

  # Retrieve one
  o = client.affiliates.overrides.retrieve("aff_xxxxxxxxxxxxx", "aovr_xxxxxxxxxxxxx")

  # Update
  client.affiliates.overrides.update(
      "aff_xxxxxxxxxxxxx",
      "aovr_xxxxxxxxxxxxx",
      commission_value=50,
      applies_to_payments="all_payments",
  )

  # Delete (for standard overrides, this also removes the affiliate from that plan)
  client.affiliates.overrides.delete("aff_xxxxxxxxxxxxx", "aovr_xxxxxxxxxxxxx")
  ```

  ```rust Rust theme={null}
  // List all overrides for an affiliate
  let overrides = client
      .affiliates
      .overrides
      .list(
          &"aff_xxxxxxxxxxxxx".to_string(),
          &AffiliatesOverridesListQueryRequest::default(),
          None,
      )
      .await?;

  // Filter by type
  let standard_only = client
      .affiliates
      .overrides
      .list(
          &"aff_xxxxxxxxxxxxx".to_string(),
          &AffiliatesOverridesListQueryRequest {
              override_type: Some(AffiliateOverrideRoles::Standard),
              ..Default::default()
          },
          None,
      )
      .await?;

  // Retrieve one
  let single = client
      .affiliates
      .overrides
      .retrieve(
          &"aff_xxxxxxxxxxxxx".to_string(),
          &"aovr_xxxxxxxxxxxxx".to_string(),
          None,
      )
      .await?;

  // Update
  client
      .affiliates
      .overrides
      .update(
          &"aff_xxxxxxxxxxxxx".to_string(),
          &"aovr_xxxxxxxxxxxxx".to_string(),
          &UpdateOverridesRequest {
              commission_value: Some(50.0),
              applies_to_payments: Some(AffiliateAppliesToPayments::AllPayments),
              ..Default::default()
          },
          None,
      )
      .await?;

  // Delete (for standard overrides, this also removes the affiliate from that plan)
  client
      .affiliates
      .overrides
      .delete(
          &"aff_xxxxxxxxxxxxx".to_string(),
          &"aovr_xxxxxxxxxxxxx".to_string(),
          None,
      )
      .await?;
  ```

  ```go Go theme={null}
  // List all overrides for an affiliate
  overridesList, err := client.Affiliates.Overrides.List(context.TODO(), &affiliates.ListOverridesRequest{
      ID: "aff_xxxxxxxxxxxxx",
  })
  if err != nil {
      log.Fatal(err)
  }
  _ = overridesList

  // Filter by type
  standardOnly, err := client.Affiliates.Overrides.List(context.TODO(), &affiliates.ListOverridesRequest{
      ID:           "aff_xxxxxxxxxxxxx",
      OverrideType: whopsdk.AffiliateOverrideRolesStandard.Ptr(),
  })
  if err != nil {
      log.Fatal(err)
  }
  _ = standardOnly

  // Retrieve one
  single, err := client.Affiliates.Overrides.Retrieve(context.TODO(), &affiliates.RetrieveOverridesRequest{
      ID:         "aff_xxxxxxxxxxxxx",
      OverrideID: "aovr_xxxxxxxxxxxxx",
  })
  if err != nil {
      log.Fatal(err)
  }
  _ = single

  // Update
  _, err = client.Affiliates.Overrides.Update(context.TODO(), &affiliates.UpdateOverridesRequest{
      ID:                "aff_xxxxxxxxxxxxx",
      OverrideID:        "aovr_xxxxxxxxxxxxx",
      CommissionValue:   whopsdk.Float64(50),
      AppliesToPayments: whopsdk.AffiliateAppliesToPaymentsAllPayments.Ptr(),
  })
  if err != nil {
      log.Fatal(err)
  }

  // Delete (for standard overrides, this also removes the affiliate from that plan)
  _, err = client.Affiliates.Overrides.Delete(context.TODO(), &affiliates.DeleteOverridesRequest{
      ID:         "aff_xxxxxxxxxxxxx",
      OverrideID: "aovr_xxxxxxxxxxxxx",
  })
  if err != nil {
      log.Fatal(err)
  }
  ```
</CodeGroup>

## Manage affiliates

<CodeGroup>
  ```typescript TypeScript theme={null}
  // List, optionally filtered by status. The page is async-iterable and
  // fetches the next page for you as you consume it.
  const affiliates = await client.affiliates.list({
    company_id: "biz_xxxxxxxxxxxxx",
    status: "active",
  });

  for await (const affiliate of affiliates) {
    console.log(affiliate);
  }

  const single = await client.affiliates.retrieve({ id: "aff_xxxxxxxxxxxxx" });

  // Archive blocks the affiliate from earning further commissions
  await client.affiliates.archive({ id: "aff_xxxxxxxxxxxxx" });
  await client.affiliates.unarchive({ id: "aff_xxxxxxxxxxxxx" });
  ```

  ```python Python theme={null}
  # List, optionally filtered by status. Iterating the pager fetches
  # the next page for you as you consume it.
  for affiliate in client.affiliates.list(
      company_id="biz_xxxxxxxxxxxxx",
      status="active",
  ):
      print(affiliate)

  single = client.affiliates.retrieve("aff_xxxxxxxxxxxxx")

  # Archive blocks the affiliate from earning further commissions
  client.affiliates.archive("aff_xxxxxxxxxxxxx")
  client.affiliates.unarchive("aff_xxxxxxxxxxxxx")
  ```

  ```rust Rust theme={null}
  // List, optionally filtered by status. Iterate the returned page's `data`.
  let affiliates = client
      .affiliates
      .list(
          &AffiliatesListQueryRequest {
              company_id: "biz_xxxxxxxxxxxxx".to_string(),
              status: Some(Status::Active),
              ..Default::default()
          },
          None,
      )
      .await?;

  for affiliate in affiliates.data {
      println!("{affiliate:?}");
  }

  let single = client
      .affiliates
      .retrieve(&"aff_xxxxxxxxxxxxx".to_string(), None)
      .await?;

  // Archive blocks the affiliate from earning further commissions
  client.affiliates.archive(&"aff_xxxxxxxxxxxxx".to_string(), None).await?;
  client.affiliates.unarchive(&"aff_xxxxxxxxxxxxx".to_string(), None).await?;
  ```

  ```go Go theme={null}
  // List, optionally filtered by status. The page auto-fetches the next page as you iterate.
  page, err := client.Affiliates.List(context.TODO(), &whopsdk.ListAffiliatesRequest{
      CompanyID: "biz_xxxxxxxxxxxxx",
      Status:    whopsdk.StatusActive.Ptr(),
  })
  if err != nil {
      log.Fatal(err)
  }

  iter := page.Iterator()
  for iter.Next(context.TODO()) {
      fmt.Println(iter.Current())
  }
  if err := iter.Err(); err != nil {
      log.Fatal(err)
  }

  single, err := client.Affiliates.Retrieve(context.TODO(), &whopsdk.RetrieveAffiliatesRequest{
      ID: "aff_xxxxxxxxxxxxx",
  })
  if err != nil {
      log.Fatal(err)
  }
  _ = single

  // Archive blocks the affiliate from earning further commissions
  if _, err := client.Affiliates.Archive(context.TODO(), &whopsdk.ArchiveAffiliatesRequest{ID: "aff_xxxxxxxxxxxxx"}); err != nil {
      log.Fatal(err)
  }
  if _, err := client.Affiliates.Unarchive(context.TODO(), &whopsdk.UnarchiveAffiliatesRequest{ID: "aff_xxxxxxxxxxxxx"}); err != nil {
      log.Fatal(err)
  }
  ```
</CodeGroup>

## Account-level affiliate settings

Three account fields control how Whop presents the affiliate program to users. Update them through the Account resource.

<CodeGroup>
  ```typescript TypeScript theme={null}
  await client.companies.update({
    id: "biz_xxxxxxxxxxxxx",
    affiliate_instructions: "Share your link on social. 30-day cookie window.",
    affiliate_application_required: true,
    featured_affiliate_product_id: "prod_xxxxxxxxxxxxx",
  });
  ```

  ```python Python theme={null}
  client.companies.update(
      "biz_xxxxxxxxxxxxx",
      affiliate_instructions="Share your link on social. 30-day cookie window.",
      affiliate_application_required=True,
      featured_affiliate_product_id="prod_xxxxxxxxxxxxx",
  )
  ```

  ```rust Rust theme={null}
  client
      .companies
      .update(
          &"biz_xxxxxxxxxxxxx".to_string(),
          &UpdateCompaniesRequest {
              affiliate_instructions: Some(
                  "Share your link on social. 30-day cookie window.".to_string(),
              ),
              affiliate_application_required: Some(true),
              featured_affiliate_product_id: Some("prod_xxxxxxxxxxxxx".to_string()),
              ..Default::default()
          },
          None,
      )
      .await?;
  ```

  ```go Go theme={null}
  _, err := client.Companies.Update(context.TODO(), &whopsdk.UpdateCompaniesRequest{
      ID:                           "biz_xxxxxxxxxxxxx",
      AffiliateInstructions:        whopsdk.String("Share your link on social. 30-day cookie window."),
      AffiliateApplicationRequired: whopsdk.Bool(true),
      FeaturedAffiliateProductID:   whopsdk.String("prod_xxxxxxxxxxxxx"),
  })
  if err != nil {
      log.Fatal(err)
  }
  ```
</CodeGroup>

| Field                            | Description                                                              |
| -------------------------------- | ------------------------------------------------------------------------ |
| `affiliate_instructions`         | Guidelines shown to affiliates promoting this account                    |
| `affiliate_application_required` | Whether users must apply and receive approval before becoming affiliates |
| `featured_affiliate_product_id`  | Which product to feature for affiliate promotion                         |

## Tracking earnings

Each override includes a `total_referral_earnings_usd` field that reflects cumulative earnings (in United States dollars) for that specific override. Re-fetch the override (or list overrides on an affiliate) to get the current total.

<Note>
  There is no dedicated `affiliate.*` webhook in v1, and the public `payment` schema doesn't expose affiliate linkage. Poll `affiliates.overrides.list` on your payout schedule and compare the latest `total_referral_earnings_usd` values with the last values you stored.
</Note>

## Override response fields

| Field                         | Description                                                                 |
| ----------------------------- | --------------------------------------------------------------------------- |
| `override_type`               | `"standard"` or `"rev_share"`                                               |
| `commission_type`             | `"percentage"` or `"flat_fee"`                                              |
| `commission_value`            | Percentage (1–100) or flat fee in dollars                                   |
| `applies_to_payments`         | `"first_payment"` or `"all_payments"` (standard only, `null` for rev-share) |
| `plan_id`                     | Plan ID (standard only)                                                     |
| `product_id`                  | Product ID (rev-share only, `null` if account-wide)                         |
| `applies_to_products`         | `"single_product"` or `"all_products"` (rev-share only)                     |
| `product_direct_link`         | Referral link to product page (standard only)                               |
| `checkout_direct_link`        | Referral link to checkout page (standard only)                              |
| `total_referral_earnings_usd` | Cumulative earnings for this override                                       |

## Next steps

<CardGroup cols={2}>
  <Card title="Pay out affiliate earnings" href="/developer/platforms/collect-payments-for-connected-accounts#transfers">
    Transfer accrued commissions from your balance to affiliate accounts.
  </Card>

  <Card title="Accept payments" href="/developer/guides/accept-payments">
    Checkout configurations attribute sales automatically when referral cookies are set.
  </Card>

  <Card title="Listen to webhooks" href="/developer/guides/webhooks">
    Use `payment.succeeded` for general sales telemetry. Affiliate attribution still polls.
  </Card>

  <Card title="Affiliates API reference" href="/api-reference/affiliates/affiliate">
    Full resource. Endpoints, fields, and override schemas.
  </Card>
</CardGroup>
