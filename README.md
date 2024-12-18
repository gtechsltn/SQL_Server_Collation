# SQL Server Collation
+ Case Insensitive
+ Case Sensitive
+ Case Sensitivity

Collations and Case Sensitivity

https://learn.microsoft.com/en-us/ef/core/miscellaneous/collations-and-case-sensitivity

Collation and Unicode support

https://learn.microsoft.com/en-us/sql/relational-databases/collations/collation-and-unicode-support

View Collation Information

https://learn.microsoft.com/en-us/sql/relational-databases/collations/view-collation-information


```
USE [master]
GO

DROP DATABASE [Test]
GO

CREATE DATABASE [Test] COLLATE SQL_Latin1_General_CP1_CS_AS;
GO

USE [Test]
GO

--SELECT name, collation_name 
--FROM sys.databases
--WHERE name = 'Test'

--ALTER DATABASE [Test] COLLATE Latin1_General_CI_AS;
--ALTER DATABASE [Test] COLLATE Latin1_General_CS_AS;

--ALTER DATABASE [Test] COLLATE SQL_Latin1_General_CP1_CI_AI;
--ALTER DATABASE [Test] COLLATE SQL_Latin1_General_CP1_CS_AS;

-- You can get a list of all available collations on the server using:
SELECT * FROM ::fn_helpcollations()

-- You can get the current server's collation using:
SELECT SERVERPROPERTY ('Collation')



--DROP TABLE [dbo].[Users]

CREATE TABLE [dbo].[Users]
(
	[Id] bigint PRIMARY KEY NOT NULL IDENTITY (1, 1),
	[Name] nvarchar(255) NULL
)

--DROP TABLE [dbo].[Persons]

CREATE TABLE [dbo].[Persons]
(    
	RowID int NOT NULL IDENTITY (1, 1),
	FirstName nvarchar(30) NOT NULL,
	LastName nvarchar(30) NOT NULL
)
GO

ALTER TABLE [dbo].[Persons] ADD CONSTRAINT [PK_Person] PRIMARY KEY CLUSTERED (RowID);
GO



-- How I can obtain the collation of a specific table in a database?
-- https://stackoverflow.com/questions/2304992/how-i-can-obtain-the-collation-of-a-specific-table-in-a-database

SELECT c.name, 
    c.collation_name
FROM sys.columns c
JOIN sys.tables t ON t.object_id = c.object_id
WHERE t.name = 'Users'



-- How I can obtain the collation of a specific table in a database?
-- https://stackoverflow.com/questions/2304992/how-i-can-obtain-the-collation-of-a-specific-table-in-a-database

SELECT c.name, 
    c.collation 
FROM sys.syscolumns c
WHERE [id] = OBJECT_ID('Persons')



-- INSERT DATA
-- ====================================================================================================

INSERT INTO [dbo].[Users] ([Name]) VALUES(N'mẠnH')
INSERT INTO [dbo].[Persons] ([FirstName], [LastName]) VALUES(N'mẠnH', N'nGuyễn vIẾt')

-- Case insensitive
-- ====================================================================================================

SELECT * FROM [dbo].[Users] WHERE [Name] = N'Mạnh' --> Case insensitive (không phân biệt hoa thường)

SELECT * FROM [dbo].[Users] WHERE UPPER([Name]) LIKE UPPER(N'%Mạnh%')

SELECT * FROM [dbo].[Users] WHERE LOWER([Name]) LIKE LOWER(N'%Mạnh%')

-- Case insensitive Latin1_General_CI_AS

SELECT * FROM [dbo].[Users] WHERE [Name] = N'Mạnh' COLLATE Latin1_General_CI_AS --> Case insensitive (không phân biệt hoa thường)

SELECT * FROM [dbo].[Users] WHERE [Name] COLLATE Latin1_General_CI_AS = N'Mạnh' --> Case insensitive (không phân biệt hoa thường)

-- Case insensitive SQL_Latin1_General_CP1_CI_AS

SELECT * FROM [dbo].[Users] WHERE [Name] = N'Mạnh' COLLATE SQL_Latin1_General_CP1_CI_AS --> Case insensitive (không phân biệt hoa thường)

SELECT * FROM [dbo].[Users] WHERE [Name] COLLATE SQL_Latin1_General_CP1_CI_AS = N'Mạnh' --> Case insensitive (không phân biệt hoa thường)



-- Case sensitive
-- ====================================================================================================
-- Case sensitive Latin1_General_CS_AS

SELECT * FROM [dbo].[Users] WHERE [Name] = N'Mạnh' COLLATE Latin1_General_CS_AS --> Case sensitive (có phân biệt hoa thường)

SELECT * FROM [dbo].[Users] WHERE [Name] COLLATE Latin1_General_CS_AS = N'Mạnh' --> Case sensitive (có phân biệt hoa thường)

-- Case sensitive SQL_Latin1_General_CP1_CS_AS

SELECT * FROM [dbo].[Users] WHERE [Name] = N'Mạnh' COLLATE SQL_Latin1_General_CP1_CS_AS --> Case sensitive (có phân biệt hoa thường)

SELECT * FROM [dbo].[Users] WHERE [Name] COLLATE SQL_Latin1_General_CP1_CS_AS = N'Mạnh' --> Case sensitive (có phân biệt hoa thường)


--Normally SQL Server is not case sensitive. So 'ABC' = 'abc' is true in a WHERE clause.
--To make a where clause case sensitive, you can use COLLATE. 

--SELECT
--    [Name],
--    [ConvertFirstLettertoCapital]([Address]) AS [Address_Standard],
--    UPPER([Address]) AS [Address_Upper],
--    LOWER([Address]) AS [Address_Lower],
--    [Address],
--    [City],
--    [State]
--FROM [Address_List]
--WHERE
--    [Address] = UPPER([Address]) COLLATE SQL_Latin1_General_CP1_CS_AS
--    OR
--    [Address] = LOWER([Address]) COLLATE SQL_Latin1_General_CP1_CS_AS

-- Case sensitive example
SELECT *
FROM [dbo].[Users] 
WHERE [Name] collate Latin1_General_CS_AI like N'%Mạnh%'

-- Case insensitive example
SELECT *
FROM [dbo].[Users] 
WHERE [Name] collate Latin1_General_CI_AI like N'%Mạnh%'
```
