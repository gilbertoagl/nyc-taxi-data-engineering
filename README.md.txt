# NYC Taxi Data Engineering Project: End-to-End Fabric Solution

## Project Overview
High-performance data engineering solution processing over **9.3 million records** of NYC Taxi trip data. Leveraging **Microsoft Fabric** and **PySpark**, the pipeline ingests, cleans, and transforms raw data into a Star Schema, enabling a "Direct Lake" Power BI report for near real-time operational insights.

![Dashboard Preview](assets/dashboard_main.png)

## Tech Stack
* **Orchestration:** Azure Data Factory (Fabric Pipelines).
* **Processing:** Apache Spark (PySpark) on Fabric Capacity.
* **Storage:** Azure Data Lake Gen2 (Delta Lake format).
* **Modeling:** Star Schema optimized with Z-ORDER.
* **Reporting:** Power BI (Direct Lake mode).

---

## Architecture
The solution implements a **Medallion Architecture** (Bronze -> Silver -> Gold) orchestrating data flow from public APIs to analytical models.

![Architecture Diagram](assets/Fabric_Project_Diagram.png)

---

## Advanced Orchestration: Metadata-Driven Pipeline
Instead of hardcoding pipelines for each dataset (Yellow/Green), I implemented a Dynamic Parameterized Pattern to ensure scalability.

![Pipeline Overview](assets/pipeline_orchestration.png)

### 1. Configuration Array
The pipeline accepts a JSON Array parameter to define the execution batch dynamically.
![Config Array](assets/pipeline_config_array.png)

### 2. Iteration Logic (ForEach)
A `ForEach` activity iterates through the array, passing specific context (`taxi_type`, `date`) to the notebooks in parallel.
![Loop Logic](assets/pipeline_config_loop.png)

### 3. Context Injection
Dynamic expressions inject pipeline parameters directly into Spark variables.
![Parameter Mapping](assets/pipeline_config_mapping.png)

---

## Code Implementation Highlights

### 1. Incremental Loading (SCD Type 1)
To ensure data integrity, I implemented Delta Lake's `MERGE` operation. This logic updates existing records if `trip_id` matches and inserts new ones, preventing duplicates during re-runs.
![Delta Merge Logic](assets/code_silver_merge.png)

### 2. Schema Normalization & Robustness
The Silver layer dynamically handles schema drift between taxi types, allowing a single notebook to process multiple sources.
![Schema Handling](assets/code_schema_handling.png)

### 3. Storage Optimization (z-ORDER)
To minimize query costs and latency, the Gold tables are optimized using **Z-ORDER Clustering** on the `pickup_zone_id` column and maintained with `VACUUM` commands.
![Optimization](assets/code_gold_optimization.png)

---

## Repository Structure
* `src/`: PySpark Notebooks for Bronze, Silver, and Gold layers.
* `reports/`: Power BI file (.pbix) optimized for Direct Lake.
* `assets/`: Project documentation and evidence.

---
*Developed by Gilberto Agramont | Tech Stack: Microsoft Fabric & PySpark*