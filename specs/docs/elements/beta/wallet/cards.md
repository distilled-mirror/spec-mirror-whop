> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Cards

> An account's card surfaces, mounted from one place: the compact issued-card list, the full sortable roster, the spend chart, and a single revealable card. Mount the faces the page needs — they share the account and the credential this unit is minted with, so a page showing a chart above a roster wires them once.

<Info>This page documents `@whop/elements@1.0.0-beta.3` and `@whop/elements-react@1.0.0-beta.3`.</Info>

*Pre-release, not yet part of a stable release.*

Mounts inside [`Wallet`](/elements/beta/wallet/overview). Create it to get a handle, then mount its elements on that handle. Call `destroy()` to remove the sub-controller. Create it again to get a fresh handle.

## Preview

A live, interactive demo of this sub-controller's default arrangement with example data:

<div data-whop-demo-shell style={{ position: "relative", minHeight: "480px", transition: "min-height 200ms ease" }}>
  <div data-whop-demo-skeleton style={{ position: "absolute", inset: "0", borderRadius: "12px", background: "rgba(140, 140, 140, 0.12)", pointerEvents: "none", transition: "opacity 200ms ease" }} />

  <div data-whop-demo-native="unit:cards" data-whop-elements-version="1.0.0-beta.3" style={{ position: "relative" }} />
</div>

<div data-whop-usage="wallet/cards">
  <CodeGroup>
    ```tsx React theme={null}
    import { WhopElements, Wallet, Cards, CardsElement, CardsTableElement, CardsChartElement, WhopCardElement, CardTransactionsElement } from "@whop/elements-react";
    import { loadWhop } from "@whop/elements";

    function Example() {
      return (
        <WhopElements elements={loadWhop()}>
          <Wallet /* options */>
            <Cards>
              <CardsElement />
              <CardsTableElement />
              <CardsChartElement />
              <WhopCardElement />
              <CardTransactionsElement />
            </Cards>
          </Wallet>
        </WhopElements>
      );
    }
    ```

    ```html JavaScript theme={null}
    <script src="https://js.whop.cloud/elements/amber/elements.js" data-whop-elements></script>
    <script type="module">
      const wallet = window.WhopElements().wallet.create({ /* options */ });
      const cards = wallet.create('cards', { /* options */ });
      cards.create('cards').mount('#wallet-cards-cards');
      cards.create('cardsTable').mount('#wallet-cards-cardsTable');
      cards.create('cardsChart').mount('#wallet-cards-cardsChart');
      cards.create('whopCard').mount('#wallet-cards-whopCard');
      cards.create('cardTransactions').mount('#wallet-cards-cardTransactions');
    </script>
    ```
  </CodeGroup>
</div>

## Options

Pass these to `wallet.create('cards', { … })`, or as props on `<Cards>` in React. Parent-injected props never appear here.

<ResponseField name="accessToken" type="string">
  A scoped token for the card reads. Card rows need `payout:account:read`; cardholder names also need `company:authorized_user:read`. Omitted, the viewer's same-origin session is used, which only answers same-origin.
</ResponseField>

## Methods

Call these on the sub handle from `wallet.create('cards', { … })`.

### `update`

Merges new props and callbacks into the sub-controller.

**Signature:** `(options: Partial<CardsSubOptions>) => void`

### `destroy`

Destroys the sub-controller and its elements, then frees its exclusive slot. A later `create("cards")` starts fresh.

**Signature:** `() => void`

## Elements

The elements this sub-controller mounts. Each has its own page:

<CardGroup cols={2}>
  <Card title="CardsElement" href="/elements/beta/wallet/cards-cards">
    Lists the account's active issued cards, most recently issued first. Needs an `accessToken`. The title and rows are click targets that emit events instead of navigating — a host wires up its own routing and card-detail UI.
  </Card>

  <Card title="CardsTableElement" href="/elements/beta/wallet/cards-cardsTable">
    A sortable table of every issued card on an account, including cardholder, last month's spend, limit, and creation date. Rows and the create button emit events so the host owns card details and issuance flows.
  </Card>

  <Card title="CardsChartElement" href="/elements/beta/wallet/cards-cardsChart">
    A business account's card spend over time, using the same interactive bar chart as Whop's cards dashboard. The period picker changes the mounted chart and emits an event so the host can persist the selection.
  </Card>

  <Card title="WhopCardElement" href="/elements/beta/wallet/cards-whopCard">
    Renders one issued Whop Card. The element prefetches authorized secrets immediately after it mounts with a card ID, but keeps them masked until the viewer clicks it or selects `View details`. The API remains the permission authority. Includes lock and unlock controls by default.
  </Card>

  <Card title="CardTransactionsElement" href="/elements/beta/wallet/cards-cardTransactions">
    Every card transaction on an account, as the sortable table the dashboard shows: date, merchant, amount, cashback, status, card and cardholder, with filters for status, card and cardholder. A host can seed any of those three so the table opens on a narrowed view the reader can still widen. Card and cardholder narrow the read itself; search and status narrow the rows already loaded, so the count and total describe the current view rather than the account's lifetime. Selecting a row reports it — the element never opens a drawer of its own.
  </Card>
</CardGroup>
