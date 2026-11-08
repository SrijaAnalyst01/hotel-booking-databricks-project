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

## Business Questions
### 1. Why do we keep the raw data in Bronze instead of cleaning the file directly?

     We keep the original data in the Bronze layer so that we preserve the source data for auditing, troubleshooting, and reprocessing. If we clean the original file directly, we may lose information or make it difficult to trace errors.
     
  - Bronze: Raw ingested data.
  - Silver: Cleaned and validated data.
  - Gold: Business-ready tables for analysis and dashboards.
    
### 2. Why is a cancelled booking kept, but a booking with check-out before check-in rejected?

     A cancelled booking is still a valid business record. Keeping it allows us to analyze cancellation rates and booking behavior.
However, a booking with check-out before check-in has an invalid date sequence. It is rejected because its stay duration cannot be validly calculated.

### 3. Why can booking_id find duplicates but guest_id cannot?

     booking_id identifies an individual booking, so duplicate booking IDs can indicate duplicate records.
guest_id identifies a guest, not an individual booking. The same guest may make several legitimate bookings, so repeated guest IDs do not necessarily mean duplicate records.

### 4. How does DATEDIFF fill a blank nights value, and what happens if you swap its arguments?

     In Databricks SQL, DATEDIFF(endDate, startDate) calculates the number of days between two dates.

For example:
DATEDIFF(check_out, check_in)

If check-in is November 5 and check-out is November 8, the result is 3 nights.

If you swap the arguments:
DATEDIFF(check_in, check_out)

The result is -3, which is incorrect for the booking's stay duration. We use the difference to fill a blank nights value when both dates are valid.

### 5. Why do we leave a blank rating as NULL instead of replacing it with 0?

     NULL means the rating is missing or was not provided. A rating of 0 is an actual numeric value and could be interpreted as a real rating.
Keeping missing ratings as NULL avoids misleading averages and preserves the difference between missing data and a genuine value.
     
### 6. What is the average daily rate (ADR), and why is it calculated as net revenue ÷ room nights?

     ADR measures the average revenue earned per occupied room night.
     
\[
\text{ADR}=\frac{\text{Net Revenue}}{\text{Room Nights}}
\]

We divide net revenue by room nights to understand the average revenue earned for each room night, rather than simply the average value of a booking.
In this project, cancelled bookings are excluded from revenue and room-night calculations, so they do not inflate the ADR.

### 7. Which booking channel has the highest cancellation rate?
     The exact channel depends on the results in your Gold tables.

SELECT
    channel,
    COUNT(*) AS total_bookings,
    SUM(
        CASE
            WHEN booking_status = 'Cancelled' THEN 1
            ELSE 0
        END
    ) AS cancelled_bookings,
    ROUND(
        100.0 * SUM(
            CASE
                WHEN booking_status = 'Cancelled' THEN 1
                ELSE 0
            END
        ) / COUNT(*),
        2
    ) AS cancellation_rate_pct
FROM silver_clean
GROUP BY channel
ORDER BY cancellation_rate_pct DESC;

This calculates the cancellation rate as cancelled bookings divided by total bookings for each channel. Using the query result to name the channel with the highest rate.

### 8. Which dashboard tool did you choose, and why?

  I chose Databricks AI/BI Dashboards because the project data is already stored and processed in Databricks. The dashboards can query the Gold tables directly, allowing me to present business insights without exporting the data to another tool.

I created three dashboard pages:
- Overview: Overall booking performance and key metrics.
- Hotels, Rooms & Channels: Comparisons across hotels, room types, and booking channels.
- Data Quality: Data-quality metrics and reconciliation results.
This keeps the data processing, SQL analysis, and dashboard reporting within the same platform.

### Overview Dashboard
<img width="1144" height="544" alt="image" src="https://github.com/user-attachments/assets/545164c7-6355-418a-8d9c-22d6e68f36cc" />

### Hotels, Rooms & Channels Dashboard
<img width="1140" height="548" alt="image" src="https://github.com/user-attachments/assets/bde65765-4097-44dc-add2-f0898a654d5a" />

### Data Quality
<img width="1145" height="549" alt="image" src="https://github.com/user-attachments/assets/2254cbe6-c807-4819-90c1-23130e7207b3" />


