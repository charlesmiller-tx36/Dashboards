# The Texas Growth Story — GDP Dashboard

## Author
Charles Miller, Texas 2036

## Creation Date
March 3, 2026

## Purpose
Visualize Texas's GDP growth from 2000 to 2024 in both absolute and relative terms, telling the story of the state's explosive economic expansion. The dashboard answers several related questions: How large is the Texas economy compared to peer states? How has its share of national GDP changed over 24 years? Is the growth accelerating? And which states have gained or lost ground in the national picture?

The dashboard was developed for internal exploration and potential use in policy communications around Texas economic performance.

## Data Sources

| Dataset | Source | Access |
|---------|--------|--------|
| SAGDP1 — State Annual Gross Domestic Product Summary | U.S. Bureau of Economic Analysis (BEA) | Public; accessed February 23, 2026 |

All data is current-dollar (nominal) GDP in millions of dollars, covering all 50 states and the United States total for 2000–2024. The 2024 figures are BEA preliminary annual estimates. Data was downloaded via the BEA Interactive Data Application and provided as an Excel workbook (`State GDP Data_2.23.26.xlsx`).

No individual-level, protected, or restricted data is used. All data is publicly available from BEA.

## AI Usage
- **AI Tool:** Claude (Anthropic), via Claude Cowork desktop application
- **Model:** Claude Opus 4.6
- **Rendering:** Self-contained HTML file using Chart.js 4.5.1, chartjs-plugin-datalabels 2.2.0

## Prompt Summary
The dashboard was built through an iterative conversation with Claude:

1. **Initial request:** User asked for a scatter chart plotting 2024 state GDP (x-axis) against change in national GDP share from 2000 to 2024 (y-axis). Claude sourced BEA data and produced a static matplotlib chart.
2. **Dashboard concept:** User requested a comprehensive set of data visualizations telling the story of Texas's growth — in absolute size, relative share, and acceleration — including animated charts. Claude proposed a seven-panel interactive dashboard.
3. **Build:** User approved the proposal. Claude extracted data from the user's uploaded BEA spreadsheet (`State GDP Data_2.23.26.xlsx`), computed derived metrics (GDP shares, year-over-year growth, CAGR, share redistribution), and generated a single self-contained HTML dashboard with embedded data and Chart.js visualizations.

**Derived calculations include:**
- State share of U.S. GDP by year: `State GDP / Sum of 50 States GDP`
- Share change: `2024 share − 2000 share` (in percentage points)
- CAGR: `(GDP_2024 / GDP_2000)^(1/24) − 1`
- Year-over-year growth: `GDP_year − GDP_(year−1)` (in millions $)
- Average state growth: `U.S. YoY growth / 50`

**Note on denominator:** GDP shares use the sum of 50 states as the denominator (excluding DC and territories) to keep shares internally consistent, since DC is excluded from the state-level scatter and ranking charts.

## Update Notes

| Date | Change |
|------|--------|
| March 3, 2026 | Initial version created and pushed to GitHub |
