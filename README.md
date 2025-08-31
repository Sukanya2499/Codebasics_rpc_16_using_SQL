# Codebasics_rpc_16_using_SQL
** Overall Avg AQI:

Query: select round(avg(aqi_value),2) as Overall_Avg_AQI from aqi_data;

Result: 

<img width="171" height="55" alt="Screenshot (348)" src="https://github.com/user-attachments/assets/946a40d7-600f-4043-90bd-2658eea12ed8" />

** Avg AQI in Last 6 Months:

Query: SELECT 
    ROUND(AVG(aqi_value),2) AS avg_aqi_last_6_months
FROM aqi_data
WHERE _date >= DATE_SUB(
                    (SELECT MAX(_date) FROM aqi_data), 
                    INTERVAL 6 MONTH
                  )
  AND _date <= (SELECT MAX(_date) FROM aqi_data);

  Result:
  
<img width="165" height="47" alt="Screenshot (340)" src="https://github.com/user-attachments/assets/e7e900b8-d808-4c38-8080-ef6d8849ec25" />
