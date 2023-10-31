# Google Data Analytics Capstone Project

## Cyclistic Bike-Share

## Table of Contents
- [Introduction](#introduction)
- [Business Task](#business-task)
- [Data](#data)
- [Tools](#tools)
- [Data Cleaning and Processing](#data-processing-and-cleaning)
- [Analysis and Visualizations](#analysis-and-visualizations)
- [Limitiations](#limitations)

  
### Introduction

This is the Capstone project for the Google Data Analytics Certifcate through Coursera. I am analyzing trip data from a fictional bike-share company in Chicago called Cyclistic.

I am a junior data analyst on the marketing analyst team. My manager, Lily Moreno, believes the company’s future success depends on maximizing the number of annual memberships. Until now, Cyclistic’s marketing strategy relied on building general awareness and appealing to broad consumer segments. Cyclistic's finance analysts have concluded that annual members are much more profitable than casual riders. Although the pricing flexibility helps Cyclistic attract more customers, Moreno believes that maximizing the number of annual members will be key to future growth. Rather than creating a marketing campaign that targets all-new customers, Moreno believes there is a very good chance to convert casual riders into members. She notes that casual riders are already aware of the Cyclistic program and have chosen us for their mobility needs.

Cyclistic offers 3 different passes - single-ride passes, full-day passes, and annual memberships. There are two types of users - Customers and Subscribers. Users who purchase single-ride and full-day passes are **Customers**. Users who have annual memberships are **Subscribers**. The bikes can be unlocked from one station and returned to any other station in the system at anytime.

### Business Task
How do Customers and Subscribers use Cyclistic bikes differently?

### Data
This data has been made available by Motivate International Inc. under this [license](https://divvybikes.com/data-license-agreement).

[Cyclistic's trip data](https://divvy-tripdata.s3.amazonaws.com/index.html) from 2013 to present is publically available in .csv and .xlsx formats.

I am using Cyclistic's 2014 datasets for this project.

### Tools
- Excel
- SQL - BigQuery
- Data Visualization - [Tableau](https://public.tableau.com/views/2014CyclisticCapstoneProject/UsertypeCountandAvgRideLength?:language=en-US&:display_count=n&:origin=viz_share_link)

### Exploratory Analysis

You can view my initial exploratory analysis in Excel using this [link](https://github.com/PaxtonTaylor/Google-Data-Analytics-Capstone---Cyclistic-Bike-Share/blob/main/Analysis%20and%20Visualizations/Exploratory%20Analysis%20in%20Excel.md).

I ran different functions for each month and quarter to gain a better understanding of all the data available for this project. I also created pivot tables to better view and filter specific aspects of this dataset.

### Data Processing and Cleaning

You can view my full process for processing and cleaning this data using SQL (BigQuery) using this [link](https://github.com/PaxtonTaylor/Google-Data-Analytics-Capstone---Cyclistic-Bike-Share/blob/main/Analysis%20and%20Visualizations/Data%20Processing%20and%20Cleaning%20with%20SQL%20BigQuery.md).

I combined all 12 .csv files I converted from the original quarterly .xlsx files available to me into one table. I performed different SQL queries to explore, clean, and add necessary columns to this dataset. Lastly,I created a clean version of the dataset using many of the queries I performed during the cleaning process. This was beneficial to both understand the data better and prepare it for my data visualizations.

### Analysis and Visualizations

[My original Tableau dashboard](https://public.tableau.com/views/2014CyclisticCapstoneProject/UsertypeCountandAvgRideLength?:language=en-US&:display_count=n&:origin=viz_share_link) is interactive where anyone can filter each chart between all users, Customers, or Subscribers. I also included text for each worksheet that includes analysis using these filters.

You can view screenshots from my Tableau charts, details about each chart, and my final recommendations for stakeholders using this [link](https://github.com/PaxtonTaylor/Google-Data-Analytics-Capstone---Cyclistic-Bike-Share/blob/main/Analysis%20and%20Visualizations/Final%20Analysis%20and%20Tableau%20Visualizations.md).

### Limitations

To better analyze this data, the following points would make an impact:
- Gender and birth year information for Customers. We only have 49 Customers that filled out both of these initially, where essentially all of the Subscribers provided gender and birthyear information. With this information, we could better target and convert our Customers to Subscribers.
- If we had data on which bikes each user chose, we could work to improve the less used bikes and promote our popular bikes more.
