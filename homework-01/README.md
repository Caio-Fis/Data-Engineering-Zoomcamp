# Homework 01 — Docker & SQL

This homework covers Docker basics, docker-compose networking, SQL queries and Terraform workflow from
[data-engineering-zoomcamp - docker-terraform](https://github.com/DataTalksClub/data-engineering-zoomcamp/tree/1887db054296717ca36dd934472e7f072e5b0ce6/01-docker-terraform).

---

## Question 1 — Understanding Docker images

```bash
docker run -it --rm --entrypoint bash python:3.13
python -m pip --version
```

## Question 2 — Understanding Docker networking and docker-compose

```bash
Hostname: db
Port: 5432
```

## Question 3 — Counting short trips

```bash
SELECT COUNT(*) AS trips_le_1_mile
FROM read_parquet('data/green_tripdata_2025-11.parquet')
WHERE lpep_pickup_datetime >= TIMESTAMP '2025-11-01'
  AND lpep_pickup_datetime <  TIMESTAMP '2025-12-01'
  AND trip_distance <= 1;
```

## Question 4 — Longest trip for each day

```bash
SELECT
  CAST(lpep_pickup_datetime AS DATE) AS pickup_day,
  MAX(trip_distance) AS max_trip_distance
FROM read_parquet('data/green_tripdata_2025-11.parquet')
WHERE lpep_pickup_datetime >= TIMESTAMP '2025-11-01'
  AND lpep_pickup_datetime <  TIMESTAMP '2025-12-01'
  AND trip_distance < 100
GROUP BY 1
ORDER BY max_trip_distance DESC
LIMIT 1;
```

## Question 5 — Biggest pickup zone

```bash
SELECT
  z."Zone" AS pickup_zone,
  SUM(g.total_amount) AS total_amount_sum
FROM read_parquet('data/green_tripdata_2025-11.parquet') g
JOIN read_csv_auto('data/taxi_zone_lookup.csv') z
  ON g.PULocationID = z."LocationID"
WHERE CAST(g.lpep_pickup_datetime AS DATE) = DATE '2025-11-18'
GROUP BY 1
ORDER BY total_amount_sum DESC
LIMIT 1;
```

## Question 6 — Largest tip

```bash
SELECT
  zdo."Zone" AS dropoff_zone,
  MAX(g.tip_amount) AS max_tip
FROM read_parquet('data/green_tripdata_2025-11.parquet') g
JOIN read_csv_auto('data/taxi_zone_lookup.csv') zpu
  ON g.PULocationID = zpu."LocationID"
JOIN read_csv_auto('data/taxi_zone_lookup.csv') zdo
  ON g.DOLocationID = zdo."LocationID"
WHERE zpu."Zone" = 'East Harlem North'
  AND g.lpep_pickup_datetime >= TIMESTAMP '2025-11-01'
  AND g.lpep_pickup_datetime <  TIMESTAMP '2025-12-01'
GROUP BY 1
ORDER BY max_tip DESC
LIMIT 5;
```

## Question 7 — Terraform Workflow

```bash
1. terraform init
2. terraform apply -auto-approve
3. terraform destroy
```



