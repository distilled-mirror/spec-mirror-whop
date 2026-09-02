> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# TaxIdElement

> Collects a business tax registration accepted by the API. Labels use buyer-facing names. The placeholder matches the selected format. `country` preselects a type. `onChange` emits committed pairs. The host supplies API validation errors.

<div data-whop-platform="web">
  Mounts inside [`Payments`](/elements/upcoming/payments/overview). Pass props and callbacks through the create options or React props.
</div>

<div data-whop-platform="react-native" style={{ display: "none" }}>
  Mount inside `<Payments>`, which owns the charge and the confirmation token. `<Payments>` itself mounts inside `<WhopElements>`. A registration-type picker beside a value input, so a buyer entering a VAT or CUIT number picks the right kind first.
</div>

<div data-whop-split style={{ display: "flex", gap: "1.5rem", alignItems: "flex-start", flexWrap: "wrap" }}>
  <div style={{ flex: "1 1 26rem", minWidth: 0 }}>
    <div data-whop-usage="payments/taxId">
      <CodeGroup>
        ```tsx React theme={null}
        import { WhopElements, Payments, TaxIdElement } from "@whop/elements-react";
        import { loadWhop } from "@whop/elements";

        function Example() {
          return (
            <WhopElements elements={loadWhop()}>
              <Payments /* options */>
                <TaxIdElement onChange={(e) => console.log(e)} />
              </Payments>
            </WhopElements>
          );
        }
        ```

        ```tsx React Native theme={null}
        import { Payments, TaxIdElement } from '@whop/elements-react-native';

        export function TaxStep({ country }: { country: string }) {
          return (
            <Payments accountId="biz_xxxxxxxx" plan="plan_xxxxxxxx">
              <TaxIdElement country={country} onChange={({ taxId }) => console.log(taxId.type, taxId.value)} />
            </Payments>
          );
        }
        ```

        ```html JavaScript theme={null}
        <script src="https://js.whop.cloud/elements/amber/elements.js" data-whop-elements></script>
        <script type="module">
          const payments = window.WhopElements().payments.create({ /* options */ });
          payments.create('taxId', { onChange: (e) => console.log(e) }).mount('#payments-taxId');
        </script>
        ```
      </CodeGroup>
    </div>
  </div>

  <div style={{ flex: "1 1 20rem", minWidth: 0 }}>
    <div data-whop-platform="web">
      <div data-whop-demo-shell style={{ position: "relative", minHeight: "320px", transition: "min-height 200ms ease" }}>
        <div data-whop-demo-skeleton style={{ position: "absolute", inset: "0", borderRadius: "12px", background: "rgba(140, 140, 140, 0.12)", pointerEvents: "none", transition: "opacity 200ms ease" }} />

        <div data-whop-demo-native="element:payments/taxId" data-whop-elements-version="" style={{ position: "relative" }} />
      </div>

      <p style={{ fontSize: "0.8125rem", opacity: 0.7 }}>Example data. [Open the Playground](/elements/upcoming/payments/overview#playground).</p>
    </div>

    <div data-whop-platform="react-native" style={{ display: "none" }}>
      <div style={{ width: "22rem", maxWidth: "100%" }}>
        <div data-whop-simulator-shell className="whop-ios-simulator" style={{ position: "relative", aspectRatio: "390 / 800", overflow: "hidden" }}>
          <iframe src={"https://app.revyl.ai/embed/713e08f7-91e6-4449-905b-ceb4c18b83c2?controls=0"} title="TaxIdElement running on Android, in the React Native example app" loading="lazy" allow="fullscreen; clipboard-read; clipboard-write" style={{ position: "absolute", inset: 0, width: "100%", height: "100%", border: 0, background: "transparent", display: "block" }} />
        </div>
      </div>
    </div>
  </div>
</div>

<div data-whop-platform="web">
  ## Props

  <ResponseField name="disabled" type="boolean">
    Disables both fields while the host saves the last committed registration. Defaults to `false`.
  </ResponseField>

  <ResponseField name="country" type="string">
    ISO 3166-1 alpha-2 buyer country used to preselect the type. `AR` selects `ar_cuit`, `LI` selects `li_uid`. EU and unmapped or empty values select `eu_vat`. Other mapped countries select their own first type. A manual selection persists across country changes. Defaults to `""`.
  </ResponseField>

  <ResponseField name="defaultValue" type="{ type: string; value: string; }">
    Initial `type` and `value` from a resumed session. An unsupported `type` falls back to the country default.
  </ResponseField>

  <ResponseField name="error" type="string">
    API validation error shown below the value input. Empty (default) shows nothing. Defaults to `""`.
  </ResponseField>

  ## Events

  Pass callbacks in the create options or React props.

  ### `onChange`

  Fires on blur or Enter when the value is nonempty, or when the type changes with a value present. Identical consecutive values fire once. Clearing the value doesn't fire. The host must clear stored registrations.

  **Signature:** `((payload: { taxId: { type: "ad_nrt" | "ao_tin" | "ar_cuit" | "al_tin" | "am_tin" | "aw_tin" | "au_abn" | "au_arn" | "eu_vat" | "az_tin" | "bs_tin" | "bh_vat" | "bd_bin" | "bb_tin" | "by_tin" | "bj_ifu" | "bo_tin" | "ba_tin" | "br_cnpj" | "br_cpf" | "bg_uic" | "bf_ifu" | "kh_tin" | "cm_niu" | "ca_bn" | "ca_gst_hst" | "ca_pst_bc" | "ca_pst_mb" | "ca_pst_sk" | "ca_qst" | "cv_nif" | "cl_tin" | "cn_tin" | "co_nit" | "cd_nif" | "cr_tin" | "hr_oib" | "do_rcn" | "ec_ruc" | "eg_tin" | "sv_nit" | "et_tin" | "eu_oss_vat" | "ge_vat" | "gh_tin" | "de_stn" | "gb_vat" | "gn_nif" | "hk_br" | "hu_tin" | "is_vat" | "in_gst" | "id_npwp" | "il_vat" | "jp_cn" | "jp_rn" | "jp_trn" | "kz_bin" | "ke_pin" | "kg_tin" | "la_tin" | "li_uid" | "li_vat" | "my_frp" | "my_itn" | "my_sst" | "mr_nif" | "mx_rfc" | "md_vat" | "me_pib" | "ma_vat" | "np_pan" | "nz_gst" | "ng_tin" | "mk_vat" | "no_vat" | "no_voec" | "om_vat" | "pe_ruc" | "ph_tin" | "pl_nip" | "ro_tin" | "ru_inn" | "ru_kpp" | "sa_vat" | "sn_ninea" | "rs_pib" | "sg_gst" | "sg_uen" | "si_tin" | "za_vat" | "kr_brn" | "es_cif" | "ch_uid" | "ch_vat" | "tw_vat" | "tj_tin" | "tz_vat" | "th_vat" | "tr_tin" | "ug_tin" | "ua_vat" | "ae_trn" | "us_ein" | "uy_ruc" | "uz_tin" | "uz_vat" | "ve_rif" | "vn_tin" | "zm_tin" | "zw_tin" | "sr_fin" | "xi_vat"; value: string; }; }) => void)`

  ### `onLoaderStart`

  Runs after the loading skeleton first paints and before `onReady`.

  **Signature:** `(() => void)`

  ### `onReady`

  Runs after the element's first complete paint.

  **Signature:** `(() => void)`

  ### `onError`

  Runs when the element fails to load or crashes. The fallback remains visible. Use `code` for programmatic handling. `sourceKey` identifies a failed host-state source.

  **Signature:** `((e: { message: string; code?: string | undefined; sourceKey?: string | undefined; }) => void)`

  ## Methods

  Call these on the handle returned by `create`, or through a React `ref`.

  ### `mount`

  Mounts the element in `target` and starts loading. React components mount themselves.

  **Signature:** `(target: string | HTMLElement) => void`

  ### `destroy`

  Removes the element and releases its frame and subscriptions. You can call it more than once. React removes the element automatically.

  **Signature:** `() => void`

  ### `update`

  Merges new props into the mounted element. In React, change the component props instead.

  **Signature:** `(options: Partial<TaxIdElementProps>) => void`

  ## Styling

  Style these parts through `appearance.classes`. Use camel case or kebab case for property names and include units. Page stylesheets can't reach the element's frame. The framework validates each declaration before injecting it.

  | Class                   | Targets                    |
  | ----------------------- | -------------------------- |
  | `.whop-TaxId`           | Tax registration root      |
  | `.whop-TaxIdError`      | API validation error       |
  | `.whop-TaxIdInput`      | Registration value input   |
  | `.whop-TaxIdLabel`      | Registration value label   |
  | `.whop-TaxIdTypeLabel`  | Registration type label    |
  | `.whop-TaxIdTypeSelect` | Registration type selector |

  ```ts theme={null}
  const payments = whop.payments.create({
    appearance: {
      classes: {
        'whop-TaxId': { borderRadius: '8px', fontWeight: '600' },
        'whop-TaxIdError': { borderRadius: '8px', fontWeight: '600' },
        'whop-TaxIdInput': { borderRadius: '8px', fontWeight: '600' }
      }
    }
  });

  // 6 classes use this shape
  payments.update({
    appearance: { classes: { 'whop-TaxId': { fontWeight: '700' } } }
  });
  ```

  In React, pass `appearance` to `<Payments>`. Set it globally with `WhopElements({ appearance })`.
</div>

<div data-whop-platform="react-native" style={{ display: "none" }}>
  ## Props

  <ResponseField name="country" type="string">
    ISO 3166-1 alpha-2 buyer country, used to preselect the type. `AR` selects `ar_cuit`; EU and unmapped values select `eu_vat`.
  </ResponseField>

  <ResponseField name="defaultValue" type="{ type: string; value: string }">
    Initial pair from a resumed session. An unsupported type falls back.
  </ResponseField>

  <ResponseField name="disabled" type="boolean">
    Disables both fields, for while you save the last committed registration.
  </ResponseField>

  <ResponseField name="error" type="string">
    A validation message from your API, shown below the value input.
  </ResponseField>

  <ResponseField name="onChange" type="(payload: TaxIdChangePayload) => void">
    Fires on every type or value change.
  </ResponseField>

  <ResponseField name="style" type="StyleProp<ViewStyle>">
    Applied to the element's outer `View`. For theming, prefer `appearance.parts` on the provider, which covers every element on this surface. Note the React Native part names are their own set today, not the web's `whop-*` class names, so a web appearance object does not port across unchanged.
  </ResponseField>

  <ResponseField name="fallback" type="ReactNode">
    Rendered instead of the built-in skeleton while the element loads.
  </ResponseField>

  <ResponseField name="onReady" type="() => void">
    Fires once the element is interactive. `<Payments>` groups these, so its own `onLoadingChange` is usually the one you want.
  </ResponseField>

  <ResponseField name="onError" type="(error: { message: string; code?: string }) => void">
    A load or configuration failure for this element. The element renders its own error face either way.
  </ResponseField>

  ## `TaxIdChangePayload`

  What `onChange` hands back:

  * `taxId: { type: TaxIdType; value: string }`: the selected registration type and what the buyer typed

  ## States

  Renders immediately. The type preselects from `country` and the value input validates against that type's format. `onChange` fires with the current pair.

  ## Good to know

  * `country` only preselects. A buyer who picks a type manually keeps it across country changes.
  * Pass `error` to show a message from your own API validation beneath the value input.
  * The element reports the pair. Sending it is yours: attach it to your own order or customer record.

  <Note>
    Wrap your app in `<WhopElements getToken={…}>` once, then mount `<Payments>` around the elements. See [Getting started](/elements/upcoming/getting-started) and [Appearance](/elements/upcoming/appearance).
  </Note>
</div>
