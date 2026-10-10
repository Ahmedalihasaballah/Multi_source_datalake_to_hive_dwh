# Multi-Source Data Lake to Hive Warehouse Pipeline

An end-to-end data engineering project that consolidates data from multiple source systems and file formats into an HDFS data lake, processes it with PySpark, and builds a dimensional data warehouse in Apache Hive.

## Project Overview

This project demonstrates a batch data pipeline that brings together point-of-sale (POS), e-commerce, and CRM data. It standardizes and cleans the data, unifies sales records into a common schema, and organizes the results into a star-schema warehouse for analytics.

## Architecture

```text
POS Source (CSV) ─────────┐
E-commerce Source (Parquet) ─┼──> HDFS Landing Zone
CRM Source (CSV + Parquet) ──┘          |
                                        v
                              PySpark Processing
                         (cleaning and standardization)
                                        |
                                        v
                                Hive Staging Tables
                                        |
                                        v
                              Dimensional Modeling
                                        |
                                        v
                              Hive Data Warehouse
                                  (Star Schema)
```

## Technology Stack

- **Linux** — execution environment
- **Apache Hadoop HDFS** — distributed storage and landing zone
- **Apache Spark / PySpark** — data cleaning, transformation, and integration
- **Apache Hive** — staging tables and dimensional warehouse
- **Python** — pipeline and transformation logic
- **Bash** — pipeline execution scripts

## Data Sources

| Source directory | Format | Data |
|---|---|---|
| `source_1_pos/` | CSV | Store point-of-sale transactions |
| `source_2_ecommerce/` | Parquet | Online order lines |
| `source_3_crm/` | CSV and Parquet | Customer and product master data |

The pipeline consolidates source files into the HDFS landing directory:

```text
/lake/landing
```

## Pipeline Stages

1. **Landing:** Consolidate files from the three source directories into HDFS.
2. **Staging:** Use PySpark to clean, standardize, and integrate source data, then load the results into Hive staging tables.
3. **Warehouse:** Build the dimensional model in the Hive `dw` database, including dimensions and the sales fact table.

## Data Quality and Transformation

The processing layer performs the following operations:

- Trims whitespace and standardizes text fields, including SKU, names, emails, cities, and payment methods.
- Casts columns to appropriate data types.
- Removes duplicate records.
- Rejects invalid records with non-positive quantities or prices, or missing dates or SKUs.
- Matches guest e-commerce orders to CRM customers using email.
- Unifies POS and e-commerce records into a common sales schema.

## Data Warehouse Design

The warehouse follows a **star schema**.

### Dimension tables

| Table | Purpose |
|---|---|
| `dim_date` | Date-related attributes for sales analysis |
| `dim_customer` | Customer details |
| `dim_product` | Product details |
| `dim_store` | Store details |
| `dim_channel` | Sales channel, such as POS or e-commerce |

### Fact table

| Table | Purpose |
|---|---|
| `fact_sales` | Sales transactions and measurable business outcomes |

The `fact_sales` table is partitioned by `sale_year`.

### Measures

- `quantity`
- `unit_price`
- `gross_amount`
- `discount_amount`
- `net_amount`
- `cost_amount`
- `margin_amount`

This model supports analysis by date, customer, product, store, and sales channel.

## Project Structure

```text
.
├── config/          # Pipeline settings
├── scripts/         # Shell and Python execution scripts
├── spark_jobs/      # PySpark processing jobs
├── sql/             # Hive SQL scripts
├── docs/             # Architecture and schema documentation
└── data/             # Source data directories
```

## How the Pipeline Works

At a high level, the pipeline:

1. Reads source files from the configured local directories.
2. Places the files in the HDFS landing zone.
3. Runs PySpark transformations to validate, clean, and combine the data.
4. Loads the transformed data into Hive staging tables.
5. Builds the dimensional tables and partitioned sales fact table in the Hive `dw` database.

> **Note:** Adapt the commands and configuration to your Hadoop, Spark, and Hive installation. Exact run commands depend on the scripts and configuration included in the repository.

## Key Data Engineering Concepts Demonstrated

- Multi-source data ingestion
- Working with CSV and Parquet data
- HDFS data lake organization
- Batch ETL processing with PySpark
- Data cleaning and validation
- Cross-source data integration
- Hive staging and warehouse layers
- Dimensional modeling and star schemas
- Partitioning a fact table
- Bash-based pipeline orchestration

## Future Improvements

- Add automated data quality reports and pipeline-level logging.
- Introduce incremental processing instead of full reloads.
- Add orchestration and scheduling with Apache Airflow.
- Add tests for transformation logic and warehouse constraints.
- Add a BI dashboard for sales and margin analysis.

## Author

**Ahmed Ali**

Data Engineering | Hadoop | PySpark | Hive | SQL | Python
