# data-engineering-zoomcamp

## Module 1 Homework Supplementary Code

### Question 1

docker run -it \
    --rm \
    --entrypoint=bash \
    python:3.13

pip -V

### Question 2

N/A

### Question 3

SELECT COUNT(*) AS short_trip_count
FROM green_taxi_trips
WHERE lpep_pickup_datetime >= '2025-11-01 00:00:00'
  AND lpep_pickup_datetime < '2025-12-01 00:00:00'
  AND trip_distance <= 1;

### Question 4

SELECT
    CAST(lpep_pickup_datetime AS DATE) AS pickup_day,
    MAX(trip_distance) AS max_distance
FROM
    green_taxi_trips
WHERE
    trip_distance < 100
GROUP BY
    CAST(lpep_pickup_datetime AS DATE)
ORDER BY
    max_distance DESC
LIMIT 1;

### Question 5

SELECT
    z."Zone" AS pickup_zone,
    SUM(t.total_amount) AS total_revenue
FROM
    green_taxi_trips t
JOIN
    taxi_zone_lookup z ON t."PULocationID" = z."LocationID"
WHERE
    t.lpep_pickup_datetime >= '2025-11-18 00:00:00'
    AND t.lpep_pickup_datetime < '2025-11-19 00:00:00'
GROUP BY
    z."Zone"
ORDER BY
    total_revenue DESC
LIMIT 1;

### Question 6

SELECT
    do_zone."Zone" AS dropoff_zone
FROM
    green_taxi_trips t
JOIN
    taxi_zone_lookup pu_zone ON t."PULocationID" = pu_zone."LocationID"
JOIN
    taxi_zone_lookup do_zone ON t."DOLocationID" = do_zone."LocationID"
WHERE
    pu_zone."Zone" = 'East Harlem North'
    AND t.lpep_pickup_datetime >= '2025-11-01 00:00:00'
    AND t.lpep_pickup_datetime < '2025-12-01 00:00:00'
ORDER BY
    t.tip_amount DESC
LIMIT 1;

### Question 7

N/A

## Module 2 Homework Supplementary Code

### Question 1

N/A

### Question 2

N/A

### Question 3

SELECT COUNT(*)
FROM yellow_tripdata
WHERE filename LIKE 'yellow_tripdata_2020%';

### Question 4

SELECT COUNT(*)
FROM green_tripdata
WHERE filename LIKE 'green_tripdata_2020%';

### Question 5

SELECT COUNT(*) 
FROM yellow_tripdata 
WHERE filename = 'yellow_tripdata_2021-03.csv';

### Question 6

N/A

## Module 3 Homework Supplementary Code

### Question 1

SELECT COUNT(*) 
FROM `dtc-de-course-485815.zoomcamp.yellow_tripdata_2024_ext`;

### Question 2

SELECT COUNT(DISTINCT PULocationID)
FROM `dtc-de-course-485815.zoomcamp.yellow_tripdata_2024_ext`;

SELECT COUNT(DISTINCT PULocationID)
FROM `dtc-de-course-485815.zoomcamp.yellow_tripdata_2024_non_partitioned`;

### Question 3

N/A

### Question 4

SELECT COUNT(*)
FROM `dtc-de-course-485815.zoomcamp.yellow_tripdata_2024_non_partitioned`
WHERE fare_amount = 0;

### Question 5

CREATE OR REPLACE TABLE `dtc-de-course-485815.zoomcamp.yellow_tripdata_2024_optimized`
PARTITION BY DATE(tpep_dropoff_datetime)
CLUSTER BY VendorID
AS
SELECT * FROM `dtc-de-course-485815.zoomcamp.yellow_tripdata_2024_non_partitioned`;

### Question 6

SELECT DISTINCT VendorID
FROM `dtc-de-course-485815.zoomcamp.yellow_tripdata_2024_non_partitioned`
WHERE tpep_dropoff_datetime BETWEEN '2024-03-01' AND '2024-03-15';

SELECT DISTINCT VendorID
FROM `dtc-de-course-485815.zoomcamp.yellow_tripdata_2024_optimized`
WHERE tpep_dropoff_datetime BETWEEN '2024-03-01' AND '2024-03-15';

### Question 7

N/A

### Question 8

N/A

### Question 9

N/A
