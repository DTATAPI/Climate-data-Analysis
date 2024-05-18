# Climate-data-Analysis
#a comprehensive climate data analysis project to explore and understand historical climate patterns and trends.The objective is to derive
#valuable insights from climate data, enabling a better understanding of weather
#conditions over time.
import pandas as pd
import matplotlib.pyplot as plt

# Reading CSV files and parsing dates
daily_df = pd.read_csv(r'daily_data.csv', parse_dates=['DATE'])
hourly_df = pd.read_csv(r'hourly_data.csv', parse_dates=['DATE'])
monthly_df = pd.read_csv(r'monthly_data.csv', parse_dates=['DATE'])
three_hour_df = pd.read_csv(r'three_hour_data.csv', parse_dates=['DATE'])

# Setting 'DATE' column as index
daily_df.set_index('DATE', inplace=True)
hourly_df.set_index('DATE', inplace=True)
monthly_df.set_index('DATE', inplace=True)
three_hour_df.set_index('DATE', inplace=True)

# Combine all datasets into a single DataFrame
combined_df = pd.concat([daily_df, hourly_df, monthly_df, three_hour_df], axis=1)

# Data Exploration
print(combined_df.head())
print(combined_df.describe())

# Handle missing or non-numeric data
combined_df['DailyAverageDryBulbTemperature'] = pd.to_numeric(combined_df['DailyAverageDryBulbTemperature'], errors='coerce')
combined_df['DailyPrecipitation'] = pd.to_numeric(combined_df['DailyPrecipitation'], errors='coerce')
combined_df['MonthlyDepartureFromNormalPrecipitation'] = pd.to_numeric(combined_df['MonthlyDepartureFromNormalPrecipitation'], errors='coerce')

# Drop rows with NaN values in columns of interest for plotting
combined_df.dropna(subset=['DailyAverageDryBulbTemperature', 'DailyPrecipitation', 'MonthlyDepartureFromNormalPrecipitation'], inplace=True)

# Visualizations

# Line Plot for Temperature and Precipitation Trends
plt.figure(figsize=(10, 6))
plt.plot(combined_df.index, combined_df['DailyAverageDryBulbTemperature'], label='DailyAverageDryBulbTemperature', color='b')
plt.plot(combined_df.index, combined_df['DailyPrecipitation'], label='DailyPrecipitation', color='g')
plt.xlabel('Date')
plt.ylabel('Values')
plt.title('Temperature and Precipitation Trends')
plt.legend()
plt.show()

# Subplots for Decomposition
plt.figure(figsize=(10, 12))

# Original
plt.subplot(411)
plt.plot(combined_df.index, combined_df['DailyAverageDryBulbTemperature'], label='Original')
plt.legend(loc='upper left')

# Trend (replace with actual trend data if available)
plt.subplot(412)
plt.plot(combined_df.index, combined_df['MonthlyDepartureFromNormalPrecipitation'], label='Trend')
plt.legend(loc='upper left')

# Seasonal (replace with actual seasonal data if available)
plt.subplot(413)
plt.plot(combined_df.index, combined_df['DailyAverageDryBulbTemperature'], label='Seasonal')
plt.legend(loc='upper left')

# Residual (replace with actual residual data if available)
plt.subplot(414)
plt.plot(combined_df.index, combined_df['DailyPrecipitation'], label='Residual')
plt.legend(loc='upper left')

plt.tight_layout()
plt.show()
