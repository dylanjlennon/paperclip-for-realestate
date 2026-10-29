---
name: market-report
description: Produce a sourced weekly market snapshot for the agent's farm area from recent sales, listings, price changes and inventory.
metadata:
  provenance:
    - P-014
---

# Weekly Market Snapshot

## Inputs

Whatever the owner has connected: MLS/IDX export, county recorder or assessor data, GIS parcels,
or a CSV the owner provides. List every source used, with its pull date.

## Metrics (last 7 days, with 4-week and 12-month comparison)

1. New listings and total active inventory
2. Closed sales and median sale price
3. Median days on market
4. Share of listings with a price cut, and the median cut
5. Sale-to-list price ratio
6. Months of inventory = active listings divided by closed sales per month

## Method

- Compute each metric from rows, and show the sample size next to it.
- If a sample is under 10 sales, label the number "small sample" and do not headline it.
- Separate property types (single-family, condo/townhome) when the sample allows.
- Distinguish list price from sold price everywhere.

## Output

1. **Three takeaways** in plain English, each tied to one number
2. **Table** of the metrics above with week-over-week change
3. **One notable listing or sale**, chosen by data (largest cut, fastest sale), not opinion
4. **Sources and caveats** footer with dates

Hand the result to the Content Editor. Do not interpret the numbers as advice to buy or sell.
