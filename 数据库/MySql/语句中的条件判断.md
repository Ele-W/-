







```mysql
SELECT
CASE
	WHEN alias = '' THEN
	name ELSE alias 
	END AS name,
FROM
	mapconfig 
WHERE
```

