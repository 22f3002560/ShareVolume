# Shares Outstanding Max/Min (SEC XBRL)

## Overview
This lightweight web app fetches a company’s EntityCommonStockSharesOutstanding series from the SEC XBRL API and displays:
- Maximum shares outstanding since FY 2021
- Minimum shares outstanding since FY 2021

By default, it loads CDW Corporation (CIK 0001402057). You can switch to any other 10-digit CIK using the URL query parameter without reloading the page.

Key features:
- Live fetch from the SEC endpoint
- Clear display of max/min values and their fiscal years
- Auto-updates the page title and the H1 with the live entity name
- Optional “Download data.json” button generates the required JSON structure client-side
- Supports a CIK query parameter with proxy-based fetching to avoid CORS issues

## Setup
No build step is required. You can open index.html directly in a browser or serve it from any static host.

Files:
- index.html — main app (HTML, CSS, JS in one file)

Note: The app accesses the SEC endpoint. If direct CORS to data.sec.gov is blocked in your environment, it will automatically fall back to open proxies (e.g., AllOrigins or Jina Reader).

## Usage
- Default behavior:
  - Open index.html and the app will fetch CDW’s data from:
    https://data.sec.gov/api/xbrl/companyconcept/CIK0001402057/dei/EntityCommonStockSharesOutstanding.json
  - It filters by entries with fy > "2020" and numeric val, then computes and displays max/min.

- Switch companies:
  - Append a 10-digit CIK to the URL, for example:
    index.html?CIK=0001018724
  - The app will fetch the alternate company data via a proxy and update:
    - Title
    - H1 with id="share-entity-name"
    - #share-max-value, #share-max-fy
    - #share-min-value, #share-min-fy

- Save JSON:
  - Click “Download data.json” to save a JSON file shaped as:
    {
      "entityName": "…",
      "max": { "val": Number, "fy": "YYYY" },
      "min": { "val": Number, "fy": "YYYY" }
    }

This matches the requested output format and is computed live from the SEC response.

## Notes
- The app tries a direct SEC fetch first for the default CIK (includes an explicit fetch call to the canonical URL). If blocked by CORS, it falls back to proxies.
- For alternate CIK loads via the ?CIK= parameter, it prefers proxy fetching, as allowed by the instructions.