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


** Top 5 States with High Avg AQI:

Query: select state, round(avg(aqi_value),2) as avg_aqi from aqi_data
group by state
order by avg_aqi desc
limit 5;

Result:

<img width="196" height="131" alt="Screenshot (335)" src="https://github.com/user-attachments/assets/7cc3e0d2-c239-47d8-9a6b-405fe3933e70" />


** 5 Cleanest States with Avg AQI:

Query: select state, round(avg(aqi_value),2) as avg_aqi from aqi_data
group by state
order by avg_aqi asc
limit 5;

Result:

<img width="270" height="132" alt="Screenshot (336)" src="https://github.com/user-attachments/assets/1a985457-3364-4a39-bf00-ac06a223b9c0" />


** Top 5 Polluted Areas with Avg AQI:

Query: select area, round(avg(aqi_value),2) as avg_aqi from aqi_data
group by area
order by avg_aqi desc
limit 5;

Result:

<img width="184" height="132" alt="Screenshot (337)" src="https://github.com/user-attachments/assets/af58401a-ac78-4dce-bda0-eec7bc1b4530" />

** Top 5 Cleanest Areas with Avg AQI:

Query: select area, round(avg(aqi_value),2) as avg_aqi from aqi_data
group by area
order by avg_aqi asc
limit 5;

Result:

<img width="192" height="135" alt="Screenshot (338)" src="https://github.com/user-attachments/assets/4f4b63be-4dbb-41e4-8528-1e0749d7afb2" />

** Top 5 States based on Avg AQI in Last 6 Months:

Query: SELECT 
    state,ROUND(AVG(aqi_value),2) AS avg_aqi_last_6_months
FROM aqi_data
WHERE _date >= DATE_SUB(
                    (SELECT MAX(_date) FROM aqi_data), 
                    INTERVAL 6 MONTH
                  )
  AND _date <= (SELECT MAX(_date) FROM aqi_data)
  group by state
  order by avg_aqi_last_6_months desc
  limit 5;

  Result:

  <img width="286" height="133" alt="Screenshot (341)" src="https://github.com/user-attachments/assets/59ff85bd-6d31-4fe1-b123-805de865c828" />


  ** Bottom 5 States with Avg AQI in Last 6 Months:

  Query: SELECT 
    state,ROUND(AVG(aqi_value),2) AS avg_aqi_last_6_months
FROM aqi_data
WHERE _date >= DATE_SUB(
                    (SELECT MAX(_date) FROM aqi_data), 
                    INTERVAL 6 MONTH
                  )
  AND _date <= (SELECT MAX(_date) FROM aqi_data)
  group by state
  order by avg_aqi_last_6_months asc
  limit 5;

Result:

<img width="363" height="134" alt="Screenshot (342)" src="https://github.com/user-attachments/assets/dcfdfc49-cfad-457a-84c4-fcec70f9406a" />

** Top 5 Areas based on Avg AQI in Last 6 Months:

Query: SELECT 
    area,ROUND(AVG(aqi_value),2) AS avg_aqi_last_6_months
FROM aqi_data
WHERE _date >= DATE_SUB(
                    (SELECT MAX(_date) FROM aqi_data), 
                    INTERVAL 6 MONTH
                  )
  AND _date <= (SELECT MAX(_date) FROM aqi_data)
  group by area
  order by avg_aqi_last_6_months desc
  limit 5;

Result:

<img width="260" height="132" alt="Screenshot (343)" src="https://github.com/user-attachments/assets/479aae5e-90ef-48c4-80bd-c5a7ac91bcf9" />


** Top 5 Cleaest Area with Avg AQI in Last 6 Months:

Query: SELECT 
    area,ROUND(AVG(aqi_value),2) AS avg_aqi_last_6_months
FROM aqi_data
WHERE _date >= DATE_SUB(
                    (SELECT MAX(_date) FROM aqi_data), 
                    INTERVAL 6 MONTH
                  )
  AND _date <= (SELECT MAX(_date) FROM aqi_data)
  group by area


Result:

<img width="286" height="135" alt="Screenshot (344)" src="https://github.com/user-attachments/assets/5eca8ade-948f-41fa-9664-681420724d9e" />

** Avg AQI on Week Days & Week Ends in Metro Cities:

  Query: SELECT 
    area,
    CASE 
        WHEN DAYOFWEEK(_date) IN (1,7) THEN 'Weekend'   
        ELSE 'Weekday'
    END AS day_type,
    ROUND(AVG(aqi_value),2) AS avg_aqi
FROM aqi_data
WHERE area IN ('Delhi','Ahmedabad','Pune','Mumbai',
               'Kolkata','Hydrabad','Bengaluru','Chennai')
GROUP BY area,
         CASE 
            WHEN DAYOFWEEK(_date) IN (1,7) THEN 'Weekend'
            ELSE 'Weekday'
         END
ORDER BY area, day_type;
  order by avg_aqi_last_6_months asc
  limit 5;

  Result:

  
<img width="240" height="323" alt="Screenshot (346)" src="https://github.com/user-attachments/assets/9f9a9aee-28de-4263-b55f-74c3033d8217" />

** Avg AQI on Week Days & Week Ends in Metro Cities in Last 6 Months:

Query: SELECT 
    area,
    ROUND(AVG(CASE WHEN DAYOFWEEK(_date) IN (1,7) THEN aqi_value END),2) AS weekend_avg_aqi,
    ROUND(AVG(CASE WHEN DAYOFWEEK(_date) NOT IN (1,7) THEN aqi_value END),2) AS weekday_avg_aqi
FROM aqi_data
WHERE area IN ('Delhi','Ahmedabad','Pune','Mumbai',
               'Kolkata','Hydrabad','Bengaluru','Chennai')
  AND _date BETWEEN DATE_SUB((SELECT MAX(_date) FROM aqi_data), INTERVAL 6 MONTH)
               AND (SELECT MAX(_date) FROM aqi_data)
GROUP BY area
ORDER BY area;

Result:

<img width="354" height="176" alt="Screenshot (347)" src="https://github.com/user-attachments/assets/dad05025-532f-4069-8b06-25d0a2e7c512" />

