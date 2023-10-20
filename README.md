# Google Data Analytics Capstone Project

## Cyclistic Bike-Share

## Table of Contents
- [Introduction](#introduction)
- [Business Task](#business-task)
- [Tools](#tools)
- [Data Cleaning and Processing](#data-processing-and-cleaning)
- [Analysis and Visualizations](#analysis-and-visualizations)
- [Recommendations](#recommendations)
- [Limitiations](#limitations)

  
### Introduction

This is the Capstone project for the Google Data Analytics Certifcate through Coursera. I am analyzing trip data from a fictional bike-share company in Chicago called Cyclistic.

I am a junior data analyst on the marketing analyst team. My manager, Lily Moreno, believes the company’s future success depends on maximizing the number of annual memberships. Until now, Cyclistic’s marketing strategy relied on building general awareness and appealing to broad consumer segments. Cyclistic's finance analysts have concluded that annual members are much more profitable than casual riders. Although the pricing flexibility helps Cyclistic attract more customers, Moreno believes that maximizing the number of annual members will be key to future growth. Rather than creating a marketing campaign that targets all-new customers, Moreno believes there is a very good chance to convert casual riders into members. She notes that casual riders are already aware of the Cyclistic program and have chosen us for their mobility needs.

Cyclistic offers 3 different passes - single-ride passes, full-day passes, and annual memberships. There are two types of users. Users who purchase single-ride and full-day passes are **Customers**. Users who have annual memberships are **Subscribers**. The bikes can be unlocked from one station and returned to any other station in the system at anytime.

### Business Task
How do Customers and Subscribers use Cyclistic bikes differently?

### Data
This data has been made available by Motivate International Inc. under this [license](https://divvybikes.com/data-license-agreement).

[Cyclistic's trip data](https://divvy-tripdata.s3.amazonaws.com/index.html) from 2013 to present is publically available in .csv and .xlsx formats.

I am using Cyclistic's 2014 datasets for this project.

### Tools
- Excel
- SQL - BigQuery
- [Tableau](https://public.tableau.com/views/2014CyclisticCapstoneProject/UsertypeCountandAvgRideLength?:language=en-US&:display_count=n&:origin=viz_share_link)

### Data Processing and Cleaning

#### Excel
I downloaded the original quarter 1 through 4 .xlsx files and turned them into monthly .csv files. This makes uploading into BigQuery simplier and also makes it easier to work with in Excel and in SQL.


#### SQL

I took the 12 Excel files that I converted from quarterly reports in .csv files and uploaded all 12 into BigQuery. I combined all 12 into one table as 'all_data_2014' with the following query.
```
CREATE TABLE IF NOT EXISTS `engaged-precept-376000.cyclistic_bike_data.all_data_2014` AS
(
  SELECT *
FROM `engaged-precept-376000.cyclistic_bike_data.divvy_trips_2014_01`
UNION ALL
SELECT *
FROM `engaged-precept-376000.cyclistic_bike_data.divvy_trips_2014_02`
UNION ALL
SELECT *
FROM `engaged-precept-376000.cyclistic_bike_data.divvy_trips_2014_03`
UNION ALL
SELECT *
FROM `engaged-precept-376000.cyclistic_bike_data.divvy_trips_2014_04`
UNION ALL
SELECT *
FROM `engaged-precept-376000.cyclistic_bike_data.divvy_trips_2014_05`
UNION ALL
SELECT *
FROM `engaged-precept-376000.cyclistic_bike_data.divvy_trips_2014_06`
UNION ALL
SELECT *
FROM `engaged-precept-376000.cyclistic_bike_data.divvy_trips_2014_07`
UNION ALL
SELECT *
FROM `engaged-precept-376000.cyclistic_bike_data.divvy_trips_2014_08`
UNION ALL
SELECT *
FROM `engaged-precept-376000.cyclistic_bike_data.divvy_trips_2014_09`
UNION ALL
SELECT *
FROM `engaged-precept-376000.cyclistic_bike_data.divvy_trips_2014_10`
UNION ALL
SELECT *
FROM `engaged-precept-376000.cyclistic_bike_data.divvy_trips_2014_11`
UNION ALL
SELECT *
FROM `engaged-precept-376000.cyclistic_bike_data.divvy_trips_2014_12`
);
```

The new table has the following column names and datatypes:
- trip_id - integer
- starttime - timestamp
- stoptime - timestamp
- bikeid - integer
- tripduration - integer
- from_station_id - integer
- from_station_name - string
- to_station_id - integer
- to_station_name - string
- usertype - string
- gender - string
- birthyear - integer

I ran the following query to see how many rows are in this dataset
```
SELECT COUNT(*)
FROM `engaged-precept-376000.cyclistic_bike_data.all_data_2014`;
```
There are 2,454,634 total rows.

---

To explore this data table more, I ran the following query to find the average ride length
```
SELECT AVG(stoptime - starttime) AS avg_ride_length
FROM `engaged-precept-376000.cyclistic_bike_data.all_data_2014`;
```
I got the result 0-0 0 0:17:4.941828394, meaning the average ride length for all of 2014 is 17 minutes and 5 secs. I don't like this format because it's too messy to work with since it shows month, day, year and the seconds with 9 decimals. I will format this later to show times in hh:mm:ss format.

---

I wanted to check for nulls in this dataset and used the following query:
```
SELECT COUNT(*) - COUNT(trip_id) AS trip_id,
  COUNT(*) - COUNT(starttime) AS starttime,
  COUNT(*) - COUNT(stoptime) AS starttime,
  COUNT(*) - COUNT(bikeid) AS bikeid,
  COUNT(*) - COUNT(tripduration) AS tripduration,
  COUNT(*) - COUNT(from_station_id) AS from_station_id,
  COUNT(*) - COUNT(from_station_name) AS from_station_name,
  COUNT(*) - COUNT(to_station_id) AS to_station_id,
  COUNT(*) - COUNT(to_station_name) AS to_station_name,
  COUNT(*) - COUNT(usertype) AS usertype,
  COUNT(*) - COUNT(gender) AS gender,
  COUNT(*) - COUNT(birthyear) AS birthyear,
FROM `engaged-precept-376000.cyclistic_bike_data.all_data_2014`
```
Only 2 columns had nulls:
- gender had 791,280 null values.
- birthyear had 791,216 null values.

These will be removed later to not skew the data.

---

I wanted to create 4 new columns showing when each ride happened for the day of the week (1 is Sunday, 7 is Saturday), week of the year (between 1 and 52), month, and quarter. I ran the following query:
```
SELECT starttime,
EXTRACT(DAYOFWEEK FROM starttime) AS day_of_week,
EXTRACT(WEEK FROM starttime) AS week,
EXTRACT(MONTH FROM starttime) AS month,
EXTRACT(QUARTER FROM starttime) AS quarter,
usertype
FROM `engaged-precept-376000.cyclistic_bike_data.all_data_2014`
```

---

I wanted to create a better column that showed ride_length because the tripduration column included in the original data showed it in seconds. I ran the following query to show ride_length in the hh:mm:ss format. I also removed any rides that are less than 1 minute and longer than 1,440 minutes/24 hours as they could also skew the data.
```
SELECT EXTRACT(TIME FROM starttime) AS starttime1,
       EXTRACT(TIME FROM stoptime) AS stoptime1,
       TIME_ADD(
         TIME '0:0:0',
         INTERVAL TIMESTAMP_DIFF(stoptime, starttime, SECOND) SECOND
       ) AS ride_length
   FROM `engaged-precept-376000.cyclistic_bike_data.all_data_2014`
WHERE TIMESTAMP_DIFF(stoptime, starttime, SECOND) > 1
AND TIMESTAMP_DIFF(stoptime, starttime, SECOND) < 1440;
```

---

Lastly, I created a new table 'cleaned_all_data_2014' to include:
- ride_length column in hh:mm:ss format
- day_of_week column
- week column
- month column
- quarter column

This table also removes:
- Any rides less than 1 minute and longer than 24 hours

Originally, I wanted to remove all rows that had nulls in the gender and birthyear columns. After further analysis, I found that only 49 Customers had values for both gender and birthyear skewing the data to show only data for Subscribers. I added birthyear and gender back to this table and will discuss this more later.

```
CREATE TABLE IF NOT EXISTS `engaged-precept-376000.cyclistic_bike_data.cleaned_all_data_2014` AS
(
  SELECT trip_id
  , starttime
  , stoptime
  , tripduration
  , TIME_ADD(
         TIME '0:0:0',
         INTERVAL TIMESTAMP_DIFF(stoptime, starttime, SECOND) SECOND
       ) AS ride_length
  , from_station_id
  , from_station_name
  , to_station_id
  , to_station_name
  , usertype
  , gender
  , birthyear
  , EXTRACT(DAYOFWEEK FROM starttime) AS day_of_week
  , EXTRACT(WEEK FROM starttime) AS week
  , EXTRACT(MONTH FROM starttime) AS month
  , EXTRACT(QUARTER FROM starttime) AS quarter
FROM `engaged-precept-376000.cyclistic_bike_data.all_data_2014`
WHERE TIMESTAMP_DIFF(stoptime, starttime, SECOND) > 1
  AND TIMESTAMP_DIFF(stoptime, starttime, SECOND) < 1440
);
```
Total number of rows in original data = 2,454,634.

Total number of rows in cleaned data = 2,010,065.


### Analysis and Visualizations

The original Tableau dashboard is interactive where we can filter each chart between all users, Customers, or Subscribers. My points for each include analysis using these filters.

#### User Count, User Percentage, and Average Ride Length by User

<img width="744" alt="Usertype Count, %, and Avg Ride Length" src="https://github.com/PaxtonTaylor/Google-Data-Analytics-Capstone---Cyclistic-Bike-Share/assets/147224800/2bf89973-68eb-451a-bca2-36dcdffcd336">

- We had 2,010,065 total rides in 2014.
- A majority of our users are Subscribers at 77%.
- Customers are almost 23% of our annual users.
- Customers have a longer average ride length by 4 and a half minutes.


#### Day of the Week

<img width="739" alt="Day of Week" src="https://github.com/PaxtonTaylor/Google-Data-Analytics-Capstone---Cyclistic-Bike-Share/assets/147224800/1b906f85-fc27-4b4c-a2e2-4be3e7809f37">

Total Rides
- Subscribers ride more frequently during the week peaking on Wednesdays, likely using our service to commute to/from work and run errands.
- Customers ride more on weekends peaking on Saturdays.
- Friday is the busiest day for all users, which is likely due to Friday being the end of the work week for Subscribers and beginning of the weekend for Customers.

Average Ride Length

- Customers have a longer average ride length of 14 minutes and their longest average rides on Sundays with 14 minutes, 45 seconds. This is likely due to them exploring the city more leisurely.
- Subscribers have an average ride length of 10 minutes and their longest average rides on Sundays with 10 minutes, 28 seconds. They are likely using our bikes for intentional tasks like commuting to/from work, running errands, dining, etc.

#### Total Rides by Hour

<img width="743" alt="Total Rides by Hour" src="https://github.com/PaxtonTaylor/Google-Data-Analytics-Capstone---Cyclistic-Bike-Share/assets/147224800/bcefad8c-6ce1-44d8-a16b-c9c39b071546">

- Late afternoon is the busiest time to begin rides with 5pm being the most popular start time.
- Subscribers most popular start times are 5pm followed by 8am, which lines up with work commute hours.
- Customers most popular start times are 4pm followed by 5pm, but there is a consistently heavy use all afternoon between noon and 5pm.

#### Total Rides by Month

<img width="745" alt="Total Rides by Month" src="https://github.com/PaxtonTaylor/Google-Data-Analytics-Capstone---Cyclistic-Bike-Share/assets/147224800/c156a9ad-eeb7-4674-8ac0-f96a7f2e87db">

- Summer is the busiest season with July being our busiest month.
- Subscribers have a longer high volume usage between May and October, with July being their busiest month.
- Customers are most active during the Summer between June and August, with Augest being their busiest month.
- Weather plays an important role in both memberships. Since Summer is sunnier, warmer, and people tend to be more active compared to Winter when it's colder and the days are shorter.


### Recommendations

Cyclistic Highlights

<img width="722" alt="Cyclistic Highlights" src="https://github.com/PaxtonTaylor/Google-Data-Analytics-Capstone---Cyclistic-Bike-Share/assets/147224800/f9e1a377-6e53-405d-a2d1-5f21680710af">

- Since our busiest season is summer, we could begin aggressively marketing to our casual customers in either April or May by offering a slightly discounted subscriber plan so they can experience the benefit during those peak months.

- Since most Customers ride more on weekends, we can market that if they switch to Subscribers they can still ride during the weekends for leasure, but can also use our bikes for more daily routines such as work commute and other daily tasks.

- To help incentivize Customers converting to Subscribers, we could offer more memberships options. If we added a monthly subscription option, a quarterly subscription, and kept the annual subscription, we could likely convince more Customers to become Subscribers. Pricing could push for cheaper prices for the longer subscriptions in the long run making our annual subscription the most expensive up front but cheapest long term.

- We could place ads throughout the city in newspapers, TV ads, billboards, magazines, and even throughout the airport as tourists enter the city or residents return home to spread more awareness of our service, especially before our peak Summer season.

### Limitations

To better analyze this data, the following points would make an impact:
- Gender and birth year information for Customers. We only have 49 Customers that filled out that information initially. We could have more marketing options to convert Subscribers to Customers with this information.
- If we had data on which bikes users chose to ride, we could work to improve the less used bikes and promote our popular bikes more with marketing.
