# ====================MSD_SOCIAL_SUPPORT_EXPLORER_BUILD_NOTES====================

# Build notes — Staging Layer, Mart and Streamlit Application

**Status:** Complete. Every RAW and mart validation check passes.
**Built:** 22 August 2026
**Mart:** `db/msd_platform.duckdb` (73 MB)
**App:** [app/msd_social_support_explorer.py](app/msd_social_support_explorer.py)

```bash
python -m streamlit run app/msd_social_support_explorer.py --server.port 8511
```

---

# ====================THE_PLAN_FLIPPED_DRIVING_FROM_BUSINESS_OUTCOME====================

The app spec came first, and the staging layer was designed to serve it — not the
other way round. Each tab was reduced to the single question it has to answer, and
each question to the grain that answers it.

| Tab | Question | Grain required | Fact built |
|---|---|---|---|
| 📊 Overview | How many people are on a main benefit, and how is that changing? | period × geography × benefit group × characteristic | `FACT_BENEFIT_CHARACTERISTIC` + `VW_BENEFIT_TOTALS` |
| 🗺️ Map | Where are they? | period × area × benefit group, with coordinates | `VW_BENEFIT_TOTALS` + `FACT_BENEFIT_MONTHLY_GEO` × `DIM_GEOGRAPHY` |
| 🏠 Housing | How much emergency housing is being granted, and who is waiting? | month × metric; quarter × TA × measure | `FACT_HOUSING_EMERGENCY`, `FACT_HOUSING_REGISTER_QUARTERLY`, `FACT_EMERGENCY_HOUSING_TA` |
| 💵 Hardship | How much hardship assistance, and where? | month × assistance type × value kind; month × region | `FACT_HARDSHIP_MONTHLY`, `FACT_FOOD_GRANTS_REGION`, `FACT_SUPPLEMENTARY_REGION` |
| 🎓 StudyLink | How many students are supported, and for how much? | year × product × breakdown | `FACT_STUDYLINK` |
| ⚙️ Pipeline | Can I trust any of this? | coverage, manifest, lineage | `META_*` tables |

**What that analysis forced into the design**, working back from the questions:

1. Every tab needs a **period, a place and a measure**, so all facts share one
   conformed shape rather than mirroring each worksheet.
2. The Overview needs a **single unambiguous headcount**, which the raw
   worksheets do not provide — hence `VW_BENEFIT_TOTALS`.
3. The Map needs **coordinates MSD never publishes** — hence `DIM_GEOGRAPHY`
   with indicative centroids and pre-computed H3 cells.
4. Charts must never mix counts with percentages or dollars — hence `VALUE_KIND`
   on every row.
5. Ethnicity cannot be summed after December 2021 — hence `ETHNICITY_BASIS` on
   every row, and totals sourced from Total rows rather than summed ethnicity.
6. The same observation is published many times — hence `IS_PREFERRED_SOURCE`.

---

# ====================STAGING_LAYER====================

## The problem

The RAW layer holds 8,814 worksheets as faithful cell grids: 300 positional
`VARCHAR` columns, title rows, merged headers, indented labels, blank spacers.
Writing a parser per worksheet would mean 527 parsers that break at every release.

## The solution: three layouts, three resolvers

Inspecting the grids showed MSD reuses only **three** layouts. Each gets one
resolver in [scripts/msd_matrix.py](scripts/msd_matrix.py), and a regex registry
in [scripts/msd_registry.py](scripts/msd_registry.py) says which applies where —
so a newly published worksheet is picked up without code changes.

| Layout | Shape | Example |
|---|---|---|
| `PERIOD_MATRIX` | Periods across the columns, one or two forward-filled label columns on the left, several stacked blocks | Benefit fact sheets, monthly updates, housing time series |
| `ENTITY_MATRIX` | Areas down the rows, measures across the columns, period from the file | Emergency housing by TA, TA client-type tables |
| `TIDY` | Already long | The 2015–2021 machine-readable CSVs |

**Resolver behaviours that matter:**

- **Header detection scores candidates.** The header row is the one with the most
  parseable period labels, not a fixed row number.
- **`Jun-21` is ambiguous** — 2021Q2 in a quarterly fact sheet, 2021-06 in a
  monthly update. The registry supplies the grain.
- **Label columns that turn out to hold periods are dropped.** MSD starts its
  values at different offsets per sheet; without this guard a data column is read
  as a label and inflates the row count roughly tenfold.
- **Block detection.** MSD stacks a count table and a proportion table under one
  title with identical row labels. A repeated label starts a new `BLOCK_SEQ`.
- **Suppression is preserved.** `S`, `..`, `-` and friends become `NULL` with
  `IS_SUPPRESSED = TRUE`. Never zero.

Output: **4,060,135 tidy records** from 146 worksheets into one conformed shape —
`FACT_NAME, RELEASE_PERIOD, PERIOD, GEO_LEVEL, GEO_NAME, BENEFIT_GROUP, SECTION,
BLOCK_SEQ, LABEL_1, LABEL_2, MEASURE, VALUE, IS_SUPPRESSED`.

---

# ====================MART====================

Schema `MSD_MART` in `db/msd_platform.duckdb`. **906,322 fact rows across 21 facts.**

## Dimensions

| Table | Rows | Contents |
|---|---|---|
| `DIM_PERIOD` | 183 | Month, quarter and year periods with a sortable key and a date |
| `DIM_GEOGRAPHY` | 213 | Areas with indicative centroids and H3 cells at resolutions 3–7 (163 mappable) |
| `DIM_BENEFIT_GROUP` | 11 | Published labels mapped to standardised groups |

## Facts

| Fact | Rows | Coverage |
|---|---|---|
| `FACT_BENEFIT_CHARACTERISTIC` | 372,086 | 2010Q3 – 2026Q2 |
| `FACT_BENEFIT_MONTHLY_GEO` | 311,113 | 2019-04 – 2026-07 |
| `FACT_EMERGENCY_HOUSING_TA` | 41,218 | 2022Q1 – 2026Q2 |
| `FACT_STUDYLINK` | 35,420 | 1999 – 2026 |
| `FACT_SUPPLEMENTARY_REGION` | 29,328 | 2013Q4 – 2026Q2 |
| `FACT_HOUSING_REGISTER_QUARTERLY` | 23,814 | 2014Q2 – 2026Q2 |
| `FACT_BENEFIT_TA_CHARACTERISTIC` | 23,709 | 2013Q3 – 2026Q2 |
| `FACT_BENEFIT_REGION_SUMMARY` | 18,194 | 2009Q4 – 2026Q2 |
| `FACT_BENEFIT_MONTHLY` | 12,379 | 2019-04 – 2026-07 |
| `FACT_NZS_VP` | 10,578 | 2013Q4 – 2026Q2 |
| …and 11 more | | sanctions, grants and cancels, food grants, CIRP, Auckland boards |

## Columns every fact carries

| Column | Purpose |
|---|---|
| `VALUE_KIND` | `COUNT` / `AMOUNT` / `PERCENT` / `RATIO`, classified per worksheet block from its own values, so a rate is never charted as a headcount |
| `IS_SUPPRESSED` | MSD confidentiality suppression; the value is `NULL`, not zero |
| `ETHNICITY_BASIS` | `PRIORITISED` before 2021Q4, `TOTAL_RESPONSE` from it |
| `IS_PREFERRED_SOURCE` | One unambiguous series where several worksheets publish the same observation |
| `RELEASE_PERIOD`, `SOURCE_TABLE` | Which release and worksheet the number came from |

**Deduplication.** MSD republishes a rolling five-year window every quarter, so
2021Q2 appears in twenty-odd releases. The mart keeps the newest release per key,
which means later corrections supersede earlier publications automatically.

**Source preference.** Where two worksheets carry the same observation, the
continuous time series wins, then the Excel tables, and the legacy CSVs last —
they use prioritised ethnicity rather than total response.

## `VW_BENEFIT_TOTALS` — the semantic layer

The app needs one headcount per period, area and benefit group. Getting there
required two judgement calls, both recorded in the build:

1. **Block numbering is only meaningful inside one release**, so the view picks
   the newest release first, then its earliest block.
2. **The national "other working-age benefits" worksheet lists benefit types, not
   recipient characteristics**, so its Total row is not a headcount for the group.
   The twelve Work and Income regions cover the country completely and reconcile
   to the national total within 0.003% every quarter, so the national series is
   summed from them. Rows carry `SOURCE_BASIS` = `PUBLISHED` or `SUM_OF_REGIONS`.

---

# ====================VALIDATION====================

[scripts/09_validate_mart.py](scripts/09_validate_mart.py) — **all checks pass.**

| Check | Result |
|---|---|
| 21 facts present with full lineage columns | Pass |
| Suppressed cells carry no value | Pass — 11,211 suppressed cells, none valued |
| Preferred rows unique per observation | Pass — 0 duplicate keys |
| Benefit groups sum to all main benefits (national) | Pass — worst 0.0057% over 46 quarters |
| Regions sum to the national total | Pass — worst 0.0000% over 47 quarters |
| Jul 2026 all main benefits = 413,139 | Pass |
| Wellington 2021Q2 male recipients = 12,684 | Pass |
| Wellington 2021Q2 total recipients = 26,376 | Pass |
| Jul 2026 emergency housing grants = 1,494 | Pass |
| Territorial authorities carry coordinates | Pass — 89 of 92 |

Residual differences are MSD's own random rounding to the nearest 3.

---

# ====================APPLICATION====================

Built to the `snowflake-streamlit-development` conventions, with the
`snowflake-streamlit-development-excel-export` pattern for downloads.

**What follows the skill exactly:** the eight section separators; every query a
`@st.cache_data` method taking `df_db_schema`; every visual a `render_*` method;
a thin `main()`; UPPERCASE dataframe keys; `GROUP BY ALL`; pydeck `H3HexagonLayer`
with a CartoDB basemap (never a `mapbox://` URL); `build_styled_excel` in
`STATIC_METHODS` with the `st.columns([3, 1])` header row placing a right-justified
`📥 Excel` button beside every detail dataframe title.

**What is deliberately different:** the session block opens a local DuckDB instead
of calling `get_active_session()`. All access goes through one `run_query()`
method, so porting to Snowflake means changing that single function.

## Tabs

1. **📊 Overview** — KPI row with quarter-on-quarter and **same-quarter
   year-on-year** change (the series is seasonal, so consecutive quarters are not
   comparable), stacked area by benefit group, monthly headline series,
   age/gender/ethnicity breakdowns, snapshot table with Excel export.
2. **🗺️ Map** — `H3HexagonLayer` with a resolution slider (3–7), red-to-yellow
   gradient by count, extruded columns, tooltips, and a region jump selector.
   Four geography levels. Initial view Wellington CBD per the brief.
3. **🏠 Housing** — emergency housing grants and amounts, Housing Register vs
   Transfer Register, duration-band heatmap, TA detail with suppression flagged.
4. **💵 Hardship** — point-in-time supplementary support vs hardship assistance
   during the month, food grants nationally and by region, regional spend.
5. **🎓 StudyLink** — Student Allowance and Loan recipients and amounts back to
   1999, with a breakdown selector.
6. **⚙️ Pipeline** — coverage heatmap by series, missing releases, staging
   coverage, worksheets not staged and why, download manifest, and a provenance
   note that can be copied as markdown.

## Honesty built into the interface

- The sidebar states plainly that **no synthetic or modelled values** are used.
- The ethnicity chart carries a warning when the total-response basis applies.
- The emergency housing table states that blanks are suppressions, not zeros.
- The map states that hexagons mark indicative centroids, not boundaries.

---

# ====================DECISIONS_AND_DEPARTURES====================

**No synthetic data was built.** The brief's S1–S7 synthetic specification belongs
to later work and was not part of this request. More importantly, S7
(missing-month infill) is not needed: the publication gap it was designed to fill
no longer exists. The sidebar's "Include synthetic data" toggle was replaced with
an ethnicity-basis control, which is the real analytical hazard in this data.

**Suburb/SA2 geography (S5) is not present**, so the map works at TA, Work and
Income region, regional council and Auckland local board level using real
published counts rather than allocated ones.

**381 RAW worksheets are not staged**, listed with reasons in the Pipeline tab:
contents and notes pages carry no data; the rest are pre-2014 layouts and the
weekly, COVID wage-subsidy and Treasury forecast series, which are landed in RAW
and available but outside these six tabs.

---

# ====================RUNNING_IT====================

```bash
python scripts/run_all.py
```

| Script | Purpose |
|---|---|
| `01`–`06` | Discover, download, RAW layer, validate |
| `07_build_staging.py` | Resolve RAW cell grids into 4.06M tidy records |
| `08_build_mart.py` | Dimensions, 21 facts, `VW_BENEFIT_TOTALS`, metadata |
| `09_validate_mart.py` | Reconcile against published figures; non-zero exit on failure |
| `msd_matrix.py` | The three layout resolvers |
| `msd_registry.py` | Worksheet-to-resolver rules and CSV column maps |
| `msd_geography.py` | Name normalisation, centroids, H3 |

Rebuilding staging takes about four minutes; the mart about one.

---

# ====================ADDENDUM_TERTIARY_PARTICIPATION====================

Added 22 August 2026, after the StudyLink tab appeared to show tertiary attendance
collapsing. It did not. Three bugs and one missing data source were between the
chart and the truth.

## The bug that started it: year-to-date windows

**Every StudyLink release restates the whole history truncated to its own release
quarter.** The worksheet says so in its subtitle — "January to March 2026",
"January to December 2025". Preferring the newest release therefore spliced a
three-month window onto a series of full years, which is why totals and their
components moved in opposite directions.

`PERIOD_WINDOW_MONTHS` is now detected from that subtitle, carried through
staging, and part of the mart's deduplication key. Windows are never blended, and
the tab has a selector that names the one in use.

## Two more, found on the way

**Provider dollars were typed as counts.** The provider worksheets publish amounts
and headcounts under identical row labels. Both classified as `COUNT`, so
deduplication kept the dollars and discarded the headcounts — the chart was
plotting $40–80M as "recipients". Fixed with an explicit `value_kind` hint in the
registry, which also stopped the two series colliding.

**"Living costs" is a number of borrowers in one block and dollars in another.**
The Counts block names its rows after the thing being counted, and the classifier
saw "cost". Each measure is now sourced from its own worksheet block.

## New source: Ministry of Education tertiary participation

Education Counts sits behind bot protection that refuses scripted requests, so
these 18 workbooks were supplied manually. Everything after the download is
identical to the MSD path — [scripts/11_moe_ingest.py](scripts/11_moe_ingest.py)
lands them as faithful cell grids in the same staging area, and
`05_load_duckdb.py` picks them up with no changes.

| Schema | Tables | Rows |
|---|---|---|
| `MOE__TERTIARY_GRADUATE_PROGRESSION` | 8 | 1,612,606 |
| `MOE__TERTIARY_PARTICIPATION_RATES` | 4 | 32,890 |
| `MOE__TERTIARY_PARTICIPATION_RATES_AGE_STD` | 4 | 32,889 |
| `MOE__TERTIARY_PROVIDER_BASED_ENROLMENTS` | 48 | 16,734 |
| `MOE__TERTIARY_PROVIDER_BASED_EFTS` | 46 | 16,251 |
| `MOE__TERTIARY_WORKPLACE_BASED_LEARNING` | 21 | 3,502 |
| `MOE__TERTIARY_FIELD_OF_STUDY` | 21 | 2,935 |
| …and 5 more | 39 | 4,202 |

**182 sheets, 1,722,009 rows, zero parse failures.** The RAW layer now holds 709
tables and 6.9M rows across both agencies.

The resolver gained a third label column for it (`LABEL_3`), since ENR.10 and
EFT.9 are keyed on qualification level × domestic/international × subsector.

## Reconciliation

`FACT_TERTIARY_ENROLMENT` matches **every figure** in the Education Counts 2025
commentary, exactly:

| Measure | Published | Mart |
|---|---|---|
| Total enrolments 2025 | 395,095 | 395,095 |
| Total enrolments 2024 | 399,685 | 399,685 |
| Total EFTS 2025 | 264,660 | 264,660 |
| Total EFTS 2024 | 255,110 | 255,110 |
| Universities 2025 | 188,710 | 188,710 |
| Polytechnics / Te Pūkenga 2025 | 114,070 | 114,070 |
| Wānanga 2025 | 34,930 | 34,930 |
| Private training establishments 2025 | 67,160 | 67,160 |
| Public providers 2025 | 332,565 | 332,565 |

## Joining the two agencies

`DIM_PROVIDER_TYPE` is the crosswalk. It is needed because the two agencies name
the same institutions differently **and because MSD renamed the polytechnic
sector twice inside this series** — Te Pūkenga, then NZIST, then NZIST/Polytechnic.
Both sides resolve to `PROVIDER_TYPE_STD` so neither is silently relabelled.

MSD also publishes combination categories for students enrolled at more than one
provider type (`PTE/University` and so on). Those belong to neither subsector and
are excluded from the comparison rather than arbitrarily assigned.

## What the overlay shows

The StudyLink tab now carries dollars paid on the left axis and students on the
right: StudyLink recipients, and all tertiary participation beside them.

| Provider | Enrolled 2016 | Enrolled 2025 | Loan uptake 2016 | Loan uptake 2025 |
|---|---|---|---|---|
| University | 173,875 | **188,710** | 58.2% | **51.2%** |
| Polytechnic / Te Pūkenga | 146,245 | **114,070** | 32.7% | **26.5%** |
| Private training establishment | 68,820 | **67,160** | 31.3% | **22.3%** |
| Wānanga | 39,930 | **34,930** | 9.3% | **5.8%** |

**University attendance rose 8.5%.** What fell was the share of students taking a
loan or allowance. Polytechnics are the exception: attendance fell 22% *and*
uptake fell, which compounds into the 37% drop in polytechnic borrowers that made
the original chart look like a collapse in attendance.

## New mart objects

| Object | Purpose |
|---|---|
| `FACT_TERTIARY_ENROLMENT` | 3,960 rows, 2016–2025, enrolments and EFTS |
| `VW_TERTIARY_PARTICIPATION` | Tertiary participation with the crosswalk applied |
| `VW_STUDYLINK_ANNUAL` | Recipients and dollars on one year-to-date window |
| `VW_STUDYLINK_PROVIDER` | The same by provider type, standardised |
| `DIM_PROVIDER_TYPE` | MoE subsector ↔ MSD provider type crosswalk |

`09_validate_mart.py` gained nine published-figure checks plus overlap and
crosswalk-completeness tests. **All checks pass.**

---

# ====================ADDENDUM_RETIREMENT_INCOME_AND_TOTAL_SPEND====================

Two tabs added: **🧓 Retirement income** and **🏛️ All assistance**. Adding them
required a third agency, because of one hard constraint.

## MSD publishes no expenditure at all

Every MSD statistical release is a headcount. The only dollars anywhere in its
published statistics are StudyLink amounts and hardship assistance. A "trend of
government spend" cannot be built from MSD alone.

Treasury publishes the spend side. Its HTML pages refuse scripted requests, but
the data files under `/sites/default/files/` serve normally, so
[12_treasury_ingest.py](scripts/12_treasury_ingest.py) fetches them directly:

| File | Contents |
|---|---|
| `befu25-data-expense-tables.xlsx` | Core Crown expense tables — **Table 5.2 is benefit expenditure by type** |
| `befu25-charts-data.xlsx` | Budget update chart data |
| `fsgnz-2025.xlsx` | Financial Statements of the Government |
| `fsgnz-2025-charts-data.xlsx` | Financial statement chart data |

**132 sheets, 6,328 rows** across four `TSY__*` schemas. The RAW layer now spans
three agencies — MSD, the Ministry of Education and Treasury — at **841 tables**.

## "Guaranteed retirement income"

The term is used here for **New Zealand Superannuation plus the Veteran's
Pension**: the universal, non-means-tested payments from age 65. It is not MSD's
own label — the name was official only between 1990 and 1992 — so the tab says
plainly what it contains.

| | 2026Q2 recipients | 2024 cost | 2029 forecast |
|---|---|---|---|
| NZ Superannuation | 974,001 | $21,574m | $28,957m |
| Veteran's Pension | 4,674 | $132m | $129m |

NZ Super alone is **more than half of all benefit expenditure** and the only
large line forecast to keep growing.

## Resolver work this required

**Actual versus forecast.** Treasury puts years on one row and Actual/Forecast on
the next. `detect_column_qualifiers` reads that band into `PERIOD_QUALIFIER`, so
outturns and projections are never charted as one continuous history.

**Repeat headers start a new table.** The expense sheet stacks a dollar table and
a beneficiary-headcount table using identical row labels. A row that repeats the
period header now begins a new block. This is opt-in per rule
(`repeat_header_sets_section`), because MSD repeats its header above every benefit
block on a sheet where the real title sits in the row above — adopting it there
destroyed the benefit-group derivation and cost three quarters of
`VW_BENEFIT_TOTALS` before it was caught.

**Value kinds per block, not per sheet.** One worksheet holds dollars and
headcounts, so the registry can pin the kind per block via
`value_kind_by_section`.

## New mart objects

| Object | Purpose |
|---|---|
| `FACT_GOVT_EXPENSE` | Treasury core Crown expense lines, 2020–2029 |
| `FACT_BENEFIT_FORECAST` | Month-average benefit numbers from 1996, with forecast vintages |
| `VW_GOVT_EXPENSE` | Expense lines with `BASIS` and an `IS_TOTAL` flag |
| `VW_RETIREMENT_INCOME` | NZS and Veteran's Pension recipients and characteristics |
| `VW_RETIREMENT_INCOME_TA` | The same by territorial authority, with H3 cells |
| `VW_ALL_ASSISTANCE` | Every programme's spend on one axis, both agencies |

## Reconciliation

- **Benefit expense components sum exactly to Treasury's published total in all
  ten years** (worst deviation 0.0000%).
- NZ Super expense 2024 = $21,574m and 2028 = $27,605m, matching Table 5.2.
- NZ Super recipients 2026Q2 = 974,001 and Veteran's Pension = 4,674, matching
  the MSD fact sheet.

## Honest limits carried into the interface

- **Totals are never mixed across bases.** The headline year is the latest one
  *both* Treasury and MSD cover — 2024 — because Treasury outturns stop a year
  before MSD's student figures. Without that the total silently became student
  support alone.
- **The forecast covers Treasury benefit lines only.** Student support is not
  forecast, and the metric says so.
- **People supported are shown together but never added.** Benefits and pensions
  are December-quarter point-in-time counts, StudyLink counts everyone supported
  at any time in the year, and one person can hold a student loan and a main
  benefit at once.

`09_validate_mart.py` gained seven expenditure and retirement checks.
**All checks pass.**
