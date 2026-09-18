# HR Workforce, Attrition & Cost Analytics
## Comprehensive Data Quality Assessment & Profiling Report

This document delivers a thorough, empirical data quality assessment of the IBM HR dataset (`data/IBM-HR-Analytics-Employee-Attrition-and-Performance-Revised.csv`). Every finding in this report is supported by verifiable statistical audits conducted directly on the source data.

---

### 1. Executive Summary & Quality Scorecard

| Quality Dimension | Assessment Result | Status | Key Observation |
| :--- | :---: | :---: | :--- |
| **Completeness** | 100.0% | **Pass** | 0 missing, null, blank, or NaN values across 1,470 rows $\times$ 31 columns (45,570 data points). |
| **Record Uniqueness** | 100.0% | **Pass** | 0 duplicate records. All 1,470 rows are completely distinct. |
| **Employee Identifier** | Non-Existent | **Noted** | No explicit `EmployeeID` or `EmployeeNumber` column exists in this revised CSV. |
| **Profile Distinctness** | 100.0% | **Pass** | Multi-attribute composite test on biographical fields yielded 1,470 unique employee profiles (0 collisions). |
| **Text Consistency** | 100.0% | **Pass** | 0 leading/trailing whitespace errors; 0 mixed-case discrepancies across all categorical fields. |
| **Numerical Validity** | 100.0% | **Pass** | 0 negative values; all tenure, promotion, and experience relationships satisfy strict relational logic. |
| **Statistical Outliers** | Valid Domain Skew | **Documented** | Right-skewed distribution in `MonthlyIncome` (114 high earners) confirmed to be 100% legitimate executive/director roles. |

---

### 2. Missing Value & Completeness Audit

A row-by-row and column-by-column inspection was conducted across all 31 attributes.

| # | Column Name | Raw Data Type | Total Records | Null / Blank Count | Completeness Rate |
| :-: | :--- | :--- | :-: | :-: | :-: |
| 1 | `Age` | Integer | 1,470 | 0 | 100.0% |
| 2 | `Attrition` | String (Categorical) | 1,470 | 0 | 100.0% |
| 3 | `BusinessTravel` | String (Categorical) | 1,470 | 0 | 100.0% |
| 4 | `DailyRate` | Integer | 1,470 | 0 | 100.0% |
| 5 | `Department` | String (Categorical) | 1,470 | 0 | 100.0% |
| 6 | `DistanceFromHome` | Integer | 1,470 | 0 | 100.0% |
| 7 | `Education` | String (Ordinal) | 1,470 | 0 | 100.0% |
| 8 | `EducationField` | String (Categorical) | 1,470 | 0 | 100.0% |
| 9 | `EnvironmentSatisfaction` | String (Ordinal) | 1,470 | 0 | 100.0% |
| 10 | `Gender` | String (Categorical) | 1,470 | 0 | 100.0% |
| 11 | `HourlyRate` | Integer | 1,470 | 0 | 100.0% |
| 12 | `JobInvolvement` | String (Ordinal) | 1,470 | 0 | 100.0% |
| 13 | `JobLevel` | String (Ordinal) | 1,470 | 0 | 100.0% |
| 14 | `JobRole` | String (Categorical) | 1,470 | 0 | 100.0% |
| 15 | `JobSatisfaction` | String (Ordinal) | 1,470 | 0 | 100.0% |
| 16 | `MaritalStatus` | String (Categorical) | 1,470 | 0 | 100.0% |
| 17 | `MonthlyIncome` | Integer | 1,470 | 0 | 100.0% |
| 18 | `MonthlyRate` | Integer | 1,470 | 0 | 100.0% |
| 19 | `NumCompaniesWorked` | Integer | 1,470 | 0 | 100.0% |
| 20 | `OverTime` | String (Categorical) | 1,470 | 0 | 100.0% |
| 21 | `PercentSalaryHike` | Integer | 1,470 | 0 | 100.0% |
| 22 | `PerformanceRating` | String (Ordinal) | 1,470 | 0 | 100.0% |
| 23 | `RelationshipSatisfaction` | String (Ordinal) | 1,470 | 0 | 100.0% |
| 24 | `StockOptionLevel` | Integer | 1,470 | 0 | 100.0% |
| 25 | `TotalWorkingYears` | Integer | 1,470 | 0 | 100.0% |
| 26 | `TrainingTimesLastYear` | Integer | 1,470 | 0 | 100.0% |
| 27 | `WorkLifeBalance` | String (Ordinal) | 1,470 | 0 | 100.0% |
| 28 | `YearsAtCompany` | Integer | 1,470 | 0 | 100.0% |
| 29 | `YearsInCurrentRole` | Integer | 1,470 | 0 | 100.0% |
| 30 | `YearsSinceLastPromotion` | Integer | 1,470 | 0 | 100.0% |
| 31 | `YearsWithCurrManager` | Integer | 1,470 | 0 | 100.0% |

> [!NOTE]
> **Audit Conclusion**: With 0 missing values detected, no statistical imputation, default replacement, or row dropping is required.

---

### 3. Record & Entity Duplication Audit

#### A. Global Row Duplication
- An evaluation across the full 31-column vector identified **0 duplicate rows**.
- Total unique rows: **1,470 / 1,470 (100.0%)**.

#### B. Employee Identifier Verification
- **Explicit Identifier**: No explicit identifier column (`EmployeeNumber` or `EmployeeID`) is present in this revised dataset. In the original raw benchmark, `EmployeeNumber` existed but was removed during initial revision alongside the uninformative constants (`EmployeeCount`, `Over18`, `StandardHours`).
- **Policy Compliance**: In accordance with the requirement to *not create fake employee IDs*, no synthetic surrogate IDs will be invented in the raw data.
- **Biographical Composite Uniqueness**: To verify whether multiple records might represent the same employee, a composite key was evaluated across 7 core biographical and career attributes:
  $$\text{Composite Key} = [\text{Age}, \text{Gender}, \text{Department}, \text{JobRole}, \text{TotalWorkingYears}, \text{MonthlyIncome}, \text{YearsAtCompany}]$$
  - **Result**: Exactly **1,470 unique combinations** out of 1,470 records.
  - **Conclusion**: There are no duplicate employee profiles in the dataset.

---

### 4. Categorical Consistency & Text Cleanliness

Every text column was audited for leading/trailing whitespace, unexpected delimiters, casing divergence, and irregular category codes.

| Column Name | Distinct Values | Case-Insensitive Count | Whitespace Discrepancies | Quality Observations |
| :--- | :-: | :-: | :-: | :--- |
| `Attrition` | 2 | 2 | 0 | Clean: `No` (1,233), `Yes` (237) |
| `BusinessTravel` | 3 | 3 | 0 | Underscores present: `Travel_Frequently` (277), `Travel_Rarely` (1,043), `Non-Travel` (150). Recommended for cosmetic replacement in ETL. |
| `Department` | 3 | 3 | 0 | Clean: `Human Resources` (63), `Research & Development` (961), `Sales` (446) |
| `Education` | 5 | 5 | 0 | Clean ordinal text: `Below College`, `College`, `Bachelor`, `Master`, `Doctor` |
| `EducationField` | 6 | 6 | 0 | Clean: `Human Resources`, `Life Sciences`, `Marketing`, `Medical`, `Other`, `Technical Degree` |
| `EnvironmentSatisfaction` | 4 | 4 | 0 | Clean ordinal text: `Low`, `Medium`, `High`, `Very High` |
| `Gender` | 2 | 2 | 0 | Clean: `Female` (588), `Male` (882) |
| `JobInvolvement` | 4 | 4 | 0 | Clean ordinal text: `Low`, `Medium`, `High`, `Very High` |
| `JobLevel` | 5 | 5 | 0 | Clean ordinal text: `Entry Level`, `Junior Level`, `Mid Level`, `Senior Level`, `Executive Level` |
| `JobRole` | 9 | 9 | 0 | Clean: 9 distinct functional roles |
| `JobSatisfaction` | 4 | 4 | 0 | Clean ordinal text: `Low`, `Medium`, `High`, `Very High` |
| `MaritalStatus` | 3 | 3 | 0 | Clean: `Divorced`, `Married`, `Single` |
| `OverTime` | 2 | 2 | 0 | Clean binary text: `No` (1,054), `Yes` (416) |
| `PerformanceRating` | 2 | 2 | 0 | Only 2 categories present: `Excellent` (1,244), `Outstanding` (226). Ratings 1 and 2 are absent from the dataset. |
| `RelationshipSatisfaction` | 4 | 4 | 0 | Clean ordinal text: `Low`, `Medium`, `High`, `Very High` |
| `WorkLifeBalance` | 4 | 4 | 0 | Clean ordinal text: `Bad` (80), `Good` (344), `Better` (893), `Best` (153) |

---

### 5. Numerical Integrity & Logical Cross-Field Validation

#### A. Logical Cross-Field Relational Rules
To guarantee corporate validity, structural cross-field relationships were audited:

| Logical Rule Evaluated | Business Rationale | Invalidation Criteria | Violations Found | Result |
| :--- | :--- | :--- | :-: | :---: |
| **Company Tenure $\le$ Experience** | An employee cannot work at current firm longer than total career. | `YearsAtCompany > TotalWorkingYears` | 0 | **Pass (100%)** |
| **Role Tenure $\le$ Company Tenure** | Time in current role cannot exceed time at company. | `YearsInCurrentRole > YearsAtCompany` | 0 | **Pass (100%)** |
| **Manager Tenure $\le$ Company Tenure** | Time with manager cannot exceed time at company. | `YearsWithCurrManager > YearsAtCompany` | 0 | **Pass (100%)** |
| **Promotion Latency $\le$ Company Tenure** | Time since promotion cannot exceed time at company. | `YearsSinceLastPromotion > YearsAtCompany` | 0 | **Pass (100%)** |
| **Working Age Plausibility** | Legal working age boundary (assumed min starting age $\ge 16$). | `Age - TotalWorkingYears < 16` | 0 | **Pass (100%)** |

#### B. Audit of Zero Values in Numerical Fields
Zero values were analyzed to differentiate valid business states from placeholder corruption:

- `YearsSinceLastPromotion = 0` (581 records): **Valid**. Indicates employee received a promotion within the current calendar year (< 12 months ago).
- `YearsInCurrentRole = 0` (244 records): **Valid**. Indicates a recent internal job transition or newly onboarded hire.
- `YearsWithCurrManager = 0` (263 records): **Valid**. Indicates a newly assigned manager or recent organizational restructuring.
- `YearsAtCompany = 0` (44 records): **Valid**. Represents new hires in their first year of employment.
- `NumCompaniesWorked = 0` (197 records): **Valid**. Indicates the current company is the employee's first employer.
- `TotalWorkingYears = 0` (11 records): **Valid**. Fresh graduates/interns at the onset of their professional career.
- `StockOptionLevel = 0` (631 records): **Valid**. Entry/junior roles with no equity participation grant.
- `TrainingTimesLastYear = 0` (54 records): **Valid**. Employees who attended 0 formal training sessions during the review period.

---

### 6. Statistical Outlier Audit (Interquartile Range - IQR Analysis)

A statistical distribution audit was performed using the standard Tukey IQR method ($[\text{Q1} - 1.5 \times \text{IQR}, \text{Q3} + 1.5 \times \text{IQR}]$) across all 15 numerical variables:

| Column Name | Min | Q1 (25%) | Median | Q3 (75%) | Max | IQR | Lower Bound | Upper Bound | Outlier Count |
| :--- | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: |
| `Age` | 18 | 30.0 | 36.0 | 43.0 | 60 | 13.0 | 10.5 | 62.5 | **0** |
| `DailyRate` | 102 | 465.0 | 802.0 | 1157.0 | 1,499 | 692.0 | -573.0 | 2,195.0 | **0** |
| `DistanceFromHome` | 1 | 2.0 | 7.0 | 14.0 | 29 | 12.0 | -16.0 | 32.0 | **0** |
| `HourlyRate` | 30 | 48.0 | 66.0 | 84.0 | 100 | 36.0 | -6.0 | 138.0 | **0** |
| `MonthlyIncome` | 1,009 | 2,911.0 | 4,930.0 | 8,380.0 | 19,999 | 5,469.0 | -5,292.5 | 16,583.5 | **114 (High)** |
| `MonthlyRate` | 2,094 | 8,045.0 | 14,242.0 | 20,462.0 | 26,999 | 12,417.0 | -10,580.5 | 39,087.5 | **0** |
| `NumCompaniesWorked`| 0 | 1.0 | 2.0 | 4.0 | 9 | 3.0 | -3.5 | 8.5 | **52 (High)** |
| `PercentSalaryHike` | 11 | 12.0 | 14.0 | 18.0 | 25 | 6.0 | 3.0 | 27.0 | **0** |
| `StockOptionLevel` | 0 | 0.0 | 1.0 | 1.0 | 3 | 1.0 | -1.5 | 2.5 | **85 (High)** |
| `TotalWorkingYears` | 0 | 6.0 | 10.0 | 15.0 | 40 | 9.0 | -7.5 | 28.5 | **63 (High)** |
| `TrainingTimesLastYear`| 0 | 2.0 | 3.0 | 3.0 | 6 | 1.0 | 0.5 | 4.5 | **184 (High)** |
| `YearsAtCompany` | 0 | 3.0 | 5.0 | 9.0 | 40 | 6.0 | -6.0 | 18.0 | **104 (High)** |
| `YearsInCurrentRole`| 0 | 2.0 | 3.0 | 7.0 | 18 | 5.0 | -5.5 | 14.5 | **21 (High)** |
| `YearsSinceLastPromotion`| 0 | 0.0 | 1.0 | 3.0 | 15 | 3.0 | -4.5 | 7.5 | **107 (High)** |
| `YearsWithCurrManager`| 0 | 2.0 | 3.0 | 7.0 | 17 | 5.0 | -5.5 | 14.5 | **14 (High)** |

#### Deep-Dive: MonthlyIncome Outlier Investigation
- **Statistical finding**: 114 records exceed the upper IQR threshold of $16,583.50 (ranging from $16,799 to $19,999).
- **Empirical verification**:
  - **Job Roles**: 74 are `Manager` and 40 are `Research Director`.
  - **Job Levels**: 69 are `Executive Level` (Level 5) and 45 are `Senior Level` (Level 4).
- **Conclusion**: These high earners are not data anomalies or input errors; they accurately model executive compensation bands. **They must be retained in full.**

#### Deep-Dive: Promotion Latency Outliers
- **Statistical finding**: 107 records exhibit $\ge 8$ years since their last promotion.
- **Analytical relevance**: This is a direct operational indicator of career stagnation. In our attrition modeling, this cohort represents an important risk group for voluntary departure.

---

### 7. Power Query & ETL Transformation Recommendations

1. **Text Cleanup**: Replace underscores in `BusinessTravel` (`Travel_Frequently` $\rightarrow$ `Travel Frequently`, `Travel_Rarely` $\rightarrow$ `Travel Rarely`).
2. **Explicit Ordinal Sorting**: Create numerical sort keys (1–5) for `JobLevel`, `Education`, `WorkLifeBalance`, `JobSatisfaction`, `EnvironmentSatisfaction`, `JobInvolvement`, and `RelationshipSatisfaction` to prevent alphabetical chart misordering.
3. **Feature Binning**: Construct standardized cohort bins (`Age_Group`, `Tenure_Bracket`, `Distance_Bracket`, `Salary_Bracket`) in the prepared layer for intuitive dashboard filtering.
4. **Binary Calculations**: Generate binary numeric flags (`Attrition_Numeric` = 1/0, `OverTime_Numeric` = 1/0) to accelerate DAX aggregations.
