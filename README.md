# CalcSql

## Description
This project involves creating and querying a database table to store daily wheather information, including rainfall, temperatures and the times they were recorded.

## Schema
The `daily_weather_information` table schema:

```sql
CREATE TABLE IF NOT EXISTS daily_weather_information (
    ID DATE NOT NULL PRIMARY KEY,
    rainfall_mm DECIMAL(5, 2) UNSIGNED,
    min_temp_celcius DECIMAL(5, 2),
    time_min_temp_recorded TIME,
    max_temp_celcius DECIMAL(5, 2) UNSIGNED,
    time_max_temp_recorded TIME
);
  ```

## **Sample Data**
Insert fake data for the last thirsty days:
```sql
INSERT INTO daily_weather_information (ID, rainfall_mm, min_temp_celcius, time_min_temp_recorded, max_temp_celcius, time_max_temp_recorded)
VALUES 
('2024-05-01', 12.5, 15.2, '05:30:00', 28.3, '14:00:00'),
('2024-05-02', 5.0, 17.0, '06:00:00', 32.1, '15:30:00'),
-- Add more data for the remaining days
  ```

## **Queries**
### **Rainfall in Inches**
```sql
SELECT ID, rainfall_mm / 25.4 AS rainfall_in_inches FROM daily_weather_information;
  ```

### **Rainfall in Inches on Hot Days**
```sql
SELECT ID, rainfall_mm / 25.4 AS rainfall_in_inches 
FROM daily_weather_information 
WHERE max_temp_celcius > 32;
  ```

### **Rainfall in Inches on Hot Days, Sorted**
```sql
SELECT ID, rainfall_mm / 25.4 AS rainfall_in_inches 
FROM daily_weather_information 
WHERE max_temp_celcius > 32 
ORDER BY rainfall_mm DESC;
  ```

### Maximum Temperature in Fahrenheit
```sql
SELECT ID, (max_temp_celcius * 1.8 + 32) AS max_temp_fahrenheit 
FROM daily_weather_information;
  ```

### **Temperature Range in Fahrenheit**
```sql
SELECT ID, 
       (max_temp_celcius * 1.8 + 32) - (min_temp_celcius * 1.8 + 32) AS temp_range_fahrenheit 
FROM daily_weather_information;
  ```

### **Average Rainfall**
```sql
SELECT AVG(rainfall_mm) AS average_rainfall FROM daily_weather_information;
  ```

### **Days with Rainfall Over 10mm**
```sql
SELECT COUNT(ID) AS days_rainfall_over_10mm 
FROM daily_weather_information 
WHERE rainfall_mm > 10;
  ```

### **Days with High Temperatures and Any Rainfall**
```sql
SELECT COUNT(ID) AS days_max_temp_over_30_with_rainfall 
FROM daily_weather_information 
WHERE max_temp_celcius > 30 AND rainfall_mm > 0;
  ```

### **Weather Details for Last Week**
```sql
SELECT * 
FROM daily_weather_information 
WHERE ID = CURDATE() - INTERVAL 1 WEEK;
  ```

### **Time Between Min and Max Temperature**
```sql
SELECT TIMEDIFF(time_max_temp_recorded, time_min_temp_recorded) AS temp_recorded_time_diff 
FROM daily_weather_information 
WHERE ID = '2024-05-01';
  ```

### **Time Between Min and Max Temperature Yesterday**
```sql
SELECT TIMEDIFF(time_max_temp_recorded, time_min_temp_recorded) AS temp_recorded_time_diff 
FROM daily_weather_information 
WHERE ID = CURDATE() - INTERVAL 1 DAY;
  ```

### **Time Differences for Each Day**
```sql
SELECT ID, TIMEDIFF(time_max_temp_recorded, time_min_temp_recorded) AS temp_recorded_time_diff 
FROM daily_weather_information;
  ```

### **Average Time for Max Temperature**
```sql
SELECT AVG(time_max_temp_recorded) AS avg_time_max_temp_recorded 
FROM daily_weather_information;
  ```

### **Difference from Average Time for Max Temperature**
```sql
SELECT ID, 
       TIMEDIFF(time_max_temp_recorded, (SELECT AVG(time_max_temp_recorded) FROM daily_weather_information)) AS time_diff_from_avg 
FROM daily_weather_information;
  ```

### **Percentage of Total Rainfall**
```sql
SELECT ID, 
       (rainfall_mm / (SELECT SUM(rainfall_mm) FROM daily_weather_information) * 100) AS rainfall_percentage 
FROM daily_weather_information;
  ```

## **Setup Instructions**
1. **Create the database:**
   - Use the provided schema to create the `daily_weather_information` table.
2. **Insert Sample Data:**
   - Populate the table with fake data for the last thirty days using the sample data provided.
  
# License
[GNU General Public License](https://www.gnu.org/licenses/gpl-3.0.txt)
