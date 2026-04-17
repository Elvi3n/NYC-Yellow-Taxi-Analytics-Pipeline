# NYC Yellow Taxi Analytics Pipeline

This is an end-to-end data project using NYC Yellow Taxi (TLC) trip record data for 2025, leveraging Fabric's Data Warehousing, Data Factory, Data Engineering, and Power BI experiences.

**_Please refer to the [Wiki](https://github.com/Elvi3n/Fabric-Analytic-Pipeline-NYC-Taxi-Project/tree/main/Wiki) for the code used in the data pipelines, stored procedures, variables and parameters._**

## Project Overview

This project delivers a fully automated analytics pipeline built on Microsoft Fabric, processing **~18 million** New York City Yellow Taxi (TLC) trip records from July to Dec 2025. The solution covers the full data lifecycle — from raw file ingestion through to an interactive Power BI report. The project demonstrates real-world data engineering practices including dynamic pipeline automation, metadata logging, data quality enforcement, and multi-layer warehouse architecture.

### Objectives

- Build a scalable, automated monthly data pipeline using Microsoft Fabric's unified analytics platform
- Implement a multi-layer data warehouse architecture (Staging → Presentation → Reporting)
- Enforce data quality through cleansing and outlier removal
- Enable self-advancing pipeline logic to process new monthly data without manual intervention
- Surface insights through an interactive Power BI report connected to a live semantic model


### Data Sources

Data sourced from the [NYC Taxi & Limousine Commission (TLC)](https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page). The monthly Yellow Taxi Trip Records (July–Dec 2025) include pickup/dropoff datetime, location IDs, passenger count, distance, payment type, and fare amount. The Taxi Zone Lookup Table (CSV) include maps location IDs to borough, zone, and service zone.

---

## Solution Architecture - Automated Medallion Architecture

<img width="1000" height="585" alt="image" src="https://github.com/user-attachments/assets/8efced4f-b09e-47f5-9473-e60c98fd72b8" />

The result is an automated ELT pipeline in Microsoft Fabric to process millions of NYC Taxi records, transitioning data from raw Parquet files to an analytics-ready Gold layer.

---

### Key Technical Highlights

- **Metadata-Driven Orchestration:** Engineered a pipeline that automatically identifies the next data batch by querying a SQL audit log. It utilizes dynamic date logic (formatDateTime) to target specific monthly files without manual intervention.

- **Medallion Design:** Implemented automated data quality enforcement via Stored Procedures to purge temporal outliers and errors.

- **Silver/Gold:** Built transformation logic using Dataflow Gen2 to denormalize data, performing joins for location enrichment and translating system codes into business dimensions.

- **Metadata Logging:** Integrated a metadata.processing_log system that tracks pipeline_run_id, row counts, and processing timestamps, for observability.

---

### Power BI Report

Built from a blank canvas connected to the ProjectWarehouse default semantic model.

<img width="1200" height="666" alt="image" src="https://github.com/user-attachments/assets/ec36dfd9-0662-4270-a690-98d2609e8fce" />

---

## Fabric Project Structure
```
NYCTaxiDataProject/
│
├── ProjectLakehouse/
│   └── Files/
│       ├── nyctaxi_yellow/          # 5x monthly parquet files
│       └── nyctaxi_lookup_zones/    # Taxi zone CSV
│
├── ProjectWarehouse/
│   ├── stg/                         # Staging schema (Silver)
│   │   ├── nyctaxi_yellow
│   │   └── taxi_zone_lookup
│   ├── dbo/                         # Presentation schema (Gold)
│   │   └── nyctaxi_yellow
│   └── metadata/                    # Governance
│       └── processing_log
│
├── NYC Taxi Data Pipelines/
│    ├── pl_stage_lookup              # One-off lookup table load
│    ├── pl_stg_processing_nyctaxi    # Monthly staging pipeline
│    ├── pl_pres_processing_nyctaxi   # Presentation pipeline
│    └── pl_orchestrate_nyctaxi       # Master orchestration pipeline
│
├── sm_nyctaxi_analytics/             # Semantic Model
│
└── NYC Taxi Executive Dashboard/      # Power BI Report
```
<img width="700" height="323" alt="image" src="https://github.com/user-attachments/assets/9d6cef9e-214c-41c9-a14b-dffcce52e8e9" />

---
### Repo Structure
```
NYC Yellow Taxi Analytics Pipeline
├── Raw data/                 # Taxi zone lookup CSV
├── Wiki/                     # Documentation of code used in data pipelines, stored procedures, variables and parameters.
├── LICENSE                   # Project licensing information
├── NYC Yellow Taxi Report.pbix # Final Power BI report file
└── README.md                 
```
