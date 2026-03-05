# District Relative Performance Dashboard — 2025

## Author
Charles Miller, Texas 2036

## Creation Date
February 26, 2025 (initial version); last updated March 5, 2025

## Purpose
This dashboard visualizes TEA Domain 2b (Relative Performance) accountability data for Texas public school districts. It enables users to explore how districts perform relative to peers with similar demographics, compare RP scores against raw STAAR outcomes, and filter by district type, ESC region, county, and state legislative district (House and Senate).

The dashboard supports internal policy analysis on school accountability, particularly the question of whether TEA's Relative Performance metric effectively adjusts for economic disadvantage — and how districts perform when that adjustment is applied versus when it is not.

## Data Sources

| Source | Description | Access |
|--------|-------------|--------|
| TEA Accountability Reports (Domain 2b) | Relative Performance scores, STAAR Component scores, CCMR scores, % Economically Disadvantaged for each district | Public — fetched via `rptsvr1.tea.texas.gov` using the `marykay` SAS broker service |
| TEA Texas School Directory 2025 (`tsd_2025.xlsx`) | District names, CDNs, ESC region assignments, county associations | Public — downloaded from TEA website |
| U.S. Census Bureau TIGER/Line Shapefiles (2025) | Texas State House district boundaries (`tl_2025_48_sldl`), State Senate district boundaries (`tl_2025_48_sldu`), Unified School District boundaries (`tl_2025_48_unsd`) | Public — downloaded from `census.gov` |
| Texas Legislature Online / Texas Senate | 89th Legislature member rosters (House and Senate) for legislator name labels on filters | Public — `capitol.texas.gov`, `senate.texas.gov` |
| Texas 2036 Brand Standards | Visual identity guide (colors, fonts, logo) applied to dashboard styling | Internal |

All data used in the dashboard is publicly available, aggregated at the district level, and contains no individual student data.

## AI Usage
This dashboard was built using **Claude** (Anthropic) via Claude Desktop (Cowork mode). Claude was used for:

- Writing all HTML, CSS, and JavaScript code (Chart.js visualizations, filter logic, data table)
- Fetching and parsing TEA accountability data from the SAS broker service (1,106 districts)
- Building the school district → state legislative district crosswalk using spatial intersection of TIGER/Line shapefiles (pyshp + Shapely)
- Performing linear regression and residual-based performance band calculations
- Matching TEA district names to Census shapefile names (normalization + manual alias resolution)
- Applying TX2036 brand standards from a PDF visual identity guide

## Prompt Summary
The dashboard was developed iteratively across multiple sessions. High-level prompt sequence:

1. **Initial build**: "Create an interactive dashboard visualizing TEA Domain 2b Relative Performance data for all Texas school districts" — including scatter plots, histogram, sortable table, and KPI cards.
2. **Branding**: "Apply Texas 2036 brand standards from this PDF" — colors (#3A4A9F blue, #F26852 orange), Montserrat font, SVG logo.
3. **Regression + bands**: "Add guide-lines to the scatter charts — hybrid approach with best-fit regression line and shaded performance bands based on standard deviations from median."
4. **STAAR chart treatment**: "Apply the same hybrid regression + band treatment to the STAAR vs. Econ chart."
5. **Terminology**: "Change 'expected' to 'median' framing across both charts."
6. **Legislative filters**: "Add state House and Senate district filters with many-to-many mapping — one ISD can appear in multiple legislative districts. Exclude charters from legislative mapping."
7. **Crosswalk construction**: Built from TIGER/Line shapefiles using polygon intersection (not centroids) after confirming no pre-built Census crosswalk exists for state legislative districts.
8. **Legislator names**: "Add the first initial and last name of each legislator next to the district number in the dropdowns" — sourced from 89th Legislature rosters.
9. **Data recovery**: "Fetch missing districts using the TEA URL-hack method" — recovered 243 additional districts (38 from district-level pages, 205 from campus-level pages for single-campus districts), bringing total from 863 to 1,106.

## Update Notes

| Date | Change |
|------|--------|
| 2025-02-26 | Initial dashboard created with 863 districts, 5 chart types, sortable table, type/ESC/county/search filters |
| 2025-02-26 | Applied TX2036 brand standards (colors, fonts, logo, footer) |
| 2025-02-26 | Added hybrid regression + performance band visualization to RP vs. Econ and STAAR vs. Econ scatter charts |
| 2025-02-26 | Changed "Expected" terminology to "Median" across all charts and tooltips |
| 2025-02-27 | Built school district → state legislative district many-to-many crosswalk from TIGER shapefiles (967 non-charter districts mapped) |
| 2025-02-27 | Added House District (HD 1–150) and Senate District (SD 1–31) dropdown filters |
| 2025-03-05 | Added 89th Legislature member names to House and Senate dropdown options |
| 2025-03-05 | Recovered 243 missing districts by re-fetching from TEA using alternate URL pattern; dashboard now covers 1,106 districts |
| 2025-03-05 | Mapped all new districts to ESC regions using TEA Texas School Directory |
| 2025-03-05 | Created this README per TX2036 AI-Generated Data Analysis and Product Guidelines |

## Key Methodological Notes

**Regression and performance bands**: Both scatter charts use ordinary least squares (OLS) linear regression. Performance bands are defined at ±0.33 and ±1.0 standard deviations of residuals from the regression line, creating five tiers: Well Above Median, Above Median, Near Median, Below Median, Well Below Median.

**Legislative crosswalk**: Uses actual polygon intersection (not centroids) between school district and legislative district boundaries. A school district is mapped to a legislative district if their polygons share non-zero area overlap. Boundary-only touches are excluded. Charter schools are excluded from legislative mapping.

**Single-campus districts**: 205 districts are classified by TEA as "single campus" and only report accountability data at the campus level. For these districts, the campus-level RP score is used as the district score, since the single campus IS the district.

**Still missing**: 123 districts remain without RP scores — primarily districts "not considered for accountability" (81), "not rated" on RP (10), or special-purpose districts like university lab schools and the Windham School District (32).

## Files

| File | Description |
|------|-------------|
| `Texas_Relative_Performance_Dashboard_2025.html` | Self-contained interactive dashboard (HTML + embedded JS/CSS) |
| `Districts_Missing_RP_Scores_2025.xlsx` | Recovery report: 243 recovered districts and 123 still missing, with reasons |
| `README.md` | This documentation file |
