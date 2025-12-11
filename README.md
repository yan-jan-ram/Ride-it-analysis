![Status](https://img.shields.io/badge/Status-Complete-brightgreen)
![Built with - Power BI](https://img.shields.io/badge/Built%20with-Power%20BI-teal?logo=powerbi)
![ETL - Excel](https://img.shields.io/badge/ETL-Excel-orange?logo=microsoft-excel)
![DAX](https://img.shields.io/badge/Measures-DAX-blueviolet)
![Dataset](https://img.shields.io/badge/Dataset-RideIt-lightgrey)

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

## Tech stack
- Power BI Desktop (data model, DAX, visuals)
- Microsoft Excel (initial ETL/merge)
- DAX for measures & time-intelligence
- GitHub — documentation & screenshots

---

## What’s included in this repo
- `screenshots/` — dashboard & model images (visual proof; upload these)
- `docs/` (optional) — metric definitions & notes
- `README.md` (this file)

> **Note:** The original CSVs (`rideit_drivers.csv`, `rideit_drivers_activity.csv`) are confidential and therefore not stored in this public repo. Instead, screenshots and metric definitions are provided.

---

## Key dashboards / screenshots
Place the following screenshots inside `screenshots/` (recommended filenames shown). Then these images will render automatically.

```md
![Analytics dashboard](screenshots/analytics-dashboard.png)
![Overview KPIs](screenshots/rideit-kpis.png)
![Offers vs Bookings trend](screenshots/offers-vs-bookings.png)
![Drivers MoM change](screenshots/drivers-mom-change.png)
![Model view and measures](screenshots/model-view.png)
![Performance dashboard](screenshots/performance-dashboard.png)
![Tabular / Top offers table](screenshots/tabular-dashboard.png)
![Top 10 drivers](screenshots/top-10-drivers.png)
