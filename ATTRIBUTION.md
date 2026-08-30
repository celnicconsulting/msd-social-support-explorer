# Attribution

This repository demonstrates a data-platform build method. It is **not**
an official publication of any agency named below, and no agency has
endorsed it. Data has been **modified** (downloaded, staged, transformed,
and reshaped from the publishers' worksheet layouts into a conformed mart) —
treat every figure as untrusted demonstration output.

No synthetic or modelled values appear in this platform. Every figure traces to
a published file recorded in `META_DOWNLOAD_MANIFEST` inside the DuckDB extract.
The one set of non-agency values is the indicative map centroids in
`DIM_GEOGRAPHY`, authored here because no agency publishes coordinates alongside
these statistics; they position a marker and never change a count.

## Source datasets

**Licence status:** no licence is recorded for any dataset anywhere in this
repository or in the pipeline that built it, and none was verified from the
publishers' pages. Every row below is therefore marked ⚠️ **unverified** rather
than asserted as CC BY 4.0. See [Licence to confirm](#licence-to-confirm).

### Ministry of Social Development

| Dataset | Publisher | Licence | Source |
|---|---|---|---|
| Quarterly Benefit Fact Sheets | Ministry of Social Development | ⚠️ Unverified (expected CC BY 4.0) | [MSD benefit statistics](https://www.msd.govt.nz/about-msd-and-our-work/publications-resources/statistics/benefit/index.html) |
| Monthly Benefits Update | Ministry of Social Development | ⚠️ Unverified (expected CC BY 4.0) | [MSD monthly reporting](https://www.msd.govt.nz/about-msd-and-our-work/publications-resources/statistics/monthly-reporting/) |
| Monthly Housing Update, and the Housing and Transfer Registers | Ministry of Social Development | ⚠️ Unverified (expected CC BY 4.0) | [MSD monthly housing reporting](https://www.msd.govt.nz/about-msd-and-our-work/publications-resources/statistics/housing/monthly-housing-reporting.html) |
| Emergency Housing Special Needs Grants by territorial authority | Ministry of Social Development | ⚠️ Unverified (expected CC BY 4.0) | [MSD emergency housing](https://www.msd.govt.nz/about-msd-and-our-work/publications-resources/statistics/housing/emergency-housing.html) |
| StudyLink quarterly statistics (Student Allowance and Student Loan) | Ministry of Social Development (StudyLink) | ⚠️ Unverified (expected CC BY 4.0) | [MSD StudyLink statistics](https://www.msd.govt.nz/about-msd-and-our-work/publications-resources/statistics/studylink/index.html) |
| Weekly income support update | Ministry of Social Development | ⚠️ Unverified (expected CC BY 4.0) | [MSD weekly reporting](https://www.msd.govt.nz/about-msd-and-our-work/publications-resources/statistics/weekly-reporting/index.html) |
| COVID-19 wage subsidy statistics | Ministry of Social Development | ⚠️ Unverified (expected CC BY 4.0) | [MSD COVID-19 wage subsidy releases](https://www.msd.govt.nz/about-msd-and-our-work/publications-resources/statistics/covid-19/who-received-the-covid-19-wage-subsidies-may-2022.html) |
| Summary of benefit forecasts (BEFU, HYEFU, PREFU vintages) | Ministry of Social Development | ⚠️ Unverified (expected CC BY 4.0) | [MSD budget update statistics](https://www.msd.govt.nz/about-msd-and-our-work/publications-resources/statistics/befu/budget-economic-and-fiscal-update-2025.html) |
| Child, Youth and Family national and local level data (June 2017) | Ministry of Social Development | ⚠️ Unverified (expected CC BY 4.0) | [MSD CYF statistics](https://www.msd.govt.nz/about-msd-and-our-work/publications-resources/statistics/cyf/findings.html) |

### Ministry of Education (Education Counts)

All twelve workbook sets come from the same Education Counts tertiary
participation collection. Education Counts refuses scripted requests, so the
workbooks were supplied manually and no per-file URL was captured.

| Dataset | Publisher | Licence | Source |
|---|---|---|---|
| Provider-based enrolments (ENR.10) | Ministry of Education (Education Counts) | ⚠️ Unverified (expected CC BY 4.0) | [Tertiary participation](https://www.educationcounts.govt.nz/statistics/tertiary-participation) |
| Provider-based equivalent full-time students (EFT.9) | Ministry of Education (Education Counts) | ⚠️ Unverified (expected CC BY 4.0) | [Tertiary participation](https://www.educationcounts.govt.nz/statistics/tertiary-participation) |
| Provider-based enrolments and EFTS by field of study | Ministry of Education (Education Counts) | ⚠️ Unverified (expected CC BY 4.0) | [Tertiary participation](https://www.educationcounts.govt.nz/statistics/tertiary-participation) |
| Graduate progression rates | Ministry of Education (Education Counts) | ⚠️ Unverified (expected CC BY 4.0) | [Tertiary participation](https://www.educationcounts.govt.nz/statistics/tertiary-participation) |
| Tertiary participation rates, 2003–2025 | Ministry of Education (Education Counts) | ⚠️ Unverified (expected CC BY 4.0) | [Tertiary participation](https://www.educationcounts.govt.nz/statistics/tertiary-participation) |
| Age-standardised tertiary participation rates, 2003–2025 | Ministry of Education (Education Counts) | ⚠️ Unverified (expected CC BY 4.0) | [Tertiary participation](https://www.educationcounts.govt.nz/statistics/tertiary-participation) |
| Participation in workplace-based learning | Ministry of Education (Education Counts) | ⚠️ Unverified (expected CC BY 4.0) | [Tertiary participation](https://www.educationcounts.govt.nz/statistics/tertiary-participation) |
| Targeted training programmes | Ministry of Education (Education Counts) | ⚠️ Unverified (expected CC BY 4.0) | [Tertiary participation](https://www.educationcounts.govt.nz/statistics/tertiary-participation) |
| Secondary-Tertiary Alignment Resource (STAR) | Ministry of Education (Education Counts) | ⚠️ Unverified (expected CC BY 4.0) | [Tertiary participation](https://www.educationcounts.govt.nz/statistics/tertiary-participation) |
| Adult and Community Education | Ministry of Education (Education Counts) | ⚠️ Unverified (expected CC BY 4.0) | [Tertiary participation](https://www.educationcounts.govt.nz/statistics/tertiary-participation) |
| Te reo Māori language course enrolments, 2016–2025 | Ministry of Education (Education Counts) | ⚠️ Unverified (expected CC BY 4.0) | [Tertiary participation](https://www.educationcounts.govt.nz/statistics/tertiary-participation) |
| Language course enrolments, 2016–2025 | Ministry of Education (Education Counts) | ⚠️ Unverified (expected CC BY 4.0) | [Tertiary participation](https://www.educationcounts.govt.nz/statistics/tertiary-participation) |

### The Treasury

| Dataset | Publisher | Licence | Source |
|---|---|---|---|
| Budget Economic and Fiscal Update 2025 — core Crown expense tables | The Treasury | ⚠️ Unverified (expected CC BY 4.0) | [befu25-data-expense-tables.xlsx](https://www.treasury.govt.nz/sites/default/files/2025-05/befu25-data-expense-tables.xlsx) |
| Budget Economic and Fiscal Update 2025 — chart data | The Treasury | ⚠️ Unverified (expected CC BY 4.0) | [befu25-charts-data.xlsx](https://www.treasury.govt.nz/sites/default/files/2025-05/befu25-charts-data.xlsx) |
| Financial Statements of the Government of New Zealand 2025 | The Treasury | ⚠️ Unverified (expected CC BY 4.0) | [fsgnz-2025.xlsx](https://www.treasury.govt.nz/sites/default/files/2025-10/fsgnz-2025.xlsx) |
| Financial Statements of the Government of New Zealand 2025 — chart data | The Treasury | ⚠️ Unverified (expected CC BY 4.0) | [fsgnz-2025-charts-data.xlsx](https://www.treasury.govt.nz/sites/default/files/2025-10/fsgnz-2025-charts-data.xlsx) |

All datasets were retrieved on **2026-08-22**.

## Licence to confirm

Every dataset above carries `licence_issue: true` in
[DATA_SOURCES.yaml](DATA_SOURCES.yaml), for one reason: **the licence was never
recorded.** The pipeline captured each file's URL, size, checksum and download
date, but no licence field — and this attribution pass was run offline, so no
publisher page was read to fill the gap.

New Zealand government statistical releases are normally licensed CC BY 4.0
under NZGOAL, and that is the expectation for all three publishers. It remains
an expectation. Before anyone relies on this repository as a redistribution of
that data, the licence should be confirmed on each publisher's own page:

- [MSD statistics](https://www.msd.govt.nz/about-msd-and-our-work/publications-resources/statistics/)
- [Education Counts](https://www.educationcounts.govt.nz/statistics/tertiary-participation)
- [The Treasury](https://www.treasury.govt.nz/publications/efu/budget-economic-and-fiscal-update-2025)

Two entries need more than a page check:

- **Child, Youth and Family national and local level data** is a legacy series;
  the function transferred to Oranga Tamariki in 2017, so the publisher of any
  current equivalent differs from the publisher of these 2017 files.
- **Treasury data files** were fetched directly from `/sites/default/files/`
  because the Treasury's HTML pages refuse scripted requests, so the publication
  page carrying the licence statement was never read.

Nothing has been removed on account of this. It is a flag, not a deletion.

---

Source data © the named publishers, used under the terms recorded in
[DATA_SOURCES.yaml](DATA_SOURCES.yaml) — expected to be CC BY 4.0
(https://creativecommons.org/licenses/by/4.0/), unconfirmed. Attribution does
not imply endorsement.
