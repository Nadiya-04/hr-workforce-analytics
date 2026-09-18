# HR Workforce, Attrition & Cost Analytics
## Power BI Dashboard Build Guide & Validation Benchmark

This document provides the implementation manual for assembling the 5-page Power BI dashboard suite. It details canvas settings, visual coordinates, field mappings, slicer configurations, and empirical validation tables to audit your calculations against ground-truth data.

---

### 1. Canvas Settings, Theme & Design System

- **Page Dimensions**: 16:9 Widescreen (`1920 x 1080` px recommended; scale-to-fit compatible with `1280 x 720` px).
- **Typography**: `Segoe UI` or `Aptos` (Clean executive sans-serif).
  - *Page Titles*: 18–20 pt Bold.
  - *Card Callout Values*: 24–28 pt Bold.
  - *Card Category Labels*: 10 pt Regular / Semi-bold.
  - *Chart Titles*: 12–14 pt Bold.
  - *Axis Labels / Legends*: 9–10 pt Regular.
- **Theme Palette**:
  - Background: Light Gray/Slate (`#F8F9FA`) or Dark Charcoal (`#1E222B`).
  - Primary Corporate Navy: `#1E3A8A`
  - Active / Retained Emerald: `#059669`
  - Alert / Attrition Coral Red: `#DC2626`
  - Muted Comparison Gray: `#64748B`
  - Accent Teal: `#0D9488`
- **Navigation Bar**: A persistent sidebar (width: 220 px) or top ribbon (height: 60 px) containing page navigation buttons:
  1. Executive HR Overview
  2. Attrition Analysis
  3. Compensation & Retention
  4. Employee Experience
  5. HR Insights

---

### 2. Page-by-Page Construction Specifications

---

#### Page 1: Executive HR Overview

- **Intended Business Question**: *"What is the current operational footprint, demographic balance, total payroll burden, and turnover baseline of our organization?"*
- **Recommended Layout**:
  - **Top Row (Y: 20 to 140 px)**: 6 KPI Cards.
  - **Middle Row (Y: 160 to 580 px)**: 2 Major distribution visuals (Department & Role).
  - **Bottom Row (Y: 600 to 1020 px)**: Demographics (Age Pyramid & Academic Treemap).
  - **Right Sidebar (X: 1650 to 1900 px)**: Interactive Slicers.

##### 1. KPI Cards
| Card Order | Card Label | Field / Measure | Display Format | Expected Baseline |
| :-: | :--- | :--- | :---: | :---: |
| 1 | **Total Employees** | `[Total Employees]` | `#,##0` | `1,470` |
| 2 | **Active Headcount** | `[Active Employees]` | `#,##0` | `1,233` |
| 3 | **Departed Staff** | `[Employees Lost]` | `#,##0` | `237` |
| 4 | **Attrition Rate** | `[Attrition Rate]` | `0.0%` | `16.1%` |
| 5 | **Average Tenure** | `[Average Company Tenure]` | `0.0 yrs` | `7.0 yrs` |
| 6 | **Total Annual Payroll** | `[Total Annual Payroll]` | `$#,##0` | `$114.71M` |

##### 2. Visual Charts
- **Chart 1: Headcount & Attrition by Department**
  - *Visual Type*: Donut Chart or Clustered Bar Chart.
  - *Legend / Axis*: `'HR_Data'[Department]`.
  - *Values*: `[Total Employees]`.
  - *Tooltips*: `[Employees Lost]`, `[Attrition Rate]`, `[Average Monthly Income]`.
- **Chart 2: Staff Distribution across Job Roles**
  - *Visual Type*: Horizontal Clustered Bar Chart (sorted descending by headcount).
  - *Y-Axis*: `'HR_Data'[JobRole]`.
  - *X-Axis*: `[Total Employees]` (Bar 1), `[Employees Lost]` (Bar 2).
- **Chart 3: Workforce Age Pyramid by Gender**
  - *Visual Type*: Clustered Column Chart.
  - *X-Axis*: `'HR_Data'[Age_Group]`.
  - *Y-Axis*: `[Total Employees]`.
  - *Legend*: `'HR_Data'[Gender]`.
- **Chart 4: Education Level & Discipline Breakdown**
  - *Visual Type*: Treemap.
  - *Group*: `'HR_Data'[EducationField]`.
  - *Details*: `'HR_Data'[Education]`.
  - *Values*: `[Total Employees]`.

##### 3. Slicers
- `Department` (Dropdown)
- `Job Level` (Tile selection)
- `Gender` (Toggle buttons: All, Female, Male)
- `Business Travel` (Dropdown)

---

#### Page 2: Attrition Analysis

- **Intended Business Question**: *"Where is turnover concentrated, and which operational risk factors (overtime, travel, role, tenure) display the strongest flight risk?"*
- **Recommended Layout**:
  - **Top Row**: 5 Diagnostic KPI Cards.
  - **Middle Row**: Overtime Impact (Bar) & Job Role Attrition Ranking (Bar).
  - **Bottom Row**: Tenure Departure Curve (Line & Column) & Commute Distance Impact (Column).

##### 1. KPI Cards
| Card Order | Card Label | Field / Measure | Display Format | Expected Baseline |
| :-: | :--- | :--- | :---: | :---: |
| 1 | **Global Attrition Rate** | `[Attrition Rate]` | `0.0%` | `16.1%` |
| 2 | **Overtime Attrition Rate** | `[Overtime Attrition Rate]` | `0.0%` | `30.5%` |
| 3 | **Non-Overtime Attrition Rate** | `[Non-Overtime Attrition Rate]` | `0.0%` | `10.4%` |
| 4 | **Overtime Risk Ratio** | `[Overtime Attrition Ratio]` | `0.0x` | `2.9x` |
| 5 | **New Hire Attrition Rate** | Filtered `[Attrition Rate]` ($\le 2$ yrs) | `0.0%` | `29.8%` |

##### 2. Visual Charts
- **Chart 1: Overtime Attrition Disparity**
  - *Visual Type*: 100% Stacked Column Chart.
  - *X-Axis*: `'HR_Data'[OverTime]`.
  - *Y-Axis*: `% Share of Headcount`.
  - *Legend*: `'HR_Data'[Attrition]`.
- **Chart 2: Ranked Attrition Rate by Job Role**
  - *Visual Type*: Horizontal Bar Chart (sorted descending by `[Attrition Rate]`).
  - *Y-Axis*: `'HR_Data'[JobRole]`.
  - *X-Axis*: `[Attrition Rate]`.
  - *Conditional Formatting*: Fill color changes to Crimson (`#DC2626`) if $> 20.0\%$.
- **Chart 3: Tenure Departure Curve**
  - *Visual Type*: Line and Clustered Column Chart.
  - *X-Axis*: `'HR_Data'[Tenure_Bracket]` (`<1 Year (New Hire)`, `1-2 Years`, `3-5 Years`, `6-10 Years`, `10+ Years`).
  - *Column Y-Axis*: `[Employees Lost]` (Volume).
  - *Line Y-Axis*: `[Attrition Rate]` (Percentage).
- **Chart 4: Commute Distance vs. Flight Risk**
  - *Visual Type*: Clustered Column Chart.
  - *X-Axis*: `'HR_Data'[Distance_Bracket]` (`Near (1-5 mi)`, `Moderate (6-15 mi)`, `Far (16+ mi)`).
  - *Y-Axis*: `[Attrition Rate]`.

##### 3. Slicers
- `OverTime` (Toggle: Yes / No)
- `Department` (Dropdown)
- `Business Travel` (Dropdown)
- `Marital Status` (Tile buttons)

---

#### Page 3: Compensation & Retention

- **Intended Business Question**: *"How does compensation correlate with retention, what is the direct salary lost to churn, and what is our replacement cost exposure?"*
- **Recommended Layout**:
  - **Top Row**: 5 Financial & Cost KPI Cards.
  - **Middle Row**: Salary Distribution by Role & Departed Salary Burden by Department.
  - **Bottom Row**: Turnover Rate by Salary Band & Stock Option Retention Curve.

##### 1. KPI Cards
| Card Order | Card Label | Field / Measure | Display Format | Expected Baseline |
| :-: | :--- | :--- | :---: | :---: |
| 1 | **Total Monthly Payroll** | `[Total Monthly Payroll]` | `$#,##0` | `$9.56M` |
| 2 | **Departed Annual Payroll** | `[Departed Annual Payroll]` | `$#,##0` | `$13.61M` |
| 3 | **Turnover Cost (50% Base)** | `[Estimated Turnover Cost (Conservative - 50%)]` | `$#,##0` | `$6.81M` |
| 4 | **Turnover Cost (100% Std)** | `[Estimated Turnover Cost (Standard - 100%)]` | `$#,##0` | `$13.61M` |
| 5 | **Income Gap (Retained - Lost)** | Displaying `$6,833` vs `$4,787` | `$#,##0` | `-$2,046` |

##### 2. Visual Charts
- **Chart 1: Salary Distribution by Job Level & Role**
  - *Visual Type*: Clustered Bar Chart.
  - *Y-Axis*: `'HR_Data'[JobRole]`.
  - *X-Axis*: `[Average Monthly Income]`.
  - *Legend*: `'HR_Data'[Attrition]`.
- **Chart 2: Departed Payroll Exposure by Department**
  - *Visual Type*: Bar Chart or Waterfall.
  - *Category*: `'HR_Data'[Department]`.
  - *Values*: `[Departed Annual Payroll]`.
  - *Data Labels*: Formatted in `$M`.
- **Chart 3: Attrition Rate Across Salary Bands**
  - *Visual Type*: Clustered Column Chart.
  - *X-Axis*: `'HR_Data'[Salary_Bracket]` (`<$3,000` to `$12,000+`).
  - *Y-Axis*: `[Attrition Rate]`.
- **Chart 4: Equity Retention Anchor**
  - *Visual Type*: Clustered Column Chart.
  - *X-Axis*: `'HR_Data'[StockOptionLevel]` (`0`, `1`, `2`, `3`).
  - *Y-Axis*: `[Attrition Rate]`.

##### 3. Slicers
- `Salary Bracket` (Multi-select)
- `Job Level` (Tile buttons)
- `Department` (Dropdown)
- `Stock Option Level` (Buttons)

---

#### Page 4: Employee Experience

- **Intended Business Question**: *"How do employee satisfaction, workplace climate, work-life balance, and managerial continuity influence employee retention?"*
- **Recommended Layout**:
  - **Top Row**: 5 Sentiment Scorecards.
  - **Middle Row**: 4-Pillar Sentiment Scorecard & Work-Life Balance $\times$ Overtime Matrix.
  - **Bottom Row**: Job Involvement vs. Flight Risk & Manager Tenure Retention Curve.

##### 1. KPI Cards
| Card Order | Card Label | Field / Measure | Display Format | Expected Baseline |
| :-: | :--- | :--- | :---: | :---: |
| 1 | **Bad Work-Life Balance Staff**| `[Bad Work-Life Balance Count]` | `#,##0` | `80` |
| 2 | **Bad WLB Attrition Rate** | `[Bad Work-Life Balance Attrition Rate]` | `0.0%` | `31.3%` |
| 3 | **Low Job Satisfaction Staff** | `[Low Job Satisfaction Count]` | `#,##0` | `289` |
| 4 | **Low Environment Sat. Staff** | `[Low Environment Satisfaction Count]` | `#,##0` | `284` |
| 5 | **High Involvement Retention** | Filtered `[Retention Rate]` (High/V.High) | `0.0%` | `85.4%` |

##### 2. Visual Charts
- **Chart 1: 4-Pillar Sentiment Attrition Matrix**
  - *Visual Type*: Grouped Column or 100% Stacked Bar.
  - *Categories*: Ratings across `JobSatisfaction`, `EnvironmentSatisfaction`, `RelationshipSatisfaction`, `WorkLifeBalance` (ordered using companion `_Sort` keys).
  - *Values*: `[Attrition Rate]`.
- **Chart 2: Burnout Interaction Matrix (Work-Life Balance $\times$ OverTime)**
  - *Visual Type*: Matrix Visual with conditional color formatting.
  - *Rows*: `'HR_Data'[WorkLifeBalance]` (`Bad`, `Good`, `Better`, `Best`).
  - *Columns*: `'HR_Data'[OverTime]` (`Yes`, `No`).
  - *Values*: `[Attrition Rate]`.
  - *Highlight*: Bad WLB + OverTime = **45.5%** turnover.
- **Chart 3: Job Involvement vs. Attrition**
  - *Visual Type*: Column Chart.
  - *X-Axis*: `'HR_Data'[JobInvolvement]` (`Low`, `Medium`, `High`, `Very High`).
  - *Y-Axis*: `[Attrition Rate]`.
- **Chart 4: Supervisory Continuity Retention Curve**
  - *Visual Type*: Line Chart.
  - *X-Axis*: `'HR_Data'[YearsWithCurrManager]`.
  - *Y-Axis*: `[Attrition Rate]`.

##### 3. Slicers
- `Department` (Dropdown)
- `Job Role` (Dropdown)
- `WorkLifeBalance` (Multi-select)
- `EnvironmentSatisfaction` (Multi-select)

---

#### Page 5: HR Insights

- **Intended Business Question**: *"What are the strategic conclusions, risk hotspots, and phased recommendations that HR leadership should prioritize?"*
- **Recommended Layout**:
  - **Top Row (Executive Takeaways)**: 3 Structured Text/Card containers outlining the core statistical findings.
  - **Middle Section (Risk Hotspot Matrix)**: Cross-tabulation of highest-risk cohorts (Roles, Overtime, Low Income).
  - **Bottom Section (Phased Action Roadmap)**: 3-column structured action guide (Immediate 30–60d, Medium-Term 6–12m, Long-Term 12–24m).

##### 1. Visual Containers & Structured Cards
- **Container 1: Operational Strain & Overtime**
  - Highlights the **2.9x relative risk disparity** between overtime workers (30.5%) and non-overtime peers (10.4%).
  - Prompts immediate operational workload reviews in Sales and Laboratory teams.
- **Container 2: The First-24-Months Retention Window**
  - Emphasizes that **43.0% of all turnover** occurs within the first 2 years of tenure (29.8% attrition rate).
  - Recommends structured onboarding, 6-month check-ins, and mentorship.
- **Container 3: Entry Compensation & Equity Anchoring**
  - Documents the **$2,046 monthly salary gap** between departed ($4,787) and retained ($6,833) employees.
  - Documents that Stock Option Level 0 suffers **24.4% attrition** vs. **9.4%** for Level 1.
  - Outlines recommendations for entry wage benchmarking (<$3,000/mo) and micro-equity eligibility.

##### 2. Strategic Action Cards (Bottom Grid)
- *Card A (Immediate 30–60d)*: Overtime caps, workload rebalancing, and proactive stay interviews.
- *Card B (Medium-Term 6–12m)*: 18-month onboarding journey overhaul and entry wage adjustments.
- *Card C (Long-Term 12–24m)*: Career progression ladder redesign for Sales Representatives and Lab Techs.

---

### 3. Factual Validation Benchmark Tables

Use these exact empirical tables to audit and validate visual results inside Power BI:

#### Validation Table 1: Department Breakdown
| Department | Total Employees | Active Employees | Departures (Lost) | Attrition Rate | Average Monthly Income | Total Monthly Payroll |
| :--- | :-: | :-: | :-: | :-: | :-: | :-: |
| **Research & Development** | 961 | 828 | 133 | 13.84% | $6,281.25 | $6,036,277 |
| **Sales** | 446 | 354 | 92 | 20.63% | $6,959.17 | $3,103,791 |
| **Human Resources** | 63 | 51 | 12 | 19.05% | $6,654.51 | $419,241 |
| **Total Enterprise** | **1,470** | **1,233** | **237** | **16.12%** | **$6,502.93** | **$9,559,309** |

#### Validation Table 2: Job Role Breakdown
| Job Role | Headcount | Departures | Attrition Rate | Average Monthly Income | Total Monthly Payroll |
| :--- | :-: | :-: | :-: | :-: | :-: |
| **Sales Representative** | 83 | 33 | 39.76% | $2,626.00 | $217,958 |
| **Laboratory Technician** | 259 | 62 | 23.94% | $3,237.17 | $838,428 |
| **Human Resources** | 52 | 12 | 23.08% | $4,235.75 | $220,259 |
| **Sales Executive** | 326 | 57 | 17.48% | $6,924.28 | $2,257,314 |
| **Research Scientist** | 292 | 47 | 16.10% | $3,239.97 | $946,072 |
| **Manufacturing Director** | 145 | 10 | 6.90% | $7,295.14 | $1,057,795 |
| **Healthcare Representative** | 131 | 9 | 6.87% | $7,528.76 | $986,267 |
| **Manager** | 102 | 5 | 4.90% | $17,181.68 | $1,752,531 |
| **Research Director** | 80 | 2 | 2.50% | $16,033.55 | $1,282,684 |
| **Total** | **1,470** | **237** | **16.12%** | **$6,502.93** | **$9,559,309** |

#### Validation Table 3: Overtime Impact
| OverTime Status | Headcount | Departures | Attrition Rate | Average Monthly Income | Total Annual Payroll |
| :--- | :-: | :-: | :-: | :-: | :-: |
| **No** | 1,054 | 110 | 10.44% | $6,484.93 | $82,020,936 |
| **Yes** | 416 | 127 | 30.53% | $6,548.55 | $32,690,772 |
| **Total** | **1,470** | **237** | **16.12%** | **$6,502.93** | **$114,711,708** |

#### Validation Table 4: Tenure Bracket Breakdown
| Company Tenure Bracket | Headcount | Departures | Attrition Rate | Average Monthly Income |
| :--- | :-: | :-: | :-: | :-: |
| **0 Years (< 1 Year)** | 44 | 16 | 36.36% | $4,113.50 |
| **1–2 Years** | 298 | 86 | 28.86% | $4,797.31 |
| **3–5 Years** | 434 | 60 | 13.82% | $5,364.89 |
| **6–10 Years** | 448 | 55 | 12.28% | $6,567.37 |
| **11+ Years** | 246 | 20 | 8.13% | $10,886.87 |
| **Total** | **1,470** | **237** | **16.12%** | **$6,502.93** |

#### Validation Table 5: Salary Band Breakdown
| Salary Band | Headcount | Departures | Attrition Rate | Average Monthly Income |
| :--- | :-: | :-: | :-: | :-: |
| **1. < $3,000** | 395 | 113 | 28.61% | $2,394.07 |
| **2. $3,000–$4,999** | 354 | 50 | 14.12% | $4,077.16 |
| **3. $5,000–$7,999** | 340 | 34 | 10.00% | $6,155.27 |
| **4. $8,000–$11,999** | 186 | 29 | 15.59% | $9,889.62 |
| **5. $12,000+** | 195 | 11 | 5.64% | $16,605.50 |
| **Total** | **1,470** | **237** | **16.12%** | **$6,502.93** |

#### Validation Table 6: Work-Life Balance $\times$ OverTime Interaction
| Work-Life Balance | OverTime | Headcount | Departures | Attrition Rate |
| :--- | :--- | :-: | :-: | :-: |
| **Bad** | No | 58 | 15 | 25.86% |
| **Bad** | **Yes** | **22** | **10** | **45.45%** |
| **Good** | No | 240 | 26 | 10.83% |
| **Good** | Yes | 104 | 32 | 30.77% |
| **Better** | No | 639 | 54 | 8.45% |
| **Better** | Yes | 254 | 73 | 28.74% |
| **Best** | No | 117 | 15 | 12.82% |
| **Best** | Yes | 36 | 12 | 33.33% |
