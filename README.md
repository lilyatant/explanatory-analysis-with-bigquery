# explanatory-analysis-with-sql

number of stations
```
SELECT count(distinct station_id)
FROM `bigquery-public-data.new_york_citibike.citibike_stations`
```
average capacity
```
SELECT ROUND(AVG(capacity),2)
FROM `bigquery-public-data.new_york_citibike.citibike_stations`
```
maximum capacity
```
SELECT MAX(capacity)
FROM `bigquery-public-data.new_york_citibike.citibike_stations`
```
minimum capacity
```
SELECT MIN(capacity)
FROM `bigquery-public-data.new_york_citibike.citibike_stations`
WHERE (capacity !=0)
```
number of regions covered
```
SELECT count(distinct region_id)
FROM `bigquery-public-data.new_york_citibike.citibike_stations`
```
average number of trips per week in the last full year of data
```
SELECT
week,
ROUND(COUNT(*)/7,2) AS avg_trips_per_week
FROM (
SELECT
EXTRACT(WEEK FROM starttime) AS week,
EXTRACT(YEAR FROM starttime) AS year
FROM `bigquery-public-data.new_york_citibike.citibike_trips`
) AS t
WHERE year = 2014
GROUP BY week
ORDER BY week;
```
average duration of trips in minutes
```
SELECT AVG(tripduration/60)
FROM `bigquery-public-data.new_york_citibike.citibike_trips`
```
breakdown by gender
```
SELECT
gender, COUNT(*) AS number
FROM `bigquery-public-data.new_york_citibike.citibike_trips`
WHERE gender IN ("male", "female")
GROUP BY gender;
```
average age of users
```
SELECT
AVG((EXTRACT(YEAR FROM CURRENT_DATE) - birth_year)) AS age
FROM `bigquery-public-data.new_york_citibike.citibike_trips
```
Evolution of the number of trips per year, per month
```
SELECT
PARSE_DATE('%d/%m/%Y',CONCAT('01', "/", month,'/', year)) as Date,
number_of_trips
FROM
(SELECT
year,
month,
COUNT(*) AS number_of_trips
FROM (
SELECT
EXTRACT(YEAR FROM starttime) AS year,
EXTRACT(MONTH FROM starttime) AS month
FROM `bigquery-public-data.new_york_citibike.citibike_trips`
WHERE starttime IS NOT NULL AND gender IS NOT NULL
) AS t
GROUP BY year, month
HAVING year BETWEEN 2013 AND 2018
ORDER BY year, month
) AS t2;
```
Evolution of breakdown by gender
```
SELECT
year,
COUNT(case WHEN gender = "male" then 1 end) as num_males,
COUNT(case WHEN gender = "female" then 1 end) as num_females
FROM (
SELECT gender,
starttime,
EXTRACT(YEAR FROM starttime) AS year
FROM `bigquery-public-data.new_york_citibike.citibike_trips`
WHERE starttime IS NOT NULL AND gender IS NOT NULL) AS t
GROUP BY year
HAVING year BETWEEN 2013 AND 2018
ORDER BY year;
```
Evolution of customers by age
```
SELECT
year,
COUNT(case WHEN age BETWEEN 20 AND 24 then 1 end) as age_20_24,
COUNT(case WHEN age BETWEEN 25 AND 34 then 1 end) as age_25_34,
COUNT(case WHEN age BETWEEN 35 AND 44 then 1 end) as age_35_44,
COUNT(case WHEN age BETWEEN 45 AND 54 then 1 end) as age_45_54,
COUNT(case WHEN age BETWEEN 55 AND 64 then 1 end) as age_55_64,
COUNT(case WHEN age BETWEEN 65 AND 74 then 1 end) as age_65_74
FROM (
SELECT
EXTRACT(YEAR FROM starttime) AS year,
(2022 - birth_year) AS age
FROM `bigquery-public-data.new_york_citibike.citibike_trips`
) AS t
GROUP BY year
HAVING year BETWEEN 2013 AND 2018
ORDER BY year;
```
Stations with the most starting point
```
SELECT start_station_id,
COUNT(*) AS number
FROM `bigquery-public-data.new_york_citibike.citibike_trips`
WHERE start_station_id IS NOT NULL
GROUP BY start_station_id
ORDER BY number DESC
LIMIT 20;
```
Stations with the most ending point
```
SELECT end_station_id,
COUNT(*) AS number
FROM `bigquery-public-data.new_york_citibike.citibike_trips`
WHERE end_station_id IS NOT NULL
GROUP BY end_station_id
ORDER BY number DESC
LIMIT 20;
```
Stations with the highest capacity
```
SELECT station_id,
capacity
FROM `bigquery-public-data.new_york_citibike.citibike_stations`
ORDER BY capacity DESC
LIMIT 20;
```
Stations with highest number of available bikes
```
SELECT
station_id,
num_bikes_available
FROM `bigquery-public-data.new_york_citibike.citibike_stations`
ORDER BY num_bikes_available DESC
LIMIT 20;
```
