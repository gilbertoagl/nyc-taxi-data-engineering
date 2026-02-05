# NYC Taxi Data Engineering Project

## Overview
This project demonstrates an **End-to-End Data Engineering Pipeline** built entirely on **Microsoft Fabric**. The goal is to ingest, transform, and model historical New York City Taxi data to analyze trip patterns, costs, and demand using a **Medallion Architecture (Bronze, Silver, Gold)**.

The system processes large datasets using **Apache Spark** and optimizes data availability for reporting via **Direct Lake** in Power BI, ensuring high performance without data duplication.

## Architecture
![Architecture Diagram](architecture.png)
*(Logical Solution Architecture)*

## Tech Stack
* **Platform:** Microsoft Fabric
* **Storage:** OneLake (Parquet & Delta Lake)
* **Processing:** Apache Spark (PySpark)
* **Orchestration:** Data Factory Pipelines
* **Serving:** SQL Analytics Endpoint & Power BI (Direct Lake Mode)

## Data Flow (Medallion Architecture)
1.  **Bronze Layer (Raw):** Ingestion of raw data from the official NYC TLC website (Parquet format) into OneLake.
2.  **Silver Layer (Clean):** Data cleaning, schema enforcement, deduplication, and conversion to optimized **Delta Tables**.
3.  **Gold Layer (Star Schema):** Dimensional modeling (Facts & Dimensions) optimized for analytical queries and reporting.

## Dashboard
The final executive dashboard visualizes key metrics, including:
* Average fare trends based on distance.
* High-demand pickup/drop-off zones.
* Temporal analysis of trips (daily/monthly seasonality).

---
*Created by Gilberto Agramont - 2026*
