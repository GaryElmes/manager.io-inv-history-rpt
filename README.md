# Portfolio Valuation — a Manager.io extension

A custom button extension for [Manager.io](https://www.manager.io) that reports
the value of your investment portfolio **as at any chosen date**. It renders as
an expandable panel at the bottom of the Investments tab.

## What it does

Pick a date — by typing it, using the calendar picker, or via quick-jump chips
(today, last month end, recent financial year ends) — and the report shows, for
every investment:

- **Quantity held as at that date**, reconstructed from starting balances and
  every transaction that moved units (payments, receipts, journal entries,
  purchase and sales invoices)
- **Market price** — the latest Investment Market Price recorded on or before
  the date, with the price date shown and prices older than 35 days highlighted
- **Exchange rate** prevailing at the date for foreign-currency holdings, with
  the rate date shown
- **Value in your base currency**, plus each holding's percentage of the total

Also included: a control-account filter (appears only if you use more than one
control account for investments), sortable columns, an option to hide
zero-quantity holdings, and CSV export of whatever is displayed.

## Valuation method

Quantity as at date = starting balance + buys − sells up to and including the
date. Direction is taken from the **document type** (receipts and sales
invoices reduce units; payments and purchase invoices add them; journal entries
are signed per line by debit/credit). The sign you happened to type on the
quantity is ignored — this matches Manager's own treatment.

Value = quantity × latest market price on or before the date × exchange rate as
at the date. The rate comes from the market-price row's stamped exchange rate
where present, falling back to the Exchange Rates table. Holdings with units
but no price on or before the date are flagged and excluded from the total.

**Not included:** holdings tracked outside the Investments tab (e.g. special
accounts).

## Manager settings honoured

The report reads and applies your Manager settings at load:

- **Base currency** — code, decimal places, and prefix/suffix in the Value column
- **Date format** — in every displayed date, including the as-at field and CSV
- **Number format** — thousands and decimal separators throughout, including
  percentages

If a setting can't be read, the report falls back to the defaults in the CONFIG
block and says so in the load log.

## Install

1. Host `portfolio-valuation.html` anywhere reachable by your browser — or use
   this repository's GitHub Pages URL directly.
2. In Manager: **Settings → Custom Buttons → New**
   - Source: **URL**, pointing at the hosted file
   - Placement: `/investments`
3. Open the **Investments** tab and expand the panel at the bottom.

## Requirements

- Manager.io **26.x** (Desktop, Server, or Cloud) — the version that introduced
  the Custom Buttons hosting model and the api4 endpoints this extension uses.
  Note that api4 is still evolving; endpoints have been observed to change
  between releases. If a Manager update breaks something, the load log (bottom
  of the panel) names the failing endpoint, and all endpoint names live in the
  CONFIG block at the top of the file.

## Configuration

Open the file and edit the CONFIG block at the top if needed:

- `fyEnd` — balance date for the "FY end" quick-jump chips. Default is
  31 March (New Zealand); set your own month/day.
- `staleDays` — age at which a price is highlighted as stale (default 35).
- `baseCurrency` — fallback only; normally detected from your Manager settings.

## Privacy and security

This is a single self-contained HTML file with no external dependencies, no
analytics, and no calls to anything except your own Manager instance. It is
**read-only** — it never creates or modifies records. The source is short and
unminified; you're encouraged to read it before installing, as you should with
any extension.

## Troubleshooting

Expand **Load log** at the bottom of the panel. It reports the record count for
every data set and names any endpoint or settings fetch that failed, in plain
language. If quantities look wrong, check whether your investment units are
moved by a document type not listed in `CONFIG.txSources`.

## Verification

Before relying on the figures, spot-check a few holdings against the
Investments tab (set the date to today), and run a date you can verify against
existing workpapers. The report is only as good as the transactions and market
prices recorded in your books.

## Licence

MIT — see [LICENSE](LICENSE).
