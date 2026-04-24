# FRED US Macro Core Data

Core U.S. macro history and replay-ready market event data sourced from FRED.

This repository now publishes the high-impact P1 macro dataset used for market replay. The older broad dataset whose event coverage effectively started in 2019 has been removed and replaced with full available history for the selected core releases.

## What's inside

- `data/fred-us-macro-history.json`
  - raw historical FRED observations for the included series
- `data/fred-us-macro-events.json`
  - replay-ready macro event records derived from the same series
- `metadata/series-catalog.json`
  - series metadata, transformed event value type, event counts, coverage, and source URLs
- `metadata/dataset-metadata.json`
  - generation metadata, dataset stats, and scope notes
- `metadata/excluded-series.json`
  - known third-party FRED series that are not shipped because their notes include copyright or redistribution restrictions

## Current coverage

Latest refresh in this repository:

- 15 core high-impact series
- 18,359 historical observations
- 11,986 replay-ready events
- oldest observation: 1913-01-01
- oldest event: 1914-01-01T13:30:00.000Z
- newest event: 2026-04-23T12:30:00.000Z
- 5,752 events with FRED vintage release dates
- 6,234 older events with approximate release dates

## Included core series

| FRED ID | Event value | Market use |
| --- | --- | --- |
| `DFEDTARU` | Federal funds target upper limit | Fed policy rate changes |
| `CPIAUCNS` | CPI year-over-year percent | Headline inflation |
| `CPILFENS` | Core CPI year-over-year percent | Core inflation |
| `PPIACO` | PPI year-over-year percent | Producer inflation |
| `PCEPI` | PCE price index year-over-year percent | Fed-preferred inflation trend |
| `PCEPILFE` | Core PCE year-over-year percent | Fed-preferred core inflation |
| `UNRATE` | Unemployment rate level | Labor market slack |
| `PAYEMS` | Nonfarm payroll monthly change | Employment growth |
| `ADPMNUSNERSA` | ADP employment monthly change | Private payroll preview |
| `CES0500000003` | Average hourly earnings month-over-month percent | Wage inflation |
| `ICSA` | Initial jobless claims level | High-frequency labor stress |
| `JTSJOL` | JOLTS job openings level | Labor demand |
| `GDP` | Nominal GDP quarter-over-quarter annualized percent | Growth |
| `GDPC1` | Real GDP quarter-over-quarter annualized percent | Real growth |
| `RSAFS` | Retail sales month-over-month percent | Consumption demand |

`DFEDTARU` is a daily source series, but this dataset only emits replay events when the policy rate changes. The noisy one-observation-per-day event stream is intentionally not published.

## Actual vs expected values

FRED provides observations and vintage/release metadata for these series, but it does not provide consensus forecast or expected-value fields. Event records therefore include actual values, previous values, changes, percentage changes, raw FRED observations, and release-date metadata. Forecast surprises can be added later only from a separate licensed or self-maintained expectations source.

## Data format

### History

`data/fred-us-macro-history.json` contains raw FRED observation history:

- `generatedAt`
- `historyStart`
- `sourceId`
- `sourceLabel`
- `stats`
- `series[]`
  - `id`
  - `label`
  - `labelZh`
  - `title`
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

`data/fred-us-macro-events.json` contains replay-ready records sorted newest first:

- `generatedAt`
- `sourceId`
- `sourceLabel`
- `stats`
- `events[]`
  - `id`
  - `createdAt`
  - `primaryCategory`
  - `text`
  - `textEn`
  - `textZh`
  - `url`
  - `metadata`
    - `seriesId`
    - `observationDate`
    - `releaseDate`
    - `releaseDateApproximate`
    - `rawValue`
    - `value`
    - `valueKind`
    - `valueUnit`
    - `previousValue`
    - `change`
    - `pctChange`

For transformed event series, `metadata.value` is the market-facing actual value used in replay. For example, CPI/PCE/PPI events use year-over-year percent, payroll events use period change, GDP events use annualized quarter-over-quarter percent, and `metadata.rawValue` preserves the original FRED observation.

## Release-date caveat

Where FRED vintage dates are available, event timestamps use those release dates with a market-standard release time. Older observations often do not have vintage release dates in FRED. Those records are still useful for long-run replay context, but they are marked with `metadata.releaseDateApproximate: true`.

## Attribution

This dataset is sourced from FRED and the original data owners behind each series. When displaying or redistributing derived views, please keep source attribution. A safe default is:

`Source: Original series owner via FRED, Federal Reserve Bank of St. Louis`

For series-specific source URLs, see `metadata/series-catalog.json`.

## Important note

This repository is a focused core macro dataset, not a complete mirror of FRED. You are still responsible for checking the original source terms for your own use case.

## Upstream references

- FRED API overview: <https://fred.stlouisfed.org/docs/api/fred/overview.html>
- FRED API terms of use: <https://fred.stlouisfed.org/docs/api/terms_of_use.html>
- FRED legal terms: <https://fred.stlouisfed.org/legal/terms/>
