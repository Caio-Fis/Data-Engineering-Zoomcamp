# Homework 02 — Workflow Orchestration

This homework covers workflow orchestration with Kestra, and ETL pipelines from
[data-engineering-zoomcamp - docker-terraform](https://github.com/DataTalksClub/data-engineering-zoomcamp/tree/1887db054296717ca36dd934472e7f072e5b0ce6/01-docker-terraform).

---

### Question 1 — Within the execution for Yellow Taxi data for the year 2020 and month 12: what is the uncompressed file size (i.e. the output file yellow_tripdata_2020-12.csv of the extract task)? 

Using a Kestra plugin:

```yaml
tasks:
  - id: get_file_size
    type: io.kestra.plugin.core.storage.Size
    uri: "{{render(vars.data)}}"
```

the output is: **134481400 bytes**

### Question 2 — What is the rendered value of the variable file when the inputs taxi is set to green, year is set to 2020, and month is set to 04 during execution?

The variable file is defined as:

```bash
{{inputs.taxi}}_tripdata_{{inputs.year}}-{{inputs.month}}.csv
```

After rendering: **green_tripdata_2020-04.csv**

### Question 3 — How many rows are there for the Yellow Taxi data for all CSV files in the year 2020?

```bash
SELECT COUNT(*) AS rows_yellow_2020
FROM public.yellow_tripdata
WHERE filename LIKE 'yellow_tripdata_2020-%.csv';
```

the output is: **24648499**

### Question 4 — How many rows are there for the Green Taxi data for all CSV files in the year 2020? 

```bash
SELECT COUNT(*) AS rows_green_2020
FROM public.green_tripdata
WHERE filename LIKE 'green_tripdata_2020-%.csv';
```

the output is: **1734051**

### Question 5 — How many rows are there for the Yellow Taxi data for the March 2021 CSV file?

```bash
SELECT COUNT(*) AS rows_yellow_2021_03
FROM public.yellow_tripdata
WHERE filename = 'yellow_tripdata_2021-03.csv';
```

the output is: **1925152**

### Question 6 — How would you configure the timezone to New York in a Schedule trigger?


```yaml
triggers:
- id: 
type: io.kestra.plugin.core.trigger.Schedule
cron: ""
timezone: America/New_York
``` 