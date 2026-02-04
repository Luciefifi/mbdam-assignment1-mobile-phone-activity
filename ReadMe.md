# Assignment 1: Mobile Communication Activity Analysis (Italy, Nov 2013)

## Dataset Description

The mobile phone activity dataset is composed of one week of Call Detail Records (CDRs) from the city of Milan and the Province of Trentino (Italy). In this assignment, we use the Milan activity files for specific dates (2013-11-02, 2013-11-04, 2013-11-06). Each row represents aggregated activity for a given time window, grid cell, and country code.

## Data Access
The raw dataset is not included in this repository due to size constraints.
To reproduce the results, download the dataset from Kaggle and place the CSV
files in a local `data/` directory.

### Main Columns (from the raw files)

- `datetime`: Timestamp of the observation
- `CellID`: Grid square identifier (a grid square is identified by CellID)
- `countrycode`: Country calling code associated with the communication (Italy is `39`)
- `smsin`: Incoming SMS activity
- `smsout`: Outgoing SMS activity
- `callin`: Incoming call activity
- `callout`: Outgoing call activity
- `internet`: Internet traffic/activity measure

### Derived Columns (created during processing)

- `date`: Date extracted from `datetime`
- `hour`: Hour extracted from `datetime`
- `total_calls`: `callin + callout`
- `total_sms`: `smsin + smsout`
- `total_activity`: `smsin + smsout + callin + callout + internet`

---

## Overview of the Approach

This project analyzes mobile communication activity data from Milan, focusing on SMS, call, and internet usage across grid cells and hours of the day. The main goal was to understand how communication activity changes over time, how domestic and international communication differ, and how different activity types relate to each other.

Three daily datasets (November 2, 4, and 6, 2013) were loaded and combined into a single dataset. After preparing a clean dataset, exploratory analysis and statistical comparisons were performed to answer the assignment questions.

The analysis was conducted using Python, with Pandas for data manipulation, NumPy for statistical computations, and Matplotlib for visualization.

---

## Key Decisions Made

### Data Preparation and Merging
The three datasets were concatenated into a single DataFrame to allow analysis across multiple days. The `ignore_index=True` option was used to ensure a clean and continuous index.

### Time Handling
The `datetime` column was converted to a proper datetime format. From this column, `date` and `hour` were extracted. This enabled consistent grouping and comparison of activity by hour across all grid cells.

### Handling Missing Values
Several activity columns (`smsin`, `smsout`, `callin`, `callout`, `internet`) contained missing values. Instead of dropping rows and losing a large portion of the data, missing values were filled using the mean of each column. The number of modified values/records was tracked to maintain transparency.

The missing values analysis `(Question 4)` was addressed before answering Questions` 1, 2, and 3` because understanding data quality is a prerequisite for reliable analysis. Identifying the presence, extent, and distribution of missing values early ensures that subsequent results, such as record counts, unique grid squares, and country codes, are interpreted with full awareness of the dataset’s completeness. This ordering reflects a standard data analysis workflow, where data quality checks are performed before descriptive statistics and comparisons.


### Domestic vs International Classification
Country code `39` was treated as domestic (Italy), while all other country codes were classified as international. This definition was applied consistently across all comparisons.

When splitting the dataset into domestic and international subsets, explicit copies were created to avoid chained assignment issues and ensure safe, independent analysis.


### Statistical Analysis
NumPy was used for numerical aggregation, ratio calculations (incoming vs outgoing), and correlation analysis. This supported clear statistical comparisons between conditions.

---

## Summary of Key Findings

### Dataset Overview
- The combined dataset contains **6,564,031 records**
- There are **10,000 unique grid squares (CellID)**
- A total of **302 unique country codes** appear in the data, including Italy (`39`)

### Missing Values
Missing values are most common in SMS and call-related columns. After mean imputation, all numeric columns were fully populated, allowing complete analysis without discarding data.

### Temporal Activity Patterns
Communication activity follows a clear daily pattern. Activity is lowest in the early morning (lowest point at `4 AM`) and increases throughout the day. The peak hour across all grids occurs at ` hour 17 (5 PM)`. Daytime hours (6 AM–8 PM) account for `78.51%` of total activity, while nighttime hours (8 PM–6 AM) account for `21.49%`.


### Domestic vs International Communication
Domestic calls are strongly concentrated during daytime hours, while international calls are more evenly distributed throughout the day and remain relatively high in the evening/night. This difference is likely influenced by time-zone differences.

### Direction of International Calls
The incoming-to-outgoing international call ratio is `1.67`, meaning international calls are **more incoming than outgoing**. This suggests that international communication during the observed period is more often initiated from abroad toward users in Italy.

### Relationship Between SMS and Calls
There is a very strong positive correlation (`≈ 0.99`) between SMS volume and call volume at the grid level. Grid areas with high SMS activity also tend to have high call activity.

---

## Libraries Used

- Pandas 
- NumPy
- Matplotlib

## Github Repository Link 
https://github.com/Luciefifi/mbdam-assignment1-mobile-phone-activity.git
