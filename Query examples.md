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
SELECT COUNT(*) AS HowMuchBookIsRentNow
FROM Rent
WHERE DateofReturn IS NULL
```

## 4. What book titles were borrowed in March?

```
SELECT b.Title
FROM Rent r
	INNER JOIN Book b
		ON b.ID = r.BookID
WHERE MONTH(r.DateofRent) = 3
```

## 5. In which category were the most books borrowed?

```
SELECT TOP 1 x.NameCategory, COUNT(*) as HowMany
FROM
	(SELECT c.NameCategory
	FROM Rent r
		INNER JOIN Book b
			ON b.ID = r.BookID
		INNER JOIN BookCategory bc
			ON b.ID = bc.BookID
		INNER JOIN Category c
			ON c.ID = bc.CategoryID) x
GROUP BY x.NameCategory
ORDER BY HowMany DESC
```

## 6. Which librarian borrowed the most books this year?

```
SELECT Top 1 l.FirstName + ' ' + l.Surname AS Librarian
	FROM Librarian l
		INNER JOIN Rent r
			ON l.ID = r.LibrarianID
	WHERE Year(r.DateofRent) = Year(GETDATE())
	ORDER BY r.LibrarianID DESC
```

## 7. How many books did a librarian named "Victor" borrow in the first quarter of this year depending on the month?  

```
SELECT COUNT(r.BookID) AS HowManyBooks,
	MONTH(DateofRent) as InWhichMonth
FROM Rent r
	INNER JOIN Librarian l
		ON l.ID = r.LibrarianID
WHERE l.FirstName = 'Victor'
	AND YEAR(DateofRent) = YEAR(GETDATE())
GROUP BY MONTH(DateofRent)
HAVING MONTH(DateofRent) IN (1,2,3)
ORDER BY InWhichMonth
```

## 8. Which reader borrowed the most books during the summer months?  

```
SELECT (r.firstname + ' ' + r.surname) AS NameOfReader, x.HowMany
FROM Reader r
	INNER JOIN (
		SELECT TOP 1 ReaderID, COUNT(BookID) AS HowMany
		FROM Rent	
		WHERE MONTH(DateofRent) IN (6,7,8)
		GROUP BY ReaderID
		ORDER BY HowMany DESC ) AS x
	ON r.id = x.ReaderID
```
