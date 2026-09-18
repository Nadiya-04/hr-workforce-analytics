# HR Workforce, Attrition & Cost Analytics
## Data Quality Assessment & Profiling Report

This document records the empirical data quality analysis, statistical validation, and integrity checks performed on the dataset (`data/IBM-HR-Analytics-Employee-Attrition-and-Performance-Revised.csv`).

---

### 1. Executive Summary & Data Health Scorecard

| Dimension | Assessment | Status | Notes |
| :--- | :---: | :---: | :--- |
| **Completeness** | 100.0% | Pass | 0 missing, null, blank, or NaN values across all 1,470 rows and 31 columns. |
| **Uniqueness** | 100.0% | Pass | 0 duplicate rows detected across all attributes. |
| **Domain Integrity** | 100.0% | Pass | All numerical values fall within realistic corporate HR ranges. |
| **Cross-Field Consistency** | 100.0% | Pass | All tenure, promotion, and supervisory logic checks passed with 0 violations. |
| **Pre-Cleaned Status** | 100.0% | Pass | 4 uninformative benchmark attributes (`EmployeeCount`, `Over18`, `StandardHours`, `EmployeeNumber`) were removed in this revised dataset. |

---

### 2. Dataset Dimensions & Schema Overview

- **Row Count**: 1,470 employee records
- **Column Count**: 31 columns
- **File Format**: UTF-8 Comma-Separated Values (CSV)
- **Granularity**: One record per individual employee snapshot

---

### 3. Missing Value & Null Analysis

A comprehensive audit was executed across all 31 fields to detect missing values, whitespace-only entries, null markers (`NA`, `NULL`, `none`), and empty strings.

| Column | Data Type | Record Count | Missing / Null Count | Missing % |
| :--- | :--- | :-: | :-: | :-: |
| `Age` | Integer | 1,470 | 0 | 0.0% |
| `Attrition` | String (Categorical) | 1,470 | 0 | 0.0% |
| `BusinessTravel` | String (Categorical) | 1,470 | 0 | 0.0% |
| `DailyRate` | Integer | 1,470 | 0 | 0.0% |
| `Department` | String (Categorical) | 1,470 | 0 | 0.0% |
| `DistanceFromHome` | Integer | 1,470 | 0 | 0.0% |
| `Education` | String (Ordinal) | 1,470 | 0 | 0.0% |
| `EducationField` | String (Categorical) | 1,470 | 0 | 0.0% |
| `EnvironmentSatisfaction` | String (Ordinal) | 1,470 | 0 | 0.0% |
| `Gender` | String (Categorical) | 1,470 | 0 | 0.0% |
| `HourlyRate` | Integer | 1,470 | 0 | 0.0% |
| `JobInvolvement` | String (Ordinal) | 1,470 | 0 | 0.0% |
| `JobLevel` | String (Ordinal) | 1,470 | 0 | 0.0% |
| `JobRole` | String (Categorical) | 1,470 | 0 | 0.0% |
| `JobSatisfaction` | String (Ordinal) | 1,470 | 0 | 0.0% |
| `MaritalStatus` | String (Categorical) | 1,470 | 0 | 0.0% |
| `MonthlyIncome` | Integer | 1,470 | 0 | 0.0% |
| `MonthlyRate` | Integer | 1,470 | 0 | 0.0% |
| `NumCompaniesWorked` | Integer | 1,470 | 0 | 0.0% |
| `OverTime` | String (Binary) | 1,470 | 0 | 0.0% |
| `PercentSalaryHike` | Integer | 1,470 | 0 | 0.0% |
| `PerformanceRating` | String (Ordinal) | 1,470 | 0 | 0.0% |
| `RelationshipSatisfaction` | String (Ordinal) | 1,470 | 0 | 0.0% |
| `StockOptionLevel` | Integer | 1,470 | 0 | 0.0% |
| `TotalWorkingYears` | Integer | 1,470 | 0 | 0.0% |
| `TrainingTimesLastYear` | Integer | 1,470 | 0 | 0.0% |
| `WorkLifeBalance` | String (Ordinal) | 1,470 | 0 | 0.0% |
| `YearsAtCompany` | Integer | 1,470 | 0 | 0.0% |
| `YearsInCurrentRole` | Integer | 1,470 | 0 | 0.0% |
| `YearsSinceLastPromotion` | Integer | 1,470 | 0 | 0.0% |
| `YearsWithCurrManager` | Integer | 1,470 | 0 | 0.0% |

**Conclusion**: The dataset displays 100% completeness with zero missing records. No data imputation is required.

---

### 4. Cross-Field Logical Consistency Validation

To ensure structural plausibility, a suite of relational logic tests was executed:

| Test Assertion | Condition Evaluated | Violations Found | Result |
| :--- | :--- | :-: | :---: |
| **Company Tenure $\le$ Total Experience** | `YearsAtCompany > TotalWorkingYears` | 0 / 1,470 | Pass |
| **Role Tenure $\le$ Company Tenure** | `YearsInCurrentRole > YearsAtCompany` | 0 / 1,470 | Pass |
| **Manager Tenure $\le$ Company Tenure** | `YearsWithCurrManager > YearsAtCompany` | 0 / 1,470 | Pass |
| **Promotion Latency $\le$ Company Tenure** | `YearsSinceLastPromotion > YearsAtCompany` | 0 / 1,470 | Pass |
| **Age vs. Total Experience Plausibility** | `Age - TotalWorkingYears < 16` | 0 / 1,470 | Pass |
| **Non-Negative Experience & Financials** | Min values for all tenure & salary fields $\ge 0$ | 0 / 1,470 | Pass |

All cross-field relationship constraints hold with 100% integrity.

---

### 5. Categorical Distributions & Cardinality

| Column Name | Cardinality | Observed Distinct Values | Distribution Breakdown |
| :--- | :-: | :--- | :--- |
| `Attrition` | 2 | `No`, `Yes` | No: 1,233 (83.9%), Yes: 237 (16.1%) |
| `Department` | 3 | `Human Resources`, `Research & Development`, `Sales` | R&D: 961 (65.4%), Sales: 446 (30.3%), HR: 63 (4.3%) |
| `BusinessTravel` | 3 | `Non-Travel`, `Travel_Frequently`, `Travel_Rarely` | Rarely: 1,043 (71.0%), Frequently: 277 (18.8%), Non-Travel: 150 (10.2%) |
| `Gender` | 2 | `Female`, `Male` | Male: 882 (60.0%), Female: 588 (40.0%) |
| `MaritalStatus` | 3 | `Divorced`, `Married`, `Single` | Married: 673 (45.8%), Single: 470 (32.0%), Divorced: 327 (22.2%) |
| `OverTime` | 2 | `No`, `Yes` | No: 1,054 (71.7%), Yes: 416 (28.3%) |
| `Education` | 5 | `Below College`, `College`, `Bachelor`, `Master`, `Doctor` | Bachelor: 38.9%, Master: 27.1%, College: 19.2%, Below College: 11.6%, Doctor: 3.3% |
| `EducationField` | 6 | `Life Sciences`, `Medical`, `Marketing`, `Technical Degree`, `Other`, `Human Resources` | Life Sciences: 41.2%, Medical: 31.6%, Marketing: 10.8%, Technical: 9.0%, Other: 5.6%, HR: 1.8% |
| `JobRole` | 9 | 9 operational functions | Sales Exec (22.2%), Research Scientist (19.9%), Lab Tech (17.6%), Mfg Dir (9.9%), Healthcare Rep (8.9%), Manager (6.9%), Sales Rep (5.6%), Research Dir (5.4%), HR (3.5%) |
| `JobLevel` | 5 | `Entry Level`, `Junior Level`, `Mid Level`, `Senior Level`, `Executive Level` | Entry: 36.9%, Junior: 36.3%, Mid: 14.8%, Senior: 7.2%, Executive: 4.7% |
| `EnvironmentSatisfaction` | 4 | `Low`, `Medium`, `High`, `Very High` | High: 30.8%, Very High: 30.3%, Medium: 19.5%, Low: 19.3% |
| `JobSatisfaction` | 4 | `Low`, `Medium`, `High`, `Very High` | Very High: 31.2%, High: 30.1%, Low: 19.7%, Medium: 19.0% |
| `RelationshipSatisfaction` | 4 | `Low`, `Medium`, `High`, `Very High` | High: 31.2%, Very High: 29.4%, Medium: 20.6%, Low: 18.8% |
| `JobInvolvement` | 4 | `Low`, `Medium`, `High`, `Very High` | High: 59.0%, Medium: 25.5%, Very High: 9.8%, Low: 5.6% |
| `WorkLifeBalance` | 4 | `Bad`, `Good`, `Better`, `Best` | Better: 60.7%, Good: 23.4%, Best: 10.4%, Bad: 5.4% |
| `PerformanceRating` | 2 | `Excellent`, `Outstanding` | Excellent: 1,244 (84.6%), Outstanding: 226 (15.4%) |

---

### 6. Potential Data-Quality Nuances & Modeling Guidelines

While the dataset is structurally clean, several domain nuances must be addressed during ETL and Power BI data modeling:

1. **Ordinal Sorting Requirement (Critical for Visuals)**:
   - **Issue**: Categorical ratings (`WorkLifeBalance`, `JobSatisfaction`, `JobLevel`, `Education`) are stored as text strings. By default, Power BI sorts categories alphabetically (e.g., sorting `Bad` $\rightarrow$ `Best` $\rightarrow$ `Better` $\rightarrow$ `Good`, or `Entry Level` $\rightarrow$ `Executive Level`).
   - **Resolution**: In Power Query / DAX, build an explicit companion sort-key column (or dedicated dimension table) so visual axes order logically:
     - `WorkLifeBalance`: Bad (1), Good (2), Better (3), Best (4)
     - `Satisfaction`: Low (1), Medium (2), High (3), Very High (4)
     - `JobLevel`: Entry Level (1), Junior Level (2), Mid Level (3), Senior Level (4), Executive Level (5)
     - `Education`: Below College (1), College (2), Bachelor (3), Master (4), Doctor (5)
2. **Performance Rating Skewness**:
   - **Issue**: Only 2 values exist in `PerformanceRating` (`Excellent` [equivalent to numeric 3] and `Outstanding` [equivalent to numeric 4]). There are zero records representing lower performance ratings (1 or 2).
   - **Resolution**: Note this appraisal rating inflation in documentation and avoid claiming underperformance as an attrition driver.
3. **Imbalanced Attrition Target**:
   - **Issue**: Turnover represents 16.12% of the dataset (237 positive cases vs. 1,233 negative cases).
   - **Resolution**: Use normalized percentage metrics (`Attrition Rate %` = `DIVIDE([Departed Employees], [Total Employees], 0)`) rather than raw counts when comparing cohorts across departments or roles.
4. **String Formatting Consistency**:
   - **Issue**: The `BusinessTravel` column uses underscores (`Travel_Frequently`, `Travel_Rarely`).
   - **Resolution**: In Power Query ETL, apply a clean text replacement (`Travel Frequently`, `Travel Rarely`) for clean executive presentation.
