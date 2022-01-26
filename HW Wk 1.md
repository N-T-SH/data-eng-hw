***Below are solutions for Q3-6 for data-eng-zoomcamp***

Q3: 53024


```
SELECT
SUM(1)
FROM
    trips t
WHERE CAST(tpep_pickup_datetime AS DATE) = '2021-01-15';
```

Q4: "2021-01-20"

```
SELECT
CAST(tpep_pickup_datetime AS DATE),
MAX(tip_amount) as tip
FROM
    trips t
WHERE CAST(tpep_pickup_datetime AS DATE) >= '2021-01-15'
AND CAST(tpep_pickup_datetime AS DATE) <= '2021-01-31'
GROUP BY 1
ORDER BY tip DESC
LIMIT 1;
```

Q5: "Upper East Side South"


```
SELECT
    CAST(tpep_pickup_datetime AS DATE) as "day",
	zpu."Zone",
	zdo."Zone",
    COUNT(1) as "count"

FROM
    trips t
JOIN zones zpu
    ON t."PULocationID" = zpu."LocationID"
JOIN zones zdo
    ON t."DOLocationID" = zdo."LocationID"
WHERE CAST(tpep_pickup_datetime AS DATE) = '2021-01-14'
AND zpu."Zone" = 'Central Park'
GROUP BY
    1,2,3
ORDER BY COUNT(1) DESC;
```

Q6: "Alphabet City/Unknown"


```
SELECT
	CONCAT(
		(CASE WHEN zpu."Zone" IS NULL 
            THEN 'Unknown'
            ELSE zpu."Zone"
    END),'/',
		(CASE WHEN zdo."Zone" IS NULL 
            THEN 'Unknown'
            ELSE zdo."Zone"
    END))
	as "Pickup-DropOff",
    AVG(total_amount) as average_price

FROM
    trips t
JOIN zones zpu
    ON t."PULocationID" = zpu."LocationID"
JOIN zones zdo
    ON t."DOLocationID" = zdo."LocationID"
GROUP BY
    1
ORDER BY AVG(total_amount) DESC;