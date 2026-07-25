# Bike Lakehouse Project (Databricks + PySpark)

A Medallion-architecture (Bronze → Silver → Gold) data lakehouse built on **Databricks**, using **PySpark** and **Delta Lake**, modeling CRM and ERP source data into a dimensional star schema for analytics.

This project is based on the well-known [Data with Baraa SQL Data Warehouse Project](https://www.youtube.com/@DataWithBaraa), originally built in T-SQL/SQL Server. I rebuilt the pipeline on **Databricks using PySpark and Delta tables** instead, as a way to learn the Databricks Lakehouse platform, schema inference, and Spark-native data transformations.

## Architecture

```
Bronze (raw ingest)        Silver (cleaned)              Gold (business-ready)
────────────────────       ───────────────────           ─────────────────────
CRM: cust_info        →    crm_customers            →    dim_customers
CRM: prd_info          →    crm_prd_info              →    dim_products
CRM: sales_details      →   crm_sales_details        →    fact_sales
ERP: CUST_AZ12          →   erp_cust_az12
ERP: LOC_A101           →   erp_cust_location
ERP: PX_CAT_G1V2         →  erp_product_categories
```

- **Bronze**: Raw CSVs from CRM and ERP source systems, loaded 1:1 into Delta tables with schema inference — no transformation, just a durable raw copy.
- **Silver**: Cleaned and standardized data — whitespace trimming, coded values mapped to readable business terms (e.g. `S`/`M` → `Single`/`Married`), cryptic source column names renamed to business-friendly names.
- **Gold**: Dimensional model (star schema) — `dim_customers`, `dim_products`, and a sales fact table — ready for BI/analytics consumption.

## Tech stack

- Databricks (Delta Lake, Unity Catalog volumes)
- PySpark (DataFrame API, schema inference, `withColumn`, `when`/`otherwise` transformations)
- Delta tables as the storage format across all three layers

## What I learned / built by hand

- Translating a T-SQL-based warehouse design into PySpark DataFrame transformations
- Working with Databricks Volumes for raw file storage and Unity Catalog for table management
- Applying Medallion architecture layer-by-layer with Delta table writes (`saveAsTable`, `mode("overwrite")`)
- Standardizing and renaming messy source-system columns into a clean, readable schema

## Notes on originality

The dataset, table structure, and overall pipeline design come from a public tutorial (credited above). This repo represents my **implementation on a different platform** (Databricks/PySpark vs. the original SQL Server/T-SQL), which I used as a learning exercise for the Databricks ecosystem. It's part of my data engineering portfolio for an end-to-end pipeline built from my own data source and design decisions.

## Repo structure

```
Bike_Lakehouse_Project/
├── Bronze Layer.ipynb
├── Silver.crm_cust_info.ipynb
├── Silver.crm_prd_info.ipynb
├── Silver.crm_sales_details.ipynb
├── Silver.erp_cust_az12.ipynb
├── Silver.erp_cust_location.ipynb
├── Silver.erp_product_categories.ipynb
├── Gold.dim_customers.ipynb
├── Gold.dim_products.ipynb
└── Gold_dim_sales.ipynb
```
