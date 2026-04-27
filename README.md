# 🏥 Medicare Spending Per Beneficiary (MSPB) & Readmission Signals
**Tools:** Excel · Power BI Desktop  
**Domain:** Healthcare Analytics  
**Data Source:** [CMS Provider Data](https://data.cms.gov/provider-data/search)

---

## 📌 Project Overview

Hospital leaders need a simple way to see where Medicare Spending per Beneficiary (MSPB) is high, how it varies by hospital type, ownership, and state, and whether quality signals — overall rating and readmission group results — point to drivers or trade-offs.

This project builds a two-page Power BI dashboard that flags outlier hospitals and surfaces actionable levers for two stakeholder audiences:

| Stakeholder | Need |
|-------------|------|
| **CFO / Finance** | Identify high-MSPB outliers and peer benchmarks to prioritize cost-reduction work |
| **Quality / Clinical Ops** | See where "Readmission Worse" concentrations co-exist with high MSPB to target care-pathway fixes |

---

## ❓ Business Questions

1. What is the national distribution of MSPB, and which states/types/ownerships show materially higher medians?
2. Which facilities are high-MSPB outliers within their state and peer group?
3. Does a larger share of "Readmission Worse" measures correlate with higher MSPB at the facility level?
4. How does MSPB vary by overall hospital rating bands?
5. Which lower-MSPB peers (same state/ownership) look like practical benchmarks for each outlier?

---

## 📁 Data Sources

| File | Description |
|------|-------------|
| `hospitalGen.csv` | Facility name, state, ownership type, overall hospital rating |
| `medicare.csv` | MSPB scores, readmission group measure counts (better / no different / worse), peer group keys |

Both files sourced from the [CMS Provider Data portal](https://data.cms.gov/provider-data/search).

---

## 🔧 Process & Steps

1. **Data Gathering** — Downloaded both CSV files from CMS Provider Data
2. **Data Profiling** — Checked for nulls, mismatched facility IDs, and schema alignment in Excel
3. **Feature Engineering** — Calculated the following derived fields:

| Field | Formula |
|-------|---------|
| `readm_worse_share` | `readm_worse_count / readm_total` |
| `gap` | `score - peer_group_median` |
| `outlier_flag` | High Outlier / Low Outlier / Normal based on gap threshold |
| `rating_band` | Low / Mid / High based on CMS star rating |
| `peer_group_key` | State + ownership type composite |

4. **Dashboard Design** — Planned two-page structure around stakeholder personas
5. **Power BI Build** — Built all visuals with visual-level filters and field aggregations
6. **Insight Synthesis** — Identified key findings and wrote executive summary

---

## 📊 Dashboard

### Page 1 — National Overview
> Distribution → Outlier flags → State variation → Geographic map → Correlation

![National Overview](screenshots/page2_national_overview.png)

| Visual | Purpose |
|--------|---------|
| KPI Cards (5) | Hospitals with score, median, avg gap, outlier count, avg readmission worse share |
| Histogram | National MSPB distribution across all 2,004 hospitals |
| Scatter Plot | MSPB vs. Readmission Worse Share, color-coded by outlier flag |
| State Bar Chart | Avg Readmission Worse Share — top 17 states |
| Combo Chart | MSPB score and Readmission Worse Share side-by-side by state |
| Map | Geographic distribution of higher-cost hospitals |
| Slicers | Filter by outlier flag, rating band, hospital ownership |

---

### Page 2 — Outlier Deep Dive
> Named outliers → State concentration → Ownership breakdown → Rating band comparison

![Outlier Deep Dive](screenshots/page3_outlier_deepdive.png)

| Visual | Purpose |
|--------|---------|
| Facility Table | 36 High Outlier hospitals with score, gap, state, owner, readmission worse share |
| State Count Table | Count of High Outliers by state |
| Rating Band Bar Chart | Avg MSPB by Low / Mid / High rating band |
| Ownership Bar Chart | Count of High Outliers by hospital ownership type |

---

## 🔍 Key Findings

### 1. Half of hospitals spend below benchmark — 36 spend significantly above it
50.45% of hospitals score below the national MSPB benchmark of 1.0. The **36 High Outlier hospitals** average a gap of **0.188** above their peer group median, with scores reaching 1.2–1.4 against a national median of 0.990 (STDEV = 0.07).

### 2. Florida, California, and Pennsylvania are the outlier hotspots
**FL = 7, CA = 6, PA = 4** — these three states account for nearly half of all High Outlier hospitals despite representing a smaller share of total facilities. Geographic concentration suggests state-level policy or market factors contribute to cost pressure.

### 3. High MSPB and poor readmission rates co-occur — but not always
The scatter plot reveals two distinct High Outlier profiles:
- 🔴 **Dual risk** — high MSPB + high readmission worse share (some exceeding 25–37%) → priority targets for both finance and quality teams
- 🟠 **Cost-only** — high MSPB but low readmission worse share → cost inefficiency independent of clinical quality

### 4. Lower-rated hospitals spend more per beneficiary
| Rating Band | Avg MSPB Score |
|-------------|----------------|
| Low         | 1.02           |
| Mid         | 0.99           |
| High        | 0.98           |

This consistent inverse relationship means quality improvement programs targeting Low-rated hospitals are likely to produce cost reductions simultaneously — a **dual-benefit opportunity**.

### 5. Voluntary non-profit Private hospitals dominate the outlier list
Despite not being the largest ownership category overall, **Voluntary non-profit Private hospitals** represent the biggest share of the 36 High Outliers, followed by Proprietary hospitals — a signal for finance teams to examine contracting and operational practices within this segment.

---

## 💡 Recommendations

**For CFO / Finance:**
- Prioritize the top High Outlier hospitals by gap size — several show gaps of 0.25–0.31, well above the 0.188 average
- Focus geographic attention on Florida and California (13 of 36 outliers combined)
- Use peer hospitals in the same state and ownership category as internal benchmarks
- Target the Voluntary non-profit Private segment for a dedicated financial review

**For Quality / Clinical Operations:**
- Hospitals in the top-right of the scatter plot (high MSPB + high readmission worse share) are the highest-priority dual targets
- Quality improvement in Low-rated hospitals is likely to reduce spending simultaneously
- Investigate care pathway differences between High Outlier hospitals and their lower-cost peers

---

## ⚠️ Limitations

- MSPB scores reflect a snapshot period and may not capture recent operational changes
- Some facilities returned "Insufficient data" in the outlier flag — flagged as a limitation, not excluded silently
- Peer group matching by state + ownership does not control for facility size, teaching status, or case mix index
- 36 High Outliers (1.8% of dataset) is a conservative threshold

---

## 📂 Repository Structure

```
medicare-mspb-powerbi/
│
├── screenshots/
│   ├── page2_national_overview.png
│   └── page3_outlier_deepdive.png
│
├── Medicare_MSPB_Project_Report_Final.md
├── Medicare_MSPB_Portfolio_Writeup.md
└── README.md
```

---

## 👩‍💻 Author

**Adriana Pineda** — Data Analyst  
[LinkedIn](https://www.linkedin.com/in/adrianap/) · [Portfolio](https://www.datascienceportfol.io/AdrianaPineda)
