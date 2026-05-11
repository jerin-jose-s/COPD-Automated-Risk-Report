# COPD Automated Clinical Data Pipeline

## Executive Summary
This pipeline automatically processes raw COPD patient data to detect data quality issues and identify patients requiring clinical attention. Using Python, SQL and the Gemini AI API, the pipeline ingests raw device data, runs automated quality checks across 4 critical dimensions, performs group-based anomaly detection and generates a professional HTML clinical report — all with a single function call.

Out of 101 patients monitored, **13 were flagged for clinical review** and **1 identified as critical priority.** The pipeline is built with clinical safety as the core principle — no patient data is ever silently removed.

---

## Report Preview

![Data Quality Alerts](images/report_screenshot.png)
![Priority Patients](images/report_screenshot1.png)

---

## Business Problem
In a clinical setting, device data from COPD patients arrives continuously via connected devices. Clinical teams need to quickly identify:
- Which patients have unusual readings that require attention
- Whether the incoming data has quality issues that could affect clinical decisions
- Who needs to be seen first

Without automation, this requires a data analyst to manually check data every day — which is time consuming, inconsistent and prone to human error. This pipeline solves that by automating the entire process from raw data to actionable clinical report.

---

## Methodology
1. **Raw data ingestion** — loaded into a SQLite database without modification
2. **Automated data quality checks** — 4 checks run automatically:
   - Duplicate patient ID detection
   - Missing critical clinical values (FEV1, FVC, CAT, MWT columns, AGE, gender)
   - Data entry error detection (CAT scores outside valid range 0-40)
   - Misaligned row detection (HAD scores outside valid range 0-42)
3. **Safe auto-fixing** — MWT1Best filled from available MWT1 or MWT2 where possible. All other issues flagged only — never silently changed
4. **Group-based anomaly detection** — patients flagged if readings fall outside 2 standard deviations of their severity group mean
5. **Automated HTML report generation** — includes data quality alerts, KPI summary, anomaly breakdown and AI narrative summary

---

## Skills
- **Python** — pandas, sqlite3, datetime, os
- **SQL** — complex queries for data quality checks, aggregations, filtering
- **SQLite** — relational database setup and management
- **Statistical analysis** — group-based standard deviation thresholds
- **AI integration** — Gemini API for automated narrative generation
- **Security** — API key management using environment variables

---

## Results & Business Recommendation
Out of 101 patients monitored:
- **13 patients flagged** for clinical review (13% of total)
- **1 critical patient** identified (ID 108 — 3 anomalies across FEV1, MWT1Best and CAT)
- **4 data quality issues** detected automatically — including duplicate IDs which pose a patient safety risk
- **MWT1Best** had the most anomalies (7) — likely explained by musculoskeletal comorbidities affecting walk test performance independently of lung function

**Recommendation:** Patient ID 108 should be reviewed immediately — their readings are inconsistent with their SEVERE classification and may indicate either misclassification or data recording issues. The 4 duplicate patient IDs should be investigated as a priority as these represent a patient safety risk in a clinical environment.

---

## How to Run

1. Clone this repository
2. Install required libraries
3. Add your Gemini API key
4. Place your COPD dataset (CSV) in the project folder
5. Run the notebook top to bottom
6. Open copd_pipeline_report.html in your browser to view the report

---

## Next Steps
- **Schedule automation** — deploy as a daily cron job or cloud scheduler so reports run automatically without manual intervention
- **Expand anomaly detection** — include HAD and SGRQ scores as additional measures
- **Trend analysis** — detect deterioration over time rather than just point-in-time anomalies
- **Integrate with live database** — connect directly to production device database rather than CSV ingestion
- **Alert system** — automatically email reports to clinical team rather than saving locally
