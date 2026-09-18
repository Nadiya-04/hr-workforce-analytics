# HR Workforce, Attrition & Cost Analytics
## Comprehensive Data Dictionary

This document serves as the foundational data catalog and schema reference for the **"HR Workforce, Attrition & Cost Analytics Dashboard"** portfolio project. It details all variables present in the cleaned and revised dataset (`data/IBM-HR-Analytics-Employee-Attrition-and-Performance-Revised.csv`).

---

### Dataset Summary

| Metric | Value | Description |
| :--- | :--- | :--- |
| **Total Records (Rows)** | 1,470 | Individual employee records (snapshot grain) |
| **Total Attributes (Columns)** | 31 | Workforce variables across demographics, roles, finance, and sentiment |
| **Missing / Null Values** | 0 (0.0%) | Complete dataset with zero null or empty fields |
| **Duplicate Rows** | 0 | Every record represents a distinct employee profile |
| **Target Variable** | `Attrition` | Binary outcome ('Yes': 237 [16.1%], 'No': 1,233 [83.9%]) |
| **Pre-Filtered Benchmark Columns** | 4 removed | Uninformative constants (`EmployeeCount`, `Over18`, `StandardHours`) and identifier (`EmployeeNumber`) |

---

### Master Data Dictionary Table

| # | Column Name | Meaning & Business Definition | Raw Type | Power BI / DAX Data Type | Analytical Category | Values / Range |
| :-: | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | `Age` | Chronological age of the employee in years. | Integer | Whole Number (`Int64`) | Workforce & Demographics | 18 – 60 years (Mean: 36.9) |
| 2 | `Attrition` | **Target outcome**: Whether the employee left the organization (voluntary or involuntary departure). | String | Text (`String`) | Attrition Outcome | `Yes` (237, 16.1%), `No` (1,233, 83.9%) |
| 3 | `BusinessTravel` | Required frequency of corporate business travel. | String | Text (`String`) | Employment & Role | `Non-Travel` (10.2%), `Travel_Rarely` (71.0%), `Travel_Frequently` (18.8%) |
| 4 | `DailyRate` | Daily billing / compensation benchmark rate. | Integer | Whole Number / Currency | Compensation & Financials | $102 – $1,499 (Mean: $802.5) |
| 5 | `Department` | Primary organizational division / department. | String | Text (`String`) | Employment & Role | `Human Resources` (4.3%), `Research & Development` (65.4%), `Sales` (30.3%) |
| 6 | `DistanceFromHome` | One-way commuting distance from residence to workplace (in miles/km). | Integer | Whole Number (`Int64`) | Employment & Role | 1 – 29 miles (Mean: 9.2, Median: 7.0) |
| 7 | `Education` | Highest formal level of academic qualification completed. | String | Text (Ordinal) | Workforce & Demographics | `Below College`, `College`, `Bachelor`, `Master`, `Doctor` |
| 8 | `EducationField` | Academic discipline or major field of study. | String | Text (`String`) | Workforce & Demographics | `Life Sciences` (41.2%), `Medical` (31.6%), `Marketing` (10.8%), `Technical Degree` (9.0%), `Other` (5.6%), `Human Resources` (1.8%) |
| 9 | `EnvironmentSatisfaction` | Survey rating of physical workspace, culture, and environmental conditions. | String | Text (Ordinal) | Employee Sentiment | `Low`, `Medium`, `High`, `Very High` |
| 10 | `Gender` | Biological or reported gender identity of the employee. | String | Text (`String`) | Workforce & Demographics | `Female` (588, 40.0%), `Male` (882, 60.0%) |
| 11 | `HourlyRate` | Assigned hourly labor / compensation rate. | Integer | Whole Number / Currency | Compensation & Financials | $30 – $100 (Mean: $65.9) |
| 12 | `JobInvolvement` | Degree of mental and emotional dedication to current job responsibilities. | String | Text (Ordinal) | Employee Sentiment | `Low`, `Medium`, `High`, `Very High` |
| 13 | `JobLevel` | Organizational seniority grade / tier within company hierarchy. | String | Text (Ordinal) | Employment & Role | `Entry Level` (36.9%), `Junior Level` (36.3%), `Mid Level` (14.8%), `Senior Level` (7.2%), `Executive Level` (4.7%) |
| 14 | `JobRole` | Specific functional position and job title. | String | Text (`String`) | Employment & Role | 9 distinct roles (e.g., `Sales Executive`, `Research Scientist`, `Laboratory Technician`) |
| 15 | `JobSatisfaction` | Overall level of satisfaction and morale regarding day-to-day job duties. | String | Text (Ordinal) | Employee Sentiment | `Low`, `Medium`, `High`, `Very High` |
| 16 | `MaritalStatus` | Current legal marital status of the employee. | String | Text (`String`) | Workforce & Demographics | `Single` (32.0%), `Married` (45.8%), `Divorced` (22.2%) |
| 17 | `MonthlyIncome` | Gross monthly earnings / salary in USD. | Integer | Currency (`Decimal`) | Compensation & Financials | $1,009 – $19,999 (Mean: $6,502.9, Median: $4,930) |
| 18 | `MonthlyRate` | Monthly operational expense / internal billing allocation rate. | Integer | Currency (`Decimal`) | Compensation & Financials | $2,094 – $26,999 (Mean: $14,313.1) |
| 19 | `NumCompaniesWorked` | Number of previous corporate employers prior to joining this company. | Integer | Whole Number (`Int64`) | Employment & Role | 0 – 9 companies (Mean: 2.7, Mode: 1) |
| 20 | `OverTime` | Indicates whether the employee regularly works overtime hours. | String | Text / Binary | Employment & Role | `No` (1,054, 71.7%), `Yes` (416, 28.3%) |
| 21 | `PercentSalaryHike` | Percentage increase awarded during the most recent salary evaluation. | Integer | Percentage / Whole Number | Compensation & Financials | 11% – 25% (Mean: 15.2%, Median: 14%) |
| 22 | `PerformanceRating` | Most recent formal performance evaluation score. | String | Text (Ordinal) | Employee Sentiment | `Excellent` (84.6%), `Outstanding` (15.4%) *(Note: ratings 1 & 2 unobserved)* |
| 23 | `RelationshipSatisfaction` | Rating of interpersonal dynamics with colleagues and supervisors. | String | Text (Ordinal) | Employee Sentiment | `Low`, `Medium`, `High`, `Very High` |
| 24 | `StockOptionLevel` | Corporate equity incentive tier (0 = none, 3 = maximum allocation). | Integer | Whole Number (`Int64`) | Compensation & Financials | 0 (42.9%), 1 (40.5%), 2 (10.7%), 3 (5.8%) |
| 25 | `TotalWorkingYears` | Total cumulative years of professional work experience across career. | Integer | Whole Number (`Int64`) | Employment & Role | 0 – 40 years (Mean: 11.3, Median: 10.0) |
| 26 | `TrainingTimesLastYear` | Number of formal training sessions attended in the prior calendar year. | Integer | Whole Number (`Int64`) | Employment & Role | 0 – 6 sessions (Mean: 2.8, Median: 3.0) |
| 27 | `WorkLifeBalance` | Perceived balance between job commitments and personal life. | String | Text (Ordinal) | Employee Sentiment | `Bad` (5.4%), `Good` (23.4%), `Better` (60.7%), `Best` (10.4%) |
| 28 | `YearsAtCompany` | Continuous tenure (in completed years) with the present company. | Integer | Whole Number (`Int64`) | Tenure & Mobility | 0 – 40 years (Mean: 7.0, Median: 5.0) |
| 29 | `YearsInCurrentRole` | Number of years served in the current specific job role. | Integer | Whole Number (`Int64`) | Tenure & Mobility | 0 – 18 years (Mean: 4.2, Median: 3.0) |
| 30 | `YearsSinceLastPromotion` | Years elapsed since the last formal organizational promotion. | Integer | Whole Number (`Int64`) | Tenure & Mobility | 0 – 15 years (Mean: 2.2, Median: 1.0) |
| 31 | `YearsWithCurrManager` | Number of years reporting directly to the current supervisor/manager. | Integer | Whole Number (`Int64`) | Tenure & Mobility | 0 – 17 years (Mean: 4.1, Median: 3.0) |

---

### Detailed Breakdown by Analytical Category

#### 1. Workforce & Demographics (5 Columns)
Provides demographic foundation to evaluate diversity, equity, age distribution, and academic backgrounds across the organization:
- **`Age`**: Continuous integer (18–60). Ideal for cohort binning (e.g., `<25`, `25-34`, `35-44`, `45-54`, `55+`).
- **`Gender`**: Binary categorical (`Female`: 40%, `Male`: 60%). Used for pay equity and representation analytics.
- **`MaritalStatus`**: 3 tiers (`Single`, `Married`, `Divorced`). Critical demographic variable strongly correlated with mobility and turnover.
- **`Education`**: Ordinal qualification level (`Below College`, `College`, `Bachelor`, `Master`, `Doctor`).
- **`EducationField`**: Functional academic discipline (`Life Sciences`, `Medical`, `Marketing`, `Technical Degree`, `Human Resources`, `Other`).

#### 2. Employment & Job Hierarchy (9 Columns)
Defines structural role attributes, organizational alignment, operational duties, and commute burdens:
- **`Department`**: Core divisional alignment (`Research & Development`: 65.4%, `Sales`: 30.3%, `Human Resources`: 4.3%).
- **`JobRole`**: Granular operational title (9 roles). Highest headcount in `Sales Executive` (326), `Research Scientist` (292), and `Laboratory Technician` (259).
- **`JobLevel`**: 5-tier organizational hierarchy (`Entry Level` to `Executive Level`).
- **`BusinessTravel`**: Travel intensity indicator (`Travel_Frequently`, `Travel_Rarely`, `Non-Travel`).
- **`DistanceFromHome`**: Commute distance in miles/km (1–29). Used to evaluate commute strain on employee turnover.
- **`TotalWorkingYears`**: Total industry/professional experience (0–40 years).
- **`NumCompaniesWorked`**: Job-hopping indicator (0–9 prior companies).
- **`TrainingTimesLastYear`**: Professional development investment (0–6 sessions).
- **`OverTime`**: Binary operational stressor (`Yes`: 28.3%, `No`: 71.7%).

#### 3. Tenure & Mobility (4 Columns)
Measures organizational loyalty, internal career velocity, and supervisory continuity:
- **`YearsAtCompany`**: Total company longevity (0–40 years; average 7.0 years).
- **`YearsInCurrentRole`**: Stagnation / role stability indicator (average 4.2 years).
- **`YearsSinceLastPromotion`**: Career mobility indicator; identifies employees overdue for career growth (0–15 years; median 1 year).
- **`YearsWithCurrManager`**: Leadership relationship stability (average 4.1 years).

#### 4. Compensation & Financials (6 Columns)
Enables payroll modeling, compensation fairness analysis, and turnover financial impact assessment:
- **`MonthlyIncome`**: Core salary variable ($1,009 to $19,999; total monthly payroll: $9,559,309; annual payroll: $114.71M).
- **`DailyRate`**, **`HourlyRate`**, **`MonthlyRate`**: Auxiliary operational labor billing benchmarks.
- **`PercentSalaryHike`**: Annual merit/increment percentage (11% to 25%).
- **`StockOptionLevel`**: Long-term equity retention incentive (levels 0 to 3).

#### 5. Employee Sentiment & Performance (6 Columns)
Captures psychometric feedback, cultural climate, and formal appraisal ratings:
- **`EnvironmentSatisfaction`**: Workplace condition and cultural sentiment rating (`Low` to `Very High`).
- **`JobSatisfaction`**: Daily role enjoyment and morale (`Low` to `Very High`).
- **`RelationshipSatisfaction`**: Peer and supervisor collaboration climate (`Low` to `Very High`).
- **`JobInvolvement`**: Dedication and ownership of work deliverables (`Low` to `Very High`).
- **`WorkLifeBalance`**: Work/life equilibrium rating (`Bad`, `Good`, `Better`, `Best`).
- **`PerformanceRating`**: Appraisal score (`Excellent` [84.6%], `Outstanding` [15.4%]).

#### 6. Attrition Outcome (Target Variable) (1 Column)
- **`Attrition`**: Binary flag indicating turnover (`Yes` = 237, `No` = 1,233; benchmark turnover rate = **16.12%**). Serves as the primary dependent variable for churn analytics, flight risk profiling, and cost estimation.

---

### Key Data Modeling & Power BI Considerations

1. **Ordinal Sorting Requirement**:
   Categorical ratings (`Education`, `JobLevel`, `EnvironmentSatisfaction`, `JobSatisfaction`, `JobInvolvement`, `RelationshipSatisfaction`, `WorkLifeBalance`, `PerformanceRating`) are text labels in this revised CSV. In Power BI, an explicit sort-by-column or integer dimension table must be created in Power Query/DAX to prevent default alphabetical ordering (e.g., ensuring "Entry Level" precedes "Mid Level" and "Bad" precedes "Best").
2. **Pre-Cleaned State**:
   The 4 uninformative constant columns from the original IBM raw dataset (`EmployeeCount`, `Over18`, `StandardHours`, `EmployeeNumber`) are already omitted. Zero rows have missing or null values.
3. **Imbalanced Target Class**:
   At 16.12% turnover, attrition is naturally imbalanced. Proportional KPIs (`Attrition Rate %`) and cohort rate comparisons should be prioritized over raw counts.
