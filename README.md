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
