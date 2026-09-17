# Stock Price Forecasting: ARIMA vs Prophet vs LSTM

A comparative time-series analysis of Infosys (INFY) stock prices using ARIMA, Prophet, and LSTM models.

## About the Project

Can historical stock prices be used to forecast future prices?

That was the question I wanted to explore through this project.

Instead of using a single forecasting model, I compared three different approaches:

- **ARIMA** – a classical statistical time-series model
- **Prophet** – a forecasting model designed around trend and seasonality
- **LSTM** – a deep learning model designed to learn patterns from sequential data

I also included a **naive baseline** to check whether the more complex models actually provide an improvement over a very simple forecasting approach.

The goal is not to build a trading system or claim that stock prices can be reliably predicted. The focus is on understanding how different forecasting methods behave when applied to real financial time-series data.

---

## Dataset

- **Company:** Infosys Ltd. (INFY)
- **Ticker:** `INFY.NS`
- **Market:** NSE, India
- **Data source:** Yahoo Finance
- **Period:** January 2019 – December 2025
- **Target variable:** Daily closing price
- **Train/Test split:** 90% / 10%

The data is split chronologically rather than randomly so that future observations are not used to train the models.

---

## Models

### 1. ARIMA

ARIMA is a classical statistical model commonly used for time-series forecasting.

Before fitting the model, I used the **Augmented Dickey-Fuller (ADF) test** to examine the stationarity of the stock-price series.

I then used `auto_arima()` to automatically search for a suitable combination of `(p, d, q)` parameters.

### 2. Prophet

Prophet was used as a second forecasting approach.

The model was configured with:

- Weekly seasonality
- Yearly seasonality
- Daily seasonality disabled

The model was trained only on the training period before generating forecasts for the unseen test period.

### 3. LSTM

LSTM (Long Short-Term Memory) is a recurrent neural network designed for sequential data.

For this model:

- A 60-trading-day window was used
- Closing prices were scaled using MinMaxScaler
- Two LSTM layers were used
- Dropout was included to reduce overfitting
- The model was trained for 25 epochs

### 4. Naive Baseline

A simple baseline was included for comparison.

The prediction for each day is simply the previous day's closing price.

This is important because a sophisticated model should be compared against a simple benchmark rather than evaluated in isolation.

---

## Evaluation

The models are evaluated using:

### RMSE

Root Mean Squared Error penalizes larger prediction errors more heavily.

### MAE

Mean Absolute Error measures the average absolute difference between predicted and actual prices.

For both metrics:

> Lower error indicates better forecasting accuracy on the test set.

However, lower RMSE or MAE does **not** automatically mean a model would produce better investment returns.

---

## Methodology

The project follows a chronological train-test framework:

```text
Historical INFY prices
        |
        v
Data preparation
        |
        v
90% Training / 10% Testing
        |
        +------------------+
        |                  |
        v                  v
      ARIMA             Prophet
        |                  |
        +--------+---------+
                 |
                 v
               LSTM
                 |
                 v
        Naive Baseline
                 |
                 v
      RMSE / MAE Comparison
