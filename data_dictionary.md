# Data Dictionary
## NH_Staffing_1000.csv

Source: CMS Provider Data Catalog, Provider Information File (NH_ProviderInfo_Feb2026)  
Sample: 1,000 facilities, stratified by state across all 51 US states  
Nulls: 0 in any column

---

| Column | Data Type | Description | Example |
|---|---|---|---|
| Facility Name | Text | Unique facility identifier as listed in CMS records | "St John's Health Sage Living" |
| City | Text | City where the facility is located | "Chicago" |
| State | Text | Two-letter US state abbreviation | "IL" |
| Ownership Type | Text | Simplified ownership classification (For-Profit / Non-Profit / Government) | "For-Profit" |
| Facility Size | Text | Size band derived from Certified Beds (Small under 50, Medium 50 to 149, Large 150 plus) | "Medium" |
| Certified Beds | Numeric | Number of federally certified beds | 120 |
| Overall Rating | Numeric | CMS 5-star overall quality rating (1 = lowest, 5 = highest) | 3 |
| Staffing Rating | Numeric | CMS 5-star staffing rating (1 = lowest, 5 = highest) | 4 |
| Staffing Status | Text | Pre-calculated flag: Understaffed if Total Nurse Hours < 3.48, otherwise Adequate | "Adequate" |
| Total Nurse Hours Per Resident Day | Numeric (2dp) | Reported total nursing hours per resident per day (Aide + LPN + RN combined) | 3.88 |
| RN Hours Per Resident Day | Numeric (2dp) | Reported registered nurse hours per resident per day | 0.84 |
| Weekend Nurse Hours Per Resident Day | Numeric (2dp) | Total nurse staff hours on weekends per resident per day | 3.38 |
| Total Nursing Turnover Rate | Numeric (2dp) | Annual turnover rate for all nursing staff (stored as a percentage, e.g. 50.0 = 50%) | 50.0 |
| RN Turnover Rate | Numeric (2dp) | Annual turnover rate for registered nurses specifically | 60.0 |

---

### Derived Columns (created in the original dataset before loading to Power BI)

`Staffing Status` was calculated as:
- Understaffed: Total Nurse Hours Per Resident Day < 3.48
- Adequate: Total Nurse Hours Per Resident Day >= 3.48

`Facility Size` was calculated from Certified Beds as:
- Small: fewer than 50 beds
- Medium: 50 to 149 beds
- Large: 150 beds or more

`Ownership Type` was simplified from the original CMS multi-category ownership field into three groups:
- For-Profit: includes For profit - Corporation, LLC, Individual, Partnership
- Non-Profit: includes Non profit - Corporation, Church related, Other
- Government: includes Government - County, State, City, Hospital district

---

### Notes

- Turnover rates are stored as whole percentages, not decimals. A value of 50.0 means 50%, not 0.50.
- The CMS recommended minimum for Total Nurse Hours Per Resident Day is 3.48. This benchmark is the basis for the Staffing Status column and the Understaffed % DAX measure.
- The RN Hours reference guideline of 0.75 hours per resident per day is used in the RN Hours Gap DAX measure on the Ownership Coverage page.
