# README: Texas Comptroller Revenue Forecast Dashboard

## Minimum Documentation Requirements

| Requirement | Details |
|---|---|
| **Author** | Charles Miller, with AI assistance (see AI Usage below) |
| **Creation Date** | March 4–5, 2026 |
| **Current Version** | v4 (Revenue_Forecast_Dashboard.html) |
| **Purpose** | Produce monthly and annual FY 2026 revenue projections for 23 tax and non-tax categories, with probability cones and CRE comparison, to support early-stage policy analysis of Texas state revenue trends |
| **Data Sources** | See Data Sources section below |
| **AI Usage** | See AI Usage section below |

## Purpose

This dashboard addresses the question: **How are Texas state revenue collections tracking against the Comptroller's Revenue Estimate (CRE), and what do current trends and updated economic data imply for the remainder of FY 2026?**

It was created to provide a rapid-exploration analytical tool for internal policy analysis. It is explicitly marked as a draft work in progress and does not replace the CRE as the authoritative revenue projection.

## Data Sources

All data sources are public or aggregated. No individual-level, HIPAA/FERPA, confidential, or restricted data is used.

| Source | Description | Public? | Files |
|---|---|---|---|
| Texas Comptroller of Public Accounts — Monthly Revenue Watch | Historical monthly revenue collections by category (tax and non-tax), FY 2016–2026. Published monthly as public data. | Yes | `all-funds-revenue.csv`, `all-funds-tax.csv`, `all-funds-historical.xlsx` |
| Texas Comptroller — Certification Revenue Estimate (CRE) 2026–27 | Official biennial revenue estimate published January 2025. Annual estimates by category. | Yes | `cre-2026-27-data.xlsx` |
| U.S. Energy Information Administration (EIA) — Short-Term Energy Outlook (STEO) | WTI crude oil and Henry Hub natural gas price forecasts. Accessed via the EIA API v2 (`api.eia.gov`). February 2026 edition. | Yes | Fetched live at model runtime |
| Federal Reserve Economic Data (FRED) — St. Louis Fed | Federal funds effective rate (monthly). Accessed via the public FRED CSV endpoint (`fred.stlouisfed.org`). | Yes | Fetched live at model runtime |

## AI Usage

| Tool | Role |
|---|---|
| **Claude (Anthropic) — Claude Opus 4** | Primary AI tool. Used for: model design and iterative refinement through collaborative discussion; writing all Python model code (`refined_model.py`, `gen_dashboard_data.py`, `build_dashboard_v2.py`); generating the HTML/JS/CSS dashboard; writing methodology documents (`.docx`); fetching and integrating live economic data from EIA and FRED APIs. All code and outputs were reviewed by the author. |

No other AI tools were used. The model architecture, analytical decisions (elasticity values, decay parameters, policy overlays, confidence tiers), and all judgment calls were directed by the author through iterative conversation. Claude implemented the author's design decisions and flagged potential issues for the author's consideration.

## Prompt Summary

The dashboard was developed over multiple iterative sessions. Key prompt themes included:

1. **Initial model build**: "Build a forecasting model for Texas Comptroller revenue data using historical monthly collections and CRE estimates." Directed the construction of seasonal indices, trend regression, growth momentum, and CRE-anchored blending.

2. **Outlier handling**: "Address extreme outlier months (e.g., Other Revenue October 2025) that distort the implied-annual calculation." Led to z-score-based outlier detection with exclusion from calibration.

3. **Architecture revision (v2 → v3)**: "If we have actual data for February, why is the model not starting from the actual February numbers?" Initiated a multi-round Socratic discussion that identified the top-down architecture flaw, the compression problem, and converged on bottom-up annual construction with dynamic CRE weighting via MAPE.

4. **Economic assumption update (v3 → v4)**: "Can we improve the model by taking into account explicit CRE assumptions (oil price, gas price, interest rates) and updating them with real-time data?" Led to integration of EIA STEO and FRED APIs with dampened elasticity adjustments.

5. **Dashboard refinements**: Iterative prompts for visualization adjustments including probability cone rendering, category detail table sizing, methodology tab updates, and draft watermark addition.

## Methodology Summary

The model uses a ten-step process:

1. **Seasonal Index Estimation** — 5-year average monthly shares (COVID exclusion for Federal Income)
2. **Annual Level Trend** — Weighted linear regression on recent fiscal year totals
3. **Growth Momentum** — 3-year average YoY growth rate
4. **Blended Annual Projection** — 60/40 trend/growth blend, calibrated 70/30 against actual FY 2026 collections
5. **Outlier Detection** — z-score > 3.0 exclusion from calibration
6. **Economic Assumption Update** — Adjust CRE using live oil, gas, and interest rate data with dampened elasticities (0.60–0.85)
7. **Policy Overlay Adjustments** — Manual adjustment factors for identified policy changes
8. **Dynamic CRE Weighting** — MAPE-based exponential decay: `dynamic_weight = prior_weight × exp(−5 × MAPE)`
9. **Bottom-Up Annual Construction** — Annual = Actual YTD + projected remaining months
10. **√t Probability Cones** — Confidence tiers (HIGH ±4%, MEDIUM ±10%, LOW ±25%) with √t temporal widening; zero uncertainty on actual months

Full methodology is documented in `Forecast_Methodology_Statement_v4.docx`.

## Key Files

| File | Description |
|---|---|
| `Revenue_Forecast_Dashboard.html` | Current production dashboard (v4) |
| `Revenue_Forecast_Dashboard_sandbox.html` | Sandbox copy (synced with main) |
| `Revenue_Forecast_Dashboard_v2.html` | Archived v2 dashboard |
| `Revenue_Forecast_Dashboard_v3.html` | Archived v3 dashboard |
| `Forecast_Methodology_Statement_v4.docx` | Detailed methodology document |
| `Model_Design_Executive_Summary.docx` | Executive summary of iterative design process |
| `Revenue_Forecast_FY2026-2027_v2.xlsx` | Forecast output spreadsheet |
| `all-funds-revenue.csv` | Source: non-tax revenue actuals |
| `all-funds-tax.csv` | Source: tax revenue actuals |
| `cre-2026-27-data.xlsx` | Source: CRE official estimates |

## Update Notes

| Date | Version | Changes |
|---|---|---|
| March 4, 2026 | v1 | Initial model: seasonal decomposition, trend, growth, basic CRE blending |
| March 4, 2026 | v2 | Added outlier detection, policy overlays, CRE-anchored blending with fixed weights, confidence tiers, probability cones |
| March 5, 2026 | v3 | Architectural revision: bottom-up annual construction, dynamic CRE weighting via MAPE, √t cone widening, actuals preserved exactly |
| March 5, 2026 | v4 | Live economic assumption updates from EIA STEO (oil, gas) and FRED (interest rates) with dampened elasticity adjustments; methodology tab and documentation updated |

## Data Protection Compliance

- **No individual-level data**: All data is aggregated at the state revenue category level
- **No HIPAA/FERPA data**: No health or education records
- **No confidential or restricted datasets**: All source data is publicly published by the Texas Comptroller, EIA, and Federal Reserve
- **Live API data**: Uses only public, unauthenticated endpoints (EIA DEMO_KEY, FRED public CSV)

## Quality Assurance Status

- [x] Author has reviewed model outputs for basic accuracy
- [x] Probability cone ordering verified (P10 ≤ P25 ≤ P50 ≤ P75 ≤ P90)
- [x] Monthly sums verified to match annual totals (within 0.1% rounding)
- [x] Dashboard marked as DRAFT — WORK IN PROGRESS
- [ ] Not yet reviewed by Data & Research team
- [ ] Elasticity parameters (0.60–0.85) and decay parameter (α = 5) have not been backtested
- [ ] Not approved for external sharing

## External Sharing Eligibility

| Criterion | Status |
|---|---|
| Data sources are public or aggregated | ✅ Met |
| Documentation is complete | ✅ Met (this README + methodology docs) |
| Outputs reviewed by author for accuracy | ✅ Met |
| Formal review by Data & Research team | ⬜ Not yet completed |

**Current recommendation**: This dashboard is suitable for internal use and early-stage policy analysis. It should not be shared externally or used to support major policy claims until the Data & Research team has reviewed the model methodology and outputs.
