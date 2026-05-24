# 🍽️ AI Demand Forecasting & Inventory Optimization for Restaurants

🚧 **Project Status**
Ongoing Internship Project – Week 3 Completed

---

## 🎯 Objective

The goal of this project is to build an AI-powered Demand Forecasting model for a simulated restaurant or food delivery chain. By analyzing historical point-of-sale (POS) data and external variables, the model will predict future daily sales volume for specific menu items — helping managers reduce food waste, optimize inventory, and improve supply chain decisions.

---

## 📂 Dataset

**Dataset Name:** Store Sales — Time Series Forecasting (Kaggle)

**Source:** [Kaggle – Store Sales Time Series Forecasting](https://www.kaggle.com/competitions/store-sales-time-series-forecasting/data)

**What the dataset contains:**

* Historical daily sales transactions (3M+ rows, 54 stores, 33 product families)
* Date range: January 2013 – August 2017
* External signals: oil price, store metadata, holiday events, transaction counts

> **How to set up:**
> 1. Download from the Kaggle link above
> 2. Extract the ZIP — you will get `train.csv`, `test.csv`, `stores.csv`, `oil.csv`, `holidays_events.csv`, `transactions.csv`, `sample_submission.csv`
> 3. Place all CSV files in `data/raw/`

---

## 📊 Week 1: Data Ingestion, Time-Series EDA & Preprocessing

### ✔ Data Loading
* Loaded all 7 CSV files into Pandas DataFrames
* Parsed and formatted all `date` columns as proper datetime
* Verified dataset shapes, column names, and data types

### ✔ Exploratory Data Analysis (EDA)
* Plotted overall daily sales trend (Jan 2013 – Aug 2017)
* Visualized weekly and monthly seasonality patterns
* Identified demand spikes on holidays and weekends
* Analyzed ACF/PACF to understand how far back sales history matters
* Detected and flagged outliers (holiday closures, zero-sales days)

### ✔ Data Preprocessing
* Merged all files into a single master DataFrame
* Ensured continuous datetime index with no missing dates
* Handled missing values using forward-fill and linear interpolation
* Sequential split (no random shuffling — prevents data leakage):
  * **Train:** Jan 2013 – Dec 2016 (2,596,374 rows)
  * **Validation:** Jan 2017 – May 2017 (269,082 rows)
  * **Test:** Jun 2017 – Aug 2017 (135,432 rows)

---

## ⚙️ Week 2: Advanced Feature Engineering

🚧 **Status:** Completed

### 🧠 Features Engineered

**Chronological Features**
* Day of week (0 = Monday … 6 = Sunday)
* Month, quarter
* `is_weekend` flag (1 if Saturday/Sunday)
* `is_month_start`, `is_month_end` flags

**Holiday Features**
* `is_holiday` flag (from official holiday calendar)
* `holiday_level` — National (3), Regional (2), Local (1)

**Lag Features**
* Sales from 7 days ago (same day last week)
* Sales from 14 days ago
* Sales from 28 days ago (same day last month)

**Rolling Window Statistics**
* 7-day rolling mean
* 14-day rolling standard deviation
* 7-day rolling max (captures recent demand peaks)

**Promotion Features**
* 7-day lag of promotion count
* 7-day rolling mean of promotion count

**Log Transformation**
* `sales_log1p` = log1p(total_sales) — used as training target to reduce outlier effect

### ⚙️ Data Split
* Sequential split — no random shuffling
* Final feature dataset saved to `data/processed/week2_features.csv`

### 📊 Key Observations
* Weekend sales consistently higher than weekday sales
* Holiday periods show strong demand spikes
* 7-day lag feature closely tracks the target sales pattern
* Rolling mean smooths noise and captures the underlying trend

---

## 🤖 Week 3: Model Training and Selection

🚧 **Status:** Completed

### 🎯 Objective
Train and compare three forecasting approaches — from a simple baseline to advanced ensemble models. Select the best model using Time-Series Cross-Validation.

### 🏗️ Models Trained

| Model | Description |
|---|---|
| **Linear Regression** | Baseline — establishes a performance floor |
| **Random Forest** | Ensemble of decision trees, handles non-linear patterns |
| **XGBoost (default)** | Gradient boosting, strong on tabular time-series data |
| **XGBoost (tuned)** | Best params from TimeSeriesSplit CV — final best model |

### ⚙️ Hyperparameter Tuning
* Used `TimeSeriesSplit` (5 folds) — never shuffle time-series data for CV
* Manual parameter grid over `n_estimators`, `max_depth`, `learning_rate`, `subsample`
* Best parameters selected based on lowest cross-validated MAE

### 📊 Evaluation Metrics (Validation Set)
* **MAE (Mean Absolute Error)** — average daily sales forecast error
* **RMSE (Root Mean Squared Error)** — penalizes large errors more heavily
* All metrics computed on raw sales scale (inverse of log1p transform)

### 📈 Outputs
* `data/processed/week3_model_comparison.png` — MAE/RMSE bar chart across models
* `data/processed/week3_forecast_vs_actuals.png` — predicted vs actual daily sales
* `data/processed/week3_residuals.png` — residual distribution and time plot
* `data/processed/week3_learning_curve.png` — XGBoost train vs val RMSE per boosting round
* `data/processed/week3_feature_importance.png` — top features by importance score

### 💡 Key Observations
* Lag features (especially 7-day lag) are the strongest predictors
* XGBoost consistently outperforms both baseline and Random Forest
* Residuals are approximately centered at zero — model is unbiased
* Learning curve shows the model generalizes well without overfitting

### 🔒 Security Note
* Trained model weights (`models/*.pkl`) are saved locally only
* `.gitignore` must include `models/` and `data/raw/` — do not push large files or model weights to GitHub

---

## 🛠️ Tools & Technologies

* Python 3.10
* Pandas / NumPy
* Matplotlib / Seaborn
* Scikit-Learn (Linear Regression, Random Forest, TimeSeriesSplit, StandardScaler)
* XGBoost
* Joblib (model serialization)

---

## 📂 Project Structure

```
Restaurant-Demand-Forecasting/
│── data/
│   ├── raw/                        ← Original downloaded CSV files (gitignored)
│   └── processed/                  ← Cleaned data, feature CSVs, output charts
│── models/                         ← Saved model weights (gitignored)
│── notebooks/
│   ├── Food_Restaurant_Services_week_1.ipynb
│   ├── Food_Restaurant_Services_week_2.ipynb
│   └── Food_Restaurant_Services_week_3.ipynb
│── .gitignore
│── README.md
```

---

## 📈 Evaluation Metrics

* **MAE** — Mean Absolute Error (primary metric for business reporting)
* **RMSE** — Root Mean Square Error (penalizes large forecast misses)
* Goal: minimize both on the held-out test set (evaluated in Week 4)

---

## 🌱 Week 4 (Upcoming)

* Final test-set evaluation — MAE and RMSE on Jun–Aug 2017 data
* Feature importance business report for stakeholders
* Visualized forecast overlay — predictions vs actuals
* Full GitHub push with clean commit history

---

## 💡 Conclusion

This project transitions restaurant operations from reactive guesswork to proactive, data-driven demand planning. Weeks 1–3 have established a complete pipeline from raw data to a tuned XGBoost forecasting model. Week 4 will produce the final business-ready evaluation and reporting deliverables.
