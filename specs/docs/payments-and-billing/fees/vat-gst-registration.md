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

# VAT and GST Registration Details

> Why Whop is asking whether your business is registered for VAT, GST, or a similar tax, and how to respond by November 30, 2026

Whop is asking owners of businesses that sell, or have sold, on Whop to confirm whether their businesses are registered for VAT, GST, or a similar tax. The request covers supported countries outside the United States and its territories. It takes about 2 minutes. Please respond by **November 30, 2026**.

This page doesn't provide tax or legal advice. If you're unsure what applies to your business, talk to a tax professional.

## Find the answers for you

* **I have a VAT, GST, or similar tax number:** see [How to respond](#how-to-respond) and [If you're registered](#if-youre-registered).
* **I sell as an individual or don't have a tax number:** see [Sellers without a tax number](#sellers-without-a-tax-number).
* **I no longer sell on Whop:** see [Former sellers](#former-sellers).
* **Everyone:** see [Deadline](#deadline) and [Getting help](#getting-help).

## About this request

> [accordion] What's Whop asking me to do?
>
> Confirm whether your business is registered for VAT, GST, or a similar tax in the country where it's established. If it's registered there, add that tax registration number. If not, check one box to confirm it.

> [accordion] Who is being asked?
>
> Owners of businesses that have made at least one sale on Whop, including businesses that no longer sell. The request covers supported countries outside the United States and its territories (Puerto Rico, Guam, the U.S. Virgin Islands, American Samoa, and the Northern Mariana Islands). Businesses that already added a tax number are asked to confirm it. Owners of businesses that haven't made a sale yet may also see the request in their dashboard. Team members with permission to update the business see it too.

> [accordion] Why is Whop collecting this now?
>
> Whop is required to collect and validate its sellers' tax IDs to meet its own tax obligations. Your registration status helps determine the correct tax treatment of Whop's fees and provision of services, so Whop is asking eligible business owners to confirm it.

> [accordion] Is this email legitimate?
>
> Yes. Collecting tax registration details is standard practice for online platforms. Whop will never ask for your password or bank details by email. To be safe, you can sign in to Whop directly instead of using the link in the email, then open your business dashboard and go to **Settings** > **Tax**.

## How to respond

> [accordion] How do I provide my information?
>
> 1. Sign in to Whop and open your business dashboard.
> 2. Go to **Settings** > **Tax**.
> 3. Under **Tax registrations**, select **Add your business tax details**.
> 4. Answer **Is your business registered for VAT, GST or a similar tax where it's established?** and complete the details for your answer. If a registration number is already on file, check that it's your business's number.
> 5. Select **Save** if you added a registration number, or **Confirm** otherwise.
> 6. Check that your business legal name and address on the same page are up to date.
>
> If you choose **Whop collects, I remit** when you set up tax, Whop also asks for the tax ID of every country where you're registered. You can skip that step and add the numbers later under **Tax registrations**.
>
> The business owner, or a team member with permission to update the business, can add a tax registration number. Only the owner can confirm the business isn't registered, so if that's your answer and you're not the owner, ask the owner to respond.

> [accordion] What do I provide if my business is registered?
>
> Choose **Yes, my business is registered**. Then pick the country where your business is established and the type of registration, and enter your registration number exactly as it appears on your registration certificate. Make sure your business legal name matches your registration records.

> [accordion] What do I provide if my business isn't registered?
>
> Choose **No, my business is not registered** and check the box to confirm it. Whop then treats your business as not registered and may collect and remit VAT or GST on its fees and other services where the law requires it. For more detail, see [Sellers without a tax number](#sellers-without-a-tax-number).

> [accordion] I'm registered in more than one country. Which number do I give?
>
> Provide the number for the country where your business is established. If your business isn't registered there, choose **No, my business is not registered**, even if it's registered somewhere else. If your business is based in Quebec, Canada, provide both your federal GST/HST number and your Quebec Sales Tax (QST) number.

> [accordion] I made a mistake. Can I change my answer?
>
> You can add or remove tax registration numbers on the same page at any time. To switch from registered to not registered, remove the registration number for the country where your business is established first, then answer the question again. If you confirmed you're not registered and later register, add your number under **Tax registrations**.
>
> Please keep your details current if your registration status changes.

## If you're registered

> [accordion] Does giving Whop my tax number change my tax position?
>
> No. Sharing your number with Whop doesn't by itself change your tax obligations. Those are set by the law where you operate and your circumstances.

> [accordion] Will this affect my past tax returns?
>
> Providing your number doesn't by itself create new obligations. Your obligations for past transactions depend on the law that applied at the time and your circumstances. For questions about your past returns, check with your tax advisor.

> [accordion] I registered recently. What should I do?
>
> Add your registration number as soon as you have it. If you have questions about how earlier periods are treated, contact [tax-registration@whop.com](mailto:tax-registration@whop.com).

> [accordion] I have an Australian ABN. Is that my GST number?
>
> Only if you're also registered for GST. An ABN by itself doesn't mean you're GST registered. If you're registered for GST, choose **Yes, my business is registered** and add your ABN. If you have an ABN but aren't registered for GST, choose **No, my business is not registered**.

## Sellers without a tax number

> [accordion] I sell as an individual, not a company. Does this apply to me?
>
> Yes. Whop is asking every seller, including individuals. Whether your activity counts as a business for VAT or GST depends on the law where you live.

> [accordion] Do I need a VAT or GST number to sell on Whop?
>
> No. Whop doesn't require one. Whether you must register depends on the law where you live. If you're unsure, check your local tax authority's guidance or ask a tax advisor.

> [accordion] I don't have a VAT or GST number. What should I select?
>
> Choose **No, my business is not registered**, check the box to confirm it, and select **Confirm**. That completes your response.

> [accordion] What does it mean for me if I'm not registered?
>
> * Whop treats you as not tax registered.
> * Whop may collect and remit VAT or GST on Whop fees and other services, as required under applicable law.
> * Your payouts aren't affected once you've confirmed your status.

> [accordion] Does Whop asking me mean I have to register?
>
> No. Whop doesn't decide whether you need to register. If you're unsure, check your local tax authority's guidance or ask a tax advisor.

> [accordion] Can I enter a personal ID number instead?
>
> No. Please don't enter a national ID, social security number, or other personal number as a tax registration.

> [accordion] What if my situation changes later?
>
> If you register in the future, go to **Settings** > **Tax** and add your new number under **Tax registrations**.

## Former sellers

> [accordion] I no longer sell on Whop. Why am I receiving this?
>
> Your registration status determines how tax applied to the fees on past transactions. Whop needs it to keep its tax records accurate for those periods, even if you've stopped selling.

> [accordion] My account is inactive. Can I still sign in?
>
> Yes. You can update your tax details without selling on Whop. If you can't access your account, contact Whop support to regain access.

> [accordion] I've left Whop. What happens if I don't respond?
>
> If your account still has a balance, payouts may be paused from December 1, 2026, until Whop receives your information. Either way, Whop will treat you as not tax registered for its records if you don't provide a valid VAT, GST, or similar tax ID.

## Privacy and data use

> [accordion] How will Whop use my tax number?
>
> To apply the correct tax treatment to Whop's fees and to purchases made on Whop, and to keep accurate tax records.

> [accordion] Is my information stored securely?
>
> Yes. Your tax registration numbers are encrypted when stored, and your tax details are handled in line with the Whop [Privacy Policy](https://whop.com/privacy/).

## Deadline

> [accordion] When do I need to respond?
>
> By November 30, 2026.

> [accordion] What happens if I don't respond?
>
> If you sell on Whop, payouts on your account may be paused from December 1, 2026, until you provide your information. Without your details, Whop will treat you as not tax registered, and VAT or GST may apply to Whop's fees where the law requires it.

> [accordion] Can I respond after the deadline?
>
> Yes. The same page stays open after November 30. If your payouts were paused, they resume once you provide your information.

## Getting help

> [accordion] Who can I contact with questions?
>
> If you have an assigned account manager, contact them first. Otherwise, email [tax-registration@whop.com](mailto:tax-registration@whop.com). Whop can't give tax advice about your business's own filings, so for questions about your returns, contact your tax advisor.


## Related topics

- [Sales Tax and VAT](https://docs.whop.com/payments-and-billing/fees/taxes.md)
- [Hide Approximate Location](https://docs.whop.com/account-settings/personal-details/hide-approximate-location.md)
- [Contact a Merchant](https://docs.whop.com/memberships-and-access/cancellations-and-refunds/contact-a-merchant.md)
