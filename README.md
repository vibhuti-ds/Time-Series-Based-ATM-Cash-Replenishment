# Time-Series-Based-ATM-Cash-Replenishment

This project builds a **data-driven, AI-powered framework** for **forecasting ATM cash withdrawals** and **optimizing replenishment schedules**. It combines **time series modeling**, **synthetic data generation**, **seasonality analysis**, and **reinforcement learning** to improve efficiency in ATM operations across India.

---

## 🎯 Project Objective

The goal is to address challenges in traditional ATM replenishment models such as:
- ❌ Overfilling → leads to locked capital and higher logistics costs
- ❌ Underfilling → leads to cash-out situations and customer dissatisfaction

We aim to:
- Predict **daily cash withdrawal demand**
- Dynamically adjust **replenishment schedules**
- Reduce **operational cost and cash-out risk**
- Consider **real-world behavioral patterns** like salary days, festivals, weather

---

## 🧱 Step-by-Step Project Breakdown

### 🧩 Step 1: Synthetic ATM Transaction Data Generation

📁 **File:** `utils/generate_synthetic_data.py`

- Simulates daily withdrawal data for 100 ATMs over 2024.
- ATM categories: Urban, Semi-Urban, Rural; Full-Service & Kiosks
- Simulated patterns:
  - Salary spikes at month-end (28–2nd)
  - Festival-driven spikes (e.g., Holi, Diwali)
  - Weather-related dips (e.g., rainy days)
- Uses a mix of:
  - Random distributions
  - Pre-defined date weights
  - Cluster-level adjustments

📈 **Output:** `synthetic_atm_data.csv`  
🗃️ Contains: `date`, `atm_id`, `location_type`, `atm_type`, `withdrawal_amount`

---

### 📊 Step 2: Exploratory Data Analysis (EDA)

📁 **Notebook:** `01_EDA_and_Clustering.ipynb`

- Analyzes withdrawal trends by:
  - ATM type (kiosk/full-service)
  - Area (urban/semi-urban/rural)
- Identifies:
  - Weekly/Monthly seasonality
  - Festival and salary impacts
- Clustering done using:
  - K-Means or manual tagging
- Outputs ATM categories for targeted forecasting

📊 Visualizations:
- Line plots of withdrawal volume per cluster
- Boxplots for weekday and month variations
- Heatmaps for correlation analysis

---

### 🧠 Step 3: Time Series Forecasting

#### 📌 (a) Auto ARIMA Modeling  
📁 **Notebook:** `02_ARIMA_Modeling.ipynb`

- Applies `auto_arima()` from `pmdarima` on individual ATM or cluster-level series
- Trains models using:
  - 2024 Jan–Oct data
- Forecasts for:
  - Nov–Dec 2024
- Evaluates using:
  - RMSE
  - MAPE
  - AIC/BIC

---

#### 📌 (b) SARIMAX Modeling with Exogenous Variables  
📁 **Notebook:** `03_SARIMAX_Modeling.ipynb`

- Exogenous (X) features include:
  - `is_salary_day` (binary)
  - `is_festival_day` (binary)
  - `day_of_week`, `month`
- Uses `statsmodels.tsa.statespace.SARIMAX`
- Improved accuracy in salary/festival-affected ATMs

🔍 Visual Outputs:
- Actual vs Predicted plots
- Residuals analysis
- Forecast confidence intervals

---

### 🧠 Step 4: Reinforcement Learning for Scheduling (Optional)

📁 **Notebook:** `04_RL_Agent_Scheduling.ipynb`

- Builds a **Q-learning agent** to decide:
  - When to replenish ATM
  - How much cash to refill
- States: current balance levels
- Actions: refill, skip
- Rewards:
  - Penalty for cash-outs
  - Cost for unnecessary refills

🧪 Trained with simulated data over 365 episodes (days)

---

### 📊 Step 5: Model Comparison & Evaluation

📁 In each modeling notebook:

- Compare Auto ARIMA vs SARIMAX:
  - Accuracy on test set
  - Responsiveness to external factors
- Cluster-wise summary table:
  ```text
  | Cluster         | Model     | RMSE  | Notes                         |
  |-----------------|-----------|-------|-------------------------------|
  | Urban - Full    | SARIMAX   | 450   | Best performance              |
  | Rural - Kiosk   | ARIMA     | 600   | Simpler, faster, good enough |
