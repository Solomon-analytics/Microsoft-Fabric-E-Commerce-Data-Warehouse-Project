** Microsoft Fabric: E-Commerce Data Warehouse Project**
---
**Project Overview**

This project demonstrates the design and implementation of a medallion architecture data warehouse for multiple e-commerce platform. The goal is to integrate from multiple systems, clean and transform it into a structured format suitable for business intelligence and analytics purpose.

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




