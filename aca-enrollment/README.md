# ACA Enrollment & Affordability Dashboard — Texas

## Minimum Documentation (per AI-Generated Data Analysis & Product Guidelines)

| Requirement    | Details |
|----------------|---------|
| **Author**     | Charles Miller
| **Creation date** | March 2026 |
| **Purpose**    | Interactive dashboard analyzing Texas ACA individual marketplace enrollment trends (2015–2025), cross-state comparisons, and affordability/pricing dynamics (2015–2026) — with a focus on how federal subsidy policy changes (ARP/IRA enhanced subsidies and their 2026 expiration) affect the share of enrollees with access to zero-premium plans. Intended for internal policy analysis and early-stage exploration at Texas 2036. |
| **Data sources** | See [Data Sources](#data-sources) section below |
| **AI usage**   | See [AI Tools Used](#ai-tools-used) section below |
| **Prompt summary** | See [Prompt Summary](#prompt-summary) section below |
| **Update notes** | See [Version History](#version-history) section below |

---

## Data Sources

All data sources used in this dashboard are **public**. No identifiable individual-level data, HIPAA/FERPA-protected data, confidential administrative datasets, or internal restricted datasets are included. All enrollment figures are county-level aggregates published by CMS; all premium data is plan-level public filings.

| Source | Publisher | Years | URL | Used For |
|--------|-----------|-------|-----|----------|
| OEP Public Use Files (county-level) | CMS | 2015–2025 | [data.cms.gov](https://data.cms.gov) | Total enrollment, metal level distribution, FPL bracket distribution, age distribution, observed ≤$10 premium metric |
| OEP Public Use Files (state-level) | CMS | 2015–2025 | [data.cms.gov](https://data.cms.gov) | Multi-state comparison (TX, FL, CA, NY, PA, GA, NC) |
| QHP Landscape Individual Market Medical Files | CMS | 2015–2026 | [data.cms.gov](https://data.cms.gov) | Gross premiums by metal level, SLCSP identification, lowest-cost bronze/silver/gold per rating area and household scenario |
| Federal Poverty Level Guidelines | HHS / Federal Register | 2015–2026 | [aspe.hhs.gov](https://aspe.hhs.gov/topics/poverty-economic-mobility/poverty-guidelines) | Income thresholds for subsidy eligibility calculations |
| IRS Revenue Procedure 2025-25 | IRS | 2026 | [irs.gov](https://www.irs.gov) | Indexed applicable percentage schedule for the 2026 post-ARP subsidy reversion |
| SB 1296, 88th Texas Legislature | Texas Legislature | 2023 | [capitol.texas.gov](https://capitol.texas.gov) | Rating area restructuring context (26 → 27 areas) |

### Data files used (stored in `OEP PUFs/` subfolder)

- `Individual_Market_Medical {YEAR}.xlsx` — QHP Landscape files (2015–2025)
- `individual_market_medical 2026.xlsx` — QHP Landscape file (2026)
- `OEP PUFs/` — CMS county-level Open Enrollment PUFs (2015–2025, .xlsx and .csv)

---

## AI Tools Used

| Tool | Version / Model | Role |
|------|-----------------|------|
| Claude (Anthropic) | Claude Opus 4 | Dashboard design, code generation (HTML/CSS/JS with Chart.js), data extraction and transformation (Python), subsidy formula implementation, eligibility model development, methodology documentation |

All code — including HTML, JavaScript, CSS, and Python data extraction/transformation scripts — was generated through iterative conversation with Claude. The human author directed the analysis, specified methodological requirements, reviewed outputs, identified and reported bugs, and made policy-level decisions (e.g., choice of eligibility methodology, subsidy schedule selection, county scope).

---

## Prompt Summary

The dashboard was developed over multiple iterative sessions. High-level prompt categories included:

1. **Data extraction and structuring:** Instructed the AI to extract enrollment data from CMS PUF Excel/CSV files and premium data from QHP Landscape files, standardize column names across years, and build county-to-rating-area mappings for all 254 Texas counties across 12 plan years.

2. **Subsidy formula implementation:** Directed the AI to implement the three-era ACA subsidy calculation (pre-ARP 2015–2020, ARP/IRA 2021–2025, post-ARP 2026+) with linear interpolation of applicable percentages, the 400% FPL cliff, and correct FPL dollar amounts per year. Identified a 2026 subsidy formula error (AI initially applied ARP rules to 2026) and directed correction.

3. **Dashboard design and layout:** Requested a self-contained HTML dashboard with Chart.js, organized into tabs for Texas enrollment trends, multi-state comparison, and affordability/pricing analysis. Specified Texas 2036 branding, interactive county/scenario/income dropdowns, and specific chart types (small multiples, stacked area, trend lines with policy annotations).

4. **Eligibility methodology development:** Directed iterative refinement of the $0 plan eligibility methodology from a simple 15-county threshold model to a full age-weighted, county-by-county, all-254-county model with independence assumption, CMS validation against observed ≤$10 data, and counterfactual analysis. Selected "Option 4" (hybrid model + CMS validation) from four proposed approaches.

5. **Bug identification and correction:** Reported rendering issues (Chart.js hidden canvas problem), data mismatches (county dropdown vs. pricing data), incorrect 2026 subsidy schedule, and methodology concerns (statewide vs. county-level analysis, age weighting). Directed all fixes.

6. **Methodology documentation:** Requested a dedicated Methodology tab in the dashboard explaining all visualizations, calculations, and assumptions, with special emphasis on the $0 eligibility model.

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | March 2026 | Initial dashboard with 3 tabs: Texas Deep Dive, State Comparison, Affordability & Pricing. Pricing calculator covered 15 most populous counties. Eligibility model used single FPL threshold across 15 counties. |
| 1.1 | March 2026 | Fixed Chart.js hidden canvas bug (Sections B, C, D not rendering on tab switch). Fixed county dropdown mismatch. Fixed `switchTab` implicit `event` variable. |
| 1.2 | March 2026 | Corrected 2026 subsidy formula: added post-ARP schedule (IRS Rev. Proc. 2025-25) with 400% FPL cliff reinstated. Consumer pays jumped from ~$44/mo to ~$156/mo for the 2026 reference scenario. |
| 1.3 | March 2026 | Expanded county coverage from 15 to all 254 Texas counties. Implemented per-year county-to-rating-area mappings to handle SB 1296 restructuring in 2023. Added methodology annotations to Sections C and D. |
| 1.4 | March 2026 | Rebuilt Section D eligibility model: age-weighted, county-by-county, all 254 counties. Three premium tiers (ind21/ind40/ind60). Independence assumption for age×FPL joint distribution. Added CMS observed ≤$10 validation line (2022–2025). Added counterfactual (pre-ARP schedule applied to all years). Fixed double-counting bug in original eligibility calculator (age_count double-multiplication and 100_138/100_150 overlap). |
| 1.5 | March 2026 | Added Methodology tab (Tab 4) with comprehensive documentation of all data sources, calculation methods, the eligibility model (6 subsections), assumptions table, and limitations. Added table of contents with in-page navigation. |

---

## Quality Assurance Checklist

> *Per guidelines: "Authors are responsible for reviewing outputs for basic accuracy before sharing."*

| Check | Status | Notes |
|-------|--------|-------|
| Data sources are all public | ✅ Compliant | CMS PUFs, QHP Landscape files, IRS/HHS guidelines — all publicly available |
| No identifiable individual-level data | ✅ Compliant | All data is county-level aggregate; CMS suppresses cells < 10 enrollees |
| No HIPAA/FERPA/protected data | ✅ Compliant | No health records, student data, or protected information |
| No confidential/restricted datasets | ✅ Compliant | Only public federal and state data products |
| Subsidy formula verified against IRS schedules | ✅ Verified | Three schedules cross-checked against ACA statute, ARP legislation, and IRS Rev. Proc. 2025-25 |
| Premium extraction spot-checked | ✅ Verified | SLCSP identification validated by deduplicating Plan IDs per rating area |
| Eligibility model validated against CMS observed metric | ⚠️ Partial | Model validated directionally against CMS ≤$10 metric for 2022–2025; expected gap explained in methodology (potential vs. actual take-up) |
| Rating area mappings verified across SB 1296 break | ✅ Verified | 209/254 counties confirmed reassigned in 2023; two stable eras (2015–2022 and 2023–2026) |
| 2026 projection assumptions documented | ✅ Documented | Uses 2025 enrollment as proxy; limitations noted in Methodology tab |
| Documentation is complete | ✅ Complete | Methodology tab in dashboard + this README |
| `[AUTHOR REVIEW OF OUTPUTS]` | ⬜ `PENDING` | *Author must confirm they have reviewed all charts, KPIs, and calculations for accuracy before external sharing* |

---

## External Sharing Readiness

Per guidelines, this dashboard may be shared externally when:

| Condition | Status |
|-----------|--------|
| Data sources are public or aggregated | ✅ Met |
| Documentation is complete | ✅ Met (Methodology tab + README) |
| Outputs reviewed by author for accuracy | ⬜ **Pending author review** |

> **Note:** If this dashboard supports major policy claims or is intended for formal publication/wide external distribution, the guidelines recommend additional analytic review by the Data & Research team prior to release. The $0 eligibility model and 2026 projections in particular may warrant such review given their policy significance.

---

## Repository Location

`[INSERT PATH IN TEXAS 2036 GITHUB REPO]`

*Per guidelines: "All AI-generated dashboards and analyses should be stored in the Texas 2036 GitHub repository."*

---

## File Inventory

| File | Description |
|------|-------------|
| `ACA_Enrollment_Dashboard.html` | Self-contained dashboard (HTML + CSS + JS + embedded data, ~1.8 MB) |
| `README.md` | This documentation file |
| `OEP PUFs/` | Source CMS enrollment data files (county-level, 2015–2025) |
| `Individual_Market_Medical *.xlsx` | Source QHP Landscape premium files (2015–2026) |
| `AI Generated Dashboard Guidelines.pdf` | Governance guidelines this README complies with |
