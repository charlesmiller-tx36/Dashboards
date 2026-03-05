# Wheatley HS Performance Dashboard

## Documentation (per AI Generated Data Analysis and Product Guidelines)

| Requirement | Detail |
|---|---|
| **Author** | Charles Miller |
| **Creation Date** | March 5, 2026 |
| **Purpose** | Evaluate Wheatley High School (Houston ISD, Campus #101912018) academic performance longitudinally (2011–2025) and in comparison to peer schools, to support internal policy analysis of school turnaround trajectories. |
| **Data Sources** | See detailed list below |
| **AI Usage** | Claude (Anthropic) via Cowork desktop application — used for data retrieval, extraction, analysis, and dashboard code generation |
| **Prompt Summary** | User directed Claude through a structured 4-step workflow: (1) explore available TEA data; (2) identify measurable performance aspects; (3) select which aspects to visualize; (4) develop and confirm a dashboard layout. A follow-up prompt requested extending STAAR longitudinal data from 3 years to 7 years. |
| **Update Notes** | **v1** (Mar 5, 2026): Initial 6-tab dashboard with 2023–2025 STAAR data. **v2** (Mar 5, 2026): Extended STAAR trends to 2018–2025 (7 years); added subject-level longitudinal chart and campus-vs-state gap chart. |

---

## Data Sources

All data used in this dashboard is **publicly available** from the Texas Education Agency (TEA).

| Source | URL | Description |
|---|---|---|
| TEA 2025 Statewide Summary (Excel) | https://tea.texas.gov/texas-schools/accountability/academic-accountability/performance-reporting/2025-accountability-rating-system | Campus-level A-F ratings and domain scores for all Texas campuses, 2011–2025. Used for overall ratings, domain scores, comparison group construction, and peer/state/district benchmarks. |
| TAPR 2024-25 (Wheatley HS) | https://rptsvr1.tea.texas.gov/cgi/sas/broker?_service=marern&_program=perfrept.perfmast.sas&prgopt=2025/tapr/tapr_srch.sas | STAAR EOC results (2024 & 2025), graduation rates, CCMR, attendance, chronic absenteeism, demographics, and staff data. |
| TAPR 2023-24 (Wheatley HS) | Same portal, prior year | STAAR EOC results (2023 & 2024), graduation, CCMR, attendance. |
| TAPR 2022-23 (Wheatley HS) | Same portal, prior year | STAAR EOC results (2022 & 2023). |
| TAPR 2021-22 (Wheatley HS) | Same portal, prior year | STAAR EOC results (2021 & 2022). |
| TAPR 2018-19 (Wheatley HS) | Same portal, prior year | STAAR EOC results (2018 & 2019). |

**Note:** 2020 STAAR data does not exist (testing cancelled due to COVID-19). 2020 and 2021 accountability ratings were "Not Rated: Declared State of Disaster."

---

## Comparison Groups

Two peer comparison groups were constructed from the TEA Statewide Summary:

1. **HISD High Schools** — All 42 high schools in Houston ISD rated in 2025.
2. **Custom Statewide Peer Group** — 37 high schools statewide matching Wheatley's demographic profile: 90%+ economically disadvantaged students, enrollment between 300–800 students.

State averages are drawn directly from the TEA Statewide Summary.

---

## Data Use Compliance

| Rule | Status |
|---|---|
| No identifiable individual-level data | Compliant — all data is campus-level aggregate |
| No HIPAA/FERPA protected data | Compliant — no student-level records used |
| No confidential administrative datasets | Compliant — all sources are public TEA reports |
| No internal restricted datasets | Compliant — no internal TX2036 data used |
| Data sources are public or aggregated | Compliant |

---

## Dashboard Contents

The dashboard (`wheatley_dashboard.html`) is a self-contained HTML file with embedded data and Chart.js visualizations. It contains six tabs:

1. **Big Picture** — Overall A-F rating trajectory (2011–2025), domain score radar chart, yearly domain scores
2. **STAAR Performance** — EOC pass rates at Approaches/Meets/Masters levels, subject-level longitudinal trends (2018–2025), campus-vs-state gap analysis
3. **School Progress & Growth** — Academic Growth and Relative Performance domain scores, year-over-year change
4. **Closing the Gaps** — Equity domain scores, subgroup performance comparisons
5. **Outcomes** — Graduation rates, CCMR rates, attendance, chronic absenteeism, SAT/ACT participation
6. **Peer Comparison** — Wheatley vs. HISD peers, statewide peers, and state averages across all domains

---

## Reproducibility

To recreate this dashboard, an analyst would need to:

1. Download the TEA Statewide Summary Excel from the TEA accountability page for the relevant year
2. Download TAPR reports for Wheatley HS (Campus #101912018) for each year of interest
3. Extract STAAR EOC results from each TAPR PDF (current year + prior year data are included in each report)
4. Filter the Statewide Summary for HISD high schools and the custom peer group criteria (90%+ econ disadvantaged, 300–800 students)
5. Embed the extracted data into the HTML dashboard template

All data transformations (PDF text extraction, Excel filtering, JSON conversion) were performed by Claude during the session and are reflected in the embedded JavaScript data constants within the dashboard file.

---

## Quality Assurance Notes

- JavaScript syntax validated via `node --check`
- 18 programmatic checks run on data structure integrity (all passed)
- STAAR percentages cross-referenced against source TAPR PDFs
- Domain scores cross-referenced against TEA Statewide Summary Excel
- **Author review of outputs for accuracy is required before external sharing**
