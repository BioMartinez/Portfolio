## 1. A stored procedure that adds a new reader:

```
  CREATE PROCEDURE AddNewReader
	@FirstName varchar(50),
	@Surname varchar(50),
	@Pesel varchar(11),
	@Email varchar(50) = NULL,
	@Telephone varchar(50) = NULL
AS
BEGIN
	INSERT INTO Reader(FirstName, Surname, Pesel, Email, Telephone)
		VALUES(@FirstName, @Surname, @Pesel, @Email, @Telephone)
END
```

## 2. A stored procedure that adds a new rent of book and adds a new reader if it does not exist

```
CREATE PROCEDURE AddNewRent
	@BookID int,
	@LibrarianID int,
	@ReaderID int = 0,
	@FirstName varchar(50) = NULL,
	@Surname varchar(50) = NULL,
	@Email varchar(50) = NULL,
	@Telephone varchar(50) = NULL,
	@Pesel varchar(11) = NULL
AS
BEGIN
	IF @ReaderID = 0 AND @Pesel IS NULL
	BEGIN
		PRINT 'You have not provided a reader ID nor entered data for a new one'
	END
	ELSE
	BEGIN
		IF @ReaderID = 0
		BEGIN
			EXEC AddNewReader @FirstName, @Surname, @Email, @Telephone, @Pesel
			SELECT @ReaderID = ID FROM Reader WHERE Pesel = @pesel
		END

		INSERT INTO Rent(BookID, ReaderID, LibrarianID, DateofRent)
		VALUES(@BookID, @ReaderID, @LibrarianID, GETDATE())
	END
END
```

## 3. A stored procedure that adds the return of the book

```
CREATE PROCEDURE AddReturnOfBook
	@BookID int
AS
BEGIN
	UPDATE Rent
	SET DateofReturn = GETDATE()
	WHERE BookID = @BookID
END
```
