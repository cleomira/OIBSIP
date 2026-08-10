# Task 3: Cleaning Data — Crime Incidents Dataset

## Project Overview
This project demonstrates professional-level data cleaning by taking a deliberately messy crime incidents dataset and systematically transforming it into a clean, analysis-ready dataset. Every cleaning decision is documented and justified.

## Dataset
- **File:** `crime_incidents_messy.csv`
- **Size:** 5,250 rows, 33 columns (before cleaning)
- **Content:** Crime incident records including crime type, location (district, city, state, coordinates), suspect and victim demographics, case status/resolution, officer details, and property loss

## Data Quality Issues Identified
- **Missing values** across nearly every column, ranging from <5% to over 30% (e.g. `suspect_race` at 30.5%)
- **200 fully duplicate rows**, including duplicate `incident_id` values
- **Inconsistent categorical formatting:** `crime_type` had 182 raw variants (typos, casing, whitespace) collapsing to 21 true categories; `district` had 131 raw variants collapsing to 10
- **Mixed scales within a single column:** `severity` combined a numeric scale (1–4) and a word scale (Low/Medium/High/Critical)
- **Impossible/out-of-range values:** ages up to 298 and negative ages; negative arrest counts; negative property loss; coordinates outside valid latitude/longitude bounds
- **Incorrect data types:** IDs stored as numeric, dates stored as text in 3+ different formats, monetary values stored as text

## Cleaning Process

### 1. Standardisation
Text columns were stripped of whitespace, case-normalised, and mapped to canonical categories using explicit mapping dictionaries (e.g. `crime_type`, `district`, gender, `severity`, `case_status`, `resolution`, `weapon_used`). `incident_datetime` was parsed from 3+ mixed formats into a single datetime type. `reported_online` was unified from True/False/yes/no/1/0 into a consistent boolean.

### 2. Missing Data Handling
Each column's missing-data strategy was chosen based on what "missing" actually represents for that field:
- Fields where missingness is a real category (e.g. no suspect/victim identified) were filled with an explicit label rather than imputed
- Numeric fields (age, property loss) used median imputation, grouped by relevant category where appropriate (e.g. property loss imputed by crime type)
- `incident_datetime` was deliberately **left null** rather than imputed, since fabricating a timestamp for a crime record is not defensible

### 3. Duplicate Removal
200 fully duplicate rows were identified and removed. All were confirmed to be exact re-entries of the same `incident_id` with no conflicting data, so no manual resolution was required.

### 4. Outlier Detection
Two different strategies were used depending on the nature of the column:
- **Ages:** Domain-knowledge hard bounds (0–100) were used instead of the IQR method, since extreme errors (e.g. age 298) inflated the IQR itself, producing an unreliable upper bound (127.5) that exceeded biological plausibility
- **Property loss:** Standard IQR method (1.5×IQR beyond Q1/Q3, floored at $0) was used to cap extreme-but-plausible values
- **Coordinates:** Hard geographic bounds (-90 to 90 latitude, -180 to 180 longitude)
- **Arrest counts:** Negative values set to 0

### 5. Data Type Correction
ID columns cast to string, `incident_datetime` cast to datetime, `property_loss_usd` cast to float, age/arrest counts cast to integer, and categorical text columns cast to pandas `category` dtype.

## Before vs. After Summary

| Metric | Before Cleaning | After Cleaning |
|---|---|---|
| Total rows | 5,250 | 5,050 |
| Total duplicate rows | 200 | 0 |
| Total null values | 16,478 | 329 (all in `incident_datetime`, intentionally retained) |
| Correct dtypes | No — IDs numeric, dates as text, monetary as text | Yes — IDs as string, dates as datetime, monetary as float |

**Note:** The 329 remaining nulls in `incident_datetime` were deliberately left unimputed rather than filled with a fabricated date. Any time-based analysis using this dataset should filter out these rows rather than assume a date.

## Tools Used
- Python
- pandas
- numpy
- Jupyter Notebook

## Files in This Folder
- `data_cleaning_crime_incidents.ipynb` — full cleaning notebook with code and written justifications for every decision
- `crime_incidents_messy.csv` — original raw dataset
- `crime_incidents_cleaned.csv` — final cleaned dataset
- `README.md` — this file

## How to Run
1. Clone this repository
2. Install dependencies: `pip install pandas numpy`
3. Open `data_cleaning_crime_incidents.ipynb` in Jupyter Notebook
4. Run all cells (Kernel → Restart Kernel and Run All)
