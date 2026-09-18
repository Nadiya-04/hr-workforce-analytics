# HR Workforce, Attrition & Cost Analytics

## Data Dictionary

This document describes the fields used in the IBM HR Analytics Employee Attrition and Performance dataset.

The dataset contains **1,470 employee records and 31 variables** covering demographics, employment characteristics, compensation, job satisfaction, performance, and attrition.

| Column                   | Description                                    | Data Type |
| ------------------------ | ---------------------------------------------- | --------- |
| Age                      | Age of the employee                            | Integer   |
| Attrition                | Whether the employee left the organization     | Category  |
| BusinessTravel           | Frequency of business travel                   | Category  |
| DailyRate                | Daily compensation rate                        | Integer   |
| Department               | Employee's department                          | Category  |
| DistanceFromHome         | Distance between home and workplace            | Integer   |
| Education                | Employee's education level                     | Category  |
| EducationField           | Employee's field of education                  | Category  |
| EnvironmentSatisfaction  | Satisfaction with the work environment         | Category  |
| Gender                   | Employee gender                                | Category  |
| HourlyRate               | Hourly compensation rate                       | Integer   |
| JobInvolvement           | Level of involvement in the job                | Category  |
| JobLevel                 | Employee's organizational/job level            | Category  |
| JobRole                  | Employee's job role                            | Category  |
| JobSatisfaction          | Employee's satisfaction with their job         | Category  |
| MaritalStatus            | Employee's marital status                      | Category  |
| MonthlyIncome            | Employee's monthly income                      | Integer   |
| MonthlyRate              | Monthly compensation rate                      | Integer   |
| NumCompaniesWorked       | Number of companies previously worked for      | Integer   |
| OverTime                 | Whether the employee works overtime            | Category  |
| PercentSalaryHike        | Percentage increase in salary                  | Integer   |
| PerformanceRating        | Employee performance rating                    | Category  |
| RelationshipSatisfaction | Satisfaction with workplace relationships      | Category  |
| StockOptionLevel         | Level of stock option benefits                 | Integer   |
| TotalWorkingYears        | Total years of professional experience         | Integer   |
| TrainingTimesLastYear    | Number of training sessions attended last year | Integer   |
| WorkLifeBalance          | Employee's work-life balance rating            | Category  |
| YearsAtCompany           | Number of years spent at the company           | Integer   |
| YearsInCurrentRole       | Number of years in the current role            | Integer   |
| YearsSinceLastPromotion  | Years since the employee's last promotion      | Integer   |
| YearsWithCurrManager     | Years working with the current manager         | Integer   |

## Analytical Categories

### Workforce & Demographics

- Age
- Gender
- MaritalStatus
- Education
- EducationField

### Employment

- Department
- JobRole
- JobLevel
- BusinessTravel
- TotalWorkingYears
- YearsAtCompany
- YearsInCurrentRole
- YearsSinceLastPromotion
- YearsWithCurrManager
- NumCompaniesWorked

### Compensation

- DailyRate
- HourlyRate
- MonthlyIncome
- MonthlyRate
- PercentSalaryHike
- StockOptionLevel

### Employee Experience

- EnvironmentSatisfaction
- JobInvolvement
- JobSatisfaction
- RelationshipSatisfaction
- WorkLifeBalance
- OverTime

### Performance & Development

- PerformanceRating
- TrainingTimesLastYear

### Outcome

- Attrition

## Notes

The dataset is analyzed at the individual employee level. Categorical ratings such as satisfaction, involvement, performance, and work-life balance are represented using descriptive categories in the revised dataset.

The analysis will focus on identifying workforce patterns and relationships associated with employee attrition and estimating its potential business impact.
