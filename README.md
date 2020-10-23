# Clickhouse_Query
```
SELECT userid,
       arrayJoin(campaign) AS campaign,
       arrayJoin(term) AS term
FROM
  (SELECT userid,
          groupArray(campaign) AS campaign,
          groupArray(term) AS term
   FROM
     (SELECT userid,
             CASE
                 WHEN property='utm_campaign' THEN name
                 ELSE NULL
             END AS campaign,
             CASE
                 WHEN property='utm_term' THEN name
                 ELSE NULL
             END AS term
      FROM
        (WITH '111' AS userid,
              'utm_campaign' AS property,
              'abra-cadabra' AS name SELECT userid,
                                            property,
                                            name
         UNION ALL WITH '111' AS userid,
                        'utm_term' AS property,
                        'hui' AS name SELECT userid,
                                             property,
                                             name
         UNION ALL WITH '222' AS userid,
                        'utm_campaign' AS property,
                        'abra-cadabra' AS name SELECT userid,
                                                      property,
                                                      name
         UNION ALL WITH '222' AS userid,
                        'utm_term' AS property,
                        'hui' AS name SELECT userid,
                                             property,
                                             name))
   GROUP BY userid)
```
