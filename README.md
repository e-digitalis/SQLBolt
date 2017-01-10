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


BONUS: SELECT * FROM movies;
