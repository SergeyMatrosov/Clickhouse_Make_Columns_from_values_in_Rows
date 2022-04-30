```sql
--Clickhouse
SELECT userid,
       arrayJoin(source) AS source,
       arrayJoin(campaign) AS campaign,
       arrayJoin(medium) AS medium
FROM
  (SELECT userid,
          groupArray(source) AS source,
          groupArray(campaign) AS campaign,
          groupArray(medium) AS medium
   FROM
     (
     SELECT userid,
             CASE
                 WHEN name='utm_source' THEN value
                 ELSE NULL
             END AS source,
             CASE
                 WHEN name='utm_campaign' THEN value
                 ELSE NULL
             END AS campaign,
             CASE
                 WHEN name='utm_medium' THEN value
                 ELSE NULL
             END AS medium
      FROM
        (WITH '111' AS userid, 'utm_source' AS name, 'Google' AS value 
         SELECT userid, name, value
        
         UNION ALL 
         WITH '111' AS userid, 'utm_campaign' AS name, 'Search_Promo' AS value 
         SELECT userid, name, value
         
         UNION ALL 
         WITH '111' AS userid, 'utm_medium' AS name, 'cpc' AS value 
         SELECT userid, name, value
         
         UNION ALL 
         WITH '222' AS userid, 'utm_source' AS name, 'Facebook' AS value 
         SELECT userid, name, value
         
         UNION ALL 
         WITH '222' AS userid, 'utm_campaign' AS name,'FB_Promo' AS value 
         SELECT userid, name, value
         
         UNION ALL WITH '222' AS userid, 'utm_medium' AS name,'cpc' AS value 
         SELECT userid, name, value))
   GROUP BY userid)
   
--Postgresql
WITH custom_data AS
       ( SELECT '111' AS userid, 'utm_source' AS name, 'Google' AS value 
        
         UNION ALL 
         SELECT '111', 'utm_campaign', 'Search_Promo'
         
         UNION ALL 
         SELECT '111', 'utm_medium', 'cpc'
         
         UNION ALL 
         SELECT '222', 'utm_source', 'Facebook'
         
         UNION ALL 
         SELECT '222', 'utm_campaign','FB_Promo' 
         
         UNION ALL 
         SELECT '222', 'utm_medium','cpc')
SELECT userid,
       arrayJoin(source) AS source,
       arrayJoin(campaign) AS campaign,
       arrayJoin(medium) AS medium
FROM
  (SELECT userid,
          array_to_string(source) AS source,
          array_to_string(campaign) AS campaign,
          array_to_string(medium) AS medium
   FROM
     (
     SELECT userid,
             CASE
                 WHEN name='utm_source' THEN value
                 ELSE NULL
             END AS source,
             CASE
                 WHEN name='utm_campaign' THEN value
                 ELSE NULL
             END AS campaign,
             CASE
                 WHEN name='utm_medium' THEN value
                 ELSE NULL
             END AS medium
      FROM custom_data) 
   GROUP BY userid)
GROUP BY userid
```
