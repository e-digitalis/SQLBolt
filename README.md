# SQLBolt
https://sqlbolt.com/




#SELECT statements - SELECT queries 101
- Find the title of each film ✓
- Find the director of each film ✓
- Find the title and director of each film ✓
- Find the title and year of each film ✓
- Find all the information about each film ✓

SELECT title FROM movies;
SELECT director FROM movies;
SELECT title, director FROM movies;
SELECT title, year FROM movies;
SELECT * FROM movies;

#Queries with constraints (Pt. 1)
- Find the movie with a row id of 6 ✓
- Find the movies released in the years between 2000 and 2010 ✓
- Find the movies not released in the years between 2000 and 2010 ✓
- Find the first 5 Pixar movies and their release  year ✓

SELECT * FROM movies WHERE id = 6;
SELECT * FROM movies WHERE year BETWEEN 2000 AND 2010;
SELECT * FROM movies WHERE year NOT BETWEEN 2000 AND 2010;
SELECT * FROM movies WHERE id <= 5;

#Queries with constraints (Pt. 2)
- Find all the Toy Story movies ✓
- Find all the movies directed by John Lasseter ✓
- Find all the movies (and director) not directed by John Lasseter ✓
- Find all the WALL-* movies ✓

SELECT * FROM movies WHERE title LIKE "%Toy Story%";
SELECT * FROM movies WHERE director LIKE "John Lasseter";
SELECT * FROM movies WHERE director NOT LIKE "John Lasseter";
SELECT * FROM movies WHERE title LIKE "%WALL%";

#Filtering and sorting Query results
- List all directors of Pixar movies (alphabetically), without duplicates ✓
- List the last four Pixar movies released (ordered from most recent to least) ✓
- List the first five Pixar movies sorted alphabetically ✓
- List the next five Pixar movies sorted alphabetically ✓

SELECT DISTINCT director FROM movies ORDER BY director ASC;
SELECT DISTINCT title FROM movies ORDER BY year DESC LIMIT 4;
SELECT title FROM movies ORDER BY title ASC LIMIT 5;
SELECT title FROM movies ORDER BY title ASC LIMIT 5 OFFSET 5;

#Simple SELECT Queries
- List all the Canadian cities and their populations ✓
- Order all the cities in the United States by their latitude from north to south ✓
- List all the cities west of Chicago, ordered from west to east ✓
- List the two largest cities in Mexico (by population) ✓
- List the third and fourth largest cities (by population) in the United States and their population ✓

SELECT city, population FROM north_american_cities WHERE country = "Canada";
SELECT city FROM north_american_cities WHERE country = "United States" ORDER BY latitude DESC;
SELECT city FROM north_american_cities WHERE Longitude < -87.629798 ORDER BY Longitude ASC;
SELECT * FROM north_american_cities WHERE country = "Mexico" ORDER BY population DESC LIMIT 2;
SELECT city FROM north_american_cities WHERE country = "Mexico" ORDER BY population DESC LIMIT 2
SELECT city FROM north_american_cities WHERE country = "United States" ORDER BY population DESC LIMIT 2 OFFSET 2;

BONUS: SELECT * FROM north_american_cities;

#Multi-table queries with JOINs
- Find the domestic and international sales for each movie ✓
- Show the sales numbers for each movie that did better internationally rather than domestically ✓
- List all the movies by their ratings in descending order ✓

SELECT Title, Domestic_sales, International_sales FROM Movies INNER JOIN Boxoffice ON Movies.Id = Boxoffice.Movie_id;
SELECT Title, Domestic_sales, International_sales FROM Movies INNER JOIN Boxoffice ON Movies.Id = Boxoffice.Movie_id WHERE International_sales > Domestic_sales;
SELECT Title, Rating FROM Movies INNER JOIN Boxoffice ON Movies.Id = Boxoffice.Movie_id ORDER BY Rating DESC;

BONUS: SELECT * FROM movies;

#OUTER JOINs
- Find the list of all buildings that have employees ✓
  - Find the list of all buildings and their capacity ✓
- List all buildings and the distinct employee roles in each building (including empty buildings) ✓

SELECT DISTINCT Building FROM Employees LEFT JOIN Buildings  ON Employees.Building = Buildings.Building_name;
SELECT Building_name, Capacity FROM Buildings;
SELECT DISTINCT Building_name, Role FROM Buildings LEFT JOIN Employees ON Buildings.Building_name  = Employees.Building;

BONUS: SELECT * FROM employees;

#A short note on NULLs
- Find the name and role of all employees who have not been assigned to a building ✓
- Find the names of the buildings that hold no employees ✓

SELECT  Name, Role FROM Employees LEFT JOIN Buildings ON Employees.Building = Buildings.Building_name WHERE Building IS NULL;
SELECT Building_name FROM Buildings LEFT JOIN Employees ON Buildings.Building_name  = Employees.Building WHERE Name IS NULL;

#Queries with expressions
- List all movies and their combined sales in millions of dollars ✓
- List all movies and their ratings in percent ✓

SELECT Title, (Domestic_sales + International_sales) / 1000000 AS Millions_of_dollars FROM movies INNER JOIN boxoffice ON Movies.Id = Boxoffice.Movie_id;
SELECT Title, (Rating/10)*100 AS Rating_percentage FROM movies INNER JOIN boxoffice ON Movies.Id = Boxoffice.Movie_id;

#Queries with aggregates (Pt. 1)
- Find the longest time that an employee has been at the studio ✓
- For each role, find the average number of years employed by employees in that role ✓
- Find the total number of employee years worked in each building ✓

SELECT MAX(Years_employed) FROM employees;
SELECT Role, AVG(Years_employed) FROM employees GROUP BY Role;
SELECT Building, SUM(Years_employed) AS Sum_of_years_employed FROM employees GROUP BY Building;

#Queries with aggregates (Pt. 2)
- Find the number of Artists in the studio (without a HAVING clause) ✓
- Find the number of Employees of each role in the studio ✓
- Find the total number of years employed by all Engineers ✓

SELECT Role, COUNT(Role) AS Number_of_Employees_PerRole FROM employees WHERE Role = 'Artist';
SELECT Role, COUNT(Role) AS Number_of_Employees_PerRole FROM employees GROUP BY Role;
SELECT Role, SUM(Years_employed)  AS Total_Number_of_Years_Engineers_Employed FROM employees WHERE Role = 'Engineer' GROUP BY Role;

#Order of execution of a Query
- Find the number of movies each director has directed ✓
- Find the total domestic and international sales that can be attributed to each director ✓

SELECT Director, COUNT(Title) AS Number_of_Movies_Directed FROM movies GROUP BY  Director;
SELECT Director, SUM(Domestic_sales) + SUM (International_sales) AS Gross_Earnings_Per_Director FROM movies JOIN Boxoffice ON Movies.ID = Boxoffice.Movie_id  GROUP BY  Director;

#Inserting rows
Add the studio's new production, Toy Story 4 to the list of movies (you can use any director) ✓
Toy Story 4 has been released to critical acclaim! It had a rating of 8.7, and made 340 million domestically and 270 million internationally. Add the record to the  BoxOffice table. ✓

INSERT INTO Movies VALUES (4, "Toy Story 4", "Elissa Bardia", 2017, 105);
INSERT INTO Boxoffice VALUES (4, 8.7, 340000000, 270000000);

#Updating rows
- The director for A Bug's Life is incorrect, it was actually directed by John Lasseter ✓
- The year that Toy Story 2 was released is incorrect, it was actually released in 1999 ✓
- Both the title and directory for Toy Story 8 is incorrect! The title should be "Toy Story 3" and it was directed by Lee Unkrich ✓

UPDATE movies SET Director = "John Lasseter" WHERE title = 	"A Bug's Life";
UPDATE movies SET Year = "1999" WHERE title = "Toy Story 2";
UPDATE movies SET Director = "Lee Unkrich", Title = "Toy Story 3" WHERE title = "Toy Story 8";

#Deleting rows
- This database is getting too big, lets remove all movies that were released before 2005. ✓
- Andrew Stanton has also left the studio, so please remove all movies directed by him. ✓

DELETE FROM Movies  WHERE Year <2005;
DELETE FROM Movies  WHERE Director = "Andrew Stanton";

#Creating tables
Create a new table named Database with the following columns:
- Name A string (text) describing the name of the database
- Version A number (floating point) of the latest version of this database
- Download_count An integer count of the number of times this database was downloaded
This table has no constraints. ✓

CREATE TABLE Database (Name TEXT, director TEXT, Version INTEGER, Download_count INTEGER);

#Altering tables
- Add a column named Aspect_ratio with a FLOAT data type to store the aspect-ratio each movie was released in. ✓
- Add another column named Language with a TEXT data type to store the language that the movie was released in. Ensure that the default for this language is English. ✓

ALTER TABLE movies ADD Aspect_ratio FLOAT;
ALTER TABLE movies ADD Language TEXT DEFAULT English;

#Dropping tables
- We have sadly reached the end of our lessons, let us clean up by removing the Movies table ✓
- And drop the BoxOffice table as well ✓

DROP TABLE IF EXISTS Movies;
DROP TABLE IF EXISTS BoxOffice;
