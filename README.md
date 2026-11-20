# Parcel Desk · Dallas County foreclosure auction finder

**Live site: https://lexiloughlin.github.io/dallas-foreclosure/**

A single-page research tool for Dallas County, Texas foreclosure auctions. It pulls the public records a bidder needs into one place: county notice packets, a short watchlist of residential leads, and a worksheet for working out a maximum bid.

## What it does

- **County foreclosure notices.** A filterable index of 98 Dallas County Clerk notice packets (PDFs) for the March, April and May 2026 sales, covering 23 cities. Filter by city or month and open the county's own PDF.
- **Property research watchlist.** Research leads shown as cards with the 2026 county appraisal value, living area, year built and listed sale date. Search by address, city or ZIP, sort by city or value, and save properties to a watchlist.
- **Property detail.** Each lead links out to the third-party sale listing, the filed notice (when one has been found), the Dallas Central Appraisal District parcel page and a map.
- **Bid worksheet.** Enter expected resale price, repairs, closing/holding/selling costs and target profit. The page calculates a bid ceiling:

  ```
  bid ceiling = resale price − repairs − other costs − target profit
  ```

Saved properties and worksheet numbers are kept in your browser (localStorage), so they stay on your device and persist between visits.

## Data and its limits

| Data | Source | Status |
| --- | --- | --- |
| Notice packets | [Dallas County Clerk foreclosure index](https://www.dallascounty.org/government/county-clerk/recording/foreclosures.php) | Archive of March–May 2026 sales. These sale dates have passed, and a filing does not show whether a property sold. |
| Research leads | Third-party sale listings | Snapshot from Nov 19, 2026. Sale dates are unverified until confirmed against a county filing. Opening bids are unconfirmed. |
| Property values | [Dallas Central Appraisal District](https://www.dallascad.org/) | 2026 certified market value. |

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

- **Add a lead:** append an object to `rows` with the DCAD account `id`, `street`, `city`, `zip`, `value`, `area`, `year`, `source` and `sale` (YYYY-MM-DD). Add `notice` once the county filing is found.
- **Add county packets:** append `{ file, updated, month }` entries to `archive`. File names must match the county's PDF names exactly.

## Possible next steps

- Confirm each lead's sale date against a county filing and show verified vs. unverified on the card
- Add current-month packets alongside the archive
- Pull bedrooms and bathrooms per property instead of a fixed value in the detail view
- Map view of leads by city
