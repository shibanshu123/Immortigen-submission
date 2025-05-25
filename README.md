 Mess Hall Queue Prediction — Mini Platform (Part A)

This project predicts the **evening peak queue length** (people/min) in the mess between **7 PM – 9 PM**, using information available by **4 PM**.

---

## Problem Statement

**At 4 PM**, predict the **peak queue** between 7–9 PM using:

- Today’s **menu**
- Today's **weather** up to 4 PM
- Today’s **calendar event**
- **Yesterday’s footfall** data

---

## Input Files

| File                | Description |
|---------------------|-------------|
| `menu.csv`          | Daily meals with item and popularity |
| `calendar.csv`      | Events/fests mapped by date |
| `weather.csv`       | Hourly temperature, date-wise |
| `footfall_history.csv` | Timestamped crowd data (people/min) |

---

## Key Features Used in Model

| Feature | Description |
|--------|-------------|
| `temp_c` | Avg. temperature up to 4 PM |
| `w_day_moving_avg` | 7-day moving average of past peak queues |
| `yesterday_peak_queue` | Peak count (7–9 PM) on the previous day |
| `popularity_score_dinner` | Mean popularity score of dinner items |

---

## Pipeline Steps

1. **Preprocessing**
   - Load datasets, parse datetime
   - Join menu/weather/calendar on date
   - Compute engineered features

2. **Outlier Removal**
   - Removed extreme peak values using IQR method (boxplot logic)

3. **Modeling**
   - Initial model: `RandomForestRegressor`
   - Feature scaling not needed (tree-based model)

4. **Model Tuning**
   - Tuned using `GridSearchCV`:
     - `n_estimators`, `max_depth`, `min_samples_leaf`, etc.

---

## Performance (Random Forest)

| Metric | Value |
|--------|-------|
| MAE    | 2.48 |
| RMSE   | 2.79 |
| R²     | 0.08 |

---

## 🔁 Additional Models Tried

| Model          | Notes |
|----------------|-------|
| `XGBoostRegressor` | Boosted performance with similar features |
| `LinearRegression` | Baseline|
| `KNeighborsRegressor` | Tried for fun, lower performance |
| `GradientBoostingRegressor` | Slower, slightly better than RF on some folds |
