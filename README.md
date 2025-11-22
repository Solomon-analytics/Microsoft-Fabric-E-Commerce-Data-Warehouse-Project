**Microsoft Fabric: E-Commerce Data Warehouse Project**
---
**Project Overview:**

This  project focuses on designing and implementing an end-to-end data warehousing solution using Microsoft Fabric for a global e-commerce environment. Data was sourced from two separate e-commerce systems, olist and glb, each containing customer information, product catalogues, order details, product and customer attributes.

The objective was to integrate these datasets with varying naming conventions and data quality issues into a unified analytics-ready architecture following the medallion (Bronze-silver-gold) framework, enabling clean, consistent and high quality data for business intelligence and advanced analytics.

The solution includes data ingestion, tranformation, modelling, pipeline orchestration, and dashboarding, demonstrating a full modern data platform workflow within Mircosfot fabric.

---
**Business Problem:**

E-commerce companies often collect data data from multiple ystems. However, these systems usually store information in different structure and formats, leading to innconsistent product details, duplicated customers, missing values, and incompatible date and currency formats.
Because of these inconsistencies, it becoes difficult to generate accurate sales reports, understand customer behaviour, or build relaible dashboard for decision-making.

This project addresses a typcial real world challenge: bringing together data from multiple e-commerce sources, resolving quality issues, standardising the structure, and preparing a single trusted dataset for analytics and business intelligence.

---

**Data Source:**

 The dataset was sourced from a public open-data repository. Dataset from the olist source is an official e-commerce dataset from a Brazillian online marketplace, while dataset from the the global source was also sourced from Kaggle and contains a broader collection of customer purchase recrods from multiple countries.

 For the purpose of this project, the dataset from olist sources is been named as 'olist' while the dataset from global source is named as 'glb'.

 ---

 **Architecture Summary**

 - Lakehouse
 - Data Warehouse
 - Data ingestion and transformation using SQL
 - Pipelines
 - Dataflow Gen2
 - Semantic Model
 - Power BI report

![](architecture_diagram.svg)






---

**Source Systems:**

- Olist systems: E-commerce platform data
- Key Tables:
   - customers
   - orders
   - order items
   - payments
   - products
   - product caetegories

- Glb Systems
- Ket tables:
    - Customers
    - products
    - sales details
    - customer additional attributes
    - product additional attributes

---


**Business Problems:**

- **Fragmented Data**
   - customer, product, and sales data exist in multiple systems with different formats.
- **Data Quality Issues**
   - inconsistent naming conventions
   - Missing or null values in key fields
   - Duplicates records
- **Need for anakytical Insights**
    - Business user require consolidated and clean dimensions/fact tables for reporting.
- **Scalability and Maintainability**
    - Solution must allow incremental ingestion of new files without breaking the ETL pipeline
 


---


**Solution Approach**

- **Step 1: Raw Data Ingestion**
  - Data copied from lakehouse/raw/olist and lakehouse/raw/glb into the staging schemas stg_olist and stg_glb.
  - Fabric data orchestrate:
    - Daily ingestion
    - Only new files are picked up
    - csvs are landed in staging
---

- **Step 2: consolidation**
  - **Stored Procedure": bronze.load_bronze
    - **Tasks:**
      - Create tables for each source system
      - Truncate tables and insert latest staged data
      - clean basic data issues(trim strings, handle nulls, convert date stored as strings to date)
  - **Outcome** bronze tables with curated source data.

 ---


 - **Step 3: Silver Layer Transformation**
   - **Stored Procedure:** silver.load_silver
    - **Tasks:**
      - Apply business rules and data quality checks
      - Deduplicate records
      - Standardise name, date and categories
      - Derive additional attributes (e.g., processed_date)
   - **Outcome:** Clean and standardised tables ready for analytics.

---

- **Step 4: Gold Layer - Business-Focused Views**
  - **View Created:**
    - gold.cim_customers - consolidated customer attributes
    - gold.dim_products - consolidated product attributes
    - gold.fact_sales_order -  consolidated sales fact table
  - **Tasks:**
    - Merge data from olist an glb systems
    - Generate surrogate keys using (ROW_NUMBER())
    - Add source system metadata (cource_name)
    - Convert dates to a standard string format for data modelling relationship
  - **Outcome:** Business-friendly semantic layer suitable for reporting and Business Intelligence.
 
---



**ETL Pipeline Overview**

**Pipeline 1: Raw Ingestion**

- Sources: Lakehouse/raw/olist, Lakehouse/raw/glb
- Destination: stg_olist, stg_glb
- Schedule: Daily/Weekly
- Features: incremental file detection

**Pipeline 2: Staging --> Bronze**
 - calls bronze.load_bronze stored procedure
 - ingest stg data to bronze tables

**Pipeline 3: Bronze --> Silver**
 - Calles silver.load_silver
 - Cleans and deduplicates data

**Pipeline 2: Silver --> Gold**
 - No Stored Procedure needed; tables in the gold layer are stored as views
 - Automatically refreshed based on silver tables.


---


**Data Quality Checks:**

- Deduplication using ROW_NUMBER() OVER (PARTITION BY ORDER BY)
- Null value handling for key attributes
- Standardisation of gender, marital status, country, and category
- Dericed metadata columns, processed_date and soure_name

---


**Business Benefits**

- **Unified Data Model**
  - Combines Olist and Glb sources
  - - Single source of truth for reporting
- **Improced Data Quality:**
  - Deduplication
  - Standardisation
  - Null handling
- **Scalable and Repeatable Pipleines
  - Daily ingestion
  - Handles incremental files
  - Orchestration though fabric pipelines
- **Ready for Analytics**
  - Gold views are structured for Business Intelligence tools like Power BI or Tableau
 
---


**Project Highlights**

 - **Skills Demonstrated:**
   - Data ingestion from multiple sources
   - ETL orchestration in Fabric Pipelines
   - Stored Procedure for Bronze and Silver Layers
   - Data standardisation and quality checks
   - Medallipn architecture implementation
   - Dimenstional modelling for analytics




