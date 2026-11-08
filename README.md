# Hotel Booking Data Engineering Project using Databricks

## Project Overview
This project implements an end-to-end data engineering workflow for hotel booking data using Databricks. It follows the Medallion Architecture to organize raw data, clean and validate records, and generate business-ready summary tables for reporting.

## Objectives
- Ingest and organize raw hotel booking data.
- Clean and validate booking records.
- Identify duplicate and invalid records.
- Perform data quality checks and reconciliation.
- Create aggregated Gold tables for business analysis.
- Build interactive dashboards using Databricks AI/BI.

## Architecture
<img width="2474" height="1978" alt="mermaid-diagram" src="https://github.com/user-attachments/assets/9a6b08af-9a09-45a8-8f50-d602a94c1b4a" />

**Bronze → Silver → Gold → Dashboard**

- **Bronze:** Stores the ingested hotel booking data.
- **Silver:** Separates clean booking records from rejected records using validation rules.
- **Gold:** Provides aggregated datasets for hotel, room type, channel, monthly booking, and data quality analysis.
- **Dashboard:** Presents booking KPIs, revenue trends, hotel performance, channel analysis, and data quality metrics.

## Technologies Used
- Databricks
- SQL
- Medallion Architecture
- Databricks AI/BI Dashboards

## Data Quality and Reconciliation
The project includes checks for duplicate booking IDs, invalid dates, negative room rates, and missing values.

Recorded results:
- Bronze rows: 1,316
- Clean booking rows: 1,269
- Rejected rows: 21
- Duplicate excess rows: 26

The row-count reconciliation is:

`1,316 = 1,269 + 21 + 26`

Duplicate excess rows are counted separately from clean and rejected records for raw-row reconciliation.

## Gold Tables
- `gold_channel_summary`
- `gold_data_quality`
- `gold_hotel_summary`
- `gold_monthly_bookings`
- `gold_room_type_summary`

## Dashboard Pages
1. **Overview:** Booking KPIs, revenue trends, and city-level revenue.
2. **Hotels, Rooms & Channels:** Hotel performance, room type analysis, and channel cancellation rates.
3. **Data Quality:** Record counts, duplicate checks, quality issues, and reconciliation metrics.

## Key Learning Outcomes
- Implementing a Medallion Architecture workflow.
- Writing SQL for data transformation and aggregation.
- Applying data validation and duplicate handling.
- Reconciling raw, clean, rejected, and duplicate records.
- Building dashboards for business reporting.

## Project Status
Core data processing, Gold summary tables, data quality checks, reconciliation, and dashboard publication are completed.

