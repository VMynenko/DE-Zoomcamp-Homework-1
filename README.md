## Question 1. Understanding docker first run 

Run Docker container using the official Python 3.12.8 image
```bash
docker run -it --entrypoint bash python:3.12.8
```
Running
```bash
pip --version
```
Will display the installed pip version 
- pip 24.3.1

## Question 2. Understanding Docker networking and docker-compose

Hostname: db  
Port: 5432

Answer
- db:5432

## Question 3. Trip Segmentation Count

```sql
-- Up to 1 mile
SELECT COUNT(1) AS trips_count
FROM yellow_taxi_data
WHERE trip_distance <= 1 
	AND EXTRACT(YEAR FROM lpep_dropoff_datetime) = 2019 
	AND EXTRACT(MONTH FROM lpep_dropoff_datetime) = 10;
```

```sql
-- In between 1 (exclusive) and 3 miles (inclusive)
SELECT COUNT(1) AS trips_count
FROM yellow_taxi_data
WHERE trip_distance > 1 AND trip_distance <= 3 
	AND EXTRACT(YEAR FROM lpep_dropoff_datetime) = 2019 
	AND EXTRACT(MONTH FROM lpep_dropoff_datetime) = 10;
```

```sql
-- In between 3 (exclusive) and 7 miles (inclusive)
SELECT COUNT(1) AS trips_count
FROM yellow_taxi_data
WHERE trip_distance > 3 AND trip_distance <= 7
	AND EXTRACT(YEAR FROM lpep_dropoff_datetime) = 2019 
	AND EXTRACT(MONTH FROM lpep_dropoff_datetime) = 10;
```

```sql
-- In between 7 (exclusive) and 10 miles (inclusive)
SELECT COUNT(1) AS trips_count
FROM yellow_taxi_data
WHERE trip_distance > 7 AND trip_distance <= 10
	AND EXTRACT(YEAR FROM lpep_dropoff_datetime) = 2019 
	AND EXTRACT(MONTH FROM lpep_dropoff_datetime) = 10;
```

```sql
-- Over 10 miles
SELECT COUNT(1) AS trips_count
FROM yellow_taxi_data
WHERE trip_distance > 10
	AND EXTRACT(YEAR FROM lpep_dropoff_datetime) = 2019 
	AND EXTRACT(MONTH FROM lpep_dropoff_datetime) = 10;
```

Answer
- 104,802;  198,924;  109,603;  27,678;  35,189

## Question 4. Longest trip for each day

```sql
SELECT DATE(lpep_pickup_datetime) AS lpep_pickup_datetime
FROM yellow_taxi_data
WHERE trip_distance = (SELECT MAX(trip_distance) FROM yellow_taxi_data)
```

Answer
- 2019-10-31

## Question 5. Three biggest pickup zones

```sql
SELECT "Zone"
FROM yellow_taxi_data AS ytd
LEFT JOIN zones ON ytd."PULocationID" = zones."LocationID"
WHERE DATE(lpep_pickup_datetime) = '2019-10-18'
GROUP BY 1
HAVING SUM(total_amount) > 13000
```

Answer
- East Harlem North, East Harlem South, Morningside Heights

## Question 6. Largest tip

```sql
WITH sample AS (
SELECT "DOLocationID", tip_amount
FROM yellow_taxi_data AS ytd
LEFT JOIN zones ON ytd."PULocationID" = zones."LocationID"
WHERE EXTRACT(MONTH FROM lpep_pickup_datetime) = 10
	AND "Zone" = 'East Harlem North'
)

SELECT "Zone" 
FROM sample
LEFT JOIN zones ON sample."DOLocationID" = zones."LocationID"
WHERE tip_amount = (SELECT MAX(tip_amount) FROM sample)
```

Answer
- JFK Airport

## Question 7. Terraform Workflow

**terraform init** downloads provider plugins and sets up backend  
**terraform apply -auto-approve** generates and automatically executes planned changes  
**terraform destroy** removes all resources managed by Terraform  

Answer
- terraform init, terraform apply -auto-approve, terraform destroy
