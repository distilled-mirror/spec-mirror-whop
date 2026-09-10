> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Domain

A Domain is an account's claim to a hostname and its app assignment. Publish the returned ownership TXT and routing DNS records. Verification and certificate provisioning run automatically; unverified claims expire after 48 hours. Only verified domains with active hostname and certificate status resolve through the Apps API.

An unverified claim does not reserve a hostname globally. Transferring ownership requires a fresh TXT proof and an explicit replacement request. Removing a domain stops app resolution immediately while Cloudflare cleanup finishes in the background.

## Endpoints

| Endpoint                                                       | Request                                                              |
| -------------------------------------------------------------- | -------------------------------------------------------------------- |
| [List Domains](/api-reference/beta/domains/list-domains)       | <Badge color="blue" size="sm" stroke>GET</Badge> `/domains`          |
| [Create Domain](/api-reference/beta/domains/create-domain)     | <Badge color="green" size="sm" stroke>POST</Badge> `/domains`        |
| [Retrieve Domain](/api-reference/beta/domains/retrieve-domain) | <Badge color="blue" size="sm" stroke>GET</Badge> `/domains/{id}`     |
| [Update Domain](/api-reference/beta/domains/update-domain)     | <Badge color="orange" size="sm" stroke>PATCH</Badge> `/domains/{id}` |
| [Delete Domain](/api-reference/beta/domains/delete-domain)     | <Badge color="red" size="sm" stroke>DELETE</Badge> `/domains/{id}`   |

## Attributes

<Columns cols={2}>
  <Column>
    <ResponseField name="id" type="string" required>
      Domain ID, prefixed `dom_`.
    </ResponseField>

    <ResponseField name="account_id" type="string" required>
      ID of the account claiming or owning this domain, prefixed `biz_`.
    </ResponseField>

    <ResponseField name="app_id" type="string" required>
      ID of the app assigned to this domain, prefixed `app_`.
    </ResponseField>

    <ResponseField name="certificate_status" type="string | null" required>
      Cloudflare's latest certificate issuance status.
    </ResponseField>

    <ResponseField name="created_at" type="string" required>
      When the domain claim was created, as an ISO 8601 timestamp.
    </ResponseField>

    <ResponseField name="dns_records" type="object[]" required>
      DNS records to publish: TXT for ownership, A/AAAA for root domains through Whop's Apex Proxying IPs, and CNAME for subdomains.

      <Accordion title="Properties" defaultOpen={true}>
        <ResponseField name="name" type="string" required>
          Full hostname where the record must be published.
        </ResponseField>

        <ResponseField name="type" type="string" required>
          DNS record type.

          Available options: `TXT`, `CNAME`, `A`, `AAAA`
        </ResponseField>

        <ResponseField name="value" type="string" required>
          DNS record content.
        </ResponseField>
      </Accordion>
    </ResponseField>

    <ResponseField name="dns_status" type="string" required>
      Result of the most recent DNS routing check. Ownership is verified separately.

      Available options: `pending`, `valid`, `invalid`, `unknown`
    </ResponseField>

    <ResponseField name="domain" type="string" required>
      Normalized hostname, such as checkout.example.com.
    </ResponseField>

    <ResponseField name="hostname_status" type="string | null" required>
      Cloudflare's latest hostname activation status.
    </ResponseField>

    <ResponseField name="issues" type="object[]" required>
      Setup issues to address before the domain can serve traffic.

      <Accordion title="Properties" defaultOpen={true}>
        <ResponseField name="code" type="string" required>
          The source of the setup issue.
        </ResponseField>

        <ResponseField name="message" type="string" required>
          What needs attention before the domain can serve the website.
        </ResponseField>
      </Accordion>
    </ResponseField>

    <ResponseField name="last_checked_at" type="string | null" required>
      When DNS and provider state were last checked, as an ISO 8601 timestamp.
    </ResponseField>

    <ResponseField name="metadata" type="object" required>
      Custom string keys and values attached to this domain.
    </ResponseField>

    <ResponseField name="status" type="string" required>
      Domain lifecycle. Only active domains resolve to their app.

      Available options: `pending_verification`, `provisioning`, `active`, `action_required`, `deleting`, `removed`
    </ResponseField>

    <ResponseField name="updated_at" type="string" required>
      When the domain was last updated, as an ISO 8601 timestamp.
    </ResponseField>

    <ResponseField name="verification_expires_at" type="string | null" required>
      When an unverified claim is automatically deleted, 48 hours after creation, as
      an ISO 8601 timestamp.
    </ResponseField>

    <ResponseField name="verified_at" type="string | null" required>
      When Whop verified the ownership TXT record, as an ISO 8601 timestamp.
    </ResponseField>
  </Column>

  <Column>
    <div className="api-resource-sticky-example">
      ```json Domain theme={null}
      {
      	"id": "dom_M6b2X9k4R7t1Wp",
      	"account_id": "biz_J8n2R5p9T1w4Kx",
      	"app_id": "app_P9q3K7m2W8v1Bx",
      	"domain": "shop.example.com",
      	"status": "pending_verification",
      	"dns_status": "pending",
      	"verified_at": null,
      	"verification_expires_at": "2026-09-10T12:00:00.000Z",
      	"hostname_status": null,
      	"certificate_status": null,
      	"dns_records": [
      		{
      			"type": "TXT",
      			"name": "_whop.shop.example.com",
      			"value": "whop-domain-verification=2a76d0f9f48c3e215b9ed6011a4e7328da5f70936bcaf874c2195088dedd9201"
      		},
      		{
      			"type": "CNAME",
      			"name": "shop.example.com",
      			"value": "customers.example.net"
      		}
      	],
      	"issues": [
      		{
      			"code": "ownership_required",
      			"message": "Publish the ownership TXT record, then verify the domain."
      		},
      		{
      			"code": "dns_required",
      			"message": "Point DNS to Whop using the routing records. Disable other CDN proxies while connecting."
      		}
      	],
      	"metadata": {
      		"project": "storefront"
      	},
      	"last_checked_at": null,
      	"created_at": "2026-09-09T12:00:00.000Z",
      	"updated_at": "2026-09-09T12:00:00.000Z"
      }
      ```
    </div>
  </Column>
</Columns>
