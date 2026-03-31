# Nursing Home Staffing Analytics
### A PMP-Aligned Portfolio Project | Power BI | CMS Provider Data | Healthcare Operations

This project applies data analytics techniques to a sample dataset from the CMS (Centers for Medicare and Medicaid Services). It surfaces operational insights about nursing home staffing quality across the United States, covering understaffing patterns, ownership-driven RN coverage gaps, weekend care risks, and nursing staff turnover.

## Project Overview

This project applies business intelligence and data analytics techniques to a 1,000-facility sample from the **CMS (Centers for Medicare and Medicaid Services) Provider Data Catalog**. It surfaces operational insights about nursing home staffing quality across the United States, covering understaffing patterns, ownership-driven RN coverage gaps, weekend care risks, and nursing staff turnover.

The project is structured around **five business problems** and delivered as an 8-page interactive Power BI report. It also forms the analytics deliverable within a broader **PMP-aligned project management portfolio** covering a full 21-day project lifecycle from charter to closure.

![Workflow Flowchart](images/4.Analytics%20Workflow%20Flowchart.png)

---

## Dataset

| Attribute | Detail |
|---|---|
| Source | CMS Provider Data Catalog (publicly available) |
| File | `NH_Staffing_1000.csv` |
| Rows | 1,000 facilities |
| Columns | 14 |
| Nulls | 0 (pre-cleaned, stratified sample) |
| Geography | 51 US states |

### Columns Used

| Column | Description |
|---|---|
| Facility Name | Unique facility identifier |
| City | Facility city |
| State | US state abbreviation |
| Ownership Type | For-Profit / Non-Profit / Government |
| Facility Size | Small / Medium / Large (derived from Certified Beds) |
| Certified Beds | Number of federally certified beds |
| Overall Rating | CMS 5-star overall quality rating (1-5) |
| Staffing Rating | CMS 5-star staffing rating (1-5) |
| Staffing Status | Understaffed / Adequate (derived: below 3.48 hrs = Understaffed) |
| Total Nurse Hours Per Resident Day | Total nursing hours per resident per day |
| RN Hours Per Resident Day | Registered nurse hours per resident per day |
| Weekend Nurse Hours Per Resident Day | Nurse hours on weekends per resident per day |
| Total Nursing Turnover Rate | Annual turnover rate for all nursing staff |
| RN Turnover Rate | Annual turnover rate for registered nurses |

---

## Business Problems

Five analytical questions were defined before any build activity began, each mapped to a specific hypothesis and a set of Power BI visuals.

### BP1: Which states have the highest proportion of understaffed facilities?

> The CMS minimum safe staffing benchmark is 3.48 total nurse hours per resident per day. States exceeding 40% understaffing are expected to also carry lower average CMS overall ratings, suggesting systemic rather than isolated failures.

**Visuals built:** Choropleth map (US states coloured by understaffing proportion), dual-axis bar and line chart (Understaffed % vs Average Overall Rating by state), drill-through to State Detail page, KPI card with CMS benchmark label.

**Finding:** Several states in the South and Mountain West showed the highest understaffing proportions. The dual-axis chart confirmed that states with high understaffing consistently tracked lower on the CMS overall rating, supporting the hypothesis.

---

### BP2: Does For-Profit ownership produce structurally lower RN hours than Non-Profit and Government facilities?

> For-Profit facilities are expected to show both lower absolute RN hours and a higher aide-to-RN ratio, suggesting cost-substitution of registered nurses with lower-cost aide staffing.

**Visuals built:** Small Multiple clustered bar chart (RN Hours by Ownership Type, split across facility size bands), 100% stacked bar chart (RN vs Total hours composition by ownership), conditional table (Ownership Type, Facility Size, RN Hours Gap with red and green formatting), constant reference line at 0.75.

**Finding:** For-Profit facilities showed consistently lower RN Hours Per Resident Day across all size bands. The conditional table confirmed negative RN Hours Gap values (red) concentrated in For-Profit Large and Medium categories, while Government and Non-Profit facilities remained at or above the 0.75 reference line.

---

### BP3: Does CMS Staffing Rating reliably predict Overall Rating, and do ownership types show different correlation strengths?

> The correlation will be positive but imperfect, with meaningful outlier facilities that score high on staffing but low overall due to health inspection or QM failures, and vice versa.

**Visuals built:** Scatter chart with per-series trend lines by ownership type, four quadrant text labels (Low Staffing High Overall, High Staffing High Overall, Low Staffing Low Overall, High Staffing Low Overall), outlier detail drill-through page with Rating Gap column, cross-tab matrix (Staffing Rating 1-5 vs Overall Rating 1-5 with count heatmap).

**Finding:** All three ownership types showed a positive but loose correlation. The quadrant labels revealed a meaningful cluster of facilities in the Low Staffing High Overall quadrant, likely driven by strong QM and inspection performance compensating for staffing shortfalls.

---

### BP4: How large is the weekend staffing gap and which state-ownership combinations produce the highest care continuity risk?

> For-Profit facilities in high-volume states are expected to show the largest weekend drops, as cost-minimizing incentives strengthen during lower-oversight weekend periods.

**Visuals built:** Matrix heatmap (State vs Ownership Type coloured by Weekend Gap Hrs), Gap Severity slicer in tile mode (Critical, High, Moderate, Low), Smart Narrative visual for auto-generated insight text.

**Finding:** Filtering to Low severity showed that most states had weekend gaps in the 0.20 to 0.25 range. While no states reached Critical threshold in this sample, the matrix clearly identified state-ownership combinations where the gap was most persistent.

---

### BP5: Is nursing staff turnover concentrated in specific ownership and size combinations, and does the RN turnover gap signal a leadership-level retention failure?

> Large For-Profit facilities will show the highest total turnover and the largest gap between total and RN turnover, indicating that RN retention failures are disproportionately severe at scale.

**Visuals built:** Conditional colour matrix (Ownership Type x Facility Size with Total Nursing Turnover Rate, RN Turnover Rate, and Turnover Risk Index all with green-yellow-red formatting), waterfall chart (Turnover Risk Index contribution by ownership and size segment).

**Finding:** For-Profit Large facilities showed the highest Turnover Risk Index (43.61) and the highest RN Turnover Rate (42.88). The waterfall chart made it clear that For-Profit segments collectively drove the largest cumulative risk addition. Non-Profit facilities showed significantly lower turnover across all size bands.

---

## Power BI Report Pages (8 Total)

| Page | Purpose |
|---|---|
| Executive Summary | 4 KPI cards, State slicer, Ownership Type slicer (synced across all pages) |
| Choropleth Map | US map + dual-axis bar/line chart for BP1 |
| State Detail | Drill-through page from Choropleth Map, facility-level table |
| Ownership Coverage | Small multiples, 100% stacked bar, conditional table for BP2 |
| Scatter Plot | Scatter with trend lines and quadrant labels for BP3 |
| Outlier Detail | Drill-through from scatter, rating gap table and cross-tab matrix |
| Weekend Staffing Gap | Gap severity slicer, state-ownership heat map matrix for BP4 |
| Turnover Risk Matrix | Conditional colour matrix and waterfall chart for BP5 |

---

## DAX Measures

All custom measures were created in the Power BI Modeling tab. Full measure definitions are documented in [`dax_measures.md`](dax_measures.md).

| Measure | Purpose |
|---|---|
| `Understaffed %` | Proportion of facilities below the 3.48-hour CMS benchmark |
| `Understaffed Count` | Raw count of understaffed facilities in the current filter context |
| `Weekend Gap Hrs` | Average difference between weekday and weekend staffing hours |
| `Gap Severity` | SWITCH classification: Critical / High / Moderate / Low |
| `Gap Severity Column` | Column version of Gap Severity used for the tile slicer |
| `RN Share of Total Hours` | RN hours as a proportion of total nursing hours |
| `RN Hours Gap` | Difference between average RN hours and the 0.75 guideline |
| `Rating Gap` | Difference between Overall Rating and Staffing Rating per facility |
| `Turnover Risk Index` | Weighted average of Total and RN Turnover Rates (50/50) |
| `RN Retention Pressure` | RN Turnover Rate minus Total Nursing Turnover Rate |
| `Dynamic Title` | Page title that updates based on the State slicer selection |

---

## How to Interact with the Report

### Option 1: Download and Open in Power BI Desktop
1. Download the `.pbix` file from this repository.
2. Open it in Power BI Desktop (free from Microsoft).
3. All slicers, drill-throughs, bookmarks, and tooltips are fully functional.

### Option 2: View Screenshots
All 8 pages are captured as static images in the `screenshots/` folder for quick reference without any downloads.

---

## Tools and Techniques

| Category | Detail |
|---|---|
| Primary Tool | Microsoft Power BI Desktop |
| Data Source | CSV (pre-cleaned, zero nulls) |
| Measures | 11 custom DAX measures |
| Advanced Visuals | Choropleth Map, Decomposition Tree, Small Multiples, Waterfall Chart, Custom Tooltips |
| Interactivity | Cross-page slicers, Drill-through pages, Bookmarks, Tile slicers, Gap Severity filter |
| Theme | Custom colour theme (Navy #1F4E79 and Blue #2E75B6) |
| Data Source | CMS Provider Data Catalog (publicly available US federal data) |

---

## Project Management Context

This Power BI report is the analytics deliverable within a larger **PMP-aligned project management portfolio**. The project followed a 21-day structured sprint (1 March to 21 March 2026) and produced eight PMBOK-standard artefacts including a Project Charter, WBS, Risk Register, RACI Matrix, and Lessons Learned document.

Full project management documentation is in the [`project_management.md`](project_management.md).

---

## Notes for Reviewers

This project is designed to demonstrate:

- Translating business questions into multi-page analytical reporting
- Custom DAX measure design including ratio, conditional classification, and gap analysis
- Advanced Power BI visual techniques beyond standard bar and column charts
- Cross-page interactivity using synced slicers, drill-through, and bookmarks
- Healthcare domain knowledge applied to publicly available CMS data
- End-to-end project structure from a defined scope statement through to a validated deliverable
