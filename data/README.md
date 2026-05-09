# Dataset

`scraped_news.csv` — 1,290 Spanish-language news articles labeled for veracity,
used as the training/evaluation corpus for the thesis *Spanish fake news
detection in media*.

## Schema

| Column      | Type   | Description                                   |
|-------------|--------|-----------------------------------------------|
| `TITULO`    | string | Article headline                              |
| `CORPUS`    | string | Full article body (plain text)                |
| `AUTOR`     | string | Author or outlet byline (may be empty)        |
| `FECHA`     | string | Publication timestamp, ISO-like; mixed offsets|
| `URL`       | string | Source URL                                    |
| `METADATA`  | string | Additional fields captured by the scraper     |
| `VERACIDAD` | enum   | `true`, `false`, or `satira`                  |

## Class distribution

| Label    | Count | Share |
|----------|------:|------:|
| `true`   |   433 | 33.6% |
| `satira` |   433 | 33.6% |
| `false`  |   424 | 32.9% |

The corpus is intentionally balanced across the three classes.

## Coverage

- **Articles**: 1,290
- **Unique bylines (`AUTOR`)**: 419 (152 marked `Desconocido`, 96 empty)
- **Dated articles (`FECHA` non-empty)**: 1,041 of 1,290 (249 missing)
- **Date range**: 1998 – 2025, with the bulk between 2017 – 2021
  (≈ 75% of dated articles fall in that window)

## Provenance

Articles were collected with the companion pipeline
[`fake-news-data-extraction/`](../fake-news-data-extraction), which combines
`newspaper3k` (article extraction), `BeautifulSoup` (custom selectors),
`snscrape` (social-media surfacing), and `gspread` (curation in Google Sheets).

