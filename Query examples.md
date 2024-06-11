## 1. Select all female readers: 

```
SELECT *
FROM Reader
WHERE SUBSTRING(PESEL, 11, 1) IN ('0', '2', '4', '6', '8')
```

## 2. Select all adult readers:

```
SELECT *
FROM Reader
WHERE	  CASE WHEN SUBSTRING(PESEL, 3, 1) IN ('2', '3')
		          THEN '20'
		          ELSE '19'
		    END
		    + SUBSTRING(PESEL, 1, 2) + '-' + 
		    CASE  WHEN SUBSTRING(PESEL, 3, 1) = '2' THEN '0'
		          WHEN SUBSTRING(PESEL, 3, 1) = '3' THEN '1'
		          ELSE SUBSTRING(PESEL, 3, 1) END
        + SUBSTRING(PESEL, 4, 1) + '-'
        + SUBSTRING(PESEL, 5, 2) < DATEADD(YY, -18, GETDATE())
```
