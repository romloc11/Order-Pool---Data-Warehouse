# Credit Pool Data Warehouse

## Overview

This project builds a **Data Warehouse for Credit and Collections analytics** using SQL Server.
The goal is to analyze the **order credit approval process**, identify operational bottlenecks, and measure credit risk indicators.

The warehouse follows the **Medallion Architecture** pattern to transform operational data into analytical datasets used for reporting and business intelligence.

Key areas of analysis include:

* Orders entering the credit pool
* Order approval and cancellation rates
* Credit risk indicators
* Resolution time of credit decisions
* Operational efficiency of credit analysts

---

## Architecture

The project implements the **Medallion Architecture**:

```
Source System (MySQL / ERP)
        │
        ▼
     Bronze
  (Raw ingestion)
        │
        ▼
     Silver
 (Cleaned & modeled data)
        │
        ▼
      Gold
 (Business KPIs)
```

### Bronze Layer

Stores raw data extracted from the operational system.

Tables:

* `bronze.pedidos_pool_clientes_raw`
* `bronze.estatus_pool`
* `bronze.vendedores`

Purpose:

* Preserve historical data
* Enable traceability
* Decouple analytics from operational systems

---

### Silver Layer

Contains cleaned and standardized data ready for analysis.

Main transformations include:

* Standardized column names
* Credit risk flags
* Order resolution indicators
* Time-to-resolution calculations

Table:

* `silver.pool_pedidos`

Derived fields:

* `bloqueo_sap`
* `saldo_vencido`
* `limite_excedido`
* `indicador_liberado`
* `indicador_cancelado`
* `minutos_liberacion`
* `minutos_cancelacion`

---

### Gold Layer

Aggregated tables optimized for reporting and dashboards.

Table:

* `gold.kpi_pool_credito_diario`

Key metrics:

* Orders entering the credit pool
* Orders released
* Orders cancelled
* Average resolution time
* Credit risk distribution

---

## Data Sources

Operational data is extracted from the following tables:

* `pedidos_pool_clientes`
* `estatus_pool`
* `vendedores`

These tables originate from the transactional ERP system and are accessed through a linked server connection.

---

## Key Business Metrics

### Operational Metrics

* Orders entering the credit pool per day
* Orders released
* Orders cancelled

### Efficiency Metrics

* Average time to release orders
* Average time to cancel orders
* Orders released per user

### Credit Risk Indicators

* % blocked due to SAP restrictions
* % blocked due to overdue balance
* % blocked due to exceeded credit limit

---

## ETL Process

The ETL pipeline performs the following steps:

1. **Extract**

   * Data is pulled from the operational database using `OPENQUERY`.

2. **Load to Bronze**

   * Raw data is stored with minimal transformation.

3. **Transform to Silver**

   * Business logic is applied.
   * Derived metrics are calculated.

4. **Aggregate to Gold**

   * KPIs are calculated for reporting.

---

## Technologies Used

* SQL Server
* SQL
* Linked Servers
* Medallion Architecture
* Data Warehouse Modeling

---

## Project Goal

Provide a **structured analytical environment** to monitor the credit approval workflow and support data-driven decision making in Credit and Collections operations.

---

## Future Improvements

* Automate ETL with scheduled jobs
* Add historical change tracking
* Integrate dashboards in Power BI
* Expand risk analysis models
* Implement SLA monitoring for credit decisions

