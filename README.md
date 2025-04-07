# 📌 IoT Sensor Data Analysis with PySpark SQL

## 📂 Description
This project analyzes synthetic IoT sensor data using Apache Spark (PySpark). The data includes temperature and humidity readings from sensors placed across different building floors. The goal is to explore, filter, aggregate, rank, and pivot this data using Spark SQL and DataFrame APIs.

---

## 🧑‍💻 Technologies Used
- **Python**
- **Apache Spark (PySpark)**
- **Spark SQL**
- **CSV** (Input/Output)

---

## 📁 Files in the Repository

| File               | Description                                                           |
|--------------------|-----------------------------------------------------------------------|
| `data_generator.py`| Script to generate `sensor_data.csv` with 1000 records using Faker     |
| `sensor_data.csv`  | Sample IoT sensor dataset (can be generated using the script)          |
| `main.py`          | Complete analysis script that runs all 5 Spark SQL tasks               |
| `task1_output.csv` | Output for Task 1: Basic exploration result                            |
| `task2_output.csv` | Output for Task 2: Filtered and aggregated results by location         |
| `task3_output.csv` | Output for Task 3: Hourly average temperature                          |
| `task4_output.csv` | Output for Task 4: Top 5 sensors ranked by average temperature         |
| `task5_output.csv` | Output for Task 5: Pivoted table with average temperature per location/hour |

---

## ✅ Tasks Breakdown & Outputs

---

### 📌 Task 1: Load & Basic Exploration

**Goal:** Load and inspect the sensor dataset.

**Steps:**
- Load `sensor_data.csv` into Spark
- Create a temporary view `sensor_readings`
- Display:
  - First 5 records
  - Total number of records
  - Distinct locations

**Output File:** `task1_output.csv`
```bash
+---------+-------------------+-----------+--------+----------------+-----------+
|sensor_id|          timestamp|temperature|humidity|        location|sensor_type|
+---------+-------------------+-----------+--------+----------------+-----------+
|     1011|2025-04-05 17:25:40|      28.61|   49.68|BuildingA_Floor2|      TypeB|
|     1093|2025-04-05 10:46:35|      32.79|   53.15|BuildingB_Floor1|      TypeA|
|     1013|2025-04-03 18:12:00|       19.3|   47.96|BuildingB_Floor2|      TypeC|
|     1045|2025-04-02 21:43:04|      21.39|   51.32|BuildingA_Floor2|      TypeB|
|     1018|2025-04-07 12:20:34|      17.49|   57.83|BuildingB_Floor1|      TypeB|
+---------+-------------------+-----------+--------+----------------+-----------+
```
---

### 📌 Task 2: Filtering & Aggregations

**Goal:** Identify temperature outliers and compute averages.

**Steps:**
- Filter records where `temperature` is **between 18–30°C** (in-range)
- Count in-range vs out-of-range
- Group by `location` to calculate:
  - Average `temperature`
  - Average `humidity`

**Output File:** `task2_output.csv`
In-range temperature count: 620
Out-of-range temperature count: 380
Average temperature & humidity by location:
```bash
+----------------+------------------+------------------+
|        location|   avg_temperature|      avg_humidity|
+----------------+------------------+------------------+
|BuildingA_Floor1|25.373661417322847|  54.5625590551181|
|BuildingA_Floor2|25.169090909090905| 55.13221818181823|
|BuildingB_Floor2| 25.08088495575221|54.522123893805315|
|BuildingB_Floor1| 24.54314285714287| 55.37008163265307|
+----------------+------------------+------------------+
```
---

### 📌 Task 3: Time-Based Analysis

**Goal:** Understand hourly temperature trends.

**Steps:**
- Convert `timestamp` string to Spark timestamp type
- Extract the `hour_of_day` from each timestamp
- Group by hour and calculate the **average temperature**

**Output File:** `task3_output.csv`
```bash
Average temperature by hour:
+-----------+------------------+
|hour_of_day|          avg_temp|
+-----------+------------------+
|          0|25.853500000000004|
|          1|26.574444444444445|
|          2|25.594313725490192|
|          3|25.314000000000004|
|          4|23.913488372093028|
|          5|26.178399999999996|
|          6| 25.41735294117647|
|          7| 26.25907407407407|
|          8|25.350249999999996|
|          9| 23.85025641025641|
|         10|            24.145|
|         11|24.525945945945953|
|         12|25.204042553191492|
|         13|23.713636363636365|
|         14|24.618372093023254|
|         15|25.040749999999996|
|         16|24.353999999999992|
|         17|24.894545454545455|
|         18|25.645714285714288|
|         19| 25.31209302325582|
+-----------+------------------+
```
---

### 📌 Task 4: Sensor Ranking by Temperature

**Goal:** Identify the top 5 sensors with the highest average temperature.

**Steps:**
- Group by `sensor_id` and calculate `avg_temp`
- Use a **Window Function (`RANK()`)** to rank sensors in descending order
- Show top 5 sensors

**Output File:** `task4_output.csv`
```bash
+---------+------------------+---------+
|sensor_id|          avg_temp|rank_temp|
+---------+------------------+---------+
|     1026|          28.82125|        1|
|     1096|28.454705882352943|        2|
|     1090|             28.16|        3|
|     1042|28.126875000000002|        4|
|     1020|          28.07375|        5|
+---------+------------------+---------+
```
---

### 📌 Task 5: Pivot Table by Location and Hour

**Goal:** Visualize temperature variation by hour and location.

**Steps:**
- Use `location` as rows and `hour_of_day` (0–23) as columns
- Use **average temperature** as values
- Create a **pivot table**

**Output File:** `task5_output.csv`
```bash
+------------------+-------+-------+-----+-------+
| location         |   0   |   1   | ... |  23   |
+------------------+-------+-------+-----+-------+
| BuildingA_Floor1 | 24.84 | 24.72 | ... | 25.88 |
| BuildingA_Floor2 | 26.26 | 29.10 | ... | 23.58 |
| BuildingB_Floor1 | 25.30 | 23.70 | ... | 23.64 |
| BuildingB_Floor2 | 27.81 | 27.14 | ... | 23.45 |
+------------------+-------+-------+-----+-------+
```
---

## 🚀 Running the Project

### Install Dependencies
Install the necessary Python libraries:
```bash
pip install pyspark faker
```
### Data generator and main code running 
```bash
python data_generator.py
python main.py
