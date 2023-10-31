# Exploratory Analysis in Excel

I downloaded the original quarter 1 through 4 .xlsx files and turned them into monthly .csv files. This makes uploading into BigQuery simpler and also makes it easier to work with in Excel and in SQL.

Below are samples of just one months of exploratory analysis in Excel. I performed these for each month/quarter to better understand the data and practice in Excel.

---

Added ride_length column with =C2-B2, where =stoptime - starttime to calculate the ride length of each ride.

<img width="200" alt="ride_length" src="https://github.com/PaxtonTaylor/Google-Data-Analytics-Capstone---Cyclistic-Bike-Share/assets/147224800/67afe1b0-f916-4782-bb0d-da4bfb238a13">


Added day_of_week column with function =WEEKDAY(B2, 1), where B2 is starttime and 1 tells it to label 1 as Sunday, 2 as Monday,... 7 is Saturday.

<img width="166" alt="day_of_week" src="https://github.com/PaxtonTaylor/Google-Data-Analytics-Capstone---Cyclistic-Bike-Share/assets/147224800/44de3ed5-3484-4031-8538-a2826ad2662b">


Added average_ride_length with function =AVERAGE(M:M), where it finds the average of column M(ride_length).

<img width="164" alt="average_ride_length" src="https://github.com/PaxtonTaylor/Google-Data-Analytics-Capstone---Cyclistic-Bike-Share/assets/147224800/f831c610-b317-4d83-92bf-970acf609b84">


Added mean_ride_length with function =MEDIAN(M:M) to find the most middle point of the column M(ride_length) in the dataset.

<img width="269" alt="mean_ride_length" src="https://github.com/PaxtonTaylor/Google-Data-Analytics-Capstone---Cyclistic-Bike-Share/assets/147224800/59dac169-e156-483b-97e6-133b2c9956e4">


Added max_ride_length with function =MAX(M:M) to find the longest ride length of column M(ride_length).

<img width="458" alt="max_ride_length" src="https://github.com/PaxtonTaylor/Google-Data-Analytics-Capstone---Cyclistic-Bike-Share/assets/147224800/96cab542-9005-4aa0-bec9-4b8348e7fb11">


Added min_ride_length with function =MIN(M:M) to find the shortest ride length of column M(ride_length).

<img width="455" alt="min_ride_length" src="https://github.com/PaxtonTaylor/Google-Data-Analytics-Capstone---Cyclistic-Bike-Share/assets/147224800/5b2dbabb-2114-4a6b-8e78-ad544f110484">


Added mode_ride_length with function =MODE.SNGL(M:M) to find the most frequent ride length in column M(ride_length). This was the same in each quarter/month = 00:06:00.

<img width="454" alt="mode_ride_length" src="https://github.com/PaxtonTaylor/Google-Data-Analytics-Capstone---Cyclistic-Bike-Share/assets/147224800/55e68fd2-7fbb-4e99-af09-9782cbd6c628">


#### Created 4 Pivot Tables in Excel
Calculated Average of ride_length for Customers, Subscribers, and all riders to compare the different ride length averages.

<img width="449" alt="avg_ride_length pivot table" src="https://github.com/PaxtonTaylor/Google-Data-Analytics-Capstone---Cyclistic-Bike-Share/assets/147224800/83af6b16-fe80-42be-98b7-53f4cd0b9c27">


Calculated the Count of ride_length to compare how many Customers, Subscribers, and total riders we had for each quarter/month.

<img width="416" alt="count of rides by user" src="https://github.com/PaxtonTaylor/Google-Data-Analytics-Capstone---Cyclistic-Bike-Share/assets/147224800/f832cd54-634b-4829-897c-873cc05edead">


Calculated the Average of ride_length for Customers and Subscribers by the day of the week for each quarter/month to show which days each user rode our bikes and for how long.

<img width="318" alt="count of rides by user by day_of_week" src="https://github.com/PaxtonTaylor/Google-Data-Analytics-Capstone---Cyclistic-Bike-Share/assets/147224800/e4da2f4c-b56a-4df3-928c-7c7a65d3f007">


Calculated the Count of ride_length to for Customers and Subscribers by the day of the week for each quarter/month to show how many of each membership rode and on which day of the week.

<img width="318" alt="count of rides by user by day_of_week" src="https://github.com/PaxtonTaylor/Google-Data-Analytics-Capstone---Cyclistic-Bike-Share/assets/147224800/7649b7ca-5f20-43b7-9d58-61ebe6751ad6">
