# Comedy Cellar · Monday Night · Bill Split

A static single-page app for splitting the bill from a Comedy Cellar outing on **Apr 6, 2026** (Table 166, 8 guests). The total was **$613.89** — tap what you ordered, see your share, and pay via Venmo.

## How to use

1. Open `index.html` in any browser (no server needed).
2. Tap each item you ordered — food, drinks, and/or your show ticket. Selected items highlight in amber.
3. A summary card at the bottom shows your items subtotal, your proportional tax and tip, and your grand total.
4. Tap **Pay Ilia on Venmo** — it opens Venmo with the amount and memo pre-filled.

## Bill-splitting logic

Tax and tip are split **proportionally** based on each person's item subtotal relative to the table's full subtotal.

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

## Venmo

Tapping the button opens:

```
https://venmo.com/ilia-kudryavtsev?txn=pay&amount=<YOUR_TOTAL>&note=Comedy%20Cellar%20Monday%20Apr%206%20🎤
```

The amount is calculated client-side and injected into the URL — no server, no data sent anywhere.
