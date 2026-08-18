# SQL Data Warehouse & Analytics Project

A end-to-end data warehousing solution built on **SQL Server** using **Medallion Architecture** (Bronze → Silver → Gold). Covers ETL pipeline design, dimensional data modeling, and SQL-based analytical reporting on integrated ERP and CRM datasets.

---

## Architecture

<img width="1539" height="799" alt="data_architecture" src="https://github.com/user-attachments/assets/7445e236-8de4-4814-9e46-6e2a2df460b9" />

The project follows a three-layer Medallion Architecture:

| Layer | Purpose |
|---|---|
| **Bronze** | Raw ingestion — CSV source files loaded as-is into SQL Server |
| **Silver** | Cleansing & standardization — deduplication, null handling, type casting, normalization |
| **Gold** | Business-ready analytics layer — star schema fact and dimension tables for reporting |

---

## Dataset

Source data simulates an enterprise environment with two integrated systems:

- **ERP system** — transactional sales data (orders, products, pricing)
- **CRM system** — customer master data (demographics, segmentation)

Raw CSV files are located in `datasets/`. The Silver layer joins and reconciles these two sources into a unified model.

---

## Data Model

The Gold layer is structured as a **star schema**:

```
        dim_customers
             |
dim_products — fact_sales — dim_date
```

- `fact_sales` — grain: one row per order line item; stores revenue, quantity, and foreign keys
- `dim_customers` — SCD-ready customer attributes from the CRM
- `dim_products` — product hierarchy, category, and cost attributes from the ERP
- `dim_date` — date spine for time-series analysis

Schema diagrams are in `docs/data_models.drawio`.

---

## Project Structure

```
sql-data-warehouse-project/
│
├── datasets/               # Raw source CSV files (ERP + CRM)
│
├── docs/                   # Architecture and design diagrams
│   ├── data_architecture.drawio
│   ├── data_catalog.md     # Field-level metadata for all tables
│   ├── data_flow.drawio
│   ├── data_models.drawio  # Star schema diagram
│   ├── etl.drawio          # ETL method reference
│   └── naming-conventions.md
│
├── scripts/
│   ├── init_database.sql   # Creates the DataWarehouse database
│   ├── bronze/             # Raw ingestion scripts
│   ├── silver/             # Cleansing and transformation scripts
│   └── gold/               # Analytical views and fact/dim tables
│
├── tests/                  # Data quality checks
├── LICENSE
└── README.md
```

---

## How to Run

### Prerequisites
- Microsoft SQL Server 2019 or later
- SQL Server Management Studio (SSMS) or Azure Data Studio

### Steps

**1. Initialize the database**
```sql
-- Run in SSMS connected to your SQL Server instance
scripts/init_database.sql
```

**2. Load the Bronze layer**
```sql
-- Bulk-loads raw CSV files into staging tables
scripts/bronze/
```

**3. Apply Silver transformations**
```sql
-- Cleanses, standardizes, and joins ERP + CRM data
scripts/silver/
```

**4. Build the Gold layer**
```sql
-- Creates the star schema views and reporting tables
scripts/gold/
```

**5. Run data quality tests**
```sql
-- Validates row counts, null checks, referential integrity
tests/
```

Execute scripts within each folder in alphabetical order.

---

## Analytics Queries

The Gold layer supports the following reporting use cases:

- **Sales performance** — revenue by product, category, and time period
- **Customer segmentation** — purchase behavior and demographic breakdowns
- **Cumulative trends** — month-over-month and year-over-year comparisons
- **Part-to-whole analysis** — product/category contribution to total revenue

Sample queries are included in `scripts/gold/`.

---

## Skills Demonstrated

- Medallion Architecture (Bronze / Silver / Gold)
- ETL pipeline design with SQL Server
- Dimensional modeling — star schema, fact and dimension tables
- Data cleansing — deduplication, null handling, type normalization
- Data quality testing with SQL assertions
- Technical documentation and data cataloging

---

## License

MIT — see [LICENSE](LICENSE) for details.
