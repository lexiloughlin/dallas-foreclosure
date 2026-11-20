# Parcel Desk · Dallas County foreclosure auction finder

**Live site: https://lexiloughlin.github.io/dallas-foreclosure/**

A single-page research tool for people looking to buy investment property at Dallas County, Texas foreclosure auctions. It puts the leads, the math and the auction rules in one place so an investor can set a walk-away price before sale day.

## What it does

- **Deals to size up.** Research leads shown as cards with the 2026 county appraisal value, price per square foot, living area, year built and listed sale date. Each card shows a screening price (70% of county value) as a rough starting point. Search by address, city or ZIP, sort by value or price per square foot, and save properties to a watchlist.
- **Run the numbers.** Each lead opens a deal analysis with two strategies:
  - *Fix & flip:* enter resale price, repairs, other costs and target profit to get a bid ceiling, with a bar showing where each resale dollar goes.

    ```
    bid ceiling = resale price − repairs − other costs − target profit
    ```
  - *Buy & rent:* enter your bid, repairs, rent, property tax, insurance and a vacancy/upkeep allowance to get monthly cash flow, cap rate, payback years and the 1% rule check. It assumes an all-cash purchase with no loan.

    ```
    cap rate = (yearly rent − vacancy & upkeep − property tax − insurance) ÷ (bid + repairs)
    ```
- **Auction guide.** The four steps from notice to keys, the next sale day (calculated from the first-Tuesday rule), where the sale is held, and the main risks: as-is sales, surviving liens, cancelled sales, and redemption rights after tax and HOA sales.
- **County foreclosure notices.** A filterable index of 98 Dallas County Clerk notice packets (PDFs) for the March, April and May 2026 sales, covering 23 cities.
- **Property detail.** Each lead links out to the third-party sale listing, the filed notice (when one has been found), the Dallas Central Appraisal District parcel page and a map.

Saved properties and both worksheets are kept in your browser (localStorage), so they stay on your device and persist between visits.

## Data and its limits

| Data | Source | Status |
| --- | --- | --- |
| Notice packets | [Dallas County Clerk foreclosure index](https://www.dallascounty.org/government/county-clerk/recording/foreclosures.php) | Archive of March–May 2026 sales. These sale dates have passed, and a filing does not show whether a property sold. |
| Research leads | Third-party sale listings | Snapshot from Nov 19, 2026. Sale dates are unverified until confirmed against a county filing. Opening bids are unconfirmed. |
| Property values | [Dallas Central Appraisal District](https://www.dallascad.org/) | 2026 certified market value. |

The auction rules in the guide come from [Texas Property Code ch. 51](https://statutes.capitol.texas.gov/Docs/PR/pdf/PR.51.pdf) and the County Clerk's foreclosure page.

The data is a hand-built snapshot, not a live feed. For current auctions, use the [county filing search](https://dallas.tx.publicsearch.us/) (choose **Foreclosures**, then filter by sale date or city). Tax sales run separately through the [sheriff's auction platform](https://dallas.texas.sheriffsaleauctions.com/).

This is a research aid, not legal or investment advice. Check the latest filing and sale terms before bidding.

## How it's built

One file, `index.html`: plain HTML, CSS and JavaScript with no build step, framework or dependencies. The data lives in two arrays at the top of the script (`archive` for county packets, `rows` for research leads). The masthead illustration is drawn on a `<canvas>`.

## Run it locally

```
git clone https://github.com/lexiloughlin/dallas-foreclosure.git
cd dallas-foreclosure
open index.html
```

## Updating the data

- **Add a lead:** append an object to `rows` with the DCAD account `id`, `street`, `city`, `zip`, `value`, `area`, `year`, `source` and `sale` (YYYY-MM-DD). Add `beds` and `baths` when known (the detail view leaves them out if missing), and `notice` once the county filing is found.
- **Add county packets:** append `{ file, updated, month }` entries to `archive`. File names must match the county's PDF names exactly.

## Possible next steps

- Add more leads (there are two so far)
- Confirm each lead's sale date against a county filing and show verified vs. unverified on the card
- Add current-month packets alongside the archive
- Map view of leads by city
