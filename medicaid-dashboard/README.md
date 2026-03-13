# Central Texas Medicaid & Uninsured Dashboard

## Documentation (per AI-Generated Data Analysis and Product Guidelines)

| Requirement | Detail |
|---|---|
| **Author** | Charles Miller, Texas 2036 |
| **Creation date** | March 2, 2026 |
| **Purpose** | Visualize Medicaid/CHIP enrollment trends, uninsured population estimates, and safety-net coverage for six Central Texas counties (Travis, Williamson, Hays, Burnet, Caldwell, Bastrop), with special emphasis on children. Built to inform healthcare policy discussions for AARO, Austin Healthcare Council, and Texas 2036 with data-rich, nonpartisan analysis. |
| **Data sources** | HHSC Medicaid/CHIP enrollment (county monthly Jan 2022–Nov 2025; statewide monthly Sep 2014–Dec 2025; SFY 2022 & 2024 annual finals); Census SAHIE uninsured estimates (2006–2023, all 6 counties); Census ACS 1-year B27001 (2012–2024, 4 counties); CMS ACA Open Enrollment PUFs (2020–2025, all 6 counties); Central Health MAP (FY2024). All sources are publicly available, aggregated data — no individual-level or protected data is used. |
| **AI usage** | Claude (Anthropic) via Cowork / Claude Code — used for data acquisition (Census API pulls, HHSC Excel file parsing), dashboard HTML/CSS/JS generation, Chart.js visualization code, and data integration. All outputs were reviewed by the author for accuracy. |
| **Prompt summary** | Iterative build over multiple sessions: (1) Define scope — 6 counties, child focus, dual-audience branding; (2) Download and parse HHSC enrollment Excel files, pull Census SAHIE and ACS data via API, extract CMS ACA PUFs; (3) Build single-file HTML dashboard with Chart.js and embedded data; (4) Create 6-tab layout — Overview, Medicaid/CHIP Enrollment, ACA Marketplace, SAHIE Uninsured Trends, ACS Demographic Detail, Local Programs; (5) Extend historical data ranges (SAHIE back to 2006, ACS back to 2012, statewide HHSC/CHIP back to Sep 2014); (6) Add statewide historical context chart and SFY 2022 vs. 2024 county comparison; (7) Implement THEME_CONFIG for swappable AARO/TX2036 branding. |
| **Update notes** | **v1** (Mar 2, 2026): Initial 4-tab dashboard with HHSC county enrollment, SAHIE uninsured trends, ACS demographic detail, and Overview KPIs. **v2** (Mar 2, 2026): Added ACA Marketplace and Local Programs tabs (6-tab layout); integrated CHIP data; added ACA and CHIP KPI cards to Overview. **v3** (Mar 5, 2026): Extended historical data — SAHIE back to 2006, ACS back to 2012, statewide HHSC/CHIP back to Sep 2014. Added statewide Medicaid & CHIP trend chart with COVID PHE and unwinding annotations. Added SFY 2022 vs. SFY 2024 county comparison chart. |

---

## Data Use Compliance

This dashboard uses only publicly available, aggregated data. No identifiable individual-level data, HIPAA/FERPA-protected data, confidential administrative datasets, or internal restricted datasets are included. All data sources are listed above and linked in the dashboard footer.

---

## Dashboard Tabs

1. **Overview** — KPI cards and high-level trends: total Medicaid/CHIP enrollment, children's uninsured rates (SAHIE + ACS), ACA plan selections, enrollment change since PHE unwinding.
2. **Medicaid/CHIP Enrollment** — Monthly county trends (Jan 2022–Nov 2025) with risk group breakdowns; statewide historical context (Sep 2014–Dec 2025) with COVID PHE and unwinding annotations; SFY 2022 vs. SFY 2024 county comparison.
3. **ACA Marketplace** — County-level plan selections (2020–2025), age distribution, metal tier breakdowns. Plan selections do not equal effectuated enrollment.
4. **Uninsured: Regional Trends (SAHIE)** — Annual rates for all 6 counties. Under 65: 2006–2023; Under 19: 2014–2023. Margin of error available 2019–2023 only.
5. **Uninsured: Demographic Detail (ACS)** — Age-band breakdowns (2012–2024, no 2020) for Travis, Williamson, Hays, Bastrop. Burnet and Caldwell excluded (population below 65K threshold for ACS 1-year estimates).
6. **Local Programs** — Central Health MAP (~126K enrolled, FY2024, Travis County) and CIHCP information (all 6 counties).

---

## Data Caveats

- **SAHIE:** Model-based estimates; margin of error available 2019–2023 only; 2024 data expected mid-2026.
- **ACS 1-year:** Not available for 2020 (COVID disruption); requires county population of 65K+; Burnet and Caldwell excluded.
- **HHSC county monthly:** Only available Jan 2022 onward (HHSC retention policy); some months missing.
- **CHIP county-level:** Only in quarterly prelim and annual SFY files, not monthly county files.
- **ACA:** Plan selections at open enrollment, not equivalent to year-round effectuated coverage.

---

## Technical Details

- **Format:** Single-file HTML (index.html, ~8 MB with expanded embedded data); plus `uninsured_children_map.html` standalone map visualization
- **Dependencies:** Chart.js 4.5.1 + date-fns adapter (loaded via CDN)
- **Data:** All datasets embedded as JavaScript objects within the HTML file
- **Theming:** Swappable branding via `THEME_CONFIG` object and CSS custom properties
- **Browser support:** Chrome, Firefox, Safari, Edge (modern versions)
