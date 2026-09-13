# Crude Oil Price Monitor — Power BI

A Power BI dashboard that tracks WTI and Brent crude oil benchmark prices, refreshed automatically every day from the U.S. Energy Information Administration's (EIA) public API. No manual updates required.

Live dashboard: https://bit.ly/4xW6zrc

## Overview

Monitoring benchmark crude prices is a routine task in the oil and gas industry. This project automates that tracking end-to-end: data ingestion, transformation, modeling, and a scheduled daily refresh, published as a live, publicly viewable Power BI report.

Built as a way to combine over a decade of petroleum engineering experience with hands-on data analytics skills, reading the market the way the industry does, while automating the tracking so the focus stays on interpretation, not data entry.

## What it shows

- Current price for WTI and Brent
- Day-over-day percent change for each benchmark
- Brent-WTI spread, often a more telling signal than either price alone
- Historical trend line comparing WTI vs Brent over time

## Data source

U.S. EIA API v2, free and public, no scraping involved:

- WTI (Cushing, series RWTC)
- Brent (Europe, series RBRTE)

Endpoint pattern:

```
https://api.eia.gov/v2/petroleum/pri/spt/data/?api_key=YOUR_API_KEY&frequency=daily&data[0]=value&facets[series][]=RWTC&sort[0][column]=period&sort[0][direction]=desc&length=5000
```

An EIA API key is free to request at https://www.eia.gov/opendata/register.php. Do not commit your key, see Setup below.

## How it's built

- Ingestion: Power BI connected directly to the EIA API via Power Query (Get Data, Web)
- Transformation: cleaned and typed in Power Query, Date (Date), Price_USD_Barrel (Decimal Number), Type (WTI/Brent), appended into one Crude_Prices table
- Modeling (DAX measures): Latest Date, WTI Current Price, Brent Current Price, WTI Previous Price, Brent Previous Price, WTI % Change, Brent % Change, and Brent-WTI Spread (using a VAR/RETURN pattern to avoid referencing a measure directly inside a CALCULATE filter)
- Visuals: multi-row card for KPIs, line chart of WTI vs Brent over a relative date window
- Theme: custom color theme (crude-oil-monitor-theme.json in this repo), gold and navy palette
- Automation: published to the Power BI Service with a scheduled daily refresh (10:00 AM America/Caracas)
- Sharing: published to web, generating a public, no-login-required link

## Setup

To reproduce this dashboard:

1. Get a free EIA API key: https://www.eia.gov/opendata/register.php
2. Open the .pbix in Power BI Desktop
3. Go to Transform data, Manage Parameters, and set the EIA_API_Key parameter to your own key
4. Refresh the data
5. Apply the included theme: View, Themes, Browse for themes, select crude-oil-monitor-theme.json
6. Publish to your own Power BI workspace and configure a scheduled refresh

## Repository contents

| File | Description |
|---|---|
| crude-oil-monitor-theme.json | Custom Power BI color theme used in the report |
| README.md | This file |

## Author

Ing. MSc. Gledys Chavez, petroleum engineer (PDVSA, WAHA Oil Company) turned data analytics practitioner.

## License

No license, all rights reserved. Feel free to reference the approach; please ask before reusing the code or content directly.
