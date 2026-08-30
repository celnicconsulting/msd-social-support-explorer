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

**Licence status — verified 2026-08-30.** Every one of the 25 sourced datasets
was checked against its publisher online. All 25 now carry a named licence, and
every one is a clean attribution licence. Nothing remains unverified.

| | Meaning |
|---|---|
| *(no mark)* | Licence read from the dataset's own page or its own [data.govt.nz](https://catalogue.data.govt.nz) catalogue record |
| † | Licence read only from an agency-wide or site-wide statement, not one specific to this dataset — still flagged `licence_issue: true` |
| ⚠️ | Licence could not be established — **none remain** |

**15** datasets are `dataset_page`, **10** are `agency_record`, **0** are
unverified. See [How each licence was established](#how-each-licence-was-established).

### Ministry of Social Development

| Dataset | Publisher | Licence | Evidence read from | Source |
|---|---|---|---|---|
| Quarterly Benefit Fact Sheets | Ministry of Social Development | CC BY 4.0 | [data.govt.nz record](https://catalogue.data.govt.nz/dataset/benefit-fact-sheets-december-2020) | [MSD benefit statistics](https://www.msd.govt.nz/about-msd-and-our-work/publications-resources/statistics/benefit/index.html) |
| Monthly Benefits Update | Ministry of Social Development | CC BY 4.0 † | [MSD copyright statement](https://www.msd.govt.nz/about-msd-and-our-work/tools/copyright-statement.html) | [MSD monthly reporting](https://www.msd.govt.nz/about-msd-and-our-work/publications-resources/statistics/monthly-reporting/) |
| Monthly Housing Update, and the Housing and Transfer Registers | Ministry of Social Development | CC BY 4.0 | [data.govt.nz record](https://catalogue.data.govt.nz/dataset/social-housing-register-december-2020) | [MSD monthly housing reporting](https://www.msd.govt.nz/about-msd-and-our-work/publications-resources/statistics/housing/monthly-housing-reporting.html) |
| Emergency Housing Special Needs Grants by territorial authority | Ministry of Social Development | CC BY 4.0 † | [MSD copyright statement](https://www.msd.govt.nz/about-msd-and-our-work/tools/copyright-statement.html) | [MSD emergency housing](https://www.msd.govt.nz/about-msd-and-our-work/publications-resources/statistics/housing/emergency-housing.html) |
| StudyLink quarterly statistics (Student Allowance and Student Loan) | Ministry of Social Development (StudyLink) | CC BY 4.0 † | [MSD copyright statement](https://www.msd.govt.nz/about-msd-and-our-work/tools/copyright-statement.html) | [MSD StudyLink statistics](https://www.msd.govt.nz/about-msd-and-our-work/publications-resources/statistics/studylink/index.html) |
| Weekly income support update | Ministry of Social Development | CC BY 4.0 † | [MSD copyright statement](https://www.msd.govt.nz/about-msd-and-our-work/tools/copyright-statement.html) | [MSD weekly reporting](https://www.msd.govt.nz/about-msd-and-our-work/publications-resources/statistics/weekly-reporting/index.html) |
| COVID-19 wage subsidy statistics | Ministry of Social Development | CC BY 4.0 † | [MSD copyright statement](https://www.msd.govt.nz/about-msd-and-our-work/tools/copyright-statement.html) | [MSD COVID-19 wage subsidy releases](https://www.msd.govt.nz/about-msd-and-our-work/publications-resources/statistics/covid-19/who-received-the-covid-19-wage-subsidies-may-2022.html) |
| Summary of benefit forecasts (BEFU, HYEFU, PREFU vintages) | Ministry of Social Development | CC BY 4.0 † | [MSD copyright statement](https://www.msd.govt.nz/about-msd-and-our-work/tools/copyright-statement.html) | [MSD budget update statistics](https://www.msd.govt.nz/about-msd-and-our-work/publications-resources/statistics/befu/budget-economic-and-fiscal-update-2025.html) |
| Child, Youth and Family national and local level data (June 2017) | Ministry of Social Development (CYF, to April 2017) | **CC BY 3.0 NZ** | [data.govt.nz record](https://catalogue.data.govt.nz/dataset/child-youth-and-family-key-statistics) | [MSD CYF statistics](https://www.msd.govt.nz/about-msd-and-our-work/publications-resources/statistics/cyf/findings.html) |

### Ministry of Education (Education Counts)

All twelve workbook sets come from the same Education Counts tertiary
participation collection. Education Counts refuses scripted requests, so the
workbooks were supplied manually and no per-file URL was captured.

Licence evidence for all twelve is the Ministry of Education's own data.govt.nz
catalogue record for that collection —
[Tertiary participation](https://catalogue.data.govt.nz/dataset/tertiary-participation)
— whose record URL is exactly the source URL recorded for every entry below, and
whose `licence_id` is `CC-BY-4.0`.

| Dataset | Publisher | Licence | Evidence read from | Source |
|---|---|---|---|---|
| Provider-based enrolments (ENR.10) | Ministry of Education (Education Counts) | CC BY 4.0 | [data.govt.nz record](https://catalogue.data.govt.nz/dataset/tertiary-participation) | [Tertiary participation](https://www.educationcounts.govt.nz/statistics/tertiary-participation) |
| Provider-based equivalent full-time students (EFT.9) | Ministry of Education (Education Counts) | CC BY 4.0 | [data.govt.nz record](https://catalogue.data.govt.nz/dataset/tertiary-participation) | [Tertiary participation](https://www.educationcounts.govt.nz/statistics/tertiary-participation) |
| Provider-based enrolments and EFTS by field of study | Ministry of Education (Education Counts) | CC BY 4.0 | [data.govt.nz record](https://catalogue.data.govt.nz/dataset/tertiary-participation) | [Tertiary participation](https://www.educationcounts.govt.nz/statistics/tertiary-participation) |
| Graduate progression rates | Ministry of Education (Education Counts) | CC BY 4.0 | [data.govt.nz record](https://catalogue.data.govt.nz/dataset/tertiary-participation) | [Tertiary participation](https://www.educationcounts.govt.nz/statistics/tertiary-participation) |
| Tertiary participation rates, 2003–2025 | Ministry of Education (Education Counts) | CC BY 4.0 | [data.govt.nz record](https://catalogue.data.govt.nz/dataset/tertiary-participation) | [Tertiary participation](https://www.educationcounts.govt.nz/statistics/tertiary-participation) |
| Age-standardised tertiary participation rates, 2003–2025 | Ministry of Education (Education Counts) | CC BY 4.0 | [data.govt.nz record](https://catalogue.data.govt.nz/dataset/tertiary-participation) | [Tertiary participation](https://www.educationcounts.govt.nz/statistics/tertiary-participation) |
| Participation in workplace-based learning | Ministry of Education (Education Counts) | CC BY 4.0 | [data.govt.nz record](https://catalogue.data.govt.nz/dataset/tertiary-participation) | [Tertiary participation](https://www.educationcounts.govt.nz/statistics/tertiary-participation) |
| Targeted training programmes | Ministry of Education (Education Counts) | CC BY 4.0 | [data.govt.nz record](https://catalogue.data.govt.nz/dataset/tertiary-participation) | [Tertiary participation](https://www.educationcounts.govt.nz/statistics/tertiary-participation) |
| Secondary-Tertiary Alignment Resource (STAR) | Ministry of Education (Education Counts) | CC BY 4.0 | [data.govt.nz record](https://catalogue.data.govt.nz/dataset/tertiary-participation) | [Tertiary participation](https://www.educationcounts.govt.nz/statistics/tertiary-participation) |
| Adult and Community Education | Ministry of Education (Education Counts) | CC BY 4.0 | [data.govt.nz record](https://catalogue.data.govt.nz/dataset/tertiary-participation) | [Tertiary participation](https://www.educationcounts.govt.nz/statistics/tertiary-participation) |
| Te reo Māori language course enrolments, 2016–2025 | Ministry of Education (Education Counts) | CC BY 4.0 | [data.govt.nz record](https://catalogue.data.govt.nz/dataset/tertiary-participation) | [Tertiary participation](https://www.educationcounts.govt.nz/statistics/tertiary-participation) |
| Language course enrolments, 2016–2025 | Ministry of Education (Education Counts) | CC BY 4.0 | [data.govt.nz record](https://catalogue.data.govt.nz/dataset/tertiary-participation) | [Tertiary participation](https://www.educationcounts.govt.nz/statistics/tertiary-participation) |

### The Treasury

The publication pages carrying these four workbooks were located during
verification, closing the gap left by the build (which fetched the files
straight from `/sites/default/files/`). Neither page could be read: every
`treasury.govt.nz` HTML page, the copyright page included, returns HTTP 403 to
scripted requests. All four therefore rest on the Treasury's site-wide
statement.

| Dataset | Publisher | Licence | Evidence read from | Source | Publication page |
|---|---|---|---|---|---|
| Budget Economic and Fiscal Update 2025 — core Crown expense tables | The Treasury | CC BY 4.0 † | [Treasury copyright and licensing](https://www.treasury.govt.nz/copyright-and-licensing) | [befu25-data-expense-tables.xlsx](https://www.treasury.govt.nz/sites/default/files/2025-05/befu25-data-expense-tables.xlsx) | [BEFU 2025](https://www.treasury.govt.nz/publications/efu/budget-economic-and-fiscal-update-2025) |
| Budget Economic and Fiscal Update 2025 — chart data | The Treasury | CC BY 4.0 † | [Treasury copyright and licensing](https://www.treasury.govt.nz/copyright-and-licensing) | [befu25-charts-data.xlsx](https://www.treasury.govt.nz/sites/default/files/2025-05/befu25-charts-data.xlsx) | [BEFU 2025](https://www.treasury.govt.nz/publications/efu/budget-economic-and-fiscal-update-2025) |
| Financial Statements of the Government of New Zealand 2025 | The Treasury | CC BY 4.0 † | [Treasury copyright and licensing](https://www.treasury.govt.nz/copyright-and-licensing) | [fsgnz-2025.xlsx](https://www.treasury.govt.nz/sites/default/files/2025-10/fsgnz-2025.xlsx) | [FSGNZ, year ended 30 June 2025](https://www.treasury.govt.nz/publications/year-end/financial-statements-2025) |
| Financial Statements of the Government of New Zealand 2025 — chart data | The Treasury | CC BY 4.0 † | [Treasury copyright and licensing](https://www.treasury.govt.nz/copyright-and-licensing) | [fsgnz-2025-charts-data.xlsx](https://www.treasury.govt.nz/sites/default/files/2025-10/fsgnz-2025-charts-data.xlsx) | [FSGNZ, year ended 30 June 2025](https://www.treasury.govt.nz/publications/year-end/financial-statements-2025) |

All datasets were retrieved on **2026-08-22**. Licences were verified on
**2026-08-30**.

---

## How each licence was established

**† means the licence came from an agency-wide statement, not from this dataset.**
Those rows keep `licence_issue: true` in
[DATA_SOURCES.yaml](DATA_SOURCES.yaml). The licence is very probably right — the
agency says so for its whole site, and sibling datasets in the same series carry
it explicitly — but it was inherited, not asserted for that dataset in
particular. Anyone redistributing should confirm it.

None of the three publishers puts a licence statement in the body of its
statistics pages. All three rely on a single site-wide copyright page. That is
why [data.govt.nz](https://catalogue.data.govt.nz) catalogue records, which
carry an explicit `licence_id` per dataset, are the stronger evidence wherever
one exists — and why 15 of the 25 could be settled at dataset level.

### The three site-wide statements

**Ministry of Social Development** —
[Copyright, disclaimer and privacy statements](https://www.msd.govt.nz/about-msd-and-our-work/tools/copyright-statement.html):

> "this copyright material is licensed for re-use under Creative Commons
> Attribution (CC-BY) 4.0 International Licence"

Linking https://creativecommons.org/licenses/by/4.0/, and excluding logos,
emblems, trademarks, photography, imagery and site design elements.

**Education Counts** —
[Copyright, legal & privacy](https://www.educationcounts.govt.nz/site-info/privacy):

> "The copyright material on the Education Counts website is protected by
> copyright owned by Ministry of Education \[on behalf of the Crown] or its
> licensors" and "is licensed for re-use under the Creative Commons Attribution
> 4.0 International licence"

That page returns HTTP 403 to non-browser clients and could not be fetched
directly; the wording above was read from the search index, and it agrees with
the Ministry of Education's data.govt.nz records, which were the primary
evidence used.

> **Do not substitute `education.govt.nz` for this.** The Ministry of
> Education's main site carries a *non-commercial* licence — "Material on this
> website is licensed for re-use under the Creative Commons Attribution-Non
> Commercial 4.0 New Zealand licence" — which does **not** apply to Education
> Counts. Education Counts is CC BY 4.0, commercial use included.

**The Treasury** —
[Copyright and licensing](https://www.treasury.govt.nz/copyright-and-licensing):

> "Most material on the Treasury website is protected by copyright owned by the
> Treasury on behalf of the Crown, and unless indicated otherwise for specific
> items or collections of content, this Crown copyright material is licensed for
> re-use under the Creative Commons Attribution 4.0 International licence"

Every `treasury.govt.nz` HTML page returns HTTP 403 to scripted requests, this
one included, so the wording was read from the search index. It is corroborated
in data.govt.nz, where every dataset published by The Treasury carries
`licence_id` `CC-BY-4.0` — including the directly equivalent chart-and-data
releases from earlier BEFU and HYEFU vintages, and the Month End Financial
Statements.

### MSD: 3.0 NZ or 4.0?

**Both, split by vintage.** The current MSD site-wide statement is **CC BY 4.0
International**, not CC BY 3.0 NZ. The 3.0 NZ finding is real but historic: in
MSD's own data.govt.nz records, everything of 2017 vintage or earlier carries
`licence_id` `CC-BY-NZ-3.0` (benefit fact sheets and social housing register up
to December 2017, the CYF key statistics, the Social Report 2016, the Better
Public Services results), while every record from **March 2018 onward** carries
`CC-BY-4.0`. MSD relicensed at the 3.0 NZ → 4.0 International transition; files
of 2017 vintage still carry the older mark.

This matters for exactly one dataset here. Everything this repository publishes
from the benefit fact sheets is 2019 onward and so is CC BY 4.0. The legacy
**Child, Youth and Family** series is 2017 and is **CC BY 3.0 NZ**.

### Child, Youth and Family: who publishes it now

The 2017 files in this repository are MSD's, licensed **CC BY 3.0 NZ** per their
own catalogue record, which carries the note *"CYF was part of MSD until April
2017"* and has not been touched since June 2017.

The care and protection function transferred to **Oranga Tamariki — Ministry for
Children** in April 2017, and Oranga Tamariki is the publisher of any current
equivalent — reports of concern, further assessments, family group conferences,
and entries to and children in care and protection custody — at
[Statistics about how we work with children](https://www.orangatamariki.govt.nz/about-us/information-releases/statistics-about-how-we-work-with-children/)
and [Safety of Children in Care](https://www.orangatamariki.govt.nz/about-us/performance-and-monitoring/safety-of-children-in-care/).

Oranga Tamariki's [copyright statement](https://www.orangatamariki.govt.nz/about-this-site/copyright/) reads:

> "Unless indicated otherwise, this copyright material is licensed for re-use
> under Creative Commons Attribution 3.0 New Zealand Licence."

So the successor series is CC BY 3.0 NZ as well. **No Oranga Tamariki data is
used in this repository** — only the legacy 2017 MSD files.

### Still blocked

- **educationcounts.govt.nz** — HTTP 403 to every non-browser client, including
  `robots.txt`. Resolved through the Ministry of Education's data.govt.nz
  catalogue record instead, which is dataset-level evidence and stronger.
- **treasury.govt.nz HTML** — HTTP 403 to every scripted request: the copyright
  page, the BEFU 2025 page and the FSGNZ 2025 page alike. Resolved at agency
  level only, which is why all four Treasury rows are marked †.

No bot protection was evaded in either case.

---

Source data © the named publishers, used under the licences recorded in
[DATA_SOURCES.yaml](DATA_SOURCES.yaml) — CC BY 4.0
(https://creativecommons.org/licenses/by/4.0/) for 24 of the 25 datasets, and
CC BY 3.0 NZ (https://creativecommons.org/licenses/by/3.0/nz/) for the legacy
2017 Child, Youth and Family series. Attribution does not imply endorsement.
