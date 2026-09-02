> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Verifications

> Collects and verifies identity information for an account. Mount `kyc` for a complete KYC or KYB flow backed by the Verifications API.

## Playground

Assemble the elements with example data. Drive the controls, add and arrange elements, and watch events fire live:

<div data-whop-demo-shell style={{ position: "relative", minHeight: "480px", transition: "min-height 200ms ease" }}>
  <div data-whop-demo-skeleton style={{ position: "absolute", inset: "0", borderRadius: "12px", background: "rgba(140, 140, 140, 0.12)", pointerEvents: "none", transition: "opacity 200ms ease" }} />

  <div data-whop-demo-native="playground:verifications" data-whop-elements-version="" style={{ position: "relative" }} />
</div>

<div data-whop-usage="verifications/playground">
  <CodeGroup>
    ```tsx React theme={null}
    import { WhopElements, Verifications } from "@whop/elements-react";
    import { loadWhop } from "@whop/elements";

    function Example() {
      return (
        <WhopElements elements={loadWhop()}>
          <Verifications /* options */>
            {/* mount elements here */}
          </Verifications>
        </WhopElements>
      );
    }
    ```

    ```html JavaScript theme={null}
    <script src="https://js.whop.cloud/elements/amber/elements.js" data-whop-elements></script>
    <script type="module">
      const verifications = window.WhopElements().verifications.create({ /* options */ });
    </script>
    ```
  </CodeGroup>
</div>

## Options

Pass these to `whop.verifications.create({ … })`, or as props on `<Verifications>` in React.

<ResponseField name="kind" type="&#x22;individual&#x22; | &#x22;business&#x22;">
  Use `individual` for KYC or `business` for KYB. The mounted element owns the matching intake form. Defaults to `"individual"`.
</ResponseField>

<ResponseField name="accountId" type="string" required>
  The account being verified: a company `biz_…` tag or the viewer's own `user_…` tag.
</ResponseField>

<ResponseField name="getToken" type="(() => Promise<string>)">
  Called on your page before the element mounts. Return a fresh bearer credential with `identity:read` and `identity:write`; omit it only for a same-origin Whop session.
</ResponseField>

<ResponseField name="appearance" type="Appearance">
  Visual customization for this group's elements. Overrides the global `WhopElements({ appearance })`. Change it live with `update({ appearance })`.
</ResponseField>

<ResponseField name="locale" type="WhopElementsLocale">
  Locale for this group's element UI text. Set it to one of the app's built locales to override the global configuration. Any other value falls back to the default locale.
</ResponseField>

## Events

Pass callbacks in the create options or React props.

### `onLoadingChange`

Runs when the grouped loading state changes. The value is `true` while any mounted element is still loading.

**Signature:** `((loading: boolean) => void)`

## Methods

Call these on the Verifications handle from `whop.verifications.create({ … })` or `useVerifications()`.

### `update`

Merges new handle options into every mounted element. In React, change the namespace props instead.

**Signature:** `(options: Partial<VerificationsOptions>) => void`

### `destroy`

Destroys every element and sub-controller this handle created, removes the controller frame, and releases its subscriptions. You can call it more than once, but a destroyed handle refuses any other call — create a new handle to start over. React removes the group automatically when the provider unmounts.

**Signature:** `() => void`

## Types

Named types used throughout this page.

## `ActionRequiredItem`

Fields on `ActionRequiredItem`.

### `id`

**Signature:** `string`

### `requirement`

**Signature:** `string`

### `type`

**Signature:** `string`

### `label`

**Signature:** `string`

### `source`

**Signature:** `"identity" | "payout" | "audit" | "card_issuing" | "bank" | "ads" | "application" | undefined`

### `optional`

**Signature:** `boolean | undefined`

### `options`

**Signature:** `string[] | undefined`

### `errors`

**Signature:** `{ code: string; reason: string; }[] | undefined`

## Elements

The elements this group mounts. Each has its own page:

<CardGroup cols={2}>
  <Card title="KycElement" href="/elements/upcoming/verifications/kyc">
    A complete identity-verification flow. It collects individual KYC or business KYB details, starts or resumes the hosted provider session, handles follow-up information and document requests, polls status, and renders the final result without requiring the host to build verification UI.
  </Card>

  <Card title="CapabilitiesElement" href="/elements/upcoming/verifications/capabilities">
    The account's verification standing: whether individual and business verification are done, and which capabilities that unlocks. Read-only — pressing a verify button reports `verificationRequested` and stays put, so the host mounts its own flow, such as this namespace's `kyc` element. Both halves can be hidden, so a host that only wants the capability list, or only the two verification rows, can drop the other.
  </Card>

  <Card title="RfiElement" href="/elements/upcoming/verifications/rfi">
    Everything the account still owes compliance, and the forms to answer it. Each row is one group of requirements — grouped by the system that asked, because each relays to its provider once its own items are answered — and pressing it opens that group’s form in place. Rows already with a reviewer are shown but not answerable. Renders an all-clear once nothing is outstanding, so it can sit permanently in a settings page.
  </Card>
</CardGroup>
