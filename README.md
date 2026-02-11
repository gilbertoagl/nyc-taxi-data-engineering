# NYC Taxi Data Engineering: End-to-End Microsoft Fabric Solution

## Project Overview
This repository implements a scalable data engineering pipeline capable of processing over **9.3 million records** of New York City Taxi trip data. Built on **Microsoft Fabric** and **PySpark**, the solution handles the full data lifecycle: ingestion, cleaning, transformation, and modeling into a Star Schema. The final output is a Power BI report using **Direct Lake** mode for high-performance analysis without data duplication.

![Dashboard Preview](assets/dashboard_main.png)

---

## Tech Stack
* **Orchestration:** Azure Data Factory (Fabric Pipelines)
* **Processing:** Apache Spark (PySpark) on Fabric Capacity
* **Storage:** Azure Data Lake Gen2 (Delta Lake format)
* **Modeling:** Star Schema (Kimball Architecture)
* **Optimization:** Z-ORDER Clustering & V-Order
* **Reporting:** Power BI (Direct Lake mode)

---

## Architecture
The system follows a **Medallion Architecture** (Bronze, Silver, Gold) to structure the data flow from external APIs to the analytical layer.

![Architecture Diagram](assets/Fabric_Project_Diagram.png)

---

## Data Modeling & Semantic Layer

### Star Schema
To optimize query performance for the Direct Lake report, the data is modeled into a strict Star Schema. This design reduces join complexity and enables fast aggregation on the Fabric backend.

![Star Schema](assets/starSchema_model.png)

* **Fact Table:** `fact_trips` (Partitioned by Date)
* **Dimensions:** `dim_date`, `dim_zone`, `dim_payment_type`

### Business Logic (DAX)
Business logic is encapsulated within the Semantic Layer using DAX measures, ensuring consistent KPI definitions across all reporting views.

![DAX Measures](assets/measures_factTable.png)

---

## Lakehouse Storage Structure
Data is physically organized in Microsoft Fabric's OneLake, maintaining a clear separation between raw files and managed tables.

![Lakehouse Structure](assets/lakehouse_files.png)

* **Files (Bronze Layer):** Raw Parquet files stored hierarchically.
* **Tables (Silver & Gold Layers):** Managed Delta Tables optimized for Spark and SQL endpoints.

---

## Orchestration: Metadata-Driven Pipeline
The ETL process avoids hardcoded logic. It uses a **Dynamic Parameterized Pattern** to handle multiple datasets (Yellow and Green taxis) within a single pipeline structure.

![Pipeline Overview](assets/pipeline_orchestration.png)

### 1. Configuration Array
The pipeline accepts a JSON Array parameter to define the execution batch dynamically.

![Config Array](assets/pipeline_config_array.png)

### 2. Iteration Logic
A `ForEach` activity iterates through the configuration array, triggering parallel execution for different datasets.

![Loop Logic](assets/pipeline_config_loop.png)

### 3. Context Injection
Dynamic expressions inject pipeline parameters directly into the Spark Notebook variables at runtime.

![Parameter Mapping](assets/pipeline_config_mapping.png)

---

## Code Implementation Details

### Incremental Loading (SCD Type 1)
To ensure data integrity, the Silver layer implements Delta Lake's `MERGE` operation. This logic updates existing records if the `trip_id` matches and inserts new ones, preventing duplicates during re-runs.

![Delta Merge Logic](assets/code_silver_merge.png)

### Schema Normalization
The pipeline dynamically handles schema drift between taxi types (e.g., standardizing `tpep_pickup_datetime` vs `lpep_pickup_datetime`), allowing a single notebook to process multiple sources without failure.

![Schema Handling](assets/code_schema_handling.png)

### Storage Optimization
Gold tables are optimized using **Z-ORDER Clustering** on the `pickup_zone_id` column. This physical data arrangement minimizes the amount of data read during queries by skipping irrelevant file blocks.

![Optimization](assets/code_gold_optimization.png)

---

## Repository Structure
* `src/`: PySpark Notebooks for Bronze, Silver, and Gold layers.
* `reports/`: Power BI file (.pbix).
* `assets/`: Project documentation images.

---
*Developed by Gilberto Agramont*