# 🏗️ Azure Databricks End-to-End Retail Lakehouse

[![Azure](https://img.shields.io/badge/Azure-0089D6?style=for-the-badge&logo=microsoft-azure&logoColor=white)](https://azure.microsoft.com/)
[![Databricks](https://img.shields.io/badge/Databricks-FF3621?style=for-the-badge&logo=databricks&logoColor=white)](https://databricks.com/)
[![Delta Lake](https://img.shields.io/badge/Delta_Lake-00ADD8?style=for-the-badge&logo=delta&logoColor=white)](https://delta.io/)
[![PySpark](https://img.shields.io/badge/PySpark-E25A1C?style=for-the-badge&logo=apachespark&logoColor=white)](https://spark.apache.org/)
[![Unity Catalog](https://img.shields.io/badge/Unity_Catalog-1B3139?style=for-the-badge&logo=databricks&logoColor=white)](https://www.databricks.com/product/unity-catalog)
[![ADLS Gen2](https://img.shields.io/badge/ADLS_Gen2-0078D4?style=for-the-badge&logo=microsoftazure&logoColor=white)](https://azure.microsoft.com/en-us/products/storage/data-lake-storage/)

An **end-to-end data engineering project** on Azure Databricks, built using the **Medallion Architecture** (Bronze → Silver → Gold). The pipeline ingests raw retail data, refines it incrementally, and delivers **analytics-ready dimension and fact tables** using Delta Lake and Unity Catalog — designed to be **free-tier compatible** while following enterprise-grade design principles.

---

## 📑 Table of Contents

- [Overview](#-overview)
- [Business Use Case](#-business-use-case)
- [Architecture](#-architecture)
- [Tech Stack](#-tech-stack)
- [Data Layers](#-data-layers)
  - [Bronze Layer](#bronze-layer-raw-ingestion)
  - [Silver Layer](#silver-layer-refined-data)
  - [Gold Layer](#-gold-layer-business-ready)
- [Data Modeling](#-data-modeling)
- [Orchestration Strategy](#-orchestration-strategy)
- [Key Skills Demonstrated](#-key-skills-demonstrated)
- [Conclusion](#-conclusion)

---

## 📋 Overview

This repository demonstrates a **production-style Azure Databricks Lakehouse implementation** using industry-standard data engineering practices — from raw ingestion to analytics-ready datasets, implemented through a scalable Medallion Architecture.

## 💼 Business Use Case

A retail analytics platform enabling:

- 📈 Sales and revenue analysis
- 🏷️ Product price and category history tracking
- 👥 Customer-based reporting
- 📅 Time-based trend analysis (monthly / yearly)

---

## 🏛️ Architecture

![Architecture Diagram](https://github.com/user-attachments/assets/27210726-dda7-4d18-b29d-6e5d1f6cdce0)

### Key Characteristics

| Characteristic | Description |
|---|---|
| **Incremental Ingestion** | Powered by Databricks Auto Loader |
| **Parallel Processing** | Silver and Gold layers process in parallel where possible |
| **Append-Only Fact Design** | FactOrders is append-only, no updates required |
| **Dimensional Modeling** | SCD Type 1 & Type 2 strategies applied per business need |

---

## 🛠️ Tech Stack

| Category | Tools |
|---|---|
| **Compute** | Azure Databricks |
| **Storage** | Delta Lake, Azure Data Lake Storage (ADLS Gen2) |
| **Governance** | Unity Catalog |
| **Processing** | PySpark, Spark SQL |
| **Ingestion** | Databricks Auto Loader |

---

## 🗂️ Data Layers

### Bronze Layer (Raw Ingestion)
- Incremental file ingestion using Auto Loader
- Schema inference and append-only storage
- No transformations

### Silver Layer (Refined Data)
- Data cleansing and standardization
- Deduplication by business keys
- Entity-specific transformations

**Tables:** `silver_customers` · `silver_orders` · `silver_products`

### 🥇 Gold Layer (Business-Ready)

**`DimCustomers` — SCD Type 1**
- Stores latest customer attributes
- Updates overwrite existing values
- No historical tracking

**`DimProducts` — SCD Type 2**
- Tracks historical product changes
- Uses effective start/end dates and current flag
- Preserves full change history

**`FactOrders`**
- Grain: one row per product per order
- Append-only transactional fact table
- Stores only business keys and measures
- Dimensions joined at query time

<img width="1170" height="478" alt="Gold layer schema" src="https://github.com/user-attachments/assets/50f6c605-e67b-4280-9f02-ea833909c3be" />

---

## 🔗 Data Modeling

**Star Schema** — fact table at the center, dimensions joined at query time, SCD handling aligned with business requirements.

| Table | Type | Strategy |
|---|---|---|
| `DimCustomers` | Dimension | SCD Type 1 |
| `DimProducts` | Dimension | SCD Type 2 |
| `FactOrders` | Fact | Append-only |

---

## ⚙️ Orchestration Strategy

- Parameterized notebooks
- Bronze must complete before Silver
- Silver must complete before corresponding Gold
- Gold dimensions and fact tables run **in parallel**
- FactOrders does not depend on dimensions at load time

---

## 🎯 Key Skills Demonstrated

`Azure Databricks` · `End-to-End Pipeline Design` · `Medallion Architecture` · `Delta Lake MERGE` · `SCD Type 1 & Type 2` · `Fact Table Modeling` · `Lakehouse Best Practices`

---

## ✅ Conclusion

This project demonstrates a **production-style Azure Databricks Lakehouse implementation** using industry-standard data engineering practices. It showcases the complete lifecycle of data — from raw ingestion to analytics-ready datasets — implemented through a scalable Medallion Architecture.

By combining **Auto Loader–based incremental ingestion**, **Delta Lake transactional guarantees**, **Unity Catalog governance**, and **star schema modeling**, the solution delivers a robust and maintainable analytics platform. The use of **SCD Type 1 for Customers**, **SCD Type 2 for Products**, and an **append-only FactOrders table** reflects real-world business requirements and best practices.

The pipeline is intentionally designed to be **free-tier compatible**, avoiding paid features while still adhering to enterprise-grade design principles — making the project both practically executable and a strong demonstration of modern data engineering concepts, trade-offs, and architectural decisions.
