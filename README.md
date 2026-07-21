# Ex.No: 1B                     CONVERSION OF NON STATIONARY TO STATIONARY DATA
# Date: 21-7-26

### AIM:
To perform regular differncing,seasonal adjustment and log transformatio on international airline passenger data
### ALGORITHM:
1. Import the required packages like pandas and numpy
2. Read the data using the pandas
3. Perform the data preprocessing if needed and apply regular differncing,seasonal adjustment,log transformation.
4. Plot the data according to need, before and after regular differncing,seasonal adjustment,log transformation.
5. Display the overall results.
### PROGRAM:
```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

from statsmodels.tsa.seasonal import seasonal_decompose

# Data loading and preprocessing
data = pd.read_csv('/Walmart_Sales.csv')
data['Date'] = pd.to_datetime(data['Date'], format='%d-%m-%Y') # Convert 'Date' column to datetime objects with specified format
data.set_index('Date', inplace=True)
data = data[~data.index.duplicated(keep='first')] # Handle duplicate dates in the index to prevent reindexing errors

# Feature Engineering / Transformations
data['weekly_sales_diff'] = data['Weekly_Sales'] - data['Weekly_Sales'].shift(1)

# First seasonal decomposition (for raw weekly sales)
result_raw = seasonal_decompose(data['Weekly_Sales'].dropna(), model='additive', period=12)
data['weekly_sales_sea_diff'] = result_raw.resid

data['weekly_sales_log'] = np.log(data['Weekly_Sales'])
data['weekly_sales_log_diff'] = data['weekly_sales_log'] - data['weekly_sales_log'].shift(1)

# Second seasonal decomposition (for log-transformed and differenced sales)
result_log_diff = seasonal_decompose(data['weekly_sales_log_diff'].dropna(), model='additive', period=12)
data['weekly_sales_log_seasonal_diff'] = result_log_diff.resid

# Plotting
plt.figure(figsize=(16, 30))

# Plot 1: Original Data
plt.subplot(6, 1, 1)
plt.plot(data['Weekly_Sales'], label='Original')
plt.legend(loc='best')
plt.title('Original Data')
plt.xlabel('Year')
plt.ylabel('Weekly Sales')
plt.xticks(rotation=45)

# Plot 2: Regular Difference
plt.subplot(6, 1, 2)
plt.plot(data['weekly_sales_diff'], label='Regular Difference')
plt.legend(loc='best')
plt.title('Regular Differencing')
plt.xlabel('Year')
plt.ylabel('Differenced Weekly Sales')
plt.xticks(rotation=45)

# Plot 3: User-requested plot (log, differenced, seasonal diff)
plt.subplot(6, 1, 3)
plt.plot(data['weekly_sales_log_seasonal_diff'], label='Log Transformed, Differenced, and Seasonally Differenced')
plt.legend(loc='best')
plt.title('Log Transformed, Differenced, and Seasonally Differenced')
plt.xlabel('Year')
plt.ylabel('SDiff(RDiff(Log(Weekly Sales)))')
plt.xticks(rotation=45)

# Plot 4: Seasonal Adjustment (from raw data)
plt.subplot(6, 1, 4)
plt.plot(data['weekly_sales_sea_diff'], label='Seasonal Adjustment (Raw Data)')
plt.legend(loc='best')
plt.title('Seasonal Adjustment (Raw Data)')
plt.xlabel('Year')
plt.ylabel('Seasonally Adjusted Weekly Sales')
plt.xticks(rotation=45)

# Plot 5: Log Transformation
plt.subplot(6, 1, 5)
plt.plot(data['weekly_sales_log'], label='Log Transformation')
plt.legend(loc='best')
plt.title('Log Transformation')
plt.xlabel('Year')
plt.ylabel('Log(Weekly Sales)')
plt.xticks(rotation=45)

# Plot 6: Log Transformation and Regular Differencing
plt.subplot(6, 1, 6)
plt.plot(data['weekly_sales_log_diff'], label='Log Transformation and Regular Differencing')
plt.legend(loc='best')
plt.title('Log Transformation and Regular Differencing')
plt.xlabel('Year')
plt.ylabel('RDiff(Log(Weekly Sales))')
plt.xticks(rotation=45)

plt.tight_layout()
plt.show()
```


### OUTPUT:


REGULAR DIFFERENCING:
<img width="1905" height="621" alt="image" src="https://github.com/user-attachments/assets/0e6f8743-b0b9-4837-b07e-90af0e8fa8db" />



SEASONAL ADJUSTMENT:
<img width="1896" height="622" alt="image" src="https://github.com/user-attachments/assets/a0759eca-9ca8-46ab-bd1a-741b3a197539" />


LOG TRANSFORMATION:

<img width="1825" height="551" alt="image" src="https://github.com/user-attachments/assets/733f5aec-c810-4c92-a9e4-a885c5aeab67" />


### RESULT:
Thus we have created the python code for the conversion of non stationary to stationary data on international airline passenger
data.
