> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

> ## Agent Instructions
> Treat these docs and the Whop MCP server as the source of truth for Whop API behavior. Don't infer endpoint behavior from marketing pages.
> MCP server: connect https://mcp.whop.com/mcp (streamable HTTP, browser sign-in, no API key in the client config) to search these docs and read or change live Whop data. Setup: https://docs.whop.com/developer/guides/ai_and_mcp.md
> Whop CLI: install with `curl -fsSL https://whop.com/install.sh | sh` to scaffold, run, and deploy Whop apps and websites. It acts on production, not the sandbox. Overview: https://docs.whop.com/cli/overview.md Agent mode: https://docs.whop.com/cli/agent-mode.md
> Hosting: Whop serves a site or app from the build `whop apps deploy` uploads, with rollbacks, secrets, and server logs. Guide: https://docs.whop.com/developer/websites/hosting.md
> Plugins: in Claude Code, run `/plugin marketplace add whopio/plugins`, then `/plugin install whop@whop`. Guide: https://docs.whop.com/developer/guides/plugins/claude-code.md Every agent tool: https://docs.whop.com/developer/ai/overview.md
> Environments: build and test in the sandbox (API `https://sandbox-api.whop.com/api/v1`, keys at https://sandbox.whop.com/dashboard/developer, SDK environment `Sandbox`; no real money). Production (API `https://api.whop.com/api/v1`, keys at https://whop.com/dashboard/developer, SDK environment `Production`, the SDK default) moves real money, so use it only at launch. The two share no accounts, keys, products, or webhooks, and their keys look identical: never mix them. Sandbox guide: https://docs.whop.com/developer/guides/sandbox.md Comparison: https://docs.whop.com/developer/quickstart.md
> API: send `Authorization: Bearer <API_KEY>` from your server only, and pin a dated version with the `Api-Version-Date` header. OpenAPI spec: https://api.whop.com/api/v1/openapi.json
> Which API: build on the Current API, whose pages live under https://docs.whop.com/api-reference/ (start at https://docs.whop.com/api-reference/overview.md). Pages under /api-reference/legacy/ are the Legacy API. Use one only when https://docs.whop.com/api-reference/stability.md lists the resource as Legacy-only.

# Sales Tax and VAT

> Choose how tax is collected and remitted on your direct sales, and see how Whop handles tax on marketplace sales.

Tax rules vary by your buyer's location, what you sell, and how much you sell. They also change over time. This help guide doesn't provide tax or legal advice. If you're unsure what applies to your business, talk to a tax professional.

An important note before you start: choosing one of Whop's managed tax options doesn't, by itself, remove your own tax obligations. You remain responsible for monitoring your own nexus and for obligations from sales outside Whop. You also remain responsible for sales beyond Whop's registered or covered jurisdictions. Tax authorities increasingly compare marketplace data with sellers' filings, so it's worth keeping your records current.

## How taxes work on Whop

There are two kinds of sales on Whop, and they're handled differently:

1. **Marketplace sales**: a buyer finds and buys your product on Whop's marketplace. On these sales, Whop automatically handles tax in jurisdictions covered by Whop's tax registrations.
2. **Direct sales through Whop (checkout links or the Checkout element on your site)**: you choose how to handle tax. The following sections explain three options.

***

## Marketplace sales: Whop handles everything in covered jurisdictions

When a buyer purchases your product on Whop's marketplace, Whop handles the tax process where marketplace laws and Whop's registrations apply.

For those sales, Whop:

* Determines whether the sale is taxable, based on the product, the buyer's location, and local rules
* Calculates the correct tax and applies it at checkout
* Collects the tax, charging it to the buyer
* Remits the tax to the right tax authorities and files the required returns

You usually don't need to hold, track, or file this tax yourself for these sales. Marketplace coverage doesn't remove your other obligations. Consult your tax advisor if you have marketplace sales outside Whop's registered jurisdictions or direct sales outside Whop. The advisor can help determine whether you have nexus or filing requirements.

***

## Direct sales through a checkout link or the checkout element

### Your three options

| | What Whop does | What you do | Where it works |
| :- | :- | :- | :- |
| **Whop collects tax, I remit** *(Recommended)* | Calculates the applicable tax at checkout and includes the collected tax in your payout. | Register where required, then remit the tax and file returns. | All states and territories in the United States, plus the international jurisdictions on [Numeral's coverage list](https://www.numeral.com/coverage) (90+ countries). |
| **Whop collects and remits tax** | Calculates and collects the tax at checkout, then remits it and files the returns. | Under the current business model, you usually need to evaluate tax requirements for jurisdictions outside the United States, EU, and UK. | The states and territories in the United States, EU, and UK. |
| **I'll handle tax myself** | Nothing. | Everything: register, collect, remit, and file the tax yourself, everywhere you sell. | You handle every jurisdiction. |

> [!NOTE]
>
> "Whop collects tax, I remit" costs 0.5% per transaction. "Whop collects and
> remits tax" costs 2% per transaction. Both fees apply only when tax is
> collected. "I'll handle tax myself" has no Whop tax fee. See the
> [Pricing page](https://docs.whop.com/payments-and-billing/fees/fees.md) for full details.

***

## Choose an option for direct sales through Whop

### Whop collects tax, I remit (recommended for most sellers)

Whop calculates the applicable tax and applies it at checkout in the regions where you've added tax registrations, based on the information you provide and the transaction details. The collected tax arrives in your payout, and you're responsible for registering where required, remitting the tax to the authorities, and filing returns.

**Best for you if:**

* You sell to buyers in **multiple countries** or outside the United States, EU, and UK.
* You want the correct tax collected automatically.
* You want the widest coverage available for tax computation.

> [!WARNING]
>
> Tax is only calculated where the product is taxable, and you may need to
> register in a jurisdiction before you can lawfully remit there. The collected
> tax sits in your payout, so set it aside, because you'll owe it to the
> government, and you're the one remitting it.

### Whop collects and remits tax

The hands-off option *within its coverage area*. For states in the United States, the EU, and the UK, Whop calculates, collects, remits, and files the tax.

**Best for you if:**

* Your buyers are **almost entirely in the United States, EU, and UK**.
* You want minimal tax admin and don't want to file returns yourself in covered regions.

**Keep in mind:** outside Whop's coverage area (usually outside the United States, EU, and UK), Whop doesn't collect or remit tax. Those obligations remain yours. Whop continues to expand its coverage area and will provide updates as it adds regions.

### I'll handle tax myself

Whop stays out of tax entirely on your direct sales. You take full responsibility everywhere you sell.

**Best for you if:**

* You (or your accountant) already manage tax registration, collection, and filing across every region you sell in.
* You have a specific reason to keep tax fully in your own systems.

**Keep in mind:** this means determining where you owe tax, registering where required, collecting the right amount, and remitting and filing it, in **every** applicable jurisdiction. Getting this wrong can mean back taxes and penalties.

***

## Frequently asked questions

> [tab] Sales tax
>
> ### Buyer questions
>
> > [accordion] Why am I being charged sales tax?
> >
> > Your location's tax rules require sales tax for this purchase. Whop adds any required sales tax at checkout based on your location and purchase. If sales tax applies to your order, you'll see it before completing payment.
>
> > [accordion] Is this a Whop fee?
> >
> > No. Sales tax is a government tax, not a Whop fee. When Whop collects sales tax, Whop remits it to the appropriate tax authority as required.
>
> > [accordion] Will I always see sales tax?
> >
> > Not necessarily. Whether tax applies depends on the delivery address and whether the item/service is taxable under local rules. If it applies, it will appear during checkout.
>
> > [accordion] Why didn't I see sales tax before?
> >
> > Starting March 1, 2026, Whop collects and remits sales tax for applicable marketplace sales in the jurisdictions listed in the Help Center when required. Outside those jurisdictions (or for non-taxable items), sales tax may not appear.
>
> > [accordion] I think the sales tax amount is wrong. What should I do?
> >
> > Share your order number and the delivery ZIP code (and a screenshot of checkout if possible). Whop Support will review the order details and escalate for investigation if anything looks incorrect.
>
> > [accordion] What happens to sales tax if I get a refund?
> >
> > When Whop refunds an order, Whop generally refunds and adjusts any applicable sales tax based on the refunded amount and transaction details.
>
> > [accordion] I'm tax-exempt. Can you remove sales tax?
> >
> > Tax exemption rules vary by jurisdiction and may require documentation. Contact Support with your order number (if available) and any exemption documentation so Whop Support can review available options.
>
> > [accordion] Can you tell me what I owe for taxes?
> >
> > Whop Support can explain how Whop handles sales tax for Whop Tax Service transactions but can't provide personalized tax advice. If you need guidance for your specific situation, consider speaking with a tax professional.
>
> ### Seller questions
>
> > [accordion] Do I need to do anything for marketplace sales?
> >
> > Not for jurisdictions where Whop holds tax registrations. In these jurisdictions, Whop acts as the marketplace facilitator and handles tax calculation, collection, and remittance, no matter which option you've chosen for your direct sales.
>
> > [accordion] Is sales tax included in the price presented to the buyer?
> >
> > It depends on your tax settings. You choose whether your prices are tax-inclusive (tax is part of the displayed price) or tax-exclusive (Whop adds tax at checkout on top of your price).
>
> > [accordion] Where can I see how much sales tax was collected?
> >
> > Order receipts and details show sales tax when it applies. You can also use available reporting and exports to view tax amounts by order.
>
> > [accordion] Will sales tax change my payout?
> >
> > Your option determines how sales tax affects your payout. For marketplace sales or under "Whop collects and remits tax," Whop keeps tax separate from your earnings and remits it. Under "Whop collects tax, I remit," Whop includes collected tax in your payout for you to remit, so set it aside. Share an order number with Support to confirm how tax appears in the order and payout details.
>
> > [accordion] Can you provide proof that Whop remitted the sales tax?
> >
> > Whop doesn't provide copies of confidential tax returns or filings. You can use transaction receipts and order records showing tax collected, plus the Help Center reference page listing where Whop currently collects and remits sales tax. If a tax authority requests additional documentation, Whop can escalate the request for review.
>
> > [accordion] How often does Whop remit the sales tax?
> >
> > Tax authorities set filing schedules (monthly, quarterly, etc.), which can vary. Whop remits taxes on the schedule required by the relevant tax authority.

> [tab] VAT
>
> ### Buyer questions
>
> > [accordion] Why am I being charged VAT (or another non-U.S. tax)?
> >
> > Indirect taxes like VAT can apply based on your location and what you purchase. In VAT/GST countries where the price is tax-inclusive, the displayed total at checkout and the tax invoice already include the tax. You won't pay more than the displayed price.
>
> > [accordion] Is this a Whop fee?
> >
> > No. VAT (or a similar indirect tax) is a government tax, not a Whop fee. When Whop is responsible for collecting it, Whop remits it to the appropriate tax authority.
>
> > [accordion] Why does my invoice show Whop as the party handling VAT?
> >
> > For certain non-U.S. transactions where Whop is responsible for indirect tax handling, tax invoices identify Whop as the party handling indirect tax for that transaction.
>
> > [accordion] I need a VAT invoice. How do I get one?
> >
> > If a tax invoice is available for your purchase, you can access it from your order details or receipt. If you can't find it, share your order ID with Support to help locate the correct invoice.
>
> > [accordion] I think the VAT amount is wrong. What should I do?
> >
> > Whop Support can help investigate. Share your order ID, your billing country (and VAT identification number if applicable), and a screenshot of the invoice or receipt showing the tax line. Whop Support will review and escalate if something looks off.
>
> > [accordion] What happens to VAT if I get a refund?
> >
> > When Whop refunds an order, Whop generally adjusts any applicable indirect tax based on the refunded amount and transaction details.
>
> ### Seller questions
>
> If Whop asked whether your business is registered for VAT or GST, see [VAT and GST registration details](https://docs.whop.com/payments-and-billing/fees/vat-gst-registration.md).
>
> > [accordion] Does Whop handle VAT for sales to the EU or UK?
> >
> > Whop maintains VAT registrations in the EU and UK. For marketplace sales, and for direct sales where you've chosen "Whop collects and remits tax," Whop collects and remits the VAT for you. If you've chosen "Whop collects tax, I remit" or "I'll handle tax myself," remitting is your responsibility for those direct sales.
>
> > [accordion] Can I handle VAT myself?
> >
> > For marketplace sales in the EU or UK, no. Whop acts as the marketplace facilitator and handles VAT. For your own direct sales, yes: you can choose "I'll handle tax myself," which leaves all calculation, collection, and remittance to you.
>
> > [accordion] Will this affect my payout?
> >
> > Tax-inclusive prices include VAT in the price you set. As a result, your net is lower than it would be if Whop added tax on top. For a specific payout, share an order ID with Support to confirm how tax appears in the transaction record.
>
> > [accordion] Can you provide proof that Whop remitted the VAT?
> >
> > Whop doesn't provide copies of confidential tax returns or filings. You can use transaction receipts and order records showing tax collected, plus the Help Center reference page listing where Whop currently collects and remits VAT. If a tax authority requests additional documentation, Whop can escalate the request for review.
>
> > [accordion] How often does Whop remit the VAT?
> >
> > Tax authorities set filing schedules (monthly, quarterly, etc.), which can vary. Whop remits taxes on the schedule required by the relevant tax authority.


## Related topics

- [VAT and GST Registration Details](https://docs.whop.com/payments-and-billing/fees/vat-gst-registration.md)
- [Whop Pricing and Fees](https://docs.whop.com/payments-and-billing/fees/fees.md)
- [Business Analytics Dashboard](https://docs.whop.com/manage-your-business/manage-business/analytics.md)
