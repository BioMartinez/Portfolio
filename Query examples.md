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

## 3. How many books are currently on loan?

```
SELECT COUNT(*) AS HowMuchBookIsRentNow FROM Rent
WHERE DateofReturn IS NULL
```

## 4. In which category were the most books borrowed?

```
SELECT TOP 1 x.NameCategory, COUNT(*) as HowMany
FROM
(SELECT c.NameCategory FROM Rent r
INNER JOIN Book b
ON b.ID = r.BookID
INNER JOIN BookCategory bc
ON b.ID = bc.BookID
INNER JOIN Category c
ON c.ID = bc.CategoryID) x
GROUP BY x.NameCategory
ORDER BY HowMany DESC
```
