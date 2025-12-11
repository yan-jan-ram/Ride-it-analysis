![Status](https://img.shields.io/badge/Status-Complete-brightgreen)
![Built with - Power BI](https://img.shields.io/badge/Built%20with-Power%20BI-teal?logo=powerbi)
![ETL - Excel](https://img.shields.io/badge/ETL-Excel-orange?logo=microsoft-excel)
![DAX](https://img.shields.io/badge/Measures-DAX-blueviolet)
![Dataset](https://img.shields.io/badge/Dataset-RideIt-darkblue)

# Ride-it analysis — Driver engagement & operational metrics

**Repository:** `https://github.com/yan-jan-ram/Ride-it-analysis`  
**Short summary:** Power BI project analysing driver engagement and operations using two primary datasets (driver master + driver activity). Excel used for initial cleaning, Power BI for model, DAX measures, dashboards and reporting. CSVs are **not** included for security reasons.

---

## Business objective
Help operations and product teams monitor driver engagement and operational health:
- Measure offers → bookings → rides funnel and conversion ratios.
- Track driver activity (active drivers, MoM changes).
- Monitor cancellation rates and surface outliers (offers/rides).
- Provide operational KPIs (total rides, total bookings, completion %, cancellations).

---

## 🛠 Tech Stack
| Component | Usage |
|------|---------|
| Power BI Desktop | data model, DAX, visuals |
| Microsoft Excel | initial ETL/merge |
| DAX | measures & time-intelligence |
| GitHub | documentation & screenshots |

---

## 🗂️ Data Model

The Power BI model uses **1-to-many relationships** with `show_id` as the key:
```
model-view
│── ride-it drivers (main table)
│── ride-it drivers activity
```

> This star-schema–like structure supports **clean filtering** across all dashboard visuals.

![Model View](screenshots/model-view.png)

## What’s included in this repo
- `screenshots/` — dashboard & model images
- `README.md` (this file)

> **Note:** The original CSVs (`rideit_drivers.csv`, `rideit_drivers_activity.csv`) are confidential and therefore not stored in this public repo. Instead, screenshots and metric definitions are provided.

---
## 🔍 Key Insights & Findings
### 1. Driver Activity & Engagement

- Active drivers drop significantly in April, followed by a strong recovery in May (≈ +47% MoM) — indicating seasonal, operational, or incentive-related effects.
- Only a small percentage of drivers maintain consistent monthly activity, suggesting the platform may need engagement campaigns or improved retention measures.
- Drivers with higher Gold-level ranks contribute disproportionately to total rides, showing correlation between loyalty tiers and higher productivity.

### 2. Offers → Bookings → Rides Funnel

- Conversion efficiency:
  - Offers → Bookings: ≈ 30%
  - Bookings → Rides (Completion %): ≈ 82%

- The largest loss in the funnel occurs between offers and bookings, meaning drivers may be receiving many low-quality, irrelevant, or poorly timed offers.
- Completion rate remains healthy across months but varies slightly with driver activity fluctuations.

### 3. Cancellations — Drivers vs Passengers

- Passenger cancellations are consistently higher than driver cancellations across all months.
- Driver cancellations spike on months with low ride volume (April), suggesting:

 - Low incentive periods,

 - Operational disruptions,

 - Misalignment between demand and supply.

### 4. Service Type Performance (TAXI vs PHV)

- TAXI drivers generate the majority of bookings and rides, significantly outperforming PHV in both volume and stability.
- PHV shows more volatility, indicating:

 - Market changes,
 - Driver churn,
 - Less predictable demand cycles.

### 5. Geographic Trends

- DE region dominates ride and booking volume, while ES shows smaller but consistent growth.
- Cancellation behaviour differs by country:

 - Germany (DE) has higher passenger cancellations.
 - Spain (ES) shows balanced cancellation patterns.

### 6. Marketing Insights

- Drivers who receive marketing communications (push/SMS/email) show:

 - Higher booking volume,
 - Higher retention,
 - Slightly higher completion %
 - suggesting marketing touchpoints positively influence engagement.

### 7. Outlier Detection (IQR + Z-score)

- Ride and offer outliers were successfully flagged:
- High-volume outliers usually correspond to long-tenure, highly active, multi-service drivers.
- Low outliers often align with:

 - New drivers,
 - Inactive periods,
 - Incorrect registration/activation dates (data quality issue).

### 8. Time-Based Behaviour

- Friday and Saturday have the highest ride volume (939K and 914K respectively).
- Monday is the lowest performing day, consistent with expected commuter and leisure patterns.
- Average bookings, offers, and rides trend upward from January → March, drop sharply in April, and recover in May & June.

### 9. Driver Experience

- Using the custom DAX driver experience calculation, most high-performing drivers:

 - Have longer tenure,
 - Are overwhelmingly TAXI drivers,
 - shows a direct positive relationship between experience and ride volume.

### 10. Top Driver Analysis

- Top 10 drivers contribute highly disproportionate ride and booking volume, confirming a power-law distribution.

- Drivers with both TAXI + PHV services perform significantly better overall.

## DAX Measures

#### Active drivers in last month (>=1 ride)
```
Active drivers in last month(>1 ride) =
CALCULATE(
    DISTINCTCOUNT(rideit_drivers_activity[id_driver]),
    FILTER(rideit_drivers_activity, rideit_drivers_activity[rides] >= 1),
    DATESINPERIOD(rideit_drivers_activity[active_date],
                  MAX(rideit_drivers_activity[active_date]),
                  -30, DAY)
)
```
#### Bookings to Rides ratio
```
Bookings to Rides =
DIVIDE([Total rides], [Total bookings])
```
#### Cancellation Rate (overall)
```
Cancellation Rate =
1 - ([Total rides] / [Total bookings])
```
#### MoM% pattern (example for bookings)
```
bookings MoM% =
IF(
    ISFILTERED('rideit_drivers_activity'[active_date]),
    ERROR("Time intelligence quick measures can only be grouped or filtered by the Power BI date hierarchy."),
    VAR __PREV_MONTH =
        CALCULATE(
            SUM('rideit_drivers_activity'[bookings]),
            DATEADD(calenderTable[Date], -1, MONTH)
        )
    RETURN DIVIDE(SUM('rideit_drivers_activity'[bookings]) - __PREV_MONTH, __PREV_MONTH)
)
```
#### IQR outlier flag for offers (global context)
```
IQR Outlier Flag(offers) =
VAR CurrentValue = SELECTEDVALUE(rideit_drivers_activity[offers])
VAR Q1 = CALCULATE(PERCENTILE.INC(rideit_drivers_activity[offers], 0.25), ALL(rideit_drivers_activity))
VAR Q3 = CALCULATE(PERCENTILE.INC(rideit_drivers_activity[offers], 0.75), ALL(rideit_drivers_activity))
VAR IQR = Q3 - Q1
VAR LowerBound = Q1 - 1.5 * IQR
VAR UpperBound = Q3 + 1.5 * IQR
RETURN
IF(
    ISBLANK(CurrentValue),
    BLANK(),
    IF(CurrentValue < LowerBound || CurrentValue > UpperBound, "Outlier", "Normal")
)
```
## Key dashboards / screenshots

## 1. Dashboard
![Dashboard](screenshots/dashboard.png)

## 2. Analytics dashboard-1
![Analytics dashboard](screenshots/analytics-dashboard.png)

## 3. Analytics dashboard-2
![Tabular / Top offers table](screenshots/tabular-dashboard.png)

## 4. Performance dashboard
![Performance dashboard](screenshots/performance-dashboard.png)

## 5. Offers vs Bookings
![Offers vs Bookings trend](screenshots/offers-vs-bookings.png)

## 6. Drivers Month-Over-Month
![Drivers MoM change](screenshots/drivers-MoM-change.png)

## 7. Rides Funnel Chart
![Rides_Funnel_Chart](screenshots/rides-funnel-chart.png)

## 8. Average rides by month
![Average Rides by Month](screenshots/avg-rides-by-month.png)

## 9. Top 10 drivers
![Top 10 drivers](screenshots/top-10-drivers.png)

---

## 📁 Project Structure
```
Ride-it-analysis/
├─ Ride-it PBIX
├─ screenshots/                                  
├─ README.md                   
```

