# HR Workforce, Attrition & Cost Analytics
## Power BI Dashboard Specification & Visual Blueprint

This document specifies the information architecture, visual layout, KPI placement, filter hierarchy, and methodological standards for the **"HR Workforce, Attrition & Cost Analytics Dashboard"**.

---

### 1. Architectural Overview & Canvas Standards

- **Aspect Ratio / Resolution**: 16:9 widescreen (`1920 x 1080` px or `1280 x 720` px canvas).
- **Design Philosophy**: Executive business intelligence — high contrast, minimal clutter, strong visual hierarchy, card-based KPI containers, and intuitive cross-filtering.
- **Color Palette**:
  - **Neutral Background**: Deep Charcoal / Clean Slate (`#F8F9FA` light theme or `#1E222B` dark theme).
  - **Primary Corporate Accent**: Navy / Steel Blue (`#1E3A8A` / `#2563EB`) for workforce totals and demographics.
  - **Risk / Attrition Accent**: Vibrant Coral / Crimson (`#DC2626` / `#EF4444`) for departures and flight risk indicators.
  - **Retention / Active Accent**: Emerald Green (`#059669` / `#10B981`) for active headcount and retention metrics.
  - **Neutral Data Fill**: Muted Blue-Gray (`#64748B`) for context bars and historical baselines.
- **Navigation**: Persistent left-hand or top tab bar connecting all 4 report pages:
  1. Executive Workforce Overview
  2. Attrition Drivers & Turnover Diagnostics
  3. Compensation & Cost Analytics
  4. Employee Experience & Workplace Sentiment

---

### 2. Page-by-Page Technical Specifications

---

#### Page 1: Executive Workforce Overview

##### A. Business Purpose & Core Questions Answered
- What is our total workforce size, and what is our enterprise turnover rate?
- How is our talent distributed across departments, operational roles, and seniority tiers?
- What are the core demographic characteristics (age cohorts, gender, educational background) of our staff?
- What is our baseline monthly and annual payroll commitment?

##### B. Header KPI Scorecards (Top Ribbon)
| KPI Card Label | Underlying DAX Measure | Target Format | Baseline Value | Visual Indicator |
| :--- | :--- | :---: | :---: | :--- |
| **Total Headcount** | `[Total Employees]` | `#,##0` | `1,470` | Neutral Primary |
| **Active Employees** | `[Active Employees]` | `#,##0` | `1,233` | Emerald Green |
| **Employees Lost** | `[Employees Lost]` | `#,##0` | `237` | Coral / Alert |
| **Attrition Rate** | `[Attrition Rate]` | `0.0%` | `16.1%` | Target: $< 15\%$ (Amber/Red) |
| **Average Tenure** | `[Average Company Tenure]` | `0.0 yrs` | `7.0 yrs` | Neutral Context |
| **Total Annual Payroll** | `[Total Annual Payroll]` | `$#,##0` | `$114.71M` | Currency Accent |

##### C. Visual Layout & Chart Configurations
1. **Department Distribution (Donut / Clustered Bar Chart)**:
   - *Dimensions*: `Department` (`Research & Development`, `Sales`, `Human Resources`).
   - *Values*: `[Total Employees]` and `[Attrition Rate]`.
   - *Tooltip*: `[Active Employees]`, `[Employees Lost]`.
   - *Insight*: R&D represents 65.4% of staff (961), Sales 30.3% (446), and HR 4.3% (63).
2. **Headcount & Departures by Job Role (Horizontal Clustered Bar Chart)**:
   - *Y-Axis*: `JobRole` (sorted by headcount descending).
   - *X-Axis*: `[Total Employees]` (Bar 1), `[Employees Lost]` (Bar 2).
   - *Data Labels*: Absolute count and percentage.
3. **Age Cohort Distribution by Gender (Clustered / Bi-Directional Bar Chart)**:
   - *Y-Axis*: `Age_Group` (`<25`, `25-34`, `35-44`, `45-54`, `55+`).
   - *X-Axis*: `[Total Employees]`.
   - *Legend*: `Gender` (`Female`, `Male`).
   - *Insight*: Identifies the 25–34 and 35–44 age bands as the dominant workforce backbone (72.2%).
4. **Academic Background Breakdown (Treemap / 100% Stacked Bar)**:
   - *Group*: `EducationField` (`Life Sciences`, `Medical`, `Marketing`, `Technical Degree`, `Other`, `Human Resources`).
   - *Values*: `[Total Employees]`.
   - *Sub-Detail*: `Education` level (`Bachelor`, `Master`, etc.).

##### D. Slicers & Page Filters
- `Department` (Multi-select dropdown)
- `Job Level` (Horizontal tile button)
- `Gender` (Radio toggle)
- `Business Travel` (Dropdown)

---

#### Page 2: Attrition Drivers & Turnover Diagnostics

##### A. Business Purpose & Core Questions Answered
- Which operational factors are most strongly associated with employee departures?
- How dramatically does mandatory overtime correlate with higher attrition across teams?
- Which specific job roles face critical turnover vulnerability?
- At what career tenure milestones do departures peak?
- Does promotion stagnation correlate with voluntary exit?

##### B. Header KPI Scorecards (Top Ribbon)
| KPI Card Label | Underlying DAX Measure | Target Format | Baseline Value | Alert Trigger |
| :--- | :--- | :---: | :---: | :--- |
| **Global Attrition Rate** | `[Attrition Rate]` | `0.0%` | `16.1%` | Enterprise Baseline |
| **Overtime Attrition Rate** | `[Overtime Attrition Rate]` | `0.0%` | `30.5%` | Critical Alert ($> 25\%$) |
| **Non-Overtime Attrition Rate** | `[Non-Overtime Attrition Rate]` | `0.0%` | `10.4%` | Healthy Range |
| **Overtime Relative Risk** | `[Overtime Attrition Ratio]` | `0.0x` | `2.9x` | High Disparity Indicator |
| **Stagnant Promotion Rate** | `[Stagnant Career Percentage]` | `0.0%` | `17.5%` | Risk Pool (257 staff) |

##### C. Visual Layout & Chart Configurations
1. **Overtime Impact on Attrition (100% Stacked Column / Clustered Bar)**:
   - *X-Axis*: `OverTime` (`Yes`, `No`).
   - *Y-Axis*: `% Share of Cohort` (`Attrition: Yes` vs. `Attrition: No`).
   - *Visual Takeaway*: Direct visualization of the 30.5% vs. 10.4% turnover disparity.
2. **Attrition Rate by Job Role (Ranked Horizontal Bar Chart)**:
   - *Y-Axis*: `JobRole` (sorted by `[Attrition Rate]` descending).
   - *X-Axis*: `[Attrition Rate]`.
   - *Conditional Formatting*: Bars highlighted in red if $> 20\%$.
   - *Key Hotspots*: Sales Representatives (39.8%), Laboratory Technicians (23.9%), Human Resources (23.1%), Sales Executives (17.5%).
3. **Tenure Milestone Departure Curve (Line & Clustered Column Chart)**:
   - *X-Axis*: `Tenure_Bracket` (`<1 Year (New Hire)`, `1-2 Years`, `3-5 Years`, `6-10 Years`, `10+ Years`).
   - *Column Y-Axis*: `[Employees Lost]` (Volume).
   - *Line Y-Axis*: `[Attrition Rate]` (Percentage).
   - *Key Finding*: Flight risk is highest during initial employment stages ($<1$ yr: 11.4%, 1–2 yrs: 27.2%), declining significantly after 5 years.
4. **Travel Intensity vs. Job Level Matrix (Heatmap / Matrix Visual)**:
   - *Rows*: `JobLevel` (`Entry Level` through `Executive Level`).
   - *Columns*: `BusinessTravel` (`Non-Travel`, `Travel Rarely`, `Travel Frequently`).
   - *Values / Background Color*: `[Attrition Rate]`.
   - *Insight*: Frequent travel compounded with Entry-Level status shows extreme turnover ($> 40\%$).
5. **Commute Distance Impact (Clustered Column Chart)**:
   - *X-Axis*: `Distance_Bracket` (`Near (1-5 mi)`, `Moderate (6-15 mi)`, `Far (16+ mi)`).
   - *Y-Axis*: `[Attrition Rate]`.

##### D. Slicers & Page Filters
- `OverTime` (Toggle button)
- `Department` (Dropdown)
- `Marital Status` (`Single`, `Married`, `Divorced`)
- `Tenure Bracket` (Dropdown)

---

#### Page 3: Compensation, Payroll & Cost Analytics

##### A. Business Purpose & Core Questions Answered
- What is the total payroll lost to employee turnover?
- What is the estimated replacement cost across departments using established HR benchmarks?
- Does compensation disparity correlate with higher flight risk in entry and mid-tier roles?
- How do stock options and salary hike percentages affect employee retention?

##### B. Header KPI Scorecards (Top Ribbon)
| KPI Card Label | Underlying DAX Measure | Target Format | Baseline Value | Strategic Context |
| :--- | :--- | :---: | :---: | :--- |
| **Total Monthly Payroll** | `[Total Monthly Payroll]` | `$#,##0` | `$9.56M` | Global payroll volume |
| **Departed Monthly Payroll** | `[Departed Monthly Payroll]` | `$#,##0` | `$1.13M` | Direct salary churned |
| **Departed Annual Payroll** | `[Departed Annual Payroll]` | `$#,##0` | `$13.61M` | Cumulative salary lost |
| **Turnover Cost (50% Baseline)** | `[Estimated Turnover Cost (Conservative - 50%)]` | `$#,##0` | `$6.81M` | Entry/operational benchmark |
| **Turnover Cost (100% Standard)** | `[Estimated Turnover Cost (Standard - 100%)]` | `$#,##0` | `$13.61M` | Professional/technical benchmark |
| **Retained vs Departed Avg Pay** | Card displaying `$6,833` vs `$4,787` | `$#,##0` | `-$2,046` | Lower paid staff leave faster |

##### C. Visual Layout & Chart Configurations
1. **Monthly Income Distribution by Job Level & Role (Box Plot / Clustered Bar)**:
   - *Y-Axis*: `JobLevel` / `JobRole`.
   - *X-Axis*: `[Average Monthly Income]`.
   - *Visual Comparison*: Split by `Attrition` (`Yes` vs `No`) to demonstrate internal pay gaps.
2. **Turnover Cost Exposure by Department (Waterfall or Bar Chart)**:
   - *Category*: `Department`.
   - *Measure*: `[Estimated Turnover Cost (Standard - 100%)]`.
   - *Data Labels*: Formatted in millions (`$M`).
3. **Attrition Rate across Salary Bands (Column Chart)**:
   - *X-Axis*: `Salary_Bracket` (`<$3,000`, `$3,000-$4,999`, `$5,000-$7,999`, `$8,000-$11,999`, `$12,000+`).
   - *Y-Axis*: `[Attrition Rate]`.
   - *Observation*: Employees in the lowest salary bracket ($< \$3,000$) experience the highest turnover rate (~29.4%), whereas employees earning $\$12,000+$ experience only ~6.7% turnover.
4. **Equity Retention Anchor (Clustered Column Chart)**:
   - *X-Axis*: `StockOptionLevel` (0, 1, 2, 3).
   - *Y-Axis*: `[Attrition Rate]`.
   - *Insight*: Employees with Stock Option Level 0 experience 24.4% turnover vs. ~9.4% for Levels 1 and 2.
5. **Salary Hike % vs Performance Rating (Scatter / Clustered Column)**:
   - *X-Axis*: `PercentSalaryHike` (11% to 25%).
   - *Y-Axis*: `[Attrition Rate]` grouped by `PerformanceRating`.

##### D. Slicers & Page Filters
- `Salary Bracket` (Multi-select)
- `Job Level` (Tile buttons)
- `Stock Option Level` (Horizontal buttons: 0, 1, 2, 3)
- `Department` (Dropdown)

---

#### Page 4: Employee Experience & Workplace Sentiment

##### A. Business Purpose & Core Questions Answered
- How strongly do employee satisfaction ratings correlate with flight risk?
- Is there a compound risk when poor work-life balance intersects with excessive overtime?
- Are high performers (`Outstanding` ratings) at risk of departure due to low satisfaction?
- Does relationship quality with colleagues or managers act as a retention buffer?

##### B. Header KPI Scorecards (Top Ribbon)
| KPI Card Label | Underlying DAX Measure | Target Format | Baseline Value | Alert Status |
| :--- | :--- | :---: | :---: | :--- |
| **Bad Work-Life Balance Staff** | `[Bad Work-Life Balance Count]` | `#,##0` | `80` | High Burnout Risk |
| **Bad WLB Attrition Rate** | `[Bad Work-Life Balance Attrition Rate]` | `0.0%` | `31.3%` | Nearly 2x Org Baseline |
| **Low Job Satisfaction Staff** | `[Low Job Satisfaction Count]` | `#,##0` | `289` | Morale Concern |
| **Low Environment Sat. Staff**| `[Low Environment Satisfaction Count]` | `#,##0` | `284` | Facility/Culture Concern |
| **Outstanding Performer Turnover**| Custom filter on `Outstanding` | `0.0%` | `15.5%` | High-Value Talent Flight |

##### C. Visual Layout & Chart Configurations
1. **Four-Pillar Sentiment Attrition Matrix (100% Stacked Bar / Grouped Grid)**:
   - Four companion visuals comparing `[Attrition Rate]` across rating levels (`Low`, `Medium`, `High`, `Very High` / `Bad` to `Best`) for:
     - `EnvironmentSatisfaction`
     - `JobSatisfaction`
     - `RelationshipSatisfaction`
     - `WorkLifeBalance`
   - *Ordering*: Sorted using the companion numerical sort keys (`_Sort`).
2. **Work-Life Balance $\times$ OverTime Interaction Heatmap (Matrix Visual)**:
   - *Rows*: `WorkLifeBalance` (`Bad`, `Good`, `Better`, `Best`).
   - *Columns*: `OverTime` (`Yes`, `No`).
   - *Values*: `[Attrition Rate]`.
   - *Observation*: Employees suffering both 'Bad' Work-Life Balance and Overtime have the highest compound attrition rate in the entire organization ($> 44\%$).
3. **Job Involvement vs. Flight Risk (Column Chart)**:
   - *X-Axis*: `JobInvolvement` (`Low`, `Medium`, `High`, `Very High`).
   - *Y-Axis*: `[Attrition Rate]`.
   - *Insight*: Employees with "Low" involvement depart at 33.7%, compared to 9.0% for "Very High" involvement.
4. **Appraisal Review vs. Merit Equity (Scatter / Clustered Bar)**:
   - Compares retention across `PerformanceRating` (`Excellent` vs. `Outstanding`) and average `PercentSalaryHike`.

##### D. Slicers & Page Filters
- `Department` (Dropdown)
- `Job Role` (Dropdown)
- `WorkLifeBalance` (Multi-select)
- `EnvironmentSatisfaction` (Multi-select)

---

### 3. Methodological Governance: Evidence Classification & Non-Causality

To ensure the highest standard of corporate integrity and avoid misleading executive stakeholders, all insights presented in this project strictly follow the three-tier evidence classification framework:

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    EVIDENCE CLASSIFICATION FRAMEWORK                    │
├───────────────────────┬───────────────────────┬─────────────────────────┤
│ 1. Descriptive        │ 2. Correlation &      │ 3. Strategic HR         │
│    Findings           │    Association        │    Recommendations      │
│ (Empirical Facts)     │ (Observed Patterns)   │ (Actionable Guidance)   │
└───────────────────────┴───────────────────────┴─────────────────────────┘
```

#### Tier 1: Descriptive Findings (Empirical Ground Truth)
- **Definition**: Factual summaries directly computed from the 1,470 employee records without interpretation or extrapolation.
- **Standards**: Must quote exact counts, percentages, and dollar amounts.
- *Examples of Valid Descriptive Phrasing*:
  - *"The company's overall baseline attrition rate across the 1,470 records is 16.12% (237 departed employees)."*
  - *"416 employees work overtime, representing 28.30% of the total workforce."*
  - *"Departed employees earned an average monthly income of $4,787, compared to $6,833 for retained employees."*

#### Tier 2: Correlation & Association (Observed Patterns)
- **Definition**: Statistical co-occurrences where two variables demonstrate a mathematical relationship in the historical data.
- **Critical Rule**: **Observational HR data cannot prove causation.** The presence of a strong statistical association does not establish that variable $A$ directly caused outcome $B$. Confounding factors, omitted variables (e.g., external market demand, leadership changes), and reverse causality may exist.
- *Forbidden Causal Claims*:
  - ❌ *"Overtime causes employees to quit."*
  - ❌ *"Low salary is the reason why Sales Representatives resign."*
- *Compliant Associative Phrasing*:
  - ✅ *"Employees working overtime exhibit an observed turnover rate of 30.5%, compared to 10.4% among non-overtime peers—a 2.9x relative disparity."*
  - ✅ *"Lower monthly compensation and entry-level job roles strongly correlate with higher attrition incidence in the historical dataset."*
  - ✅ *"A statistically significant negative association exists between stock option levels and voluntary departure."*

#### Tier 3: Strategic HR Recommendations (Hypotheses for Intervention)
- **Definition**: Actionable business proposals developed from observed patterns for leadership consideration, piloting, and further investigation.
- **Standards**: Frame recommendations as targeted business interventions or diagnostic investigations rather than definitive silver bullets.
- *Examples of Compliant Recommendation Phrasing*:
  - *"Conduct targeted workload audits within the Sales Representative and Laboratory Technician departments to identify operational drivers of mandatory overtime."*
  - *"Implement structured 30-60-90 day onboarding check-ins, as tenure diagnostics identify the first two years as the highest-risk retention window (27.2% attrition in years 1–2)."*
  - *"Explore compensation benchmarking for entry-level positions where starting salaries sit below the $3,000/month threshold."*
  - *"Pilot equity incentives or structured bonus pathways for high-turnover technical roles currently holding Stock Option Level 0."*
