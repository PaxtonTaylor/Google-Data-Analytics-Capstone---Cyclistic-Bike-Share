# Exploratory Analysis in Excel

I downloaded the original quarter 1 through 4 .xlsx files and turned them into monthly .csv files. This makes uploading into BigQuery simpler and also makes it easier to work with in Excel and in SQL.

Added ride_length column with =C2-B2, where =stoptime - starttime to calculate the ride length of each ride.

<img width="200" alt="ride_length" src="https://github.com/PaxtonTaylor/Google-Data-Analytics-Capstone---Cyclistic-Bike-Share/assets/147224800/67afe1b0-f916-4782-bb0d-da4bfb238a13">

Added day_of_week column with function =WEEKDAY(B2, 1), where B2 is starttime and 1 tells it to label 1 as Sunday, 2 as Monday,... 7 is Saturday.



Added average_ride_length with function =AVERAGE(M:M), where it finds the average of column M(ride_length).



Added mean_ride_length with function =MEDIAN(M:M) to find the most middle point of the column M(ride_length) in the dataset.



Added max_ride_length with function =MAX(M:M) to find the longest ride length of column M(ride_length).



Added min_ride_length with function =MIN(M:M) to find the shortest ride length of column M(ride_length).



Added mode_ride_length with function =MODE.SNGL(M:M) to find the most frequent ride length in column M(ride_length). This was the same in each quarter/month = 00:06:00.



#### Created 4 Pivot Tables in Excel
Calculated Average of ride_length for Customers, Subscribers, and all riders to compare the different ride length averages.



Calculated the Count of ride_length to compare how many Customers, Subscribers, and total riders we had for each quarter/month.


Calculated the Average of ride_length for Customers and Subscribers by the day of the week for each quarter/month to show which days each user rode our bikes and for how long.


Calculated the Count of ride_length to for Customers and Subscribers by the day of the week for each quarter/month to show how many of each membership rode and on which day of the week.
