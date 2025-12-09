# NYC Taxi Trip Analysis & Clustering

## Overview

This project analyzes New York City taxi trips (Kaggle: NYC Taxi Trip Duration) to understand ride patterns and segment trips into meaningful operational clusters.  
The final deliverables are:

- A **Jupyter Notebook** with the full analysis and clustering pipeline.  
- A **Power BI–ready CSV** (generated locally) with enriched trip data and cluster labels.  
- A **cluster summary table** to support business insights.

The project is designed as a portfolio piece for **data analytics / data science roles**, with focus on:

- Real-world data cleaning  
- Feature engineering (time, distance, speed)  
- Exploratory data analysis (EDA)  
- Unsupervised learning (K-Means clustering)  
- BI preparation and storytelling  

### Project Highlights

- Cleaned and processed NYC taxi trip data from the original Kaggle competition.  
- Engineered time and distance features (trip duration, speed, Haversine distance).  
- Built a **K-Means clustering model (k = 4)** to segment trips into operational profiles.  
- Prepared a **Power BI–ready dataset** for dashboarding and business analysis (generated locally to avoid GitHub size limits).  

---

## Dataset

- **Source:** Kaggle – NYC Taxi Trip Duration  
- **Link:** https://www.kaggle.com/competitions/nyc-taxi-trip-duration  

**Main columns used:**

- `pickup_datetime`, `dropoff_datetime`  
- `pickup_longitude`, `pickup_latitude`  
- `dropoff_longitude`, `dropoff_latitude`  
- `passenger_count`  
- `trip_duration` (seconds)  

> **Note:** The raw CSV is **not** included in this repository due to size.  
> Please download it directly from Kaggle and place it in the project root as `train.csv`.

---

## Business Questions Addressed

- When are the peak hours for taxi demand in NYC?  
- How do trip durations and distances vary across different times of day and days of week?  
- Can we identify distinct segments of trips (e.g., airport / long-distance vs. short urban rides)?  
- How can these segments support decisions on pricing, fleet allocation, or driver incentives?  

---

## Project Steps

### 1. Data Cleaning

- Remove rows with missing values.  
- Convert `pickup_datetime` and `dropoff_datetime` to `datetime`.  
- Filter `trip_duration` using the 0.5% and 99.5% percentiles to remove extreme outliers.  
- Keep only reasonable `passenger_count` (1–6).  
- Restrict coordinates to a bounding box around NYC to remove trips with invalid GPS data.  
- Filter out trips with unrealistic average speed (<= 0 km/h or > 100 km/h).  

### 2. Feature Engineering

- `trip_duration_minutes` – duration in minutes.  
- Time-based features:
  - `pickup_hour`  
  - `pickup_dayofweek` (0 = Monday, 6 = Sunday)  
  - `is_weekend` (1 if Saturday or Sunday)  
- Distance (km) between pickup and dropoff using the **Haversine formula**.  
- `speed_kmh` – average speed per trip.  

### 3. Exploratory Data Analysis (EDA)

- Distributions:
  - Trip duration (minutes)  
  - Distance (km)  
  - Speed (km/h)  
- Relationship:
  - Distance vs. duration (sample scatter plot)  
- Aggregations:
  - Trips and average duration by hour of day  
  - Trips and average duration by day of week  
  - Weekday vs weekend comparison  

### 4. Feature Preparation for Clustering

- Cap `trip_duration_minutes`, `distance_km`, and `speed_kmh` at the 99th percentile to reduce impact of extreme outliers.  
- Select features for clustering:
  - `trip_duration_capped`  
  - `distance_km_capped`  
  - `speed_kmh_capped`  
  - `pickup_hour`  
  - `is_weekend`  
- Standardize features using `StandardScaler` (required for K-Means).  

### 5. K-Means Clustering

- Use a 200k-row sample to test `k = 3, 4, 5`.  
- Evaluate:
  - **Elbow method** (Inertia)  
  - **Silhouette score**  
- Select **k = 4** as the best balance between cluster separation and interpretability.  
- Fit final K-Means model on the full dataset with `k = 4`.  
- Assign cluster labels to all trips.  

### 6. Cluster Interpretation

For each cluster, compute:

- Average and median duration  
- Average and median distance  
- Average and median speed  
- Average pickup hour  
- Percentage of weekend trips  
- Trip count  

Based on these metrics, the following segments were defined:

1. **Weekday morning short trips**  
2. **Long-distance trips (likely airport / inter-borough)**  
3. **Weekday evening short trips**  
4. **Weekend leisure short trips**  

These clusters represent different demand patterns and potential levers for pricing, fleet allocation, and operations planning.

---

## Files in This Repository

- `nyc_taxi_analysis.ipynb` – full notebook (cleaning, EDA, clustering, and exports).  
- `nyc_taxi_cluster_summary.csv` – cluster-level aggregated metrics.  
- `README.md` – project documentation (this file).  

> **Note:** The Power BI–ready export `nyc_taxi_for_powerbi.csv` is generated locally when running the notebook and is not tracked in Git due to GitHub’s 100 MB per-file size limit.

---

## How to Run

### 1. Clone this repository

```bash
git clone https://github.com/samuelmasseli/nyc-taxi-trip-analysis.git
cd nyc-taxi-trip-analysis
```

### 2. Download the original dataset from Kaggle

- Go to: https://www.kaggle.com/competitions/nyc-taxi-trip-duration  
- Download the `train.csv` file.  
- Save it in the **project root**, with this exact structure:

```text
nyc-taxi-trip-analysis/
  ├─ nyc_taxi_analysis.ipynb
  ├─ train.csv
  └─ ...
```

### 3. Install dependencies (recommended)

Install packages manually:

```bash
pip install pandas numpy scikit-learn matplotlib seaborn
```

Or create a `requirements.txt` with:

```text
pandas
numpy
scikit-learn
matplotlib
seaborn
```

and install with:

```bash
pip install -r requirements.txt
```

### 4. Open the notebook

```bash
jupyter notebook nyc_taxi_analysis.ipynb
```

### 5. Run all cells

- In Jupyter: `Kernel -> Restart & Run All`.

This will:

- Clean and enrich the dataset.  
- Perform exploratory data analysis (EDA).  
- Run K-Means clustering.  
- Export:
  - `nyc_taxi_for_powerbi.csv`  (not tracked in Git because of size)  
  - `nyc_taxi_cluster_summary.csv`  

---

## Power BI Dashboard (Planned)

A Power BI dashboard is being designed to:

- Visualize trip demand by hour and day of week.  
- Compare key metrics (duration, distance, speed) across clusters.  
- Show pickup and dropoff hotspots on a map.  

Once published, a link and screenshots will be added here.

---

## Technologies Used

- Python  
- Pandas, NumPy  
- Scikit-learn  
- Matplotlib, Seaborn  
- Jupyter Notebook  
- (Optional) Power BI for dashboarding  

---

## Possible Extensions

- Add geographic visualizations (e.g., Folium / Mapbox) for pickup and dropoff patterns.  
- Incorporate weather or traffic data.  
- Build a Power BI dashboard and link screenshots in this README.  
- Try different clustering algorithms (e.g., DBSCAN, Gaussian Mixture Models).  

---

## Author

**Samuel Masseli** – Data Analyst / Aspiring Data Scientist  
Based in Brazil, open to remote opportunities (US and international).

- GitHub: https://github.com/samuelmasseli
