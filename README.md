# 🍽️ AI Demand Forecasting & Inventory Optimization for Restaurants

🚧 **Project Status**
Ongoing Internship Project – Week 1 Completed

---

## 🎯 Objective

The goal of this project is to build an AI-powered Demand Forecasting model for a simulated restaurant or food delivery chain. By analyzing historical point-of-sale (POS) data and external variables, the model will predict future daily sales volume for specific menu items — helping managers reduce food waste, optimize inventory, and improve supply chain decisions.

---

## 📂 Dataset

**Dataset Name:** Restaurant Sales / Retail Demand Dataset

**Recommended Sources:**

* 🔗 [Kaggle – Restaurant Orders Dataset](https://www.kaggle.com/datasets/ahsan81/food-ordering-and-delivery-app-dataset)
* 🔗 [Kaggle – Store Sales Time Series Forecasting](https://www.kaggle.com/competitions/store-sales-time-series-forecasting/data)
* 🔗 [UCI Machine Learning Repository – Online Retail Dataset](https://archive.ics.uci.edu/dataset/352/online+retail)

> 💡 **How to collect the dataset:**
> 1. Go to any of the Kaggle links above
> 2. Create a free Kaggle account (if you don't have one)
> 3. Click **Download** on the dataset page
> 4. Extract the ZIP file — you will get `.csv` files with transactional/sales data
> 5. Place the CSV files in your project's `data/raw/` folder

**What the dataset contains:**

* Historical daily or hourly sales transactions
* Menu item names and quantities sold
* Date and time information
* (Optional) Weather data, local events, holidays

---

## 📊 Week 1: Data Ingestion, Time-Series EDA & Preprocessing

### ✔ Data Loading

* Loaded raw CSV dataset into a Pandas DataFrame
* Parsed and formatted the `date` column as a proper datetime index
* Verified dataset shape, column names, and data types

### ✔ Exploratory Data Analysis (EDA)

* Plotted overall daily sales trend over time
* Visualized weekly and monthly seasonality patterns
* Identified demand spikes (e.g., weekends, holidays)
* Analyzed autocorrelations (ACF/PACF) to understand how past sales influence future demand
* Detected and flagged outliers (holiday spikes, store closures)

### ✔ Data Preprocessing

* Aggregated raw transactions into daily sales totals
* Ensured the datetime index is continuous (no missing dates)
* Handled missing values using forward-fill / interpolation
* Split dataset sequentially:

  * Train (first 10 months)
  * Test (last 2 months)

> ⚠️ **Note:** We use sequential splitting (not random) to prevent data leakage from the future into the past.

---

## ⚙️ Preprocessing Pipeline

* Parsed and sorted datetime index
* Resampled transactions to daily frequency
* Handled missing dates and outlier events
* Created a clean, continuous time-series ready for feature engineering

---

## 🛠️ Tools & Technologies

* Python
* Pandas / NumPy
* Matplotlib / Plotly
* Scikit-Learn (for future model training)
* XGBoost / Prophet (upcoming)

---

## 📂 Project Structure

```
Restaurant-Demand-Forecasting/
│── data/
│   ├── raw/               ← Original downloaded CSV files
│   └── processed/         ← Cleaned & aggregated daily sales data
│── notebooks/
│   ├── week1_eda.ipynb
│── README.md
```

---

## 📈 Evaluation Metrics (Upcoming)

* MAE (Mean Absolute Error)
* RMSE (Root Mean Square Error)
* Focus on minimizing forecast error on unseen future dates

---

## 🌱 Future Work

* Week 2: Advanced feature engineering (lag features, rolling windows, holiday flags)
* Week 3: Train XGBoost / Random Forest / Prophet models
* Week 4: Evaluate model, extract feature importance, create business report

---

## 💡 Conclusion

This project aims to transition restaurant operations from reactive guesswork to proactive, data-driven demand planning. Accurate forecasting directly supports a 20–30% reduction in food waste and supply chain costs.

---

## 📅 Week 2: Advanced Feature Engineering

🚧 **Project Status:** Week 2 Completed

---

## 🎯 Objective

In this week, we created the key input features that will give the machine learning model its predictive power. Raw date information alone is not enough — we need to extract meaningful signals from time.

---

## 🧠 Features Engineered

### Chronological Features

* Day of week (0 = Monday … 6 = Sunday)
* Month and quarter
* `is_weekend` flag (1 if Saturday/Sunday)
* `is_holiday` flag (using a public holiday calendar)

### Lag Features

* Sales from **7 days ago** (same day last week)
* Sales from **14 days ago**
* Sales from **28 days ago** (same day last month)

### Rolling Window Statistics

* 7-day rolling mean
* 14-day rolling mean and standard deviation
* 7-day rolling max (captures recent demand peaks)

---

## ⚙️ Data Split

* Training set: first 10 months of data
* Test set: last 2 months of data
* Split is **sequential** — no random shuffling, to prevent data leakage

---

## 📊 Observation

* Weekend sales consistently higher than weekday sales
* Holiday periods show strong demand spikes
* 7-day lag feature closely tracks the target sales pattern
* Rolling mean smooths out noise and captures underlying trend

---

## 💡 Conclusion

In Week 2, we successfully built a rich feature set from raw date information. These features — especially lag values and rolling statistics — are the most important inputs for our machine learning models in Week 3.
