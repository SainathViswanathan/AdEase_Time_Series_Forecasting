# AdEase — Multilingual Time Series Forecasting Case Study

![Python](https://img.shields.io/badge/Python-3.9%2B-blue?logo=python)
![Machine Learning](https://img.shields.io/badge/Type-Time%20Series%20Forecasting-purple)
![Models](https://img.shields.io/badge/Models-SARIMA%20%7C%20SARIMAX%20%7C%20Prophet-yellow)
![Libraries](https://img.shields.io/badge/Libraries-pandas%2C%20statsmodels%2C%20fbprophet-orange)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen)


## 🚀 About AdEase
AdEase is a digital advertising analytics platform focused on optimizing global campaign performance through predictive modeling.  
In this project, AdEase aims to forecast **Wikipedia page views** across multiple languages to anticipate audience engagement and optimize ad timing, reach, and cost efficiency.

---

## 🧩 Business Problem
AdEase’s campaigns depend on accurate web traffic forecasts.  
The goal is to build a **robust forecasting pipeline** capable of predicting multilingual Wikipedia traffic while capturing **weekly, monthly, and campaign-based seasonality**.

Without such insights, campaign scheduling risks inefficiency — missing high-traffic periods or overspending during low-traffic windows.

---

## 🎯 Project Objective
Leverage advanced **time series models (ARIMA, SARIMA, SARIMAX, Prophet)** to:
- Forecast page views for high-traffic languages (English, Japanese, German).  
- Identify weekly and monthly seasonal trends.  
- Incorporate campaign effects as exogenous variables.  
- Evaluate forecasting performance and recommend deployment strategies.

---

## 🔑 Key Goals
- Clean and preprocess multilingual time series data spanning 550 days.  
- Detect and visualize **seasonality and trend patterns** across languages.  
- Achieve **MAPE within 4–8%**, consistent with benchmark expectations.  
- Recommend business actions for language-specific campaign optimization.

---

## 📚 Dataset Overview
**Period:** July 1, 2015 – December 31, 2016  
**Coverage:** 126,654 Wikipedia pages across 556 columns (post-cleaning)  

| Attribute | Description |
|------------|-------------|
| `date` | Daily timestamp |
| `title` | Page title |
| `lang` | Wikipedia language code |
| `access` | Access type (desktop, mobile-web, all-access) |
| `agent` | User type (human, spider) |
| `views` | Daily page views |
| `language_name` | Derived descriptive field |

🧹 **Cleaning Summary:**
- Removed ~12.17% rows with missing `title` or `lang`.  
- Extracted additional metadata fields (`title`, `lang`, `access`, `agent`, `language_name`).  
- Resulting dataset: `(126,654, 556)` — complete, daily, and stationarity-ready.

---

## 🌍 Language Insights
- **English**: Highest traffic and ideal for campaign targeting.  
- **Japanese & German**: High engagement, consistent structure — selected for modeling.  
- **Other languages**: French, Chinese, Russian, and Spanish show moderate but steady activity.

---

## 💻 Access & Agent Insights
| Type | Observation |
|-------|-------------|
| **Access** | Desktop > All-access > Mobile-web |
| **Agent** | Humans dominate; spiders are consistent minor traffic. |
| **Pattern** | Desktop traffic peaks on weekdays; mobile traffic slightly higher on weekends. |

---

## 📅 Seasonality Analysis
### Weekly Patterns
- **English, German, Russian:** Peak early in the week → dip by Friday/Saturday.  
- **Japanese:** Midweek dip (Wed–Thu) with peaks at start/end of week.  
- Desktop → strong weekday cycles; Mobile → evenly distributed.

### Monthly Patterns
- **Spikes:** February & August (English, Desktop).  
- **Dips:** May & July (all languages).  
- Confirmed need for **SARIMA/Prophet** to handle clear seasonal components.

---

## 🧪 Data Stationarity & Differencing
| Language | Initial ADF | Differencing | Final Result |
|-----------|--------------|--------------|----------------|
| English | Non-stationary | 1st order | ✅ Stationary |
| Japanese | Borderline | 1st order | ✅ Stationary |
| German | Non-stationary | 1st order | ✅ Stationary |

Rolling mean and variance stabilized post-differencing, validating model readiness.

---

## 🔍 ACF/PACF Analysis & Model Inference

| Language | Key Observations | Final Model |
|-----------|------------------|--------------|
| 🇬🇧 **English** | Strong weekly cycles (lags 7, 14, 21, 28), flat short-term AR | **SARIMA(0,1,0)(1,1,1,7)** |
| 🇯🇵 **Japanese** | Strong MA(1) signature, mild seasonality | **SARIMA(0,1,1)(0,0,1,7)** |
| 🇩🇪 **German** | AR(1–2) + MA(1–2), strong weekly cycles | **SARIMA(1,1,2)(1,1,2,7)** |

---

## 📈 Seasonal Decomposition
| Language | Trend | Seasonality | Residuals |
|-----------|--------|-------------|-----------|
| English | Upward trend | Clear weekly pattern | Stable residuals |
| Japanese | Stable trend | Distinct seasonal cycle | Balanced residuals |
| German | Mild trend noise | Strong weekly pattern | Low residual variance |

---

## 🔮 Model Comparison Summary

| Language | Model | MAPE (%) | RMSE | Preferred |
|-----------|--------|-----------|-------|------------|
| 🇬🇧 English | **Prophet (with campaign)** | **5.44** | 9.5M | ✅ Prophet |
| 🇬🇧 English | SARIMAX | 7.35 | — | — |
| 🇯🇵 Japanese | **SARIMA** | **7.85** | — | ✅ SARIMA |
| 🇯🇵 Japanese | Prophet | 8.15 | — | — |
| 🇩🇪 German | **SARIMA** | **7.67** | — | ✅ SARIMA |
| 🇩🇪 German | Prophet | 8.22 | Slightly better RMSE | — |

---

## 🧭 Final Recommendations
✅ **English:** Use **Prophet + campaign exogenous variable** — captures campaign-driven spikes accurately.  
✅ **Japanese & German:** Use **SARIMA models** — strong weekly cycles captured efficiently.  
✅ **Prophet** remains valuable for:
- Interpretability and quick deployment  
- Easy inclusion of holiday/campaign regressors  
- Lightweight production integration

---

## 📊 Business Recommendations
1. **Language-Specific Campaign Targeting**  
   - Focus campaigns on **English pages** (highest volume).  
   - Use campaign event variables to anticipate traffic surges.

2. **Leverage Weekly Seasonality**  
   - Schedule updates on **peak days** (Sunday–Monday).  
   - Use **Friday/Saturday** for maintenance or downtime.

3. **Device-Specific Optimization**  
   - **Desktop:** Prioritize UX and loading performance on weekdays.  
   - **Mobile:** Enhance weekend experience for casual users.

4. **Forecast-Based Resource Planning**  
   - Use Prophet/SARIMA forecasts for **server bandwidth allocation**.  
   - Implement anomaly detection for sudden traffic dips.

5. **Campaign Extensions**  
   - Extend **campaign regressors** (used in English Prophet model) to other languages.  
   - Track cross-language campaign impact for global ROI.

---

## 🧾 Model Performance Verdict
> ✅ The final models meet AdEase’s target **MAPE benchmark of 4–8%**:
> - **Prophet (English):** 5.44%  
> - **SARIMA (Japanese):** 7.85%  
> - **SARIMA (German):** 7.67%

---

## 🧰 Tools & Libraries
| Category | Tools |
|-----------|--------|
| **Data Manipulation** | pandas, numpy |
| **Visualization** | matplotlib, seaborn |
| **Time Series Models** | statsmodels (ARIMA/SARIMA/SARIMAX), fbprophet |
| **Evaluation Metrics** | RMSE, MAE, MAPE |
| **Feature Engineering** | datetime, seasonal decomposition, differencing |

---

## 🧪 How to Run
1. Clone the repository.  
2. Add dataset files to the `/data` folder.  
3. Run the notebook `AdEase_Time_Series_Case_Study.ipynb`.  
4. View seasonal decomposition and model performance outputs.

---

## 📌 Project Impact
This project empowers AdEase to:
- Predict multilingual web traffic with >90% accuracy.  
- Align campaign schedules with audience peaks.  
- Reduce cost inefficiencies via smarter timing.  
- Scale forecasting pipelines to other regional markets.

---

## 🧭 Authors & Acknowledgments
Developed by **Sainath Viswanathan** as part of Scaler’s Data Science & ML learning journey.  
Guided by Scaler’s capstone case study framework and AdEase business objective.
