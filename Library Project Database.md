```
CREATE DATABASE Library;

CREATE TABLE Book (
ID int IDENTITY NOT NULL PRIMARY KEY,
Title varchar(50) NOT NULL,
Author varchar(50) NOT NULL,
ISBN char(13) NOT NULL,
CoverType varchar(50)
);

CREATE TABLE Reader (
ID int IDENTITY NOT NULL PRIMARY KEY,
FirstName varchar(50) NOT NULL,
Surname varchar(50) NOT NULL,
Pesel varchar(11) NOT NULL,
Email varchar(50),
Telephone varchar (50)
);

CREATE TABLE Librarian (
ID int IDENTITY NOT NULL PRIMARY KEY,
FirstName varchar(50) NOT NULL,
Surname varchar(50) NOT NULL
);

CREATE TABLE Rent (
ID int IDENTITY NOT NULL PRIMARY KEY,
BookID int FOREIGN KEY REFERENCES Book(ID),
ReaderID int FOREIGN KEY REFERENCES Reader(ID),
LibrarianID int FOREIGN KEY REFERENCES Librarian(ID),
DateofRent datetime DEFAULT GETDATE(),
DateofReturn datetime
);

CREATE TABLE Category (
ID int IDENTITY PRIMARY KEY,
NameCategory varchar(50)
);

CREATE TABLE BookCategory (
BookID int FOREIGN KEY REFERENCES Book(ID),
CategoryID int FOREIGN KEY REFERENCES Category(ID)
);

CREATE VIEW BCNames AS
SELECT BookCategory.BookID, Book.Title, Category.NameCategory
FROM BookCategory
JOIN Book ON BookCategory.BookID = Book.ID
JOIN Category ON BookCategory.CategoryID = Category.ID
;

INSERT INTO Book (Title, Author, ISBN, CoverType) VALUES
('Singulair', 'Octavius Hood', '5167890914989', 'hardcover'),
('New York', 'Ainsley Rios', '2939931801441', 'softcover'),
('TriNessa', 'Xander Ashley', '5402464405367', 'softcover'),
('Nexium', 'Aiko Boyle', '6478000804768', 'hardcover'),
('Premarin', 'Finn Carey', '2242041701275', 'hardcover'),
('Lyrica', 'Preston Glenn', '1801440492624', 'hardcover'),
('California', 'Stone Boyd', '1052948097262', 'softcover'),
('Singapour', 'Willow Bright', '2196121677661', 'softcover'),
('Lyrica', 'Kareem Boyer', '7950194608368', 'softcover'),
('Perth', 'Marshall Scott', '3953613915231', 'hardcover');

INSERT INTO Librarian (FirstName, Surname) VALUES
('Dalton', 'Abbott')
('Victor', 'Stanley');

INSERT INTO Reader (FirstName, Surname, PESEL, Email, Telephone) VALUES
('Raphael', 'Irwin', '56051348835', 'porttitor.vulputate@aol.com', '1-723-318-2874'),
('Destiny', 'Cotton', '71121554916', 'dui.suspendisse.ac@outlook.couk', '1-761-774-1915'),
('Nehru', 'Moran', '09221320359', 'ligula.eu@google.org', '374-7047'),
('Eric', 'Best', '71040505783', 'ipsum.primis.in@hotmail.ca', '966-6318'),
('Herman', 'Tyson', '61072748241', 'nunc@protonmail.com', '517-9812'),
('Quynn', 'Stanton', '58041410005', 'dolor@hotmail.com', '1-230-807-3613'),
('Hiram', 'Benton', '54102963626', 'id.risus@hotmail.net', '1-604-752-1373'),
('Craig', 'Browning', '08210264535', 'aliquam.tincidunt@aol.com', '1-475-448-1811'),
('Fallon', 'Wooten', '56100706043', 'praesent.luctus.curabitur@aol.com', '1-422-862-8140'),
('Philip', 'Hunter', '80061789399', 'lectus.cum@hotmail.couk', '824-5830'),
('Lani', 'Parrish', '76100396002', 'risus.donec@aol.couk', '643-7497'),
('Quentin', 'Tucker', '78030993210', 'in.at@yahoo.com', '312-0262'),
('Violet', 'Head', '91082002044', 'pede.cum.sociis@icloud.com', '1-311-976-4531'),
('Astra', 'Rocha', '57090718478', 'non.nisi.aenean@yahoo.couk', '1-533-259-8842'),
('Omar', 'Delgado', '99020991991', 'laoreet.posuere@aol.com', '769-5398');

INSERT INTO Rent (BookID, ReaderID, LibrarianID, DateofRent, DateofReturn) VALUES 
(1, 10, 2, GETDATE(), NULL),
(8, 9, 2, GETDATE(), '2024-01-10T16:50:12.137'),
(2, 2, 2, GETDATE(), NULL),
(5, 2, 2, GETDATE(), NULL),
(4, 4, 1, GETDATE(), NULL),
(3, 3, 2, GETDATE(), NULL),
(6, 7, 2, GETDATE(), NULL),
(9, 6, 2, GETDATE(), NULL),
(7, 1, 1, GETDATE(), NULL),
(10, 12, 1, GETDATE(), NULL);

INSERT INTO Category (NameCategory) VALUES
('Science'),
('PopScience'),
('Fantasy'),
('Criminal'),
('Poetry'),
('Novel'),
('Biography'),
('Fiction'),
('NonFiction'),
('Other');

INSERT INTO BookCategory (BookID, CategoryID) VALUES
(1, 6),
(2, 1),
(3, 2),
(4, 8),
(5, 5),
(6, 9),
(7, 3),
(8, 7),
(9, 10),
(10, 4);

```
