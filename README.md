# Stock-Analysis On Intel

import yfinance as yf
import pandas as pd
import matplotlib.pyplot as plt
from sklearn.linear_model import LinearRegression
from sklearn.model_selection import train_test_split

# Download historical stock data for Intel (ticker: INTC)
ticker_symbol = 'INTC'
start_date = '2020-01-01'
end_date = '2024-10-17'
intel_data = yf.download(ticker_symbol, start=start_date, end=end_date)

# Display the first few rows of the data
print(intel_data.head())

# Calculate daily returns
intel_data['Daily_Return'] = intel_data['Close'].pct_change()

# Plot closing prices
plt.figure(figsize=(10, 5))
plt.plot(intel_data['Close'], label='Intel Closing Prices')
plt.title('Intel Corporation Closing Prices')
plt.xlabel('Date')
plt.ylabel('Price')
plt.legend()
plt.show()

# Simple linear regression to predict future stock values
X = intel_data.index.values.reshape(-1, 1)
y = intel_data['Close'].values
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=0)

model = LinearRegression()
model.fit(X_train, y_train)

# Predict future stock values
future_dates = pd.date_range(start_date, periods=30).values.reshape(-1, 1)
future_prices = model.predict(future_dates)

# Plot predicted future stock values
plt.figure(figsize=(10, 5))
plt.plot(intel_data.index, intel_data['Close'], label='Historical Prices')
plt.plot(future_dates, future_prices, label='Predicted Prices')
plt.title('Intel Corporation Stock Prediction')
plt.xlabel('Date')
plt.ylabel('Price')
plt.legend()
plt.show()
