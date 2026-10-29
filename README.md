# Marketing Budget Optimization & A/B Testing

An end-to-end marketing analytics and business intelligence project using **PostgreSQL, SQL, Python, Excel, and Power BI** to evaluate campaign performance, compare attribution methods, analyze advertising effectiveness, and explore proposed budget allocations.

**Project summary**

| | |
|---|---|
| **Technologies** | PostgreSQL · SQL · Python · pandas · NumPy · SciPy · statsmodels · Excel · Power BI · DAX |
| **Focus areas** | Marketing analytics · Attribution analysis · Statistical testing · Budget scenarios · Business intelligence |

---

## Table of Contents

1. [Project Overview](#project-overview)
2. [Business Questions](#business-questions)
3. [Technology Stack](#technology-stack)
4. [Project Workflow](#project-workflow)
5. [Data Quality Assessment](#data-quality-assessment)
6. [PostgreSQL Data Modeling](#postgresql-data-modeling)
7. [Campaign Performance Analysis](#campaign-performance-analysis)
8. [Marketing Attribution Analysis](#marketing-attribution-analysis)
9. [A/B Testing and Statistical Analysis](#ab-testing-and-statistical-analysis)
10. [Budget Scenario Analysis](#budget-scenario-analysis)
11. [Power BI Dashboards](#power-bi-dashboards)
12. [Key Findings](#key-findings)
13. [Business Value](#business-value)
14. [Methodological Limitations](#methodological-limitations)
15. [Repository Structure](#repository-structure)
16. [How to Explore the Project](#how-to-explore-the-project)
17. [Skills Demonstrated](#skills-demonstrated)
18. [Future Improvements](#future-improvements)
19. [License](#license)

---

## Project Overview

Marketing teams need to understand which campaigns and channels generate results, whether advertising is associated with improved conversion performance, and how budgets might be allocated more effectively.

This project combines campaign performance analysis, marketing attribution, statistical A/B testing, and budget scenario analysis into one analytics workflow, covering the full lifecycle from data quality checks and PostgreSQL modeling to statistical inference, Excel-based scenarios, and interactive Power BI dashboards.

**Objectives**

- Evaluate campaign and marketing-channel performance.
- Analyze impressions, clicks, advertising spend, conversions, and conversion value.
- Calculate efficiency metrics: CTR, CPC, cost per conversion, and ROAS.
- Compare first-touch and last-touch attribution.
- Compare conversion rates between advertising and public service announcement (PSA) groups, and quantify the observed lift and its statistical significance.
- Compare historical spending with proposed budget allocations.
- Communicate findings through dashboards and technical documentation.

**Analytics lifecycle**

`Data Quality Assessment → PostgreSQL Modeling → Campaign Analysis → Attribution Analysis → A/B Testing → Budget Scenarios → Power BI → Business Insights`

---

## Business Questions

1. Which campaigns and channels generate the strongest reported performance?
2. How efficiently does advertising spend translate into clicks and conversions?
3. How do first-touch and last-touch attribution distribute conversion credit?
4. Does the advertising group have a different observed conversion rate from the PSA group?
5. What is the size and statistical uncertainty of the observed conversion lift?
6. How might proposed budget changes affect expected conversion value?
7. Which assumptions and data limitations must be considered before making investment decisions?

---

## Technology Stack

| Technology | Purpose |
|---|---|
| PostgreSQL | Data validation, transformation, analytical modeling, aggregation |
| SQL | CTEs, joins, aggregations, views, attribution analysis |
| Python | Statistical analysis and experiment evaluation |
| pandas | Data manipulation and analysis |
| NumPy | Numerical calculations |
| SciPy | Statistical testing |
| statsmodels | Statistical inference and confidence intervals |
| Microsoft Excel | Budget allocation scenarios and assumption-based projections |
| Power BI | Interactive dashboards, DAX measures, visualization |
| Git and GitHub | Version control and project documentation |

---

## Project Workflow

```text
Marketing Source Data
         |
         v
Data Quality Assessment
         |
         v
PostgreSQL Data Modeling
         |
         v
Campaign Performance Analysis
         |
         +----------------------------+
         |                            |
         v                            v
Attribution Analysis            A/B Testing
         |                            |
         +-------------+--------------+
                       |
                       v
            Budget Scenario Analysis
                (Microsoft Excel)
                       |
                       v
                Power BI Dashboards
                       |
                       v
          Business Insights and Decision Support
```

| Stage | Description |
|---|---|
| Data validation | Inspect source fields, missing values, duplicate identifiers, and table relationships |
| Data modeling | Organize campaign, channel, advertising, and conversion data in PostgreSQL |
| Performance analysis | Calculate campaign KPIs and compare channels over time |
| Attribution analysis | Compare first-touch and last-touch conversion credit |
| Experiment analysis | Evaluate observed conversion differences between advertising and PSA groups |
| Budget scenarios | Compare historical spending with proposed allocations and expected conversion value |
| Business intelligence | Present results through Power BI |

---

## Data Quality Assessment

Data quality checks were performed before building the analytical model.

**Validation activities**

- Inspect column names and data types.
- Identify duplicate identifiers.
- Examine missing values.
- Validate the grain of analytical tables.
- Check relationships between source and analytical tables.
- Investigate inconsistencies in conversion-related fields.
- Review delivery-related metrics where applicable.
- Document assumptions and known data limitations.

**Documentation:** findings are in [`documentation/data_quality_audit.md`](documentation/data_quality_audit.md). The data dictionary describes source fields, analytical tables, and definitions used downstream.

---

## PostgreSQL Data Modeling

PostgreSQL is the relational layer for campaign performance analysis, attribution modeling, and reporting. Source data is organized into dimension tables, fact tables, a campaign performance mart, and analytical views.

### Dimension tables

| Table | Purpose |
|---|---|
| `dim_channel` | Marketing channel information |
| `dim_campaign` | Campaign-level attributes |
| `dim_ad_group` | Advertising group attributes |

### Fact tables

| Table | Purpose |
|---|---|
| `fact_daily_performance` | Daily campaign performance metrics |
| `fact_conversion` | Conversion events and attribution-related information |
| `fact_ab_test_user` | User-level A/B testing observations |

> The A/B testing dataset is analyzed separately because it does not share a suitable campaign-level key with the marketing campaign data.

### Campaign performance mart

`mart_campaign_performance` summarizes campaign-level metrics for reporting and comparison.

### Analytical views

| View | Purpose |
|---|---|
| `vw_attribution_by_channel` | Attribution results by channel |
| `vw_attribution_comparison` | First-touch vs. last-touch comparison |
| `vw_conversion_touchpoints` | Conversion touchpoint examination |
| `vw_campaign_budget_allocation` | Campaign budget allocation analysis |
| `vw_campaign_roas` | Return on ad spend analysis |

> **Grain warning:** campaign-level summaries and daily performance records have different grains. Do not combine them through joins without appropriate aggregation and join-key validation, or records will be duplicated and totals inflated.

---

## Campaign Performance Analysis

Evaluates delivery, spending, and reported conversion outcomes across campaigns, channels, and time periods.

**Core metrics:** impressions, clicks, advertising spend, reported conversions, reported conversion value, CTR, CPC, cost per reported conversion, and ROAS.

### KPI definitions

| KPI | Formula | Meaning |
|---|---|---|
| Click-through rate (CTR) | $\text{CTR} = \dfrac{\text{Clicks}}{\text{Impressions}} \times 100$ | Percentage of impressions that result in clicks |
| Cost per click (CPC) | $\text{CPC} = \dfrac{\text{Advertising Spend}}{\text{Clicks}}$ | Average spend per click |
| Cost per reported conversion | $\dfrac{\text{Advertising Spend}}{\text{Reported Conversions}}$ | Average spend per reported conversion |
| Return on ad spend (ROAS) | $\text{ROAS} = \dfrac{\text{Reported Conversion Value}}{\text{Advertising Spend}}$ | Reported conversion value relative to spend |

> **Important:** ROAS is based on *reported* conversion value. It does not measure profit and should not automatically be interpreted as incremental return.

**Analytical questions**

- Which campaigns generate the most reported conversion value?
- Which channels have the highest or lowest CPC?
- How does CTR vary across campaigns?
- Which campaigns have the strongest ROAS?
- How do spending and reported conversions change over time?
- Do higher-spending campaigns also generate more conversion value?

---

## Marketing Attribution Analysis

SQL views compare how conversion credit is assigned to marketing channels.

- **First-touch attribution** credits the first recorded marketing touchpoint in the defined journey, showing which channels introduce users.
- **Last-touch attribution** credits the last recorded eligible touchpoint before conversion, showing which channels appear closest to the conversion.

| Dimension | First-Touch | Last-Touch |
|---|---|---|
| Credit assignment | First eligible recorded touchpoint | Last eligible recorded touchpoint |
| Main perspective | Initial discovery | Final recorded interaction |
| Useful for | Understanding entry channels | Understanding channels closest to conversion |
| Main limitation | May understate later touchpoints | May understate earlier touchpoints |

**Analysis areas:** attributed conversions per method, channel-level attributed conversion value, differences in credit across methods, conversion touchpoint analysis, and campaign/channel comparisons.

> Attribution results depend on tracking coverage, attribution windows, touchpoint definitions, and available data. Attribution is **not automatically causal**: differences between methods describe how credit is assigned, not the incremental effect of each channel.

---

## A/B Testing and Statistical Analysis

Python (pandas, SciPy, statsmodels) is used to compare conversion outcomes between an advertising group and a PSA group. The analysis covers group sizes, converted-user counts, conversion rates, absolute and relative lift, a two-proportion z-test, p-value, a confidence interval for the rate difference, and descriptive analysis of ad exposure.

### Experiment results

| Metric | Advertising Group | PSA Group |
|---|---:|---:|
| Users | 564,577 | 23,524 |
| Converted users | 14,423 | 420 |
| Conversion rate | 2.5547% | 1.7854% |
| Average ads seen | 24.8234 | 24.7611 |

### Statistical summary

| Metric | Result |
|---|---|
| Absolute conversion lift | ≈ 0.7692 percentage points |
| Relative conversion lift | ≈ 43.09% |
| Two-proportion z-statistic | ≈ 7.3701 |
| P-value | ≈ 1.705 × 10⁻¹³ |
| 95% CI for absolute lift | ≈ 0.5873 to 0.9360 percentage points |

### Lift calculations

**Absolute lift** is the difference between the two observed rates:

$$\text{Absolute Lift} = p_{\text{ads}} - p_{\text{PSA}} \approx 2.5547\% - 1.7854\% \approx 0.7692 \text{ percentage points}$$

**Relative lift** expresses that difference relative to the PSA rate:

$$\text{Relative Lift} = \frac{p_{\text{ads}} - p_{\text{PSA}}}{p_{\text{PSA}}} \times 100 \approx 43.09\%$$

### Hypotheses

- **Null hypothesis (H₀):** the population conversion rates are equal.
- **Alternative hypothesis (H₁):** the population conversion rates differ.

The p-value is far below the conventional 0.05 threshold, and the confidence interval excludes zero, so the difference is statistically significant under the test's assumptions.

### Interpretation and causal limitations

The advertising group recorded a higher conversion rate than the PSA group. However, the experiment documentation did **not confirm random assignment**, so the difference should be read as an observed association, not proof that advertising caused the increase.

Further considerations:

- Potential selection bias
- Differences in group composition
- User-level independence assumptions
- Duplicate or repeated user observations
- The assignment mechanism
- Whether the outcome window was consistent across groups

> Statistical significance alone does not establish causality or business value.

**Methodology documentation:** [`documentation/statistical_analysis.md`](documentation/statistical_analysis.md)

---

## Budget Scenario Analysis

Microsoft Excel compares historical campaign spending with a proposed allocation, combining historical metrics with explicit assumptions about proposed budgets and expected ROAS.

**Components:** historical spending and ROAS, proposed budgets, assumed ROAS, historical conversion value, expected conversion value under scenario assumptions, and campaign-level comparisons.

**Scenario logic**

$$\text{Expected Conversion Value} = \text{Proposed Budget} \times \text{Assumed ROAS}$$

*Illustrative example:* a $10,000 budget with an assumed ROAS of 2.5 implies $25,000 in projected conversion value. This is not a claim about the project's actual budget values.

**Key assumptions**

- Historical spend and conversion value are accurate.
- Assumed ROAS values are valid.
- Historical performance can reasonably inform future outcomes.
- Campaign performance holds as budgets increase or decrease (no diminishing returns or capacity constraints).

> Expected conversion value is an assumption-based projection, not guaranteed revenue or profit.

**Assumptions documentation:** [`documentation/budget_scenario_assumptions.md`](documentation/budget_scenario_assumptions.md)

---

## Power BI Dashboards

A four-page interactive report connects campaign performance, attribution, experiment results, and budget scenarios.

### 1. Marketing Performance: Executive Overview

High-level view of campaign performance and advertising efficiency.

- Total advertising spend
- Reported conversions and conversion value
- CTR and CPC
- Monthly performance trends
- Advertising spend by channel
- Campaign-level performance details

![Marketing Performance: Executive Overview](screenshots/executive_overview.png)

### 2. Campaign & Channel Performance

Compares campaigns and channels across delivery and efficiency metrics.

- Total clicks and impressions
- CPC and campaign conversion rate
- Campaign spend comparisons
- Channel spend vs. reported conversion value
- Campaign efficiency scatter plot
- Campaign performance details

![Campaign & Channel Performance](screenshots/campaign_channel_performance.png)

### 3. A/B Testing Analysis

Presents experiment results and their statistical interpretation.

- Advertising and PSA conversion rates
- Absolute and relative conversion lift
- Conversion outcomes by test group
- Conversion rate by advertising exposure
- Statistical test results and confidence interval
- Methodological limitations

![A/B Testing Analysis](screenshots/ab_testing_analysis.png)

### 4. Budget Optimization

Compares historical spending with the proposed budget scenario.

- Historical spend and proposed budget
- Budget change by campaign
- Historical and expected conversion value
- Campaign-level budget comparisons
- Proposed budget allocation visualization

![Budget Optimization](screenshots/budget_optimization.png)

**Design goals:** help stakeholders monitor performance, compare campaign efficiency, understand attribution differences, interpret experimental findings, and explore alternative budget allocations.

<!-- Before publishing: confirm the screenshot files exist and match the final report. -->

---

## Key Findings

1. **Observed conversion-rate difference:** the advertising group converted at about 2.55% vs. 1.79% for the PSA group.
2. **Positive observed lift:** an absolute difference of about 0.77 percentage points, a relative increase of about 43.09% over the PSA rate.
3. **Statistical significance:** the two-proportion z-test gave p ≈ 1.705 × 10⁻¹³. The difference is significant under the test's assumptions, but a causal reading requires evidence that the experimental design supports it.
4. **Attribution comparisons:** the SQL views show how different attribution rules shift the distribution of reported conversion value across channels.
5. **Budget scenarios:** Excel and Power BI compare historical metrics with proposed allocations and assumption-based projections. Recommendations depend on the validity of those assumptions and on evidence of how campaigns respond to budget changes.

**Detailed documentation:** [`documentation/key_findings.md`](documentation/key_findings.md) · [`documentation/statistical_analysis.md`](documentation/statistical_analysis.md)

---

## Business Value

| Area | Value |
|---|---|
| **Campaign management** | Compare channel and campaign performance; monitor delivery and conversions; flag campaigns for further investigation |
| **Measurement and attribution** | Compare first- and last-touch credit; understand how definitions affect reported channel performance; improve reporting transparency |
| **Experiment evaluation** | Quantify observed conversion differences; interpret p-values and confidence intervals; document assumptions needed for causal conclusions |
| **Budget planning** | Compare historical spend with proposed allocations; model expected value with explicit assumptions; identify evidence needed before changing real budgets |
| **Business intelligence** | Standardize campaign metrics; communicate findings through dashboards; connect results to documented assumptions |

---

## Methodological Limitations

| Topic | Limitation |
|---|---|
| **Reported conversions** | May not represent unique customers or new acquisitions; meaning depends on source tracking and conversion definitions |
| **ROAS vs. profit** | Calculated from reported conversion value and spend; ignores costs, margins, and incremental effects |
| **Attribution** | Depends on tracking data and rules; describes credit assignment, not necessarily causal channel effects |
| **A/B test assignment** | Random assignment is not confirmed, so the difference cannot automatically be read as the causal impact of advertising |
| **Statistical assumptions** | The z-test depends on the experimental unit, independence, sample structure, and method suitability |
| **Budget scenarios** | Depend on proposed budgets and assumed ROAS; projections require validation and are not guaranteed results |
| **Table grain** | Campaign-level and daily fact tables differ in detail; joining without aggregation duplicates rows and inflates metrics |

---

## Repository Structure

```text
marketing-budget-optimization/
│
├── README.md
│
├── data/
│   └── README.md
│
├── sql/
│   ├── data_quality_checks.sql
│   ├── schema.sql
│   ├── campaign_analysis.sql
│   ├── attribution_analysis.sql
│   └── views/
│
├── python/
│   ├── ab_testing.py
│   └── notebooks/
│
├── excel/
│   └── budget_scenario.xlsx
│
├── powerbi/
│   └── marketing_analytics.pbix
│
├── screenshots/
│   ├── executive_overview.png
│   ├── campaign_channel_performance.png
│   ├── ab_testing_analysis.png
│   └── budget_optimization.png
│
└── documentation/
    ├── data_quality_audit.md
    ├── data_dictionary.md
    ├── key_findings.md
    ├── statistical_analysis.md
    └── budget_scenario_assumptions.md
```

> The filenames above illustrate a possible organization. Keep the actual filenames from your repository rather than renaming files to match this template.

---

## How to Explore the Project

1. **Data documentation:** review source datasets, field definitions, and data quality limitations.
2. **SQL scripts:** examine validation, campaign performance metrics, and attribution views.
3. **Python analysis:** review the experiment setup, conversion calculations, hypothesis test, and confidence interval.
4. **Excel workbook:** examine historical performance and proposed allocation assumptions.
5. **Power BI report:** explore campaign performance, attribution metrics, A/B testing, and budget scenarios.
6. **Methodology documentation:** read how results should be interpreted and where they are limited.

---

## Skills Demonstrated

| Area | Skills |
|---|---|
| **SQL and database analytics** | PostgreSQL data modeling · data quality assessment · joins and aggregations · CTEs · analytical views · campaign KPI development · attribution analysis |
| **Python and statistics** | pandas and NumPy · hypothesis testing · two-proportion z-tests · p-value interpretation · confidence intervals · conversion lift calculations · statistical reporting |
| **Excel and financial scenarios** | Budget allocation modeling · historical comparisons · assumption-based projections · scenario analysis |
| **Power BI and DAX** | Data modeling · DAX measures · interactive dashboards · KPI reporting · campaign and channel visualization · experiment-result communication |
| **Business communication** | Marketing performance evaluation · attribution interpretation · budget decision support · methodological documentation · data storytelling · limitations assessment |

---

## Future Improvements

- [ ] Verify and document randomized experiment assignment
- [ ] Calculate statistical power and minimum detectable effect
- [ ] Add user-level experiment balance checks
- [ ] Investigate confidence intervals for relative lift
- [ ] Evaluate conversion lift across relevant segments
- [ ] Introduce incremental ROAS analysis where suitable data exists
- [ ] Incorporate campaign-level budget constraints
- [ ] Model diminishing returns from increased ad spend
- [ ] Validate proposed budget scenarios against historical outcomes
- [ ] Automate SQL, Python, and Power BI data refresh workflows
- [ ] Add dashboard screenshots and reproducible run instructions
- [ ] Implement automated data quality checks in a pipeline

---

## License

Add a `LICENSE` file specifying the terms under which the code and documentation may be used or redistributed.

If the source data comes from an external provider, review its data-use terms before sharing the underlying records.
