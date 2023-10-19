# Google Data Analytics Capstone Project

## Cyclistic Bike-Share

## Table of Contents
- [Introduction](#introduction)
- [Business Task](#business-task)
- [Data](#data)
- [Data Preparation](#data-preparation)
- [Data Cleaning and Processing](#data-cleaning-and-processing)
- [Analysis and Visualizations](#analysis-and-visualizations)
- [Recommendations](#recommendations)

  
### Introduction

This is the Capstone project for the Google Data Analytics Certifcate through Coursera. I am analyzing trip data from a fictional bike-share company in Chicago called Cyclistic.

I am a junior data analyst on the marketing analyst team. My manager, Lily Moreno, believes the company’s future success depends on maximizing the number of annual memberships. Until now, Cyclistic’s marketing strategy relied on building general awareness and appealing to broad consumer segments. Cyclistic's finance analysts have concluded that annual members are much more profitable than casual riders. Although the pricing flexibility helps Cyclistic attract more customers, Moreno believes that maximizing the number of annual members will be key to future growth. Rather than creating a marketing campaign that targets all-new customers, Moreno believes there is a very good chance to convert casual riders into members. She notes that casual riders are already aware of the Cyclistic program and have chosen us for their mobility needs.

Cyclistic offers 3 different passes - single-ride passes, full-day passes, and annual memberships. There are two types of users. Users who purchase single-ride and full-day passes are **Customers**. Users who have annual memberships are **Subscribers**. The bikes can be unlocked from one station and returned to any other station in the system anytime.

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

### Data Preparation

#### Excel Preparation
EXPLAIN MY EXCEL PREP HERE...

#### SQL Preparation

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



### Data Cleaning and Processing


### Analysis and Visualizations

User Count, User Percentage, and Average Ride Length by User

<img width="744" alt="Usertype Count, %, and Avg Ride Length" src="https://github.com/PaxtonTaylor/Google-Data-Analytics-Capstone---Cyclistic-Bike-Share/assets/147224800/2bf89973-68eb-451a-bca2-36dcdffcd336">

Day of the Week

<img width="739" alt="Day of Week" src="https://github.com/PaxtonTaylor/Google-Data-Analytics-Capstone---Cyclistic-Bike-Share/assets/147224800/1b906f85-fc27-4b4c-a2e2-4be3e7809f37">

Total Rides by Hour

<img width="743" alt="Total Rides by Hour" src="https://github.com/PaxtonTaylor/Google-Data-Analytics-Capstone---Cyclistic-Bike-Share/assets/147224800/bcefad8c-6ce1-44d8-a16b-c9c39b071546">

Total Rides by Month

<img width="745" alt="Total Rides by Month" src="https://github.com/PaxtonTaylor/Google-Data-Analytics-Capstone---Cyclistic-Bike-Share/assets/147224800/c156a9ad-eeb7-4674-8ac0-f96a7f2e87db">


### Recommendations

Cyclistic Highlights

<img width="722" alt="Cyclistic Highlights" src="https://github.com/PaxtonTaylor/Google-Data-Analytics-Capstone---Cyclistic-Bike-Share/assets/147224800/f9e1a377-6e53-405d-a2d1-5f21680710af">

