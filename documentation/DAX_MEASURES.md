# HR Workforce, Attrition & Cost Analytics
## Comprehensive DAX Measures Library & Semantic Layer

This document defines the analytical calculation layer for the Power BI data model. All measures are designed to be placed in a dedicated measure table named `_Measures` and organized into structured display folders.

---

### Data Model Architecture Context

- **Primary Fact Table**: `'HR_Data'` (or `'Employees'`)
- **Row Grain**: One record per individual employee snapshot (1,470 total rows)
- **Convention**: Explicit table references are used for columns (e.g., `'HR_Data'[MonthlyIncome]`), while unqualified brackets are used for DAX measures (e.g., `[Total Employees]`).
- **Formatting**: Formats are specified according to Power BI standard formatting strings.

---

### Folder 1: Workforce & Headcount Metrics (`01_Workforce_Metrics`)

#### 1. `Total Employees`
- **Business Meaning**: The total headcount of all employees recorded in the organization within the reporting period.
- **DAX Formula**:
  ```dax
  Total Employees = COUNTROWS('HR_Data')
  ```
- **Explanation**: Performs a scalar row count on the core employee table. Respects all active filter contexts (e.g., Department, Job Role, Age Group).
- **Format**: Whole Number (`#,##0`)
- **Baseline Value**: `1,470`
- **Assumptions**: Each row corresponds to exactly one distinct employee.

#### 2. `Active Employees`
- **Business Meaning**: The current retained workforce currently employed by the organization.
- **DAX Formula**:
  ```dax
  Active Employees = 
  CALCULATE(
      COUNTROWS('HR_Data'),
      'HR_Data'[Attrition] = "No"
  )
  ```
- **Explanation**: Modifies filter context using `CALCULATE` to count only employees whose `Attrition` status is `"No"`.
- **Format**: Whole Number (`#,##0`)
- **Baseline Value**: `1,233`
- **Assumptions**: Employees with `Attrition = "No"` are currently active and on company payroll.

#### 3. `Employees Lost`
- **Business Meaning**: The total volume of employees who have departed the organization (voluntary or involuntary turnover).
- **DAX Formula**:
  ```dax
  Employees Lost = 
  CALCULATE(
      COUNTROWS('HR_Data'),
      'HR_Data'[Attrition] = "Yes"
  )
  ```
- **Explanation**: Filters the employee dataset to records where `Attrition` equals `"Yes"`. Can also be expressed as `SUM('HR_Data'[Attrition_Numeric])`.
- **Format**: Whole Number (`#,##0`)
- **Baseline Value**: `237`
- **Assumptions**: Represents all confirmed employee separations recorded in the snapshot period.

#### 4. `Attrition Rate`
- **Business Meaning**: The proportion of the workforce that departed during the evaluation period.
- **DAX Formula**:
  ```dax
  Attrition Rate = 
  DIVIDE(
      [Employees Lost],
      [Total Employees],
      0
  )
  ```
- **Explanation**: Divides departures by total headcount. Uses `DIVIDE` with safe divide fallback `0` to prevent division-by-zero errors when visual filters return zero rows.
- **Format**: Percentage (`0.0%` or `0.00%`)
- **Baseline Value**: `16.12%`
- **Assumptions**: Represents gross attrition rate over the dataset observation window.

#### 5. `Retention Rate`
- **Business Meaning**: The percentage of employees retained by the organization.
- **DAX Formula**:
  ```dax
  Retention Rate = 
  DIVIDE(
      [Active Employees],
      [Total Employees],
      0
  )
  ```
- **Explanation**: Complement of the Attrition Rate (`1 - [Attrition Rate]`).
- **Format**: Percentage (`0.0%` or `0.00%`)
- **Baseline Value**: `83.88%`
- **Assumptions**: Active headcount and lost headcount are mutually exclusive and collectively exhaustive.

---

### Folder 2: Compensation & Financial Metrics (`02_Financial_Metrics`)

#### 6. `Average Monthly Income`
- **Business Meaning**: The benchmark mean gross monthly salary across the selected employee cohort.
- **DAX Formula**:
  ```dax
  Average Monthly Income = AVERAGE('HR_Data'[MonthlyIncome])
  ```
- **Explanation**: Arithmetic average of base monthly earnings.
- **Format**: Currency (`$#,##0`)
- **Baseline Value**: `$6,502.93`
- **Assumptions**: `MonthlyIncome` represents base monthly pay in USD.

#### 7. `Total Monthly Payroll`
- **Business Meaning**: The enterprise-wide monthly salary expenditure.
- **DAX Formula**:
  ```dax
  Total Monthly Payroll = SUM('HR_Data'[MonthlyIncome])
  ```
- **Explanation**: Aggregates gross monthly wages across all active and historical records in context.
- **Format**: Currency (`$#,##0`)
- **Baseline Value**: `$9,559,309` (~$9.56M/month)
- **Assumptions**: Constant monthly gross wage without ad-hoc bonuses.

#### 8. `Total Annual Payroll`
- **Business Meaning**: The annualized gross baseline salary expenditure.
- **DAX Formula**:
  ```dax
  Total Annual Payroll = [Total Monthly Payroll] * 12
  ```
- **Explanation**: Multiplies total monthly payroll by 12 months.
- **Format**: Currency (`$#,##0`)
- **Baseline Value**: `$114,711,708` (~$114.71M/year)
- **Assumptions**: Annualized equivalent assuming 12 equal monthly pay cycles.

#### 9. `Departed Monthly Payroll`
- **Business Meaning**: The total monthly payroll tied to employees who departed the company.
- **DAX Formula**:
  ```dax
  Departed Monthly Payroll = 
  CALCULATE(
      SUM('HR_Data'[MonthlyIncome]),
      'HR_Data'[Attrition] = "Yes"
  )
  ```
- **Explanation**: Computes monthly wage exposure from separated talent.
- **Format**: Currency (`$#,##0`)
- **Baseline Value**: `$1,134,541` (~$1.13M/month)
- **Assumptions**: Salary at the time of departure.

#### 10. `Departed Annual Payroll`
- **Business Meaning**: The total annualized direct wage value lost to employee turnover.
- **DAX Formula**:
  ```dax
  Departed Annual Payroll = [Departed Monthly Payroll] * 12
  ```
- **Explanation**: Multiplies departed monthly payroll by 12.
- **Format**: Currency (`$#,##0`)
- **Baseline Value**: `$13,614,492` (~$13.61M/year)
- **Assumptions**: Annualized wages of separated staff.

#### 11. `Average Departed Income`
- **Business Meaning**: The mean monthly salary of employees who left the organization.
- **DAX Formula**:
  ```dax
  Average Departed Income = 
  CALCULATE(
      AVERAGE('HR_Data'[MonthlyIncome]),
      'HR_Data'[Attrition] = "Yes"
  )
  ```
- **Explanation**: Computes the mean monthly compensation exclusively for separated employees.
- **Format**: Currency (`$#,##0`)
- **Baseline Value**: `$4,787.09`
- **Assumptions**: Identifies whether turnover is concentrated in lower or higher earning tiers.

#### 12. `Average Retained Income`
- **Business Meaning**: The mean monthly salary of employees retained by the organization.
- **DAX Formula**:
  ```dax
  Average Retained Income = 
  CALCULATE(
      AVERAGE('HR_Data'[MonthlyIncome]),
      'HR_Data'[Attrition] = "No"
  )
  ```
- **Explanation**: Computes the mean monthly compensation for active employees.
- **Format**: Currency (`$#,##0`)
- **Baseline Value**: `$6,832.74`
- **Assumptions**: Higher average reflects greater seniority and tenure among retained staff.

#### 13. `Estimated Turnover Cost (Conservative - 50%)`
- **Business Meaning**: Estimated financial replacement cost using a conservative benchmark (50% of annual salary per departed employee).
- **DAX Formula**:
  ```dax
  Estimated Turnover Cost (Conservative - 50%) = [Departed Annual Payroll] * 0.50
  ```
- **Explanation**: SHRM and Gallup research indicates replacing an employee typically costs between 50% and 150% of their annual salary in recruiting, onboarding, and ramp-up productivity loss.
- **Format**: Currency (`$#,##0`)
- **Baseline Value**: `$6,807,246` (~$6.81M)
- **Assumptions**: Conservative benchmark suitable for entry-level and operational roles.

#### 14. `Estimated Turnover Cost (Standard - 100%)`
- **Business Meaning**: Estimated financial replacement cost using the standard corporate benchmark (100% of annual salary).
- **DAX Formula**:
  ```dax
  Estimated Turnover Cost (Standard - 100%) = [Departed Annual Payroll] * 1.00
  ```
- **Explanation**: Represents the standard corporate replacement cost model for professional, technical, and mid-level roles.
- **Format**: Currency (`$#,##0`)
- **Baseline Value**: `$13,614,492` (~$13.61M)
- **Assumptions**: Mid-tier benchmark accounting for search fees, training, and 6–9 months of lost productivity.

#### 15. `Average Salary Hike %`
- **Business Meaning**: The mean percentage merit increase awarded during the last compensation review.
- **DAX Formula**:
  ```dax
  Average Salary Hike % = 
  DIVIDE(
      AVERAGE('HR_Data'[PercentSalaryHike]),
      100,
      0
  )
  ```
- **Explanation**: Converts the whole number percentage (11–25) to a decimal ratio for display in percentage format.
- **Format**: Percentage (`0.0%`)
- **Baseline Value**: `15.2%`
- **Assumptions**: Applies to all employees evaluated during the prior annual review.

---

### Folder 3: Tenure, Mobility & Career Progression (`03_Tenure_Mobility`)

#### 16. `Average Total Working Years`
- **Business Meaning**: The mean overall career experience (in years) across all organizations.
- **DAX Formula**:
  ```dax
  Average Total Working Years = AVERAGE('HR_Data'[TotalWorkingYears])
  ```
- **Explanation**: Measures total workforce maturity and career seniority.
- **Format**: Decimal (`0.0 yrs`)
- **Baseline Value**: `11.3 years`
- **Assumptions**: Self-reported or verified cumulative professional experience.

#### 17. `Average Company Tenure`
- **Business Meaning**: The mean continuous tenure (in years) spent at the current company.
- **DAX Formula**:
  ```dax
  Average Company Tenure = AVERAGE('HR_Data'[YearsAtCompany])
  ```
- **Explanation**: Evaluates institutional stability and organizational retention.
- **Format**: Decimal (`0.0 yrs`)
- **Baseline Value**: `7.0 years`
- **Assumptions**: Completed consecutive years of service.

#### 18. `Average Years in Current Role`
- **Business Meaning**: The mean duration (in years) employees have spent performing their current duties.
- **DAX Formula**:
  ```dax
  Average Years in Current Role = AVERAGE('HR_Data'[YearsInCurrentRole])
  ```
- **Explanation**: Assesses role stability vs. potential lateral or vertical stagnation.
- **Format**: Decimal (`0.0 yrs`)
- **Baseline Value**: `4.2 years`
- **Assumptions**: Time elapsed since employee's last title/role transition.

#### 19. `Average Years Since Last Promotion`
- **Business Meaning**: The mean duration (in years) elapsed since an employee's last formal promotion.
- **DAX Formula**:
  ```dax
  Average Years Since Last Promotion = AVERAGE('HR_Data'[YearsSinceLastPromotion])
  ```
- **Explanation**: Key mobility metric. High values highlight promotion bottlenecks.
- **Format**: Decimal (`0.0 yrs`)
- **Baseline Value**: `2.2 years` (Median: 1.0 yr)
- **Assumptions**: Time elapsed since formal grade increase. Zero indicates promotion within the past 12 months.

#### 20. `Average Years With Current Manager`
- **Business Meaning**: The mean duration (in years) reporting directly to the current supervisor.
- **DAX Formula**:
  ```dax
  Average Years With Current Manager = AVERAGE('HR_Data'[YearsWithCurrManager])
  ```
- **Explanation**: Assesses management continuity and supervisor relationship stability.
- **Format**: Decimal (`0.0 yrs`)
- **Baseline Value**: `4.1 years`
- **Assumptions**: Continuous supervisory relationship.

#### 21. `Stagnant Career Percentage (5+ Yrs No Promotion)`
- **Business Meaning**: The percentage of employees who have not received a promotion in 5 or more years.
- **DAX Formula**:
  ```dax
  Stagnant Career Percentage = 
  DIVIDE(
      CALCULATE(
          COUNTROWS('HR_Data'),
          'HR_Data'[YearsSinceLastPromotion] >= 5
      ),
      [Total Employees],
      0
  )
  ```
- **Explanation**: Measures the cohort at risk of turnover due to perceived career stagnation.
- **Format**: Percentage (`0.0%`)
- **Baseline Value**: `17.48%` (257 / 1,470)
- **Assumptions**: 5 years without upward mobility represents a standard industry career plateau.

---

### Folder 4: Operational Strain & Overtime Diagnostics (`04_Overtime_Analytics`)

#### 22. `Overtime Employees`
- **Business Meaning**: The total count of employees required to work overtime.
- **DAX Formula**:
  ```dax
  Overtime Employees = 
  CALCULATE(
      COUNTROWS('HR_Data'),
      'HR_Data'[OverTime] = "Yes"
  )
  ```
- **Explanation**: Aggregates records where `OverTime = "Yes"`.
- **Format**: Whole Number (`#,##0`)
- **Baseline Value**: `416`
- **Assumptions**: Indicates habitual or contractual overtime burden.

#### 23. `Overtime Percentage`
- **Business Meaning**: The proportion of the workforce engaged in overtime.
- **DAX Formula**:
  ```dax
  Overtime Percentage = 
  DIVIDE(
      [Overtime Employees],
      [Total Employees],
      0
  )
  ```
- **Explanation**: Evaluates workload intensity and operational strain across departments.
- **Format**: Percentage (`0.0%`)
- **Baseline Value**: `28.30%` (416 / 1,470)
- **Assumptions**: Overtime distribution reflects organizational staffing constraints.

#### 24. `Overtime Attrition Rate`
- **Business Meaning**: The turnover rate specifically among employees working overtime.
- **DAX Formula**:
  ```dax
  Overtime Attrition Rate = 
  DIVIDE(
      CALCULATE(
          COUNTROWS('HR_Data'),
          'HR_Data'[OverTime] = "Yes",
          'HR_Data'[Attrition] = "Yes"
      ),
      CALCULATE(
          COUNTROWS('HR_Data'),
          'HR_Data'[OverTime] = "Yes"
      ),
      0
  )
  ```
- **Explanation**: Isolates the departure rate of the overtime cohort (127 departed / 416 overtime staff).
- **Format**: Percentage (`0.0%`)
- **Baseline Value**: `30.53%`
- **Assumptions**: Demonstrates the observed rate of departure among overtime personnel.

#### 25. `Non-Overtime Attrition Rate`
- **Business Meaning**: The turnover rate among employees who do not work overtime.
- **DAX Formula**:
  ```dax
  Non-Overtime Attrition Rate = 
  DIVIDE(
      CALCULATE(
          COUNTROWS('HR_Data'),
          'HR_Data'[OverTime] = "No",
          'HR_Data'[Attrition] = "Yes"
      ),
      CALCULATE(
          COUNTROWS('HR_Data'),
          'HR_Data'[OverTime] = "No"
      ),
      0
  )
  ```
- **Explanation**: Serves as the control baseline group (110 departed / 1,054 non-overtime staff).
- **Format**: Percentage (`0.0%`)
- **Baseline Value**: `10.44%`
- **Assumptions**: Baseline turnover without overtime strain.

#### 26. `Overtime Attrition Ratio (Relative Risk)`
- **Business Meaning**: The comparative multiplier of turnover rate for overtime workers vs. non-overtime workers.
- **DAX Formula**:
  ```dax
  Overtime Attrition Ratio = 
  DIVIDE(
      [Overtime Attrition Rate],
      [Non-Overtime Attrition Rate],
      0
  )
  ```
- **Explanation**: Quantifies the relative disparity ($30.53\% \div 10.44\% = 2.92\text{x}$).
- **Format**: Decimal (`0.0x`)
- **Baseline Value**: `2.9x`
- **Assumptions**: Highlights an observational association indicating employees on overtime are nearly 3 times more likely to depart.

---

### Folder 5: Employee Experience & Sentiment (`05_Sentiment_Metrics`)

#### 27. `Low Job Satisfaction Count`
- **Business Meaning**: Headcount of employees rating their job satisfaction as "Low".
- **DAX Formula**:
  ```dax
  Low Job Satisfaction Count = 
  CALCULATE(
      COUNTROWS('HR_Data'),
      'HR_Data'[JobSatisfaction] = "Low"
  )
  ```
- **Explanation**: Identifies morale risk groups.
- **Format**: Whole Number (`#,##0`)
- **Baseline Value**: `289` (19.66%)
- **Assumptions**: "Low" reflects survey level 1.

#### 28. `Low Environment Satisfaction Count`
- **Business Meaning**: Headcount of employees dissatisfied with their physical or cultural work environment.
- **DAX Formula**:
  ```dax
  Low Environment Satisfaction Count = 
  CALCULATE(
      COUNTROWS('HR_Data'),
      'HR_Data'[EnvironmentSatisfaction] = "Low"
  )
  ```
- **Explanation**: Identifies workplace and facility culture issues.
- **Format**: Whole Number (`#,##0`)
- **Baseline Value**: `284` (19.32%)
- **Assumptions**: Corresponds to survey rating "Low".

#### 29. `Bad Work-Life Balance Count`
- **Business Meaning**: Headcount of employees experiencing acute work-life imbalance.
- **DAX Formula**:
  ```dax
  Bad Work-Life Balance Count = 
  CALCULATE(
      COUNTROWS('HR_Data'),
      'HR_Data'[WorkLifeBalance] = "Bad"
  )
  ```
- **Explanation**: Identifies extreme work-life conflict.
- **Format**: Whole Number (`#,##0`)
- **Baseline Value**: `80` (5.44%)
- **Assumptions**: "Bad" reflects the lowest rating (level 1).

#### 30. `Bad Work-Life Balance Attrition Rate`
- **Business Meaning**: Attrition rate among employees reporting "Bad" work-life balance.
- **DAX Formula**:
  ```dax
  Bad Work-Life Balance Attrition Rate = 
  DIVIDE(
      CALCULATE(
          COUNTROWS('HR_Data'),
          'HR_Data'[WorkLifeBalance] = "Bad",
          'HR_Data'[Attrition] = "Yes"
      ),
      CALCULATE(
          COUNTROWS('HR_Data'),
          'HR_Data'[WorkLifeBalance] = "Bad"
      ),
      0
  )
  ```
- **Explanation**: Measures flight risk for employees in burnout territory (25 departed / 80 staff).
- **Format**: Percentage (`0.0%`)
- **Baseline Value**: `31.25%` (vs. enterprise average of 16.12%)
- **Assumptions**: Highlights retention risk associated with work-life balance friction.

---

### Analytical Dimension Thresholds & Derivation Rationale

| Dimension Name | Granular Bins / Categories | Distribution Count | Empirical & Business Rationale |
| :--- | :--- | :-: | :--- |
| **`Age_Group`** | `<25`<br>`25-34`<br>`35-44`<br>`45-54`<br>`55+` | 123 (8.4%)<br>556 (37.8%)<br>505 (34.4%)<br>245 (16.7%)<br>41 (2.8%) | Standard 10-year demographic cohorts aligned with US Bureau of Labor Statistics (BLS) and corporate workforce demographic benchmarks. |
| **`Tenure_Bracket`** | `<1 Year (New Hire)`<br>`1-2 Years`<br>`3-5 Years`<br>`6-10 Years`<br>`10+ Years (Tenured)` | 44 (3.0%)<br>298 (20.3%)<br>468 (31.8%)<br>432 (29.4%)<br>228 (15.5%) | Aligned with corporate talent lifecycle milestones: First-year onboarding risk (<1 yr), early career mobility (1–2 yrs), core contributor retention (3–5 yrs), vested seniority (6–10 yrs), and veteran retention (10+ yrs). |
| **`Salary_Bracket`** | `<$3,000`<br>`$3,000-$4,999`<br>`$5,000-$7,999`<br>`$8,000-$11,999`<br>`$12,000+` | 381 (25.9%)<br>449 (30.5%)<br>304 (20.7%)<br>173 (11.8%)<br>163 (11.1%) | Directly derived from empirical wage quartiles: Q1 is $2,911 (~$3k), Median is $4,930 (~$5k), Q3 is $8,380 (~$8k), and the leadership threshold begins at $12k+ (capturing upper management and executive director roles). |
| **`Experience_Group`** | `0-2 Years (Entry)`<br>`3-5 Years (Junior)`<br>`6-10 Years (Mid-Level)`<br>`11-20 Years (Senior)`<br>`21+ Years (Veteran)` | 123 (8.4%)<br>193 (13.1%)<br>607 (41.3%)<br>340 (23.1%)<br>207 (14.1%) | Reflects corporate recruitment career ladders: Associate/Entry (0–2 yrs), Intermediate (3–5 yrs), Senior Specialist (6–10 yrs), Lead/Principal (11–20 yrs), and Director/Executive (21+ yrs). |
| **`Distance_Bracket`** | `Near (1-5 mi)`<br>`Moderate (6-15 mi)`<br>`Far (16+ mi)` | 638 (43.4%)<br>512 (34.8%)<br>320 (21.8%) | Groups commuting burden into walking/short transit (1–5 mi), typical suburban commute (6–15 mi), and high-fatigue long-distance commute (16+ mi). |
