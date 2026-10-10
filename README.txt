# Multi-Source Data Lake to Hive Warehouse Pipeline - 2026

Hive pipeline that unifies multi-format source files into a Hive dimensional warehouse.

- Consolidated CSV and Parquet files from 3 source directories into a unified HDFS landing directory on a Linux-based Hadoop environment.
- Cleaned and transformed the data with PySpark jobs, loaded it into Hive and modeled it into a star-schema warehouse.

## Tech Stack
Linux, Hadoop (HDFS), Hive, PySpark, Bash, Python

## Data Sources
| # | Directory | Format | Content |
|---|-----------|--------|---------|
| 1 | source_1_pos | CSV | Store POS transactions |
| 2 | source_2_ecommerce | Parquet | Online order lines |
| 3 | source_3_crm | CSV + Parquet | Customers and products master data |

## Pipeline Stages
1. Landing - files from the 3 sources are consolidated into /lake/landing on HDFS
2. Staging - PySpark cleans, standardizes and unifies the data into Hive staging tables
3. Warehouse - PySpark builds the star schema in the Hive dw database

## Data Cleaning
- Trim and standardize text fields (SKU, names, emails, cities, payment methods)
- Cast columns to proper data types
- Remove duplicate records
- Reject invalid rows (quantity or price <= 0, missing date or SKU)
- Match guest web orders to CRM customers by email
- Unify POS and e-commerce data into one sales schema

## Star Schema
- dim_date
- dim_customer
- dim_product
- dim_store
- dim_channel
- fact_sales (partitioned by sale_year)

Measures: quantity, unit_price, gross_amount, discount_amount, net_amount, cost_amount, margin_amount

## Project Structure
- config/ - pipeline settings
- scripts/ - shell and Python scripts for each step
- spark_jobs/ - PySpark jobs
- sql/ - Hive SQL files
- docs/ - architecture and schema reference
- data/ - source directories
