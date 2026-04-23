# FRED US Macro Open Data

Open U.S. macro history and replay-ready event data sourced from FRED.

This repository publishes a redistributable subset of the U.S. macro dataset we built for replay and event analysis. It includes:

- full historical observations for the included FRED series
- replay-ready macro event records derived from those series
- a machine-readable series catalog
- a list of excluded series with source notes

## What's inside

- `data/fred-us-macro-history.json`
  - full historical observations for the included series
- `data/fred-us-macro-events.json`
  - replay-ready macro events derived from the same series
- `metadata/series-catalog.json`
  - series metadata, categories, frequencies, units, and source URLs
- `metadata/excluded-series.json`
  - series excluded from this public repo because their source notes contain copyright or redistribution restrictions
- `metadata/dataset-metadata.json`
  - generation metadata and dataset stats

## Current coverage

As of the latest refresh in this repo:

- 34 redistributable series
- 138,133 historical observations
- 21,918 replay-ready events
- oldest observation: 1913-01-01
- oldest event: 2019-01-01T13:30:00.000Z

## Why some series are excluded

FRED's API terms make clear that some series are owned by third parties and may carry their own copyright or redistribution restrictions. We therefore exclude series whose source notes explicitly include copyright or distribution restrictions.

Currently excluded:

- `UMCSENT` - University of Michigan: Consumer Sentiment
- `BAMLH0A0HYM2` - ICE BofA US High Yield Index Option-Adjusted Spread
- `MORTGAGE30US` - 30-Year Fixed Rate Mortgage Average in the United States

See `metadata/excluded-series.json` for the full notes and source URLs.

## Attribution

This dataset is sourced from FRED and the original data owners behind each series. When displaying or redistributing derived views, please keep source attribution. A safe default is:

`Source: Original series owner via FRED, Federal Reserve Bank of St. Louis`

For series-specific source URLs, see `metadata/series-catalog.json`.

## Data format

### History

`data/fred-us-macro-history.json` contains:

- `generatedAt`
- `historyStart`
- `sourceId`
- `sourceLabel`
- `stats`
- `series[]`
  - `id`
  - `label`
  - `labelZh`
  - `category`
  - `frequency`
  - `frequencyShort`
  - `observationStart`
  - `observationEnd`
  - `units`
  - `unitsShort`
  - `sourceUrl`
  - `observations[]`
    - `date`
    - `value`

### Events

`data/fred-us-macro-events.json` contains:

- `generatedAt`
- `sourceId`
- `sourceLabel`
- `stats`
- `events[]`
  - `id`
  - `createdAt`
  - `primaryCategory`
  - `text`
  - `textZh`
  - `url`
  - `metadata`
    - `seriesId`
    - `observationDate`
    - `releaseDate`
    - `value`
    - `previousValue`
    - `change`
    - `pctChange`

## Important note

This repository publishes a redistributable subset prepared from FRED data and excludes series with explicit redistribution restrictions that we detected in source notes. You are still responsible for checking the original source terms for your own use case.

## Upstream references

- FRED API overview: <https://fred.stlouisfed.org/docs/api/fred/overview.html>
- FRED API terms of use: <https://fred.stlouisfed.org/docs/api/terms_of_use.html>
- FRED legal terms: <https://fred.stlouisfed.org/legal/terms/>
