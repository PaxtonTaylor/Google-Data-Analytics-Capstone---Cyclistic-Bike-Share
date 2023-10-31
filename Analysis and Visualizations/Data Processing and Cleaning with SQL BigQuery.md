# Data Processing and Cleaning with SQL BigQuery


I downloaded the original quarter 1 through 4 .xlsx files and turned them into monthly .csv files.

I took the 12 Excel files that I converted from quarterly reports in .csv files and uploaded all 12 into BigQuery.
I combined all 12 into one table as 'all_data_2014' with the following query.

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
