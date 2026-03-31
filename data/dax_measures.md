# DAX Measures Reference
## NH Staffing Analytics | Power BI

All measures reference the table `NH_Staffing_1000`.

---

### Understaffed %
```dax
Understaffed % =
DIVIDE(
    COUNTROWS(
        FILTER(
            NH_Staffing_1000,
            NH_Staffing_1000[Staffing Status] = "Understaffed"
        )
    ),
    COUNTROWS(NH_Staffing_1000),
    0
)
```

Purpose: Calculates what proportion of facilities in the current filter context fall below the CMS 3.48-hour benchmark. Used as the primary metric on the Choropleth Map page.

---

### Understaffed Count
```dax
Understaffed Count =
COUNTROWS(
    FILTER(
        NH_Staffing_1000,
        NH_Staffing_1000[Staffing Status] = "Understaffed"
    )
)
```

Purpose: Raw count of understaffed facilities. Used as a KPI Card on the Executive Summary page.

---

### Weekend Gap Hrs
```dax
Weekend Gap Hrs =
AVERAGE(NH_Staffing_1000[Total Nurse Hours Per Resident Day])
- AVERAGE(NH_Staffing_1000[Weekend Nurse Hours Per Resident Day])
```

Purpose: Measures the average drop in nursing hours on weekends compared to weekdays. A positive value means fewer hours are provided on weekends. Used in the heat map matrix and Gap Severity classification.

---

### Gap Severity
```dax
Gap Severity =
SWITCH(
    TRUE(),
    [Weekend Gap Hrs] > 0.75, "Critical",
    [Weekend Gap Hrs] > 0.50, "High",
    [Weekend Gap Hrs] > 0.25, "Moderate",
    "Low"
)
```

Purpose: Classifies the weekend gap into four severity tiers. Used in the Smart Narrative visual and as the basis for the Gap Severity Column field.

---

### RN Share of Total Hours
```dax
RN Share of Total Hours =
DIVIDE(
    AVERAGE(NH_Staffing_1000[RN Hours Per Resident Day]),
    AVERAGE(NH_Staffing_1000[Total Nurse Hours Per Resident Day]),
    0
)
```

Purpose: Shows what proportion of total nurse hours are provided by registered nurses. A lower share in For-Profit facilities indicates reliance on lower-cost aide staffing. Used in the Ownership Coverage conditional table.

---

### RN Hours Gap
```dax
RN Hours Gap =
AVERAGE(NH_Staffing_1000[RN Hours Per Resident Day]) - 0.75
```

Purpose: Measures whether average RN hours are above or below the 0.75-hour reference guideline. Negative values (shown in red) indicate RN coverage below the reference level. Used in the conditional table on the Ownership Coverage page.

---

### Rating Gap
```dax
Rating Gap =
SUM(NH_Staffing_1000[Overall Rating]) - SUM(NH_Staffing_1000[Staffing Rating])
```

Purpose: Measures the difference between a facility's Overall CMS Rating and its Staffing Rating. Positive values indicate a facility rates better overall than its staffing alone would predict. Negative values indicate the opposite. Used in the Outlier Detail drill-through table.

---

### Turnover Risk Index
```dax
Turnover Risk Index =
(AVERAGE(NH_Staffing_1000[Total Nursing Turnover Rate]) * 0.5)
+ (AVERAGE(NH_Staffing_1000[RN Turnover Rate]) * 0.5)
```

Purpose: A 50/50 weighted average of total nursing and RN-specific turnover rates. Used as the primary metric in the conditional colour matrix and waterfall chart on the Turnover Risk Matrix page.

---

### RN Retention Pressure
```dax
RN Retention Pressure =
AVERAGE(NH_Staffing_1000[RN Turnover Rate])
- AVERAGE(NH_Staffing_1000[Total Nursing Turnover Rate])
```

Purpose: Measures whether RN turnover is disproportionately higher than overall nursing turnover. A large positive value signals that the retention crisis is concentrated specifically in registered nurses, which is more costly and difficult to resolve than general aide turnover.

---

### Dynamic Title
```dax
Dynamic Title =
SELECTEDVALUE(NH_Staffing_1000[State], "All States")
    & " - All Facilities Staffing Analysis"
```

Purpose: Generates a page title that updates automatically based on the State slicer selection. When no state is selected it reads "All States - All Facilities Staffing Analysis". When filtered to a specific state, the state name replaces "All States".
