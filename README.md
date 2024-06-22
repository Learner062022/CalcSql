# CalcSql

## Description
This project involves creating and querying a database table to store daily wheather information, including rainfall, temperatures and the times they were recorded.

## Schema
The `daily_weather_info` table schema:

```sql
CREATE TABLE IF NOT EXISTS daily_weather_info (
    ID DATE NOT NULL PRIMARY KEY,
    rainfall_mm TINYINT UNSIGNED,
    min_temp_cel TINYINT UNSIGNED,
    time_min_temp TIME,
    max_temp_cel TINYINT UNSIGNED,
    time_max_temp TIME
);
  ```

## **Sample Data**
Insert fake data for the last thirsty days:
```sql
INSERT INTO daily_weather_info (ID, rainfall_mm, min_temp_cel, time_min_temp, max_temp_cel, time_max_temp)
VALUES 
(CURDATE() - INTERVAL 1 DAY, 12, 15, '05:30:00', 28, '14:00:00'),
(CURDATE() - INTERVAL 2 DAY, 5, 17, '06:00:00', 32, '15:30:00'),
-- Add more data for the remaining days
  ```

## **Queries**
### **Rainfall in Inches**
```sql
SELECT rainfall_mm * 25.4 AS rainfall_inches 
FROM daily_weather_info;
  ```

### **Rainfall in Inches on Hot Days**
```sql
SELECT rainfall_mm * 25.4 AS rainfall_inches 
FROM daily_weather_info 
WHERE min_temp_cel > 32;
```

### **Rainfall in Inches on Hot Days, Sorted**
```sql
SELECT ID AS day, rainfall_mm * 25.4 AS rainfall_inches 
FROM daily_weather_info 
WHERE min_temp_cel > 32 
ORDER BY rainfall_mm * 25.4 DESC;
  ```

### **Maximum Temperature in Fahrenheit**
```sql
SELECT ID AS day, ((max_temp_cel * 1.8) + 32) AS max_temp_f 
FROM daily_weather_info;
  ```

### **Temperature Range in Fahrenheit**
```sql
SELECT ID AS day, ((max_temp_cel * 1.8) + 32) AS max_temp_f, 
       ((min_temp_cel * 1.8) + 32) AS min_temp_f 
FROM daily_weather_info;
  ```

### **Average Rainfall**
```sql
SELECT ID AS day, ((max_temp_cel * 1.8) + 32) - ((min_temp_cel * 1.8) + 32) AS temp_range_f 
FROM daily_weather_info;
  ```

### **Days with Rainfall Over 10mm**
```sql
SELECT COUNT(ID) AS days_rainfall_over_10mm 
FROM daily_weather_info 
WHERE rainfall_mm > 10;
  ```

### **Days with High Temperatures and Any Rainfall**
```sql
SELECT COUNT(ID) AS days_rainfall_and_max_temp_over_30 
FROM daily_weather_info 
WHERE max_temp_cel > 30 AND rainfall_mm > 0;
  ```

### **Weather Details for Last Week**
```sql
SELECT * 
FROM daily_weather_info 
WHERE ID = CURDATE() - INTERVAL 1 WEEK;
  ```

### **Time Between Min and Max Temperature**
```sql
SELECT TIMEDIFF(time_max_temp, time_min_temp) AS temp_recorded_diff 
FROM daily_weather_info 
WHERE ID = CURDATE() - INTERVAL 1 DAY;
  ```

### **Time Between Min and Max Temperature Yesterday**
```sql
SELECT TIMEDIFF(time_max_temp, time_min_temp) AS temp_diff 
FROM daily_weather_info 
WHERE ID = CURDATE() - INTERVAL 1 DAY;
  ```

### **Time Differences for Each Day**
```sql
SELECT ID AS day, TIMEDIFF(time_max_temp, time_min_temp) AS temp_diff 
FROM daily_weather_info;
  ```

### **Average Time for Max Temperature**
```sql
SELECT AVG(time_max_temp) AS avg_time_max_temp 
FROM daily_weather_info;
  ```

### **Difference from Average Time for Max Temperature**
```sql
SELECT TIMEDIFF(time_max_temp, 
               (SELECT AVG(time_max_temp) 
                FROM daily_weather_info)) AS diff_avg_time_max_temp 
FROM daily_weather_info;
  ```

### **Percentage of Total Rainfall**
```sql
SELECT ID AS day, rainfall_mm / (SELECT SUM(rainfall_mm) 
                                 FROM daily_weather_info) * 100 AS rainfall_pct 
FROM daily_weather_info;
  ```

## **Setup Instructions**
1. **Create the database:**
   - Use the provided schema to create the `daily_weather_info` table.
2. **Insert Sample Data:**
   - Populate the table with fake data for the last thirty days using the sample data provided.
  
# License
[GNU General Public License](https://www.gnu.org/licenses/gpl-3.0.txt)
