📊 Marketing Budget Optimization & A/B Testing

An end-to-end marketing analytics and business intelligence project using PostgreSQL, SQL, Python, Excel, and Power BI to evaluate campaign performance, compare marketing attribution methods, analyze advertising effectiveness, and explore proposed budget allocations.

The project integrates data quality assessment, analytical data modeling, campaign KPI development, statistical hypothesis testing, attribution analysis, budget scenario modeling, and interactive dashboard reporting.

📑 Table of Contents
Project Overview
Business Objectives
Technology Stack
Project Workflow
Data Quality Assessment
PostgreSQL Data Modeling
Campaign Performance Analysis
Marketing Attribution Analysis
A/B Testing and Statistical Analysis
Budget Scenario Analysis
Power BI Dashboards
Key Findings
Business Value
Repository Structure
How to Explore the Project
Methodological Limitations
Skills Demonstrated
Future Improvements
License
🎯 Project Overview

Marketing teams need to understand which campaigns and channels generate results, whether advertising is associated with improved conversion performance, and how marketing budgets might be allocated more effectively.

This project combines campaign performance analysis, marketing attribution, statistical A/B testing, and budget scenario analysis into a unified analytics workflow.

The solution covers the analytics lifecycle, from data quality checks and PostgreSQL data modeling to statistical inference, Excel-based financial scenarios, and interactive Power BI dashboards.

Project Objectives
Evaluate campaign and marketing-channel performance.
Analyze impressions, clicks, advertising spend, conversions, and conversion value.
Calculate campaign efficiency metrics, including CTR, CPC, cost per conversion, and ROAS.
Compare first-touch and last-touch attribution methods.
Compare conversion rates between advertising and public service announcement (PSA) groups.
Quantify observed conversion lift and evaluate statistical significance.
Compare historical campaign spending with proposed budget allocations.
Communicate analytical findings through interactive dashboards and technical documentation.
Analytics Lifecycle

Data Quality Assessment → PostgreSQL Modeling → Campaign Analysis → Attribution Analysis → A/B Testing → Budget Scenarios → Power BI → Business Insights

🎯 Business Objectives

The project addresses several important marketing and business questions:

Which campaigns and channels generate the strongest reported performance?
How efficiently does advertising spend translate into clicks and conversions?
How do first-touch and last-touch attribution methods distribute conversion credit?
Does the advertising group have a different observed conversion rate from the PSA group?
What is the size and statistical uncertainty of the observed conversion lift?
How might proposed budget changes affect expected conversion value?
Which assumptions and data limitations must be considered before making investment decisions?
🛠️ Technology Stack
Technology	Purpose
PostgreSQL	Data validation, transformation, analytical modeling, and aggregation
SQL	CTEs, joins, aggregations, views, and attribution analysis
Python	Statistical analysis and experiment evaluation
pandas	Data manipulation and analysis
NumPy	Numerical calculations
SciPy	Statistical testing
statsmodels	Statistical inference and confidence intervals
Microsoft Excel	Budget allocation scenarios and assumption-based projections
Power BI	Interactive dashboards, DAX measures, and visualization
Git and GitHub	Version control and project documentation
🔄 Project Workflow

The project follows a structured analytical process.

text
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
Attribution Analysis          A/B Testing
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
             Business Insights and
             Decision Support

Workflow Stages
Data validation: Inspect source fields, missing values, duplicate identifiers, and table relationships.
Data modeling: Organize campaign, channel, advertising, and conversion data in PostgreSQL.
Performance analysis: Calculate campaign KPIs and compare channels over time.
Attribution analysis: Compare first-touch and last-touch conversion credit.
Experiment analysis: Evaluate observed conversion differences between advertising and PSA groups.
Budget scenarios: Compare historical spending with proposed allocations and expected conversion value.
Business intelligence: Present campaign performance, experiment results, and budget scenarios through Power BI.
🔍 Data Quality Assessment

Data quality checks were performed before building the analytical model.

Validation Activities
Inspect column names and data types.
Identify duplicate identifiers.
Examine missing values.
Validate the grain of analytical tables.
Check relationships between source and analytical tables.
Investigate inconsistencies in conversion-related fields.
Review delivery-related metrics where applicable.
Document assumptions and known data limitations.
Documentation

Data quality findings are documented in:

documentation/data_quality_audit.md

The data dictionary and related documentation describe the source fields, analytical tables, and definitions used in downstream analysis.

🗄️ PostgreSQL Data Modeling

PostgreSQL serves as the relational database layer for campaign performance analysis, attribution modeling, and analytical reporting.

The project organizes source data into dimension tables, fact tables, a campaign performance mart, and analytical views.

Dimension Tables
Table	Purpose
dim_channel	Stores marketing channel information
dim_campaign	Stores campaign-level attributes
dim_ad_group	Stores advertising group attributes
Fact Tables
Table	Purpose
fact_daily_performance	Stores daily campaign performance metrics
fact_conversion	Stores conversion events and attribution-related information
fact_ab_test_user	Stores user-level A/B testing observations

The A/B testing dataset is analyzed separately because it does not share a suitable campaign-level key with the marketing campaign data.

Campaign Performance Mart

mart_campaign_performance

This analytical table summarizes campaign-level metrics for reporting and comparison.

Analytical Views
View	Purpose
vw_attribution_by_channel	Summarizes attribution results by channel
vw_attribution_comparison	Compares first-touch and last-touch attribution
vw_conversion_touchpoints	Examines conversion touchpoints
vw_campaign_budget_allocation	Supports campaign budget allocation analysis
vw_campaign_roas	Supports return on ad spend analysis
Data Modeling Considerations

Campaign-level summaries and daily performance records have different grains. Metrics from these tables should not be combined through joins without appropriate aggregation and join-key validation, as doing so can duplicate records and inflate totals.

📈 Campaign Performance Analysis

The campaign performance analysis evaluates marketing delivery, spending, and reported conversion outcomes across campaigns, channels, and time periods.

Core Metrics
Impressions.
Clicks.
Advertising spend.
Reported conversions.
Reported conversion value.
Click-through rate (CTR).
Cost per click (CPC).
Cost per reported conversion.
Return on ad spend (ROAS).
KPI Definitions

Click-Through Rate (CTR)

𝐶
𝑇
𝑅
=
Clicks
Impressions
×
100

Measures the percentage of impressions that result in clicks.

Cost per Click (CPC)

𝐶
𝑃
𝐶
=
Advertising Spend
Clicks

Measures average advertising spend per click.

Cost per Reported Conversion

Cost per Conversion
=
Advertising Spend
Reported Conversions

Measures average advertising spend per reported conversion.

Return on Ad Spend (ROAS)

𝑅
𝑂
𝐴
𝑆
=
Reported Conversion Value
Advertising Spend

Measures reported conversion value relative to advertising spend.

Analytical Questions
Which campaigns generate the most reported conversion value?
Which channels have the highest or lowest CPC?
How does CTR vary across campaigns?
Which campaigns have the strongest ROAS?
How do spending and reported conversions change over time?
Are higher-spending campaigns also generating more conversion value?

Important: ROAS measures reported conversion value relative to advertising spend. It does not measure profit, and it should not automatically be interpreted as incremental return.

🔗 Marketing Attribution Analysis

Marketing attribution analysis investigates how conversion credit is assigned to different marketing channels.

The project uses SQL views to compare first-touch and last-touch attribution.

First-Touch Attribution

First-touch attribution assigns conversion credit to the first recorded marketing touchpoint in the defined attribution journey.

It helps examine which channels introduce users to the marketing journey.

Last-Touch Attribution

Last-touch attribution assigns conversion credit to the last recorded eligible touchpoint before conversion.

It helps examine which channels appear closest to the conversion event under the selected attribution rules.

Attribution Analysis Areas
First-touch attributed conversions.
Last-touch attributed conversions.
Channel-level attributed conversion value.
Differences in conversion credit across attribution methods.
Conversion touchpoint analysis.
Campaign and channel comparisons.
Attribution Comparison
Analytical Dimension	First-Touch	Last-Touch
Credit assignment	First eligible recorded touchpoint	Last eligible recorded touchpoint
Main perspective	Initial discovery	Final recorded interaction
Useful for	Understanding entry channels	Understanding channels closest to conversion
Main limitation	May understate later touchpoints	May understate earlier touchpoints

Attribution results depend on tracking coverage, attribution windows, touchpoint definitions, and the available source data.

Attribution is not automatically causal. Differences between attribution methods describe how credit is assigned under the selected rules; they do not establish the incremental effect of each channel.

🧪 A/B Testing and Statistical Analysis

The project uses Python, pandas, SciPy, and statsmodels to compare conversion outcomes between an advertising group and a public service announcement (PSA) group.

The analysis includes:

Group sizes.
Converted-user counts.
Conversion rates.
Absolute conversion lift.
Relative conversion lift.
Two-proportion z-test.
P-value.
Confidence interval for the conversion-rate difference.
Descriptive analysis of advertising exposure.
Experiment Results
Metric	Advertising Group	PSA Group
Users	564,577	23,524
Converted users	14,423	420
Conversion rate	2.5547%	1.7854%
Average ads seen	24.8234	24.7611
Statistical Summary
Metric	Result
Absolute conversion lift	Approximately 0.7692 percentage points
Relative conversion lift	Approximately 43.09%
Two-proportion z-statistic	Approximately 7.3701
P-value	Approximately 
1.705
×
10
−
13

95% confidence interval for absolute lift	Approximately 0.5873 to 0.9360 percentage points
Absolute Conversion Lift

Absolute lift measures the difference between the two observed conversion rates.

Absolute Lift
=
𝑝
ads
−
𝑝
PSA

Using the reported rates:

2.5547
%
−
1.7854
%
≈
0.7692

The advertising group recorded approximately 0.77 percentage points higher conversion than the PSA group.

Relative Conversion Lift

Relative lift expresses the observed difference relative to the PSA group's conversion rate.

Relative Lift
=
𝑝
ads
−
𝑝
PSA
𝑝
PSA
×
100

The reported relative lift is approximately 43.09%.

Statistical Hypothesis

The two-proportion z-test evaluates the following hypotheses:

Null hypothesis (
𝐻
0
): The population conversion rates are equal.
Alternative hypothesis (
𝐻
1
): The population conversion rates differ.

The reported p-value is substantially below the conventional 0.05 significance threshold. Under the assumptions of the test, the observed difference is statistically significant.

The reported confidence interval also excludes zero, indicating a positive difference in observed conversion rates under the stated analysis.

Interpretation and Causal Limitations

The advertising group recorded a higher conversion rate than the PSA group in the available dataset.

However, the experiment documentation did not confirm whether users were randomly assigned to the two groups. Without verified random assignment and a suitable experimental design, the difference should be interpreted as an observed association, not definitive proof that advertising caused the increase.

Additional considerations include:

Potential selection bias.
Differences in group composition.
User-level independence assumptions.
Duplicate or repeated user observations.
The assignment mechanism.
Whether the outcome window was consistent across groups.

Statistical significance alone does not establish causality or business value.

💰 Budget Scenario Analysis

Microsoft Excel is used to compare historical campaign spending with a proposed budget allocation scenario.

The analysis combines historical performance metrics with explicit assumptions about proposed campaign budgets and expected ROAS.

Analysis Components
Historical campaign spending.
Historical ROAS.
Proposed campaign budgets.
Assumed ROAS.
Historical conversion value.
Expected conversion value under scenario assumptions.
Campaign-level budget comparisons.
Scenario Modeling Logic

A simplified projection can be expressed as:

Expected Conversion Value
=
Proposed Budget
×
Assumed ROAS

For example, a proposed budget of $10,000 and an assumed ROAS of 2.5 would imply $25,000 in projected conversion value.

This is an illustrative calculation, not a claim about the project's actual budget values.

Important Assumptions

Budget scenarios depend on:

The accuracy of historical spend and conversion value.
The validity of assumed ROAS values.
Whether historical performance can reasonably inform future outcomes.
Whether campaign performance changes as budgets increase or decrease.
Potential diminishing returns and capacity constraints.

Expected conversion value is an assumption-based projection, not guaranteed revenue or profit.

The budget scenario assumptions are documented in:

documentation/budget_scenario_assumptions.md

📊 Power BI Dashboards

The project includes a four-page interactive Power BI report designed to connect campaign performance, attribution, experiment results, and budget scenarios.

1. Marketing Performance — Executive Overview

Provides a high-level view of campaign performance and advertising efficiency.

Key features

Total advertising spend.
Reported conversions and conversion value.
CTR and CPC.
Monthly performance trends.
Advertising spend by channel.
Campaign-level performance details.

!Marketing Performance — Executive Overview

2. Campaign & Channel Performance

Compares marketing campaigns and channels across delivery and efficiency metrics.

Key features

Total clicks and impressions.
CPC and campaign conversion rate.
Campaign spend comparisons.
Channel spend versus reported conversion value.
Campaign efficiency scatter plot.
Campaign performance details.

!Campaign & Channel Performance

3. A/B Testing Analysis

Presents the experiment results and their statistical interpretation.

Key features

Advertising and PSA conversion rates.
Absolute and relative conversion lift.
Conversion outcomes by test group.
Conversion rate by advertising exposure.
Statistical test results.
Confidence interval.
Methodological limitations.

!A/B Testing Analysis

4. Budget Optimization

Compares historical campaign spending with the proposed budget scenario.

Key features

Historical spend and proposed budget.
Budget change by campaign.
Historical and expected conversion value.
Campaign-level budget comparisons.
Proposed budget allocation visualization.

!Budget Optimization

Dashboard Design Goals

The report is intended to help stakeholders:

Monitor marketing performance.
Compare campaign efficiency.
Understand attribution differences.
Interpret experimental findings.
Explore alternative budget allocations.

The screenshots above are repository references. Confirm that the files exist and represent the final report before publishing the README.

💡 Key Findings
1. Observed Conversion-Rate Difference

The advertising group recorded a conversion rate of approximately 2.55%, compared with 1.79% for the PSA group.

2. Positive Observed Conversion Lift

The absolute difference was approximately 0.77 percentage points, corresponding to a relative increase of approximately 43.09% over the PSA conversion rate.

3. Statistical Significance

The two-proportion z-test returned a p-value of approximately 
1.705
×
10
−
13
.

The observed difference is statistically significant under the assumptions of the test, although causal interpretation requires evidence that the experimental design supports it.

4. Attribution Comparisons

The SQL attribution views enable comparisons between first-touch and last-touch conversion credit.

These comparisons help explain how different attribution rules change the distribution of reported conversion value across channels.

5. Budget Scenario Analysis

Excel and Power BI compare historical campaign metrics with proposed budget allocations and assumption-based conversion value projections.

Recommendations should depend on the validity of the scenario assumptions, historical performance, and evidence about how campaigns respond to budget changes.

Evidence and Documentation

Detailed findings and interpretation are documented in:

documentation/key_findings.md

The statistical methodology is documented in:

documentation/statistical_analysis.md

💼 Business Value

The project provides a foundation for evidence-informed marketing decisions.

Campaign Management
Compare channel and campaign performance.
Monitor advertising delivery and conversion outcomes.
Identify campaigns requiring further investigation.
Measurement and Attribution
Compare first-touch and last-touch attribution.
Understand how attribution definitions affect reported channel performance.
Improve transparency in conversion reporting.
Experiment Evaluation
Quantify observed conversion differences.
Interpret p-values and confidence intervals.
Document assumptions required for causal conclusions.
Budget Planning
Compare historical spend with proposed allocations.
Model expected conversion value using explicit assumptions.
Identify the additional evidence needed before changing real budgets.
Business Intelligence
Standardize campaign metrics.
Communicate findings through interactive dashboards.
Connect analytical results with documented business assumptions.
📂 Repository Structure

The following structure represents the project organization described in the documentation. Refer to the actual repository for the complete list of files and scripts.

text
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


Note: The filenames in this example illustrate a possible organization. Retain the actual filenames from your repository rather than renaming or creating files solely to match this template.

▶️ How to Explore the Project
Review the data documentation to understand source datasets, field definitions, and data quality limitations.
Explore the PostgreSQL SQL scripts to examine data validation, campaign performance metrics, and attribution views.
Review the Python statistical analysis to understand the experiment setup, conversion calculations, hypothesis test, and confidence interval.
Examine the Excel budget workbook to understand historical performance and proposed allocation assumptions.
Open the Power BI report to explore campaign performance, attribution-related metrics, A/B testing, and budget scenarios.
Read the methodology documentation to understand the interpretation of results and limitations.
⚠️ Methodological Limitations
Reported Conversions

Reported conversions may not represent unique customers or new customer acquisitions. Their meaning depends on the source tracking and conversion definitions.

ROAS Versus Profit

ROAS is calculated from reported conversion value and advertising spend. It does not account for all business costs, margins, or incremental effects.

Attribution Limitations

First-touch and last-touch attribution depend on available tracking data and attribution rules. They describe the assignment of conversion credit rather than necessarily identifying causal channel effects.

A/B Test Assignment

The available documentation does not confirm random assignment. Therefore, the observed conversion difference cannot automatically be interpreted as the causal impact of advertising.

Statistical Assumptions

The validity of the two-proportion z-test depends on the experimental unit, independence assumptions, sample structure, and suitability of the statistical method.

Budget Scenario Assumptions

Expected conversion value depends on proposed budget values and assumed ROAS. These projections require validation and should not be presented as guaranteed results.

Analytical Table Grain

Campaign-level and daily fact tables have different levels of detail. Joining them without appropriate aggregation can duplicate rows and inflate metrics.

🧠 Skills Demonstrated
SQL and Database Analytics
PostgreSQL data modeling.
Data quality assessment.
Joins and aggregations.
Common Table Expressions (CTEs).
Analytical views.
Campaign KPI development.
Marketing attribution analysis.
Python and Statistics
pandas and NumPy.
Statistical hypothesis testing.
Two-proportion z-tests.
P-value interpretation.
Confidence interval analysis.
Conversion lift calculations.
Statistical reporting.
Excel and Financial Scenarios
Budget allocation modeling.
Historical performance comparisons.
Assumption-based projections.
Scenario analysis.
Power BI and DAX
Data modeling.
DAX measures.
Interactive dashboards.
KPI reporting.
Campaign and channel visualization.
Experiment-result communication.
Business and Analytical Communication
Marketing performance evaluation.
Attribution interpretation.
Budget decision support.
Methodological documentation.
Data storytelling.
Analytical limitations assessment.
🚀 Future Improvements

Potential enhancements include:

Verify and document randomized experiment assignment.
Calculate statistical power and minimum detectable effect.
Add user-level experiment balance checks.
Investigate confidence intervals for relative lift.
Evaluate conversion lift across relevant segments.
Introduce incremental ROAS analysis where suitable data is available.
Incorporate campaign-level budget constraints.
Model diminishing returns from increased advertising spend.
Validate proposed budget scenarios against historical outcomes.
Automate SQL, Python, and Power BI data refresh workflows.
Add dashboard screenshots and reproducible run instructions.
Implement data quality checks in an automated pipeline.
📄 License

Add a LICENSE file specifying the terms under which the project code and documentation may be used or redistributed.

If the source data comes from an external provider, review its applicable data-use terms before sharing the underlying records.

👨‍💻 Project Summary

Project: Marketing Budget Optimization & A/B Testing

Technologies: PostgreSQL · SQL · Python · pandas · NumPy · SciPy · statsmodels · Excel · Power BI · DAX

Focus areas: Marketing analytics · Attribution analysis · Statistical testing · Budget scenarios · Business intelligence

This project demonstrates an integrated marketing analytics workflow that combines campaign performance measurement, attribution comparisons, statistical inference, budget scenario modeling, and interactive reporting to support more informed marketing decisions.
