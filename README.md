# Project -> HR-Data-Analysis
A business analytics case study analyzing employee attrition trends, satisfaction, and diversity using Power BI, SQL, and Python. This project involves descriptive and predictive analytics with the goal of improving HR decision-making.

## Table of Contents
- [Objective](#objective)
- [Business Questions](#business-questions-answered)
- [Data Cleaning](#data-cleaning--preparation)
- [Data Analysis using SQL](#data-analysis-using-sql)
- [KPIs](#key-performance-indicators-kpis)
- [Dashboards](#dashboard-preview)
- [Insights](#key-insights--recommendations)
- [References](#references)

## Case Background  
A mid-sized company is facing challenges related to:

- High employee turnover
- Limited insight into diversity and pay equity
- Inconsistent hiring outcomes
  
**Tools used:** SQL • PowerBI • Python • GitHub

## Objective
To support HR managers and business leaders in reducing attrition, identifying key performance and satisfaction drivers, and gaining clarity on hiring sources, diversity, and pay equity.

### Business Questions Answered
1. What is the company's current attrition rate, and how does it differ by department?
2. What are the top reasons employees are leaving?
3. Does shorter tenure significantly increase attrition risk?
4. Are there any gender or pay equity concerns across roles and departments?
5. Which recruitment sources are associated with higher employee turnover?
6. How do satisfaction, engagement, and performance scores correlate with attrition?
7. Can we accurately predict which employees are likely to leave next?

### Key Performance Indicators (KPIs) 📊
| KPI                                  | Description                                      |
| ------------------------------------ | ------------------------------------------------ |
| **Total Employees**                  | Total number of employees in the dataset         |
| **Active Employees**                 | Employees currently employed (not terminated)    |
| **Attrition Rate (%)**               | % of employees who have left                     |
| **Avg. Satisfaction Score**          | Self-reported employee satisfaction              |
| **Male/Female Ratio**                | Gender distribution across company               |

### Areas of Analysis & Visualizations
- Overall Attrition Rate
- Termination Reasons – most common exit causes
- Attrition by Department & Position – where are exits happening most?
- Recruitment Source Analysis – which sources produce longer-term employees
- Insights on Attrition Recruitment Sources
- Gender & Race Diversity – breakdowns across departments
- Average Salary by Gender & Role – pay equity investigation
- Age Bucket - What age range do employees fall in?
- Tenure Analysis – how long employees stay before leaving
- Satisfaction vs Attrition – do low scores predict exits?
- Predictive Model Results – accuracy + key predictors of attrition

### Data Cleaning & Preparation
- Checked for blanks and duplicates
- Removed unnecessary fields
- Ensured correct data types
- Converted date columns to SQL-friendly formats
- Handled null values for satisfaction, survey, termination dates
- After initial cleaning, the data was imported in SQL for insights

### Data Analysis using SQL
```sql
Total Employees
SELECT COUNT(EmpID) FROM hr_data;
```
```sql
Active Employees
SELECT COUNT(*) FROM hr_data WHERE EmploymentStatus = 'Active';
```
```sql
Attrition Rate (%)
SELECT ROUND(SUM(CASE WHEN Termd = 1 THEN 1 ELSE 0 END) / COUNT(*) * 100, 2) AS attrition_rate FROM hr_data;
```
```sql
Avg Employee Satisfaction
SELECT ROUND(AVG(EmpSatisfaction), 2) AS avg_emp_satisfaction_rating FROM hr_data;
```

Gender Count
SELECT Sex, COUNT(*) AS gender_count FROM hr_data GROUP BY Sex;

Gender Distribution Across Departments
SELECT 
    Department,
    Sex AS Gender,
    COUNT(*) AS Num_Emp,
FROM hr_data
GROUP BY Department, Sex
ORDER BY Department, Sex;

Avg Salary by Gender
SELECT Sex AS Gender, AVG(Salary) AS avg_salary FROM hr_data GROUP BY Sex;

Age Bucket Distribution
SELECT
    COUNT(*) AS emp_count,
    CASE 
        WHEN TIMESTAMPDIFF(YEAR, DOB, '2020-01-01') BETWEEN 18 AND 25 THEN '18-25'
        WHEN TIMESTAMPDIFF(YEAR, DOB, '2020-01-01') BETWEEN 26 AND 35 THEN '26-35'
        WHEN TIMESTAMPDIFF(YEAR, DOB, '2020-01-01') BETWEEN 36 AND 45 THEN '36-45'
        WHEN TIMESTAMPDIFF(YEAR, DOB, '2020-01-01') BETWEEN 46 AND 60 THEN '46-60'
        ELSE '60+'
    END AS age_bucket
FROM hr_data
GROUP BY age_bucket
ORDER BY age_bucket;

Most Common Termination Reasons
SELECT 
    TermReason, 
    COUNT(*) AS emp_count 
FROM hr_data 
WHERE Termd = 1 
GROUP BY TermReason 
ORDER BY emp_count DESC;

Attrition by Recruitment Source (Hotspots)
SELECT 
    RecruitmentSource,
    COUNT(*) AS terminated_count
FROM hr_data 
WHERE Termd = 1 
GROUP BY RecruitmentSource 
ORDER BY terminated_count DESC;

Attrition by Department
SELECT 
    Department, 
    COUNT(CASE WHEN Termd = 1 THEN 1 END) AS terminated
FROM hr_data
GROUP BY Department
ORDER BY Department DESC;
```
 
### What’s Next ❓
This project was completed using both visual tools (Tableau) and code-driven analysis (Python) to provide a full-stack business intelligence solution.
Additional deep-dives included:
 - Revenue Forecasting using Linear Regression
 - Time-of-Day Analysis by Pizza Category (heatmap)
  
### Dashboard Preview

#### Sales & Growth Monitor
![Dashboard 1](dashboards/dashboard_main.png)

#### Customer Behavior & Category Insights
![Dashboard 2](dashboards/dashboard2.png)

### Trend Forecasting (Linear Regression)

This plot shows monthly revenue over the year and a predicted trend using linear regression.

![Trend Forecasting](python-analysis/monthly_forecast.png)


### Order Frequency by Time of Day vs Pizza Category

![Order Time Heatmap](python-analysis/heatmap_order_by_time.png)

### Insights & Recommendations

#### Sales Trends Over Time
  - Sales were highest in July, with over **$72,000** in revenue.
  - **Spring** and **Summer** were the busiest seasons, with the most orders.
  - On average, the restaurant got **60** orders per day.
  - Peak hours were **12–1 PM** and **6–8 PM** — perfect lunch and dinner times.
  - **Thursdays** and **Fridays** were the busiest days of the week.
#### Recommendations:
  - Add more staff and prep more ingredients during peak times.
  - Run offers on Thursday/Friday lunch and dinner to boost revenue.
  - Plan seasonal deals in Spring and Summer to take advantage of high demand.

#### Best & Worst Performing Pizzas
  - The **Classic** **Deluxe** and **Barbecue** **Chicken** pizzas were top favorites in both sales and revenue.
  - **Brie Carre** and **Greek Pizza** **(XXL)** were the least popular.
#### Recommendations:
  - Highlight top-sellers in combo meals and marketing.
  - Consider removing or reworking poorly performing pizzas to reduce waste and menu clutter.

#### Category & Size Insights
  - **Classic** and **Supreme** pizzas made up more than **50%** of total revenue.
  - **Medium** and **Large** sizes were ordered the most.
  - Very few customers ordered XXL pizzas.
#### Recommendations:
  - Stock more ingredients for medium and large pizzas.
  - Focus new product launches in the Classic and Supreme categories.
  - Rethink XXL pizzas — maybe turn them into limited-time offers.

#### Sales Forecast (Python Analysis)
  - A simple forecast shows sales might slightly dip in the next 3 months.
  - This could be due to off-season periods or customer fatigue.
#### Recommendations:
  - Introduce new items or limited-time deals to keep customers interested.
  - Keep tracking the forecast and update it every quarter for better planning.

#### Pizza Category vs. Time of Day (Python Heatmap)
  - Classic pizzas were most popular during lunch.
  - Other categories (like Supreme and Veggie) had steady orders throughout the day.
  - Late-night orders were low across the board.
#### Recommendations:
  - Create lunch combos with Classic pizzas to boost sales even more.
  - Reduce late-night staffing or offer special “late-night meal deals” to increase traffic.

#### References
[(https://www.youtube.com/watch?v=bYmcCTsP0Zg)]

