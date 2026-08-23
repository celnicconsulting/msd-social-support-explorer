# MSD Social Support Explorer

**A working demonstration of flipping the data team — building backwards from the
decision, not forwards from the source system.**

Built by [Celnic Consulting](https://www.linkedin.com/company/celnic-consulting).
Live app: **https://celnic-msd.streamlit.app**

---

## What this is

Every figure in this application is **published New Zealand government data**.
No synthetic, modelled or interpolated values appear anywhere.

| Source | What it provides |
|---|---|
| **Ministry of Social Development** | Benefit, housing, hardship and StudyLink statistics, 2010–2026 |
| **Ministry of Education** (Education Counts) | Tertiary participation: enrolments and EFTS, 2016–2025 |
| **The Treasury** | Core Crown expense tables — the only source of expenditure, actual and forecast to 2029 |

The full platform ingests **1,278 MSD files, 18 Education extracts and 4 Treasury
workbooks** into a raw layer of 841 tables and 6.9M rows, resolves them into a
conformed mart, and validates every figure against the agencies' own published
totals. This repository carries a trimmed extract of that mart so the app can run
publicly for free.

---

## The point: the plan was flipped

The conventional order is source system → warehouse → model → "now what can we
show?". This was built the other way round. Each tab was reduced to the single
question it has to answer, and each question to the grain that answers it. Only
then was the staging layer designed.

That inversion forced six decisions that a source-first build would have missed:

1. Every tab needs a **period, a place and a measure**, so all facts share one
   conformed shape rather than mirroring each worksheet.
2. The Overview needs a **single unambiguous headcount**, which the published
   worksheets do not provide — hence a semantic view that derives one.
3. The map needs **coordinates no agency publishes** — hence a geography dimension
   with indicative centroids and pre-computed H3 cells.
4. Charts must never mix counts with percentages or dollars — hence a
   `VALUE_KIND` on every row, classified from the data rather than the wording.
5. Ethnicity cannot be summed after December 2021 — hence an `ETHNICITY_BASIS`
   flag on every row.
6. The same observation is published many times — hence an `IS_PREFERRED_SOURCE`
   flag so one series wins, deterministically.

## What driving from the outcome actually caught

Working backwards surfaced defects that a source-first pipeline would have
loaded faithfully and charted wrongly:

- **Year-to-date windows spliced together.** Every StudyLink release restates its
  whole history truncated to its release quarter. Preferring the newest release
  grafted a three-month window onto a series of full years, so totals and their
  components moved in opposite directions.
- **Dollars typed as counts.** Provider worksheets publish amounts and headcounts
  under identical row labels; deduplication kept the dollars and silently
  discarded the headcounts.
- **Two pensions collapsing into one.** New Zealand Superannuation and the
  Veteran's Pension share row labels on the same sheet, so one gender split
  survived at random and roughly half the territorial-authority figures were the
  wrong pension.
- **Filename collisions destroying data.** MSD reuses file names across periods;
  keying local storage on period plus filename overwrote one file of each pair.

Each was caught by reconciling against the agency's own published totals, not by
inspection.

---

## Reading the numbers

- **Suppression is not zero.** Agencies withhold low counts for confidentiality.
  Those cells are held as NULL with an `IS_SUPPRESSED` flag, never charted as zero.
- **Ethnicity changed basis in December 2021** — prioritised before, total
  response after. Total-response columns must not be summed: a person counts in
  every group they identify with.
- **Compare quarters year-on-year, not consecutively.** Benefit numbers are
  seasonal.
- **Counts are randomly rounded** by MSD, so components may not sum exactly to
  totals. Regional sums reconcile to national within 0.01%.
- **Map hexagons mark indicative centroids**, not area boundaries. No agency
  publishes coordinates with these statistics.
- **Outturns and forecasts are never mixed.** Treasury years are flagged Actual or
  Forecast and charted separately.

---

## Running it locally

```bash
pip install -r requirements.txt
streamlit run app/msd_social_support_explorer.py
```

The app reads a bundled DuckDB file, so there is nothing to configure. Point it
elsewhere with the `MSD_DB_PATH` environment variable if you want.

## About the data in this repository

`data/msd_platform_public.duckdb` is a 20 MB extract of the full 88 MB mart:
every dimension and semantic view intact, the large facts trimmed to 2019 onward,
and StudyLink's full 1999–2026 history kept. The period controls are built from
the series they filter, so nothing offers a period the extract cannot answer.

## Portability

The application follows Snowflake Streamlit conventions — the same section
structure, `@st.cache_data` query methods, `render_*` visual methods and
`GROUP BY ALL`. All database access goes through a single `run_query()` function,
so moving it to Streamlit in Snowflake means changing that one function.

---

## Disclaimer

Celnic Consulting is **not affiliated with, endorsed by, or acting for** the
Ministry of Social Development, the Ministry of Education or the Treasury. This
is an independent demonstration built entirely from their public releases. Figures
are reproduced from published sources and reconciled against them, but for any
official purpose consult the agencies directly:

- [MSD statistics](https://www.msd.govt.nz/about-msd-and-our-work/publications-resources/statistics/)
- [Education Counts](https://www.educationcounts.govt.nz/statistics/tertiary-participation)
- [The Treasury](https://www.treasury.govt.nz/publications/efu/budget-economic-and-fiscal-update-2025)

## Licence

Application code is MIT licensed — see [LICENSE](LICENSE). The underlying data
remains the property of the publishing agencies and is used under their published
terms.
