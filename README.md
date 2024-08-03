# DailyWeatherDataTracker

## Description
This tool stores, manages, and analyzes daily weather data, including rainfall, minimum and maximum temperatures, and the times they were recorded.

## Why It's Useful
This project is beneficial for meteorologists, researchers and weather enthusiasts who need to maintain and analyze historical weather data. By converting temperatues and rainfall units, calculating averages and generating statistics, this tool facilitates informed decision-making based on wheather patterns and trends.

## Features 
  - **Store Daily Weather Data**: Track rainfall, minimum and maximum temperatures, and their recording times.
  - **Unit Conversion**: Convert rainfall to inches and temperatures to Fahrenheit.
  - **Data Analysis**:
    - Average:
      - Rainfall
      - Temperature
    - Temperature
      - Ranges
      - Variance
      - Extremes
      - Trends
    - Rainfall:
      - Extremes
      - Frequency
      - Distribution
    - Count of Hot Days
  - **Time-based Queries**: Analyze weather data for specific conditions and timeframes.

## Schema
The `daily_weather_info` table schema:

```sql
CREATE TABLE IF NOT EXISTS daily_weather_info (
    ID DATE NOT NULL PRIMARY KEY,
    rainfall_mm TINYINT UNSIGNED NOT NULL,
    min_temp_cel TINYINT NOT NULL,
    time_min_temp TIME NOT NULL,
    max_temp_cel TINYINT NOT NULL,
    time_max_temp TIME NOT NULL
);
  ```

## **Sample Data**
Insert sample data for the last thirsty days:
```sql
INSERT INTO daily_weather_info (ID, rainfall_mm, min_temp_cel, time_min_temp, max_temp_cel, time_max_temp)
VALUES 
(CURDATE() - INTERVAL 1 DAY, 12, 15, '05:30:00', 28, '14:00:00'),
(CURDATE() - INTERVAL 2 DAY, 5, 17, '06:00:00', 32, '15:30:00'),
-- Add more data for the remaining days
  ```

### **Usage**
Follow these steps to utilize the Daily Weather Data Tracker:
1. **Create the Database**:
   - use the provided schema to create the `daily_weather_info` table.
2. **Insert Sample Data**:
   - Populate the table with sample data for the last 30 days.
3. **Run Queries**: Execute the queries provided in `CalcSql` file to analyze and extract weather information.

## Queries
The necessary SQL queries for analyzing the weather data providde in `CalcSql` within this repository includes:
- Converting rainfall to inches
- Identify days with rainfall in inches on hot days(temperature above 32°C)
- Sorting days with rainfall in inches on hot days from highest to lowest
- Converting maximum and minimum temperatures to Fahrenheit
- Calculating temperature ranges in Fahrenheit
- Average rainfall
- Counting days with
  - Rainfall over 10mm
  - High temperatures and any rainfall
- Retrieving weather details for specific dates, such as last week or yesterday
- Calculating time difference between minimum and maximum temperatures
- Determining the average time for maximum temperature
- Finding differences from the average time for maximum temperature
- Computing the percentage of total rainfall 

## Contributing
Submit a pull request or open an issue. 

### License
[GNU General Public License](https://www.gnu.org/licenses/gpl-3.0.txt)
