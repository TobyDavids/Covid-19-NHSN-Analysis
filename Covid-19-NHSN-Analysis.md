## QUESTIONS

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

## SOLUTIONS

1. 
```sql
SELECT facility_name, ROUND(AVG(weekly_resident_covid_cases/7), 2) avg_daily_cases
FROM nhs_2021
GROUP BY 1
ORDER BY 2 DESC
LIMIT 10;
```
2. 
```sql
WITH comb_data  AS
(
SELECT facility_name, weekly_resident_covid_cases, week_ending
FROM nhs_2020
UNION ALL
SELECT facility_name, weekly_resident_covid_cases, week_ending
FROM nhs_2021
UNION ALL
SELECT facility_name, weekly_resident_covid_cases, week_ending
FROM nhs_2022
UNION ALL
SELECT facility_name, weekly_resident_covid_cases, week_ending
FROM nhs_2023
),
comb_data_2 AS
(
SELECT facility_name, week_ending,
	   ROUND((weekly_resident_covid_cases/7)::numeric, 2) AS daily_cases,
	   AVG(weekly_resident_covid_cases::numeric /7) OVER
	   (PARTITION BY facility_name ORDER BY week_ending 
		ROWS BETWEEN 6 PRECEDING AND CURRENT ROW) AS moving_avg
FROM comb_data
)
SELECT facility_name, MAX(moving_avg) AS highest_peak
FROM comb_data_2
GROUP BY 1
ORDER BY 2 DESC 
LIMIT 1;
```

3.
```sql
WITH sub AS
(SELECT DISTINCT
       CASE 
           WHEN state ='AL' THEN 'Alabama'
           WHEN state ='AK' THEN 'Alaska'
           WHEN state ='AZ' THEN 'Arizona'
           WHEN state ='AR' THEN 'Arkansas'
           WHEN state ='CA' THEN 'California'
           WHEN state ='CO' THEN 'Colorado'
           WHEN state ='CT' THEN 'Connecticut'
		       WHEN state ='DC' THEN 'Federal District of Colombia'
           WHEN state ='DE' THEN 'Delaware'
           WHEN state ='FL' THEN 'Florida'
           WHEN state ='GA' THEN 'Georgia'
		       WHEN state ='GU' THEN 'Guam'
           WHEN state ='HI' THEN 'Hawaii'
           WHEN state ='ID' THEN 'Idaho'
           WHEN state ='IL' THEN 'Illinois'
           WHEN state ='IN' THEN 'Indiana'
           WHEN state ='IA' THEN 'Iowa'
           WHEN state ='KS' THEN 'Kansas'
           WHEN state ='KY' THEN 'Kentucky'
           WHEN state ='LA' THEN 'Louisiana'
           WHEN state ='ME' THEN 'Maine'
           WHEN state ='MD' THEN 'Maryland'
           WHEN state ='MA' THEN 'Massachusetts'
           WHEN state ='MI' THEN 'Michigan'
           WHEN state ='MN' THEN 'Minnesota'
           WHEN state ='MS' THEN 'Mississippi'
           WHEN state ='MO' THEN 'Missouri'
           WHEN state ='MT' THEN 'Montana'
           WHEN state ='NE' THEN 'Nebraska'
           WHEN state ='NV' THEN 'Nevada'
           WHEN state ='NH' THEN 'New Hampshire'
           WHEN state ='NJ' THEN 'New Jersey'
           WHEN state ='NM' THEN 'New Mexico'
           WHEN state ='NY' THEN 'New York'
           WHEN state ='NC' THEN 'North Carolina'
           WHEN state ='ND' THEN 'North Dakota'
           WHEN state ='OH' THEN 'Ohio'
           WHEN state ='OK' THEN 'Oklahoma'
           WHEN state ='OR' THEN 'Oregon'
           WHEN state ='PA' THEN 'Pennsylvania'
		       WHEN state ='PR' THEN 'Puerto Rico'
           WHEN state ='RI' THEN 'Rhode Island'
           WHEN state ='SC' THEN 'South Carolina'
           WHEN state ='SD' THEN 'South Dakota'
           WHEN state ='TN' THEN 'Tennessee'
           WHEN state ='TX' THEN 'Texas'
           WHEN state ='UT' THEN 'Utah'
           WHEN state ='VT' THEN 'Vermont'
           WHEN state ='VA' THEN 'Virginia'
           WHEN state ='WA' THEN 'Washington'
           WHEN state ='WV' THEN 'West Virginia'
           WHEN state ='WI' THEN 'Wisconsin'
           WHEN state ='WY' THEN 'Wyoming'
           ELSE 'Unknown'
       END AS state_name, weekly_resident_covid_cases, weekly_resident_covid_deaths	
FROM nhs_2020

UNION ALL
SELECT DISTINCT
       CASE 
           WHEN state ='AL' THEN 'Alabama'
           WHEN state ='AK' THEN 'Alaska'
           WHEN state ='AZ' THEN 'Arizona'
           WHEN state ='AR' THEN 'Arkansas'
           WHEN state ='CA' THEN 'California'
           WHEN state ='CO' THEN 'Colorado'
           WHEN state ='CT' THEN 'Connecticut'
		       WHEN state ='DC' THEN 'Federal District of Colombia'
           WHEN state ='DE' THEN 'Delaware'
           WHEN state ='FL' THEN 'Florida'
           WHEN state ='GA' THEN 'Georgia'
		       WHEN state ='GU' THEN 'Guam'
           WHEN state ='HI' THEN 'Hawaii'
           WHEN state ='ID' THEN 'Idaho'
           WHEN state ='IL' THEN 'Illinois'
           WHEN state ='IN' THEN 'Indiana'
           WHEN state ='IA' THEN 'Iowa'
           WHEN state ='KS' THEN 'Kansas'
           WHEN state ='KY' THEN 'Kentucky'
           WHEN state ='LA' THEN 'Louisiana'
           WHEN state ='ME' THEN 'Maine'
           WHEN state ='MD' THEN 'Maryland'
           WHEN state ='MA' THEN 'Massachusetts'
           WHEN state ='MI' THEN 'Michigan'
           WHEN state ='MN' THEN 'Minnesota'
           WHEN state ='MS' THEN 'Mississippi'
           WHEN state ='MO' THEN 'Missouri'
           WHEN state ='MT' THEN 'Montana'
           WHEN state ='NE' THEN 'Nebraska'
           WHEN state ='NV' THEN 'Nevada'
           WHEN state ='NH' THEN 'New Hampshire'
           WHEN state ='NJ' THEN 'New Jersey'
           WHEN state ='NM' THEN 'New Mexico'
           WHEN state ='NY' THEN 'New York'
           WHEN state ='NC' THEN 'North Carolina'
           WHEN state ='ND' THEN 'North Dakota'
           WHEN state ='OH' THEN 'Ohio'
           WHEN state ='OK' THEN 'Oklahoma'
           WHEN state ='OR' THEN 'Oregon'
           WHEN state ='PA' THEN 'Pennsylvania'
		       WHEN state ='PR' THEN 'Puerto Rico'
           WHEN state ='RI' THEN 'Rhode Island'
           WHEN state ='SC' THEN 'South Carolina'
           WHEN state ='SD' THEN 'South Dakota'
           WHEN state ='TN' THEN 'Tennessee'
           WHEN state ='TX' THEN 'Texas'
           WHEN state ='UT' THEN 'Utah'
           WHEN state ='VT' THEN 'Vermont'
           WHEN state ='VA' THEN 'Virginia'
           WHEN state ='WA' THEN 'Washington'
           WHEN state ='WV' THEN 'West Virginia'
           WHEN state ='WI' THEN 'Wisconsin'
           WHEN state ='WY' THEN 'Wyoming'
           ELSE 'Unknown'
          END AS state_name, weekly_resident_covid_cases, weekly_resident_covid_deaths	
FROM nhs_2021

UNION ALL
SELECT DISTINCT
       CASE 
           WHEN state ='AL' THEN 'Alabama'
           WHEN state ='AK' THEN 'Alaska'
           WHEN state ='AZ' THEN 'Arizona'
           WHEN state ='AR' THEN 'Arkansas'
           WHEN state ='CA' THEN 'California'
           WHEN state ='CO' THEN 'Colorado'
           WHEN state ='CT' THEN 'Connecticut'
		       WHEN state ='DC' THEN 'Federal District of Colombia'
           WHEN state ='DE' THEN 'Delaware'
           WHEN state ='FL' THEN 'Florida'
           WHEN state ='GA' THEN 'Georgia'
	    	   WHEN state ='GU' THEN 'Guam'
           WHEN state ='HI' THEN 'Hawaii'
           WHEN state ='ID' THEN 'Idaho'
           WHEN state ='IL' THEN 'Illinois'
           WHEN state ='IN' THEN 'Indiana'
           WHEN state ='IA' THEN 'Iowa'
           WHEN state ='KS' THEN 'Kansas'
           WHEN state ='KY' THEN 'Kentucky'
           WHEN state ='LA' THEN 'Louisiana'
           WHEN state ='ME' THEN 'Maine'
           WHEN state ='MD' THEN 'Maryland'
           WHEN state ='MA' THEN 'Massachusetts'
           WHEN state ='MI' THEN 'Michigan'
           WHEN state ='MN' THEN 'Minnesota'
           WHEN state ='MS' THEN 'Mississippi'
           WHEN state ='MO' THEN 'Missouri'
           WHEN state ='MT' THEN 'Montana'
           WHEN state ='NE' THEN 'Nebraska'
           WHEN state ='NV' THEN 'Nevada'
           WHEN state ='NH' THEN 'New Hampshire'
           WHEN state ='NJ' THEN 'New Jersey'
           WHEN state ='NM' THEN 'New Mexico'
           WHEN state ='NY' THEN 'New York'
           WHEN state ='NC' THEN 'North Carolina'
           WHEN state ='ND' THEN 'North Dakota'
           WHEN state ='OH' THEN 'Ohio'
           WHEN state ='OK' THEN 'Oklahoma'
           WHEN state ='OR' THEN 'Oregon'
           WHEN state ='PA' THEN 'Pennsylvania'
		       WHEN state ='PR' THEN 'Puerto Rico'
           WHEN state ='RI' THEN 'Rhode Island'
           WHEN state ='SC' THEN 'South Carolina'
           WHEN state ='SD' THEN 'South Dakota'
           WHEN state ='TN' THEN 'Tennessee'
           WHEN state ='TX' THEN 'Texas'
           WHEN state ='UT' THEN 'Utah'
           WHEN state ='VT' THEN 'Vermont'
           WHEN state ='VA' THEN 'Virginia'
           WHEN state ='WA' THEN 'Washington'
           WHEN state ='WV' THEN 'West Virginia'
           WHEN state ='WI' THEN 'Wisconsin'
           WHEN state ='WY' THEN 'Wyoming'
           ELSE 'Unknown'
       END AS state_name, weekly_resident_covid_cases, weekly_resident_covid_deaths	
FROM nhs_2022

UNION ALL
SELECT DISTINCT
       CASE 
           WHEN state ='AL' THEN 'Alabama'
           WHEN state ='AK' THEN 'Alaska'
           WHEN state ='AZ' THEN 'Arizona'
           WHEN state ='AR' THEN 'Arkansas'
           WHEN state ='CA' THEN 'California'
           WHEN state ='CO' THEN 'Colorado'
           WHEN state ='CT' THEN 'Connecticut'
		       WHEN state ='DC' THEN 'Federal District of Colombia'
           WHEN state ='DE' THEN 'Delaware'
           WHEN state ='FL' THEN 'Florida'
           WHEN state ='GA' THEN 'Georgia'
		       WHEN state ='GU' THEN 'Guam'
           WHEN state ='HI' THEN 'Hawaii'
           WHEN state ='ID' THEN 'Idaho'
           WHEN state ='IL' THEN 'Illinois'
           WHEN state ='IN' THEN 'Indiana'
           WHEN state ='IA' THEN 'Iowa'
           WHEN state ='KS' THEN 'Kansas'
           WHEN state ='KY' THEN 'Kentucky'
           WHEN state ='LA' THEN 'Louisiana'
           WHEN state ='ME' THEN 'Maine'
           WHEN state ='MD' THEN 'Maryland'
           WHEN state ='MA' THEN 'Massachusetts'
           WHEN state ='MI' THEN 'Michigan'
           WHEN state ='MN' THEN 'Minnesota'
           WHEN state ='MS' THEN 'Mississippi'
           WHEN state ='MO' THEN 'Missouri'
           WHEN state ='MT' THEN 'Montana'
           WHEN state ='NE' THEN 'Nebraska'
           WHEN state ='NV' THEN 'Nevada'
           WHEN state ='NH' THEN 'New Hampshire'
           WHEN state ='NJ' THEN 'New Jersey'
           WHEN state ='NM' THEN 'New Mexico'
           WHEN state ='NY' THEN 'New York'
           WHEN state ='NC' THEN 'North Carolina'
           WHEN state ='ND' THEN 'North Dakota'
           WHEN state ='OH' THEN 'Ohio'
           WHEN state ='OK' THEN 'Oklahoma'
           WHEN state ='OR' THEN 'Oregon'
           WHEN state ='PA' THEN 'Pennsylvania'
	    	   WHEN state ='PR' THEN 'Puerto Rico'
           WHEN state ='RI' THEN 'Rhode Island'
           WHEN state ='SC' THEN 'South Carolina'
           WHEN state ='SD' THEN 'South Dakota'
           WHEN state ='TN' THEN 'Tennessee'
           WHEN state ='TX' THEN 'Texas'
           WHEN state ='UT' THEN 'Utah'
           WHEN state ='VT' THEN 'Vermont'
           WHEN state ='VA' THEN 'Virginia'
           WHEN state ='WA' THEN 'Washington'
           WHEN state ='WV' THEN 'West Virginia'
           WHEN state ='WI' THEN 'Wisconsin'
           WHEN state ='WY' THEN 'Wyoming'
           ELSE 'Unknown'
       END AS state_name, weekly_resident_covid_cases, weekly_resident_covid_deaths	   
FROM nhs_2023
)

SELECT DISTINCT state_name, SUM(weekly_resident_covid_cases)tot_cases, 
 	   ROUND(SUM(weekly_resident_covid_deaths)::numeric/SUM(weekly_resident_covid_cases)::numeric, 2)
	   AS mortality_rate,
 	   SUM(weekly_resident_covid_deaths) tot_deaths,
	   SUM(weekly_resident_covid_cases)-SUM(weekly_resident_covid_deaths)tot_recoveries, 
	   ROUND(((SUM(weekly_resident_covid_cases)-SUM(weekly_resident_covid_deaths)::numeric))
		/SUM(weekly_resident_covid_cases)::numeric, 2) AS recovery_rate
FROM sub
GROUP BY 1
ORDER BY 2 DESC;
```
4.
```sql
WITH sub AS
(SELECT DISTINCT
       CASE 
           WHEN state ='AL' THEN 'Alabama'
           WHEN state ='AK' THEN 'Alaska'
           WHEN state ='AZ' THEN 'Arizona'
           WHEN state ='AR' THEN 'Arkansas'
           WHEN state ='CA' THEN 'California'
           WHEN state ='CO' THEN 'Colorado'
           WHEN state ='CT' THEN 'Connecticut'
		       WHEN state ='DC' THEN 'Federal District of Colombia'
           WHEN state ='DE' THEN 'Delaware'
           WHEN state ='FL' THEN 'Florida'
           WHEN state ='GA' THEN 'Georgia'
		       WHEN state ='GU' THEN 'Guam'
           WHEN state ='HI' THEN 'Hawaii'
           WHEN state ='ID' THEN 'Idaho'
           WHEN state ='IL' THEN 'Illinois'
           WHEN state ='IN' THEN 'Indiana'
           WHEN state ='IA' THEN 'Iowa'
           WHEN state ='KS' THEN 'Kansas'
           WHEN state ='KY' THEN 'Kentucky'
           WHEN state ='LA' THEN 'Louisiana'
           WHEN state ='ME' THEN 'Maine'
           WHEN state ='MD' THEN 'Maryland'
           WHEN state ='MA' THEN 'Massachusetts'
           WHEN state ='MI' THEN 'Michigan'
           WHEN state ='MN' THEN 'Minnesota'
           WHEN state ='MS' THEN 'Mississippi'
           WHEN state ='MO' THEN 'Missouri'
           WHEN state ='MT' THEN 'Montana'
           WHEN state ='NE' THEN 'Nebraska'
           WHEN state ='NV' THEN 'Nevada'
           WHEN state ='NH' THEN 'New Hampshire'
           WHEN state ='NJ' THEN 'New Jersey'
           WHEN state ='NM' THEN 'New Mexico'
           WHEN state ='NY' THEN 'New York'
           WHEN state ='NC' THEN 'North Carolina'
           WHEN state ='ND' THEN 'North Dakota'
           WHEN state ='OH' THEN 'Ohio'
           WHEN state ='OK' THEN 'Oklahoma'
           WHEN state ='OR' THEN 'Oregon'
           WHEN state ='PA' THEN 'Pennsylvania'
		       WHEN state ='PR' THEN 'Puerto Rico'
           WHEN state ='RI' THEN 'Rhode Island'
           WHEN state ='SC' THEN 'South Carolina'
           WHEN state ='SD' THEN 'South Dakota'
           WHEN state ='TN' THEN 'Tennessee'
           WHEN state ='TX' THEN 'Texas'
           WHEN state ='UT' THEN 'Utah'
           WHEN state ='VT' THEN 'Vermont'
           WHEN state ='VA' THEN 'Virginia'
           WHEN state ='WA' THEN 'Washington'
           WHEN state ='WV' THEN 'West Virginia'
           WHEN state ='WI' THEN 'Wisconsin'
           WHEN state ='WY' THEN 'Wyoming'
           ELSE 'Unknown'
       END AS state_name, weekly_resident_covid_cases, weekly_resident_covid_deaths	
FROM nhs_2020

UNION ALL
SELECT DISTINCT
       CASE 
           WHEN state ='AL' THEN 'Alabama'
           WHEN state ='AK' THEN 'Alaska'
           WHEN state ='AZ' THEN 'Arizona'
           WHEN state ='AR' THEN 'Arkansas'
           WHEN state ='CA' THEN 'California'
           WHEN state ='CO' THEN 'Colorado'
           WHEN state ='CT' THEN 'Connecticut'
		       WHEN state ='DC' THEN 'Federal District of Colombia'
           WHEN state ='DE' THEN 'Delaware'
           WHEN state ='FL' THEN 'Florida'
           WHEN state ='GA' THEN 'Georgia'
		       WHEN state ='GU' THEN 'Guam'
           WHEN state ='HI' THEN 'Hawaii'
           WHEN state ='ID' THEN 'Idaho'
           WHEN state ='IL' THEN 'Illinois'
           WHEN state ='IN' THEN 'Indiana'
           WHEN state ='IA' THEN 'Iowa'
           WHEN state ='KS' THEN 'Kansas'
           WHEN state ='KY' THEN 'Kentucky'
           WHEN state ='LA' THEN 'Louisiana'
           WHEN state ='ME' THEN 'Maine'
           WHEN state ='MD' THEN 'Maryland'
           WHEN state ='MA' THEN 'Massachusetts'
           WHEN state ='MI' THEN 'Michigan'
           WHEN state ='MN' THEN 'Minnesota'
           WHEN state ='MS' THEN 'Mississippi'
           WHEN state ='MO' THEN 'Missouri'
           WHEN state ='MT' THEN 'Montana'
           WHEN state ='NE' THEN 'Nebraska'
           WHEN state ='NV' THEN 'Nevada'
           WHEN state ='NH' THEN 'New Hampshire'
           WHEN state ='NJ' THEN 'New Jersey'
           WHEN state ='NM' THEN 'New Mexico'
           WHEN state ='NY' THEN 'New York'
           WHEN state ='NC' THEN 'North Carolina'
           WHEN state ='ND' THEN 'North Dakota'
           WHEN state ='OH' THEN 'Ohio'
           WHEN state ='OK' THEN 'Oklahoma'
           WHEN state ='OR' THEN 'Oregon'
           WHEN state ='PA' THEN 'Pennsylvania'
		       WHEN state ='PR' THEN 'Puerto Rico'
           WHEN state ='RI' THEN 'Rhode Island'
           WHEN state ='SC' THEN 'South Carolina'
           WHEN state ='SD' THEN 'South Dakota'
           WHEN state ='TN' THEN 'Tennessee'
           WHEN state ='TX' THEN 'Texas'
           WHEN state ='UT' THEN 'Utah'
           WHEN state ='VT' THEN 'Vermont'
           WHEN state ='VA' THEN 'Virginia'
           WHEN state ='WA' THEN 'Washington'
           WHEN state ='WV' THEN 'West Virginia'
           WHEN state ='WI' THEN 'Wisconsin'
           WHEN state ='WY' THEN 'Wyoming'
           ELSE 'Unknown'
          END AS state_name, weekly_resident_covid_cases, weekly_resident_covid_deaths	
FROM nhs_2021

UNION ALL
SELECT DISTINCT
       CASE 
           WHEN state ='AL' THEN 'Alabama'
           WHEN state ='AK' THEN 'Alaska'
           WHEN state ='AZ' THEN 'Arizona'
           WHEN state ='AR' THEN 'Arkansas'
           WHEN state ='CA' THEN 'California'
           WHEN state ='CO' THEN 'Colorado'
           WHEN state ='CT' THEN 'Connecticut'
		       WHEN state ='DC' THEN 'Federal District of Colombia'
           WHEN state ='DE' THEN 'Delaware'
           WHEN state ='FL' THEN 'Florida'
           WHEN state ='GA' THEN 'Georgia'
	    	   WHEN state ='GU' THEN 'Guam'
           WHEN state ='HI' THEN 'Hawaii'
           WHEN state ='ID' THEN 'Idaho'
           WHEN state ='IL' THEN 'Illinois'
           WHEN state ='IN' THEN 'Indiana'
           WHEN state ='IA' THEN 'Iowa'
           WHEN state ='KS' THEN 'Kansas'
           WHEN state ='KY' THEN 'Kentucky'
           WHEN state ='LA' THEN 'Louisiana'
           WHEN state ='ME' THEN 'Maine'
           WHEN state ='MD' THEN 'Maryland'
           WHEN state ='MA' THEN 'Massachusetts'
           WHEN state ='MI' THEN 'Michigan'
           WHEN state ='MN' THEN 'Minnesota'
           WHEN state ='MS' THEN 'Mississippi'
           WHEN state ='MO' THEN 'Missouri'
           WHEN state ='MT' THEN 'Montana'
           WHEN state ='NE' THEN 'Nebraska'
           WHEN state ='NV' THEN 'Nevada'
           WHEN state ='NH' THEN 'New Hampshire'
           WHEN state ='NJ' THEN 'New Jersey'
           WHEN state ='NM' THEN 'New Mexico'
           WHEN state ='NY' THEN 'New York'
           WHEN state ='NC' THEN 'North Carolina'
           WHEN state ='ND' THEN 'North Dakota'
           WHEN state ='OH' THEN 'Ohio'
           WHEN state ='OK' THEN 'Oklahoma'
           WHEN state ='OR' THEN 'Oregon'
           WHEN state ='PA' THEN 'Pennsylvania'
		       WHEN state ='PR' THEN 'Puerto Rico'
           WHEN state ='RI' THEN 'Rhode Island'
           WHEN state ='SC' THEN 'South Carolina'
           WHEN state ='SD' THEN 'South Dakota'
           WHEN state ='TN' THEN 'Tennessee'
           WHEN state ='TX' THEN 'Texas'
           WHEN state ='UT' THEN 'Utah'
           WHEN state ='VT' THEN 'Vermont'
           WHEN state ='VA' THEN 'Virginia'
           WHEN state ='WA' THEN 'Washington'
           WHEN state ='WV' THEN 'West Virginia'
           WHEN state ='WI' THEN 'Wisconsin'
           WHEN state ='WY' THEN 'Wyoming'
           ELSE 'Unknown'
       END AS state_name, weekly_resident_covid_cases, weekly_resident_covid_deaths	
FROM nhs_2022

UNION ALL
SELECT DISTINCT
       CASE 
           WHEN state ='AL' THEN 'Alabama'
           WHEN state ='AK' THEN 'Alaska'
           WHEN state ='AZ' THEN 'Arizona'
           WHEN state ='AR' THEN 'Arkansas'
           WHEN state ='CA' THEN 'California'
           WHEN state ='CO' THEN 'Colorado'
           WHEN state ='CT' THEN 'Connecticut'
		       WHEN state ='DC' THEN 'Federal District of Colombia'
           WHEN state ='DE' THEN 'Delaware'
           WHEN state ='FL' THEN 'Florida'
           WHEN state ='GA' THEN 'Georgia'
		       WHEN state ='GU' THEN 'Guam'
           WHEN state ='HI' THEN 'Hawaii'
           WHEN state ='ID' THEN 'Idaho'
           WHEN state ='IL' THEN 'Illinois'
           WHEN state ='IN' THEN 'Indiana'
           WHEN state ='IA' THEN 'Iowa'
           WHEN state ='KS' THEN 'Kansas'
           WHEN state ='KY' THEN 'Kentucky'
           WHEN state ='LA' THEN 'Louisiana'
           WHEN state ='ME' THEN 'Maine'
           WHEN state ='MD' THEN 'Maryland'
           WHEN state ='MA' THEN 'Massachusetts'
           WHEN state ='MI' THEN 'Michigan'
           WHEN state ='MN' THEN 'Minnesota'
           WHEN state ='MS' THEN 'Mississippi'
           WHEN state ='MO' THEN 'Missouri'
           WHEN state ='MT' THEN 'Montana'
           WHEN state ='NE' THEN 'Nebraska'
           WHEN state ='NV' THEN 'Nevada'
           WHEN state ='NH' THEN 'New Hampshire'
           WHEN state ='NJ' THEN 'New Jersey'
           WHEN state ='NM' THEN 'New Mexico'
           WHEN state ='NY' THEN 'New York'
           WHEN state ='NC' THEN 'North Carolina'
           WHEN state ='ND' THEN 'North Dakota'
           WHEN state ='OH' THEN 'Ohio'
           WHEN state ='OK' THEN 'Oklahoma'
           WHEN state ='OR' THEN 'Oregon'
           WHEN state ='PA' THEN 'Pennsylvania'
	   WHEN state ='PR' THEN 'Puerto Rico'
           WHEN state ='RI' THEN 'Rhode Island'
           WHEN state ='SC' THEN 'South Carolina'
           WHEN state ='SD' THEN 'South Dakota'
           WHEN state ='TN' THEN 'Tennessee'
           WHEN state ='TX' THEN 'Texas'
           WHEN state ='UT' THEN 'Utah'
           WHEN state ='VT' THEN 'Vermont'
           WHEN state ='VA' THEN 'Virginia'
           WHEN state ='WA' THEN 'Washington'
           WHEN state ='WV' THEN 'West Virginia'
           WHEN state ='WI' THEN 'Wisconsin'
           WHEN state ='WY' THEN 'Wyoming'
           ELSE 'Unknown'
       END AS state_name, weekly_resident_covid_cases, weekly_resident_covid_deaths	   
FROM nhs_2023
)
SELECT DISTINCT state_name, ROUND(SUM(weekly_resident_covid_deaths)::numeric/
								  SUM(weekly_resident_covid_cases)::numeric, 2) AS mortality_rate
FROM sub
WHERE EXTRACT(YEAR from week_ending) = '2022' 
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5;
```

5. 
```sql
WITH fac_cases_2020 AS
(
SELECT facility_name, SUM(weekly_resident_covid_cases) AS tot_cases
FROM nhs_2020
GROUP BY 1
),

     fac_cases_2021 AS
(
SELECT facility_name, SUM(weekly_resident_covid_cases) AS tot_cases
FROM nhs_2021
GROUP BY 1
)
	SELECT DISTINCT f_20.facility_name, ROUND((f_21.tot_cases - f_20.tot_cases)::numeric/
	f_20.tot_cases::numeric, 1) AS covid_perc_increase
	FROM fac_cases_2020 f_20
	JOIN fac_cases_2021 f_21 ON f_20.facility_name = f_21.facility_name
	WHERE f_20.tot_cases <> 0 AND f_21.tot_cases <>0 --showing only cases greater than or equals to 1
		  AND ROUND((f_21.tot_cases - f_20.tot_cases)::numeric/
		  f_20.tot_cases::numeric, 1) > 50.0 --showing only facilities with more than 50% case increase
	ORDER BY 2 DESC;
```
  
  6. Total No of cases, deaths, recoveries
  
 ```sql
  WITH sub AS 
(
SELECT weekly_resident_covid_cases, weekly_resident_covid_deaths 
FROM nhs_2020
UNION ALL
SELECT weekly_resident_covid_cases, weekly_resident_covid_deaths 
FROM nhs_2021
UNION ALL
SELECT weekly_resident_covid_cases, weekly_resident_covid_deaths 
FROM nhs_2022
UNION ALL
SELECT weekly_resident_covid_cases, weekly_resident_covid_deaths 
FROM nhs_2023
)
SELECT  SUM(weekly_resident_covid_cases) AS tot_cases,  
 		SUM(weekly_resident_covid_deaths) AS tot_deaths, 
		SUM(weekly_resident_covid_cases)-SUM(weekly_resident_covid_deaths)AS tot_recoveries
FROM sub;
```

7. 
```sql
WITH monthly_cases AS (
  SELECT
    facility_name,
    EXTRACT(MONTH FROM week_ending) AS month,
    SUM(weekly_resident_covid_cases) AS tot_cases,
    ROW_NUMBER() OVER (PARTITION BY facility_name ORDER BY EXTRACT(MONTH FROM week_ending)) AS month_rank
  FROM nhs_2020
  GROUP BY 1, 2
  UNION ALL
  SELECT
    facility_name,
    EXTRACT(MONTH FROM week_ending) AS month,
    SUM(weekly_resident_covid_cases) AS tot_cases,
    ROW_NUMBER() OVER (PARTITION BY facility_name ORDER BY EXTRACT(MONTH FROM week_ending)) AS month_rank
  FROM nhs_2021
  GROUP BY 1, 2
  UNION ALL
  SELECT
    facility_name,
    EXTRACT(MONTH FROM week_ending) AS month,
    SUM(weekly_resident_covid_cases) AS tot_cases,
    ROW_NUMBER() OVER (PARTITION BY facility_name ORDER BY EXTRACT(MONTH FROM week_ending)) AS month_rank
  FROM nhs_2022
  GROUP BY 1, 2
  UNION ALL
  SELECT
    facility_name,
    EXTRACT(MONTH FROM week_ending) AS month,
    SUM(weekly_resident_covid_cases) AS tot_cases,
    ROW_NUMBER() OVER (PARTITION BY facility_name ORDER BY EXTRACT(MONTH FROM week_ending)) AS month_rank
  FROM nhs_2023
  GROUP BY 1, 2
),
sub_2 AS
(SELECT DISTINCT
  current_month.facility_name
FROM
  monthly_cases current_month
JOIN
  monthly_cases prev_month ON prev_month.facility_name = current_month.facility_name
    AND prev_month.month_rank = current_month.month_rank - 1
JOIN
  monthly_cases consecutive_months ON consecutive_months.facility_name = current_month.facility_name
    AND consecutive_months.month_rank BETWEEN current_month.month_rank + 1 AND current_month.month_rank + 4
    AND consecutive_months.tot_cases > prev_month.tot_cases
WHERE consecutive_months.tot_cases > prev_month.tot_cases
)
SELECT 
	DISTINCT sub_2.facility_name
FROM sub_2;
```


8. 
```sql
WITH monthly_cases AS (
  SELECT
    facility_name,
    EXTRACT(MONTH FROM week_ending) AS month,
    SUM(weekly_resident_covid_cases) AS tot_cases,
    ROW_NUMBER() OVER (PARTITION BY facility_name ORDER BY EXTRACT(MONTH FROM week_ending)) AS month_rank
  FROM nhs_2020
  GROUP BY 1, 2
  UNION ALL
  SELECT
    facility_name,
    EXTRACT(MONTH FROM week_ending) AS month,
    SUM(weekly_resident_covid_cases) AS tot_cases,
    ROW_NUMBER() OVER (PARTITION BY facility_name ORDER BY EXTRACT(MONTH FROM week_ending)) AS month_rank
  FROM nhs_2021
  GROUP BY 1, 2
  UNION ALL
  SELECT
    facility_name,
    EXTRACT(MONTH FROM week_ending) AS month,
    SUM(weekly_resident_covid_cases) AS tot_cases,
    ROW_NUMBER() OVER (PARTITION BY facility_name ORDER BY EXTRACT(MONTH FROM week_ending)) AS month_rank
  FROM nhs_2022
  GROUP BY 1, 2
  UNION ALL
  SELECT
    facility_name,
    EXTRACT(MONTH FROM week_ending) AS month,
    SUM(weekly_resident_covid_cases) AS tot_cases,
    ROW_NUMBER() OVER (PARTITION BY facility_name ORDER BY EXTRACT(MONTH FROM week_ending)) AS month_rank
  FROM nhs_2023
  GROUP BY 1, 2
),
sub_2 AS
(SELECT DISTINCT
  current_month.facility_name
FROM
  monthly_cases current_month
JOIN
  monthly_cases prev_month ON prev_month.facility_name = current_month.facility_name
    AND prev_month.month_rank = current_month.month_rank - 1
JOIN
  monthly_cases consecutive_months ON consecutive_months.facility_name = current_month.facility_name
    AND consecutive_months.month_rank BETWEEN current_month.month_rank + 1 AND current_month.month_rank + 4
    AND consecutive_months.tot_cases > prev_month.tot_cases
WHERE consecutive_months.tot_cases > prev_month.tot_cases
)
      SELECT 
          	DISTINCT sub_2.facility_name
      FROM sub_2;
```

9. 
```sql
WITH sub AS
(
SELECT facility_name, weekly_resident_covid_deaths, weekly_resident_covid_cases, 
	   weekly_resident_covid_admission
FROM nhs_2020 
UNION ALL
SELECT facility_name, weekly_resident_covid_deaths, weekly_resident_covid_cases, 
	   weekly_resident_covid_admission
FROM nhs_2021
UNION ALL
SELECT facility_name, weekly_resident_covid_deaths, weekly_resident_covid_cases, 
	   weekly_resident_covid_admission
FROM nhs_2022
UNION ALL
SELECT facility_name, weekly_resident_covid_deaths, weekly_resident_covid_cases, 
	   weekly_resident_covid_admission
FROM nhs_2023
)
SELECT 
		CASE WHEN facility_name LIKE '%HOSPITAL%' THEN 'hospital'
			 WHEN facility_name LIKE '%NURSING HOME%' THEN 'nursing_homes'
			 WHEN (facility_name LIKE '%HEALTHCARE CENTER%' 
			 		OR facility_name LIKE '%HEALTH CARE CENTER%'
			 		OR facility_name LIKE '%HEALTHCARE%' 
					OR facility_name LIKE '%HEALTH%') THEN 'health_centre'
			 WHEN facility_name LIKE '%REHAB%' THEN 'rehab_facility'
			 WHEN facility_name LIKE '%HOME%' THEN 'home_health_agencies'
			 WHEN facility_name LIKE '%ADULT%' THEN 'adult_day_care'
			 WHEN facility_name LIKE '%RETIREMENT%' THEN 'continuing_care_retirement_communities'
			 ELSE 'others' END AS facility_type,
			 SUM(weekly_resident_covid_cases) total_cases,
			 SUM(weekly_resident_covid_deaths) total_deaths,
			 SUM(weekly_resident_covid_cases)-SUM(weekly_resident_covid_deaths) total_recoveries
FROM sub
GROUP BY 1
ORDER BY 2 DESC.
```

10.   
```sql
WITH comb_cases AS
(
SELECT week_ending, weekly_resident_covid_cases
FROM nhs_2020
UNION ALL
SELECT week_ending, weekly_resident_covid_cases
FROM nhs_2021
UNION ALL
SELECT week_ending, weekly_resident_covid_cases
FROM nhs_2022
UNION ALL
SELECT week_ending, weekly_resident_covid_cases
FROM nhs_2023
)
SELECT week_ending, 
	   to_char(week_ending, 'month'),
       DATE_PART('quarter', week_ending::date) quarter,
       DATE_PART('year', week_ending::date) AS year,
	   SUM(weekly_resident_covid_cases)tot_cases
FROM comb_cases
GROUP BY 1, 2, 3, 4
ORDER BY 4;
```

11.
```
sql
WITH comb_data AS
(
SELECT state, facility_name
FROM nhs_2020
UNION ALL
SELECT state, facility_name
FROM nhs_2021
UNION ALL
SELECT state, facility_name
FROM nhs_2022
UNION ALL
SELECT state, facility_name
FROM nhs_2023
)
SELECT CASE 
           WHEN state ='AL' THEN 'Alabama'
           WHEN state ='AK' THEN 'Alaska'
           WHEN state ='AZ' THEN 'Arizona'
           WHEN state ='AR' THEN 'Arkansas'
           WHEN state ='CA' THEN 'California'
           WHEN state ='CO' THEN 'Colorado'
           WHEN state ='CT' THEN 'Connecticut'
		       WHEN state ='DC' THEN 'Federal District of Colombia'
           WHEN state ='DE' THEN 'Delaware'
           WHEN state ='FL' THEN 'Florida'
           WHEN state ='GA' THEN 'Georgia'
		       WHEN state ='GU' THEN 'Guam'
           WHEN state ='HI' THEN 'Hawaii'
           WHEN state ='ID' THEN 'Idaho'
           WHEN state ='IL' THEN 'Illinois'
           WHEN state ='IN' THEN 'Indiana'
           WHEN state ='IA' THEN 'Iowa'
           WHEN state ='KS' THEN 'Kansas'
           WHEN state ='KY' THEN 'Kentucky'
           WHEN state ='LA' THEN 'Louisiana'
           WHEN state ='ME' THEN 'Maine'
           WHEN state ='MD' THEN 'Maryland'
           WHEN state ='MA' THEN 'Massachusetts'
           WHEN state ='MI' THEN 'Michigan'
           WHEN state ='MN' THEN 'Minnesota'
           WHEN state ='MS' THEN 'Mississippi'
           WHEN state ='MO' THEN 'Missouri'
           WHEN state ='MT' THEN 'Montana'
           WHEN state ='NE' THEN 'Nebraska'
           WHEN state ='NV' THEN 'Nevada'
           WHEN state ='NH' THEN 'New Hampshire'
           WHEN state ='NJ' THEN 'New Jersey'
           WHEN state ='NM' THEN 'New Mexico'
           WHEN state ='NY' THEN 'New York'
           WHEN state ='NC' THEN 'North Carolina'
           WHEN state ='ND' THEN 'North Dakota'
           WHEN state ='OH' THEN 'Ohio'
           WHEN state ='OK' THEN 'Oklahoma'
           WHEN state ='OR' THEN 'Oregon'
           WHEN state ='PA' THEN 'Pennsylvania'
	    	   WHEN state ='PR' THEN 'Puerto Rico'
           WHEN state ='RI' THEN 'Rhode Island'
           WHEN state ='SC' THEN 'South Carolina'
           WHEN state ='SD' THEN 'South Dakota'
           WHEN state ='TN' THEN 'Tennessee'
           WHEN state ='TX' THEN 'Texas'
           WHEN state ='UT' THEN 'Utah'
           WHEN state ='VT' THEN 'Vermont'
           WHEN state ='VA' THEN 'Virginia'
           WHEN state ='WA' THEN 'Washington'
           WHEN state ='WV' THEN 'West Virginia'
           WHEN state ='WI' THEN 'Wisconsin'
           WHEN state ='WY' THEN 'Wyoming'
           ELSE 'Unknown'
       END AS state_name, COUNT(DISTINCT facility_name) AS facility_dist
FROM comb_data
GROUP BY 1
ORDER BY 2 DESC; 
```

12.
```sql
WITH comb_data AS
(
SELECT facility_name, no_all_beds, tot_no_occup_beds
FROM nhs_2020
UNION ALL
SELECT facility_name, no_all_beds, tot_no_occup_beds
FROM nhs_2021
UNION ALL
SELECT facility_name, no_all_beds, tot_no_occup_beds
FROM nhs_2022
UNION ALL
SELECT facility_name, no_all_beds, tot_no_occup_beds
FROM nhs_2023
)
SELECT facility_name, ROUND(SUM(tot_no_occup_beds)::numeric/SUM(no_all_beds)::numeric,2) AS occupancy_rate
FROM comb_data
WHERE tot_no_occup_beds <> 0 AND no_all_beds <>0 AND tot_no_occup_beds IS NOT NULL
GROUP BY 1
ORDER BY 2 DESC
LIMIT 20; 
```


