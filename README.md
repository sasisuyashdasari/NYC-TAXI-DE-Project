# NYC-TAXI-DE-Project
# 🚕 NYC Taxi Data Engineering Project

## Overview

This project demonstrates an end-to-end Azure Data Engineering pipeline using the NYC Taxi dataset.

The pipeline follows the **Medallion Architecture (Bronze → Silver → Gold)** and uses Azure services for data ingestion, storage, transformation, and analytics.

The goal of this project is to build a scalable and production-style ETL pipeline that ingests raw NYC Taxi trip data, transforms it into clean and analytics-ready datasets, and stores them in Delta format for downstream consumption.

---

## Architecture

```text
NYC Taxi Dataset
        │
        ▼
Azure Data Factory
(Ingestion Pipelines)
        │
        ▼
Azure Data Lake Storage Gen2

Bronze Layer
(Raw CSV Files)
        │
        ▼

Azure Databricks

Silver Layer
(Cleansed & Transformed Parquet Data)
        │
        ▼

Gold Layer
(Delta Tables / Business Ready Data)
        │
        ▼

Power BI
(Dashboard & Analytics)
```

---

## Tech Stack

| Service                      | Purpose                        |
| ---------------------------- | ------------------------------ |
| Azure Data Factory           | Data Ingestion & Orchestration |
| Azure Data Lake Storage Gen2 | Storage Layer                  |
| Azure Databricks             | Data Transformation            |
| PySpark                      | Data Processing                |
| Delta Lake                   | Gold Layer Storage             |
| GitHub                       | Version Control                |
|                              |                                |

---

# Project Structure

```text
NYC-TAXI-DE-Project/

├── adf/
│   ├── dataset/
│   ├── linkedService/
│   ├── pipeline/
│   ├── trigger/
│   └── factory/

├── notebooks/
│   ├── nyc-taxi-de-silver_notebook.ipynb
│   └── nyc-taxi-de-gold_notebook.ipynb
├── README.md
```

---

# Dataset

The project uses the NYC Taxi Trip dataset.

The dataset contains:

* Trip pickup and dropoff locations
* Passenger counts
* Fare amounts
* Payment types
* Trip distances
* Taxi zones
* Trip types

The raw data is stored as CSV files in the Bronze container.

---

# Data Lake Architecture

The storage account is divided into three layers:

## Bronze Layer

Purpose:

* Store raw files
* Preserve source data
* No transformations

Format:

```text
CSV
```

Contents:

* Trip Data
* Trip Type Data
* Taxi Zone Data

---

## Silver Layer

Purpose:

* Data cleansing
* Type casting
* Remove duplicates
* Standardize columns

Format:

```text
Parquet
```

Transformations:

* Schema inference
* Column formatting
* Null handling
* Data validation
* Writing optimized parquet files

The Silver notebook reads data from:

```python
abfss://bronze@<storage>.dfs.core.windows.net/
```

and writes transformed datasets to:

```python
abfss://silver@<storage>.dfs.core.windows.net/
```

---

## Gold Layer

Purpose:

Create analytics-ready datasets.

Format:

```text
Delta Tables
```

Operations:

* Read Silver Parquet data
* Create Delta tables
* Store curated business datasets
* Optimize for BI consumption

The Gold notebook:

* Creates Gold database

```sql
CREATE DATABASE gold
```

* Reads Silver data

```python
silver = "abfss://silver@<storage>.dfs.core.windows.net"
```

* Writes Delta tables

```python
.saveAsTable('gold.trip_zone')
```

---

# Azure Data Factory

ADF is used as the orchestration and ingestion layer.

Responsibilities:

* Connect to source datasets
* Configure linked services
* Create datasets
* Build ingestion pipelines
* Trigger data movement
* Load files into ADLS Gen2 Bronze layer

ADF assets stored in GitHub:

```text
adf/

dataset/
linkedService/
pipeline/
trigger/
factory/
```

The repository is integrated with GitHub for version control.

Every pipeline update is committed and tracked through Git.

---

# Databricks Notebooks

## Silver Notebook

Responsibilities:

* Connect to ADLS Gen2
* Read Bronze CSV files
* Clean and transform data
* Convert into Parquet format
* Write to Silver layer

Libraries:

```python
from pyspark.sql.functions import *
from pyspark.sql.types import *
```

Storage access:

```python
spark.conf.set(...)
```

Data written to:

```text
Silver Container
```

---

## Gold Notebook

Responsibilities:

* Create Gold Database
* Read Silver datasets
* Write Delta Tables
* Prepare data for analytics

Storage:

```python
silver = 'abfss://silver@<storage>.dfs.core.windows.net'

gold = 'abfss://gold@<storage>.dfs.core.windows.net'
```

Output:

```text
Gold Delta Tables
```

---

# Future Improvements

* Add CI/CD using GitHub Actions
* Deploy ADF using ARM templates
* Add Delta Live Tables
* Implement Unity Catalog
* Add data quality checks
* Parameterize pipelines
* Add monitoring and alerts

---

#
