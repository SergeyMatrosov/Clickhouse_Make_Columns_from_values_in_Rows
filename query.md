```sql
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
```
