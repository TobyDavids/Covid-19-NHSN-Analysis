# Covid-19-NHSN-Analysis
COVID-19 National Healthcare Safety Network (NHSN) Data Analysis
![](nhs_safety.jpg)

## INTRODUCTION 
The CDC’s National Healthcare Safety Network is the United States most widely used healthcare-associated infection (HAI) tracking system. NHSN provides facilities, health departments, tracking system, states, regions, and the nation with data needed to identify problem areas, measure progress of prevention efforts, and ultimately eliminate healthcare-associated infections.
For this project, we focused our analysis on the impact of covid-19 on nursing home residents, facilities, and to derive insights from the NHSN dataset. 

## PROBLEM STATEMENT 
- Perform an Exploratory Data Analysis on the COVID-19 National Healthcare Safety Network (NHSN) dataset!
- Perform statistical analysis to understand the impact of COVID-19 on healthcare facilities.
- Generate meaningful visualizations and reports to present the findings to stakeholders.
- Identify potential areas for improvement in healthcare practices based on the analysis.

## Tools Used
To transform, analyze and provide insights, the following tools were used
- Microsoft Excel
- Power Query
- SQL
- Power BI

## Skills Demonstrated
- Data Manipulation
- Functions used to Query (Aggregate functions, UNION Set Operations, Common Table Expressions, Correlations, Joins, Case Statements)
- DAX
- Quick Measures
- Filters use in Power BI
- Slicers in Power Bi

## DATA EXPLORATION & CLEANING
I explored the four datasets with excel, where I noted data quality issues needed for cleaning and transformation. The links to the datasets are seen here- 
- [2020](https://download.cms.gov/covid_nhsn/faclevel_2020.zip)
- [2021](https://download.cms.gov/covid_nhsn/faclevel_2021.zip)
- [2022](https://download.cms.gov/covid_nhsn/faclevel_2022.zip)
- [2023](https://download.cms.gov/covid_nhsn/faclevel_2023.zip)

For 2020, the original dataset contained 491,095 rows and 137 columns. The following steps breaks down the cleaning process I took in ensuring the dataset was clean, valid and ready for analysis.
- Dropped Irrelevant columns 
- Converted Facility no to text Datatype
- 4837 NULL records WERE dropped
- Filled missing values with 'N/A' for string/text variables
After cleaning, 486,258 rows and 27 columns remained.

For 2021, the original dataset contained 793,610 rows and 137 columns.. The following steps breaks down the cleaning process I took in ensuring the dataset was clean, valid and ready for analysis.
- Dropped Irrelevant columns 
- Converted Facility no to text datatype 
- 5222 NULL records WERE dropped
- Filled missing values with 'N/A' for string/text variables
After cleaning, 788,388 rows and 27 columns remained.

For 2022, the original dataset contained 786,883 rows and 137 columns... The following steps breaks down the cleaning process I took in ensuring the dataset was clean, valid and ready for analysis.
- Dropped Irrelevant columns 
- Converted Facility no to text datatype 
- 4748 of NULL records WERE dropped
- Filled missing values with 'N/A' for string/text variables
After cleaning, 782,134 rows and 27 columns remained.


For 2023, the original dataset contained 285,742 rows and 137 columns... The following steps breaks down the cleaning process I took in ensuring the dataset was clean, valid and ready for analysis.
- Dropped Irrelevant columns 
- Converted Facility no to text datatype 
- 1174  of NULL records WERE dropped
- Filled missing values with 'N/A' for string/text variables
After cleaning, 285,028 rows and 27 columns remained.



Cleaning_1              |           Cleaning_2
:------------------------:|:---------------------:
![](exploration.png)     |![](cleaning.png)

## DATA ANALYSIS
For this phase, I created the NHS Database in postgreSQL, to accomodate the cleaned dataset and imported the 4 tables into the NHS. After importing the tables, I further explored the datasets, to ask questions that were used to answr questions and  provide Insights. The project questions were 

1. Which healthcare facilities had the highest average number of daily COVID-19 cases in
2021? Display the top 10 facilities.
2. Calculate the 7-day moving average of new COVID-19 cases for each healthcare facility.
Which facility had the highest peak in the moving average?
3. Determine the total number of COVID-19 cases, deaths, and recoveries for each state.
Include the state's name and the corresponding counts in the result.
4. Find the top 5 states with the highest mortality rate (deaths per COVID-19 case) in 2022.
5. Identify the healthcare facilities that experienced a significant increase in COVID-19
cases from 2020 to 2021 (more than 50% increase). Display the facility names and the
percentage increase.
6. What is the total number of COVID-19 cases, deaths, and recoveries recorded in the
dataset?
7. Find the healthcare facilities that had a consistent increase in COVID-19 cases for at
least 5 consecutive months. Display the facility names and the corresponding months.
8. Calculate the mortality rate (deaths per COVID-19 case) for each healthcare facility.
9. Are there any significant differences in COVID-19 outcomes based on the type of
healthcare facility (e.g., hospital, nursing home)?
10. How has the number of COVID-19 cases evolved over time (monthly or quarterly)?
11. What is the distribution of healthcare facilities by state?
12. Find the healthcare facilities with the highest occupancy rates for COVID-19 patients.


The solutions to these questions can be found [here](https://github.com/TobyDavids/Covid-19-NHSN-Analysis/edit/main/Covid-19-NHSN-Analysis.md)


## DATA VISUALIZATION
The dashboard contains 1 page of report. It was created to provide insights and answer the questions posed during the analysis phase. 
You can interact with the dashboard [here](https://app.powerbi.com/groups/me/reports/05f48389-54f2-44ce-8eaf-2cfae370c163/ReportSection?experience=power-bi). 
![](covid_viz.jpg)

## INSIGHTS
1. Between 1970 and 2014, there were 112,000 successful attacks, 283,000 confirmed deaths and 356,000 injured victims.
2. 
