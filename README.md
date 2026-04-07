# Comedy Cellar · Monday Night · Bill Split

A static single-page app for splitting the bill from a Comedy Cellar outing on **Apr 6, 2026** (Table 166, 8 guests). The total was **$613.89** — tap what you ordered, see your share, and pay via Venmo.

## How to use

1. Open `index.html` in any browser (no server needed).
2. On page load, one Show Ticket (NDP) is automatically pre-selected so you start with at least a $15 base charge.
3. Tap additional items you ordered — food, drinks, and/or more show tickets. Selected items highlight in amber.
4. Items that have already been claimed and paid for are greyed out with a strikethrough — you cannot select them.
5. A summary card at the bottom shows your items subtotal, your proportional tax and tip, and your grand total.
6. Tap **Pay Ilia on Venmo** — it opens Venmo with the amount and memo pre-filled.

## Bill-splitting logic

Tax and tip are split **proportionally** based on each person's item subtotal relative to the table's full subtotal. All calculations happen client-side — no server, no data sent anywhere.

| Receipt line | Amount   |
|--------------|----------|
| Subtotal     | $472.00  |
| Tax          | $41.89   |
| Tip          | $100.00  |
| **Total**    | **$613.89** |

For each person:

```
ratio     = your_items / $472.00
tax_share = ratio × $41.89
tip_share = ratio × $100.00
you_owe   = your_items + tax_share + tip_share
```

**Example:** You ordered a Chicken Kebab ($26) and a Tito's ($14) → $40 in items.

```
ratio     = 40 / 472  ≈ 0.0847
tax_share = 0.0847 × 41.89 ≈ $3.55
tip_share = 0.0847 × 100  ≈ $8.47
you_owe   = 40 + 3.55 + 8.47 = $52.02
```

## Claimed items

Once someone has paid for an item, it can be marked as claimed by adding `data-claimed="true"` to its `.item-row` element in `index.html`. Claimed items are:

- Dimmed to 38% opacity with a strikethrough on the item name
- Labelled with a small `claimed` badge
- Not selectable (clicking does nothing)

To un-claim an item (e.g. if there's a mistake), remove the `data-claimed="true"` attribute from the row.

## Auto-selected ticket

On page load, the first non-claimed `Show Ticket (NDP)` is automatically pre-checked. This ensures every visitor starts with at least a $15 charge showing rather than an empty selection.

## Venmo

Tapping the button opens:

```
https://venmo.com/ilia-kudryavtsev?txn=pay&amount=<YOUR_TOTAL>&note=Comedy%20Cellar%20Monday%20Apr%206%20🎤
```

The amount is calculated client-side and injected into the URL — no server, no data sent anywhere.
