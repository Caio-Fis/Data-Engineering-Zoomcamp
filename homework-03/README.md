# Homework 03 — Data Warehouse

This homework covers concepts of data warehouse using Google Cloud and Bigquery from
[data-engineering-zoomcamp - Data Warehouse](https://github.com/DataTalksClub/data-engineering-zoomcamp/tree/1f9a4dbb041e018691a94be06bd79b14726caae4/cohorts/2026/03-data-warehouse).

---

### Question 1 — What is count of records for the 2024 Yellow Taxi Data? 

```bash
SELECT COUNT(*) AS count_records
FROM `project_name.de_zoomcamp.yellow_tripdata_2024`;
```

output: **20332093**

### Question 2 — Write a query to count the distinct number of PULocationIDs for the entire dataset on both the tables.

External:

```bash
SELECT COUNT(DISTINCT PULocationID) AS distinct_pu
FROM `project_name.de_zoomcamp.yellow_tripdata_2024_ext`;
```

This query will process 0 MB when run.

Unpartitioned (Materialized):

```bash
SELECT COUNT(DISTINCT PULocationID) AS distinct_pu
FROM `project_name.de_zoomcamp.yellow_tripdata_2024`;
```

This query will process 155.12 MB when run.

### Question 3 — Why are the estimated number of Bytes different?

Answer: BigQuery is a columnar database, and it only scans the specific columns requested in the query. Querying two columns (PULocationID, DOLocationID) requires reading more data than querying one column (PULocationID), leading to a higher estimated number of bytes processed.

```bash
SELECT PULocationID
FROM `project_name.de_zoomcamp.yellow_tripdata_2024`;
```

This query will process 155.12 MB when run.

```bash
SELECT PULocationID, DOLocationID
FROM `project_name.de_zoomcamp.yellow_tripdata_2024`;
```

This query will process 310.24 MB when run.

### Question 4 — How many records have a fare_amount of 0? 

```bash
SELECT COUNT(*) AS count_fare
FROM `project_name.de_zoomcamp.yellow_tripdata_2024`
WHERE fare_amount = 0;
```

Output: **8333**

### Question 5 — What is the best strategy to make an optimized table in Big Query if your query will always filter based on tpep_dropoff_datetime and order the results by VendorID (Create a new table with this strategy) 

Answer: Partition by tpep_dropoff_datetime and Cluster on VendorID.

```bash
CREATE OR REPLACE TABLE `project_name.de_zoomcamp.yellow_tripdata_2024_partclust`
PARTITION BY DATE(tpep_dropoff_datetime)
CLUSTER BY VendorID
AS
SELECT * FROM `project_name.de_zoomcamp.yellow_tripdata_2024`;
```

### Question 6 — How would you configure the timezone to New York in a Schedule trigger?

```bash
SELECT DISTINCT VendorID
FROM `project_name.de_zoomcamp.yellow_tripdata_2024`
WHERE DATE(tpep_dropoff_datetime) BETWEEN '2024-03-01' AND '2024-03-15';
``` 

This query will process 310.24 MB when run.

```bash
SELECT DISTINCT VendorID
FROM `project_name.de_zoomcamp.yellow_tripdata_2024_partclust`
WHERE DATE(tpep_dropoff_datetime) BETWEEN '2024-03-01' AND '2024-03-15';
``` 

This query will process 26.84 MB when run.

### Question 7 - Where is the data stored in the External Table you created?

Answer: GCP Bucket (GCS).

### Question 8 - It is best practice in Big Query to always cluster your data:

Answer: False.

### Question 9 - Write a `SELECT count(*)` query FROM the materialized table you created. How many bytes does it estimate will be read? Why?

```bash
SELECT COUNT(*)
FROM `project_name.de_zoomcamp.yellow_tripdata_2024`;
``` 

Answer: BigQuery can compute it from table metadata without need to scan the table.