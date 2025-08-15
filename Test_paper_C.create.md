## TEST_PAPER-C SCHEMA & DATA
```sql
CREATE DATABASE TestPaper_C;
```
```sql
USE TestPaper_C;
```
```sql
-- Table 1: MovieDetails
CREATE TABLE MovieDetails (
    MovieID INT PRIMARY KEY,
    Title VARCHAR(100) NOT NULL,
    Genre VARCHAR(100),
    Director VARCHAR(100),
    ReleaseYear INT
);
```
```sql
-- Table 2: MovieFinancials
CREATE TABLE MovieFinancials (
    FinancialID INT PRIMARY KEY,
    BudgetUSD DECIMAL(15,2),
    BoxOfficeUSD DECIMAL(15,2),
    MovieID INT,
    FOREIGN KEY (MovieID) REFERENCES MovieDetails(MovieID)
);
```
```sql
-- Table 3: MovieRatingsDuration
CREATE TABLE MovieRatingsDuration (
    RatingID INT PRIMARY KEY,
    DurationMin INT,
    Rating DECIMAL(3,1),
    Language VARCHAR(100),
    Country VARCHAR(100),
    MovieID INT,
    FOREIGN KEY (MovieID) REFERENCES MovieDetails(MovieID)
);
```
```sql
-- Insert into MovieDetails
INSERT INTO MovieDetails (MovieID, Title, Genre, Director, ReleaseYear) VALUES
(1, 'The Shawshank Redemption', 'Drama', 'Frank Darabont', 1994),
(2, 'The Godfather', 'Crime', 'Francis Ford Coppola', 1972),
(3, 'Inception', 'Sci-Fi', 'Christopher Nolan', 2010),
(4, 'Avengers: Endgame', 'Superhero', 'Anthony Russo', 2019),
(5, 'Parasite', 'Thriller', 'Bong Joon-ho', 2019),
(6, 'The Dark Knight', 'Action', 'Christopher Nolan', 2008),
(7, 'The Prestige', 'Drama', 'Christopher Nolan', 2006),
(8, 'Gladiator', 'Action', 'Ridley Scott', 2000),
(9, 'Interstellar', 'Sci-Fi', 'Christopher Nolan', 2014),
(10, 'Thor', 'Fantasy', 'Kenneth Branagh', 2011);
```
```sql
-- Insert into MovieFinancials
INSERT INTO MovieFinancials (FinancialID, BudgetUSD, BoxOfficeUSD, MovieID) VALUES
(1, 25000000.00, 28000000.00, 1),
(2, 6000000.00, 245000000.00, 2),
(3, 160000000.00, 829000000.00, 3),
(4, 356000000.00, 2798000000.00, 4),
(5, 11400000.00, 258000000.00, 5),
(6, 185000000.00, 1005000000.00, 6),
(7, 40000000.00, 109000000.00, 7),
(8, 103000000.00, 460000000.00, 8),
(9, 165000000.00, 677000000.00, 9),
(10, 150000000.00, 449000000.00, 10);
```
```sql
-- Insert into MovieRatingsDuration
INSERT INTO MovieRatingsDuration (RatingID, DurationMin, Rating, Language, Country, MovieID) VALUES
(1, 142, 9.3, 'English', 'USA', 1),
(2, 175, 9.2, 'English', 'USA', 2),
(3, 148, 8.8, 'English', 'USA', 3),
(4, 181, 8.4, 'English', 'USA', 4),
(5, 132, 8.6, 'Korean', 'South Korea', 5),
(6, 152, 9.0, 'English', 'USA', 6),
(7, 130, 8.5, 'English', 'USA', 7),
(8, 155, 8.5, 'English', 'UK', 8),
(9, 169, 8.6, 'English', 'USA', 9),
(10, 115, 7.0, 'English', 'USA', 10);
```

## Question
```sql
-- 1. Retrive first five distinct movies along with their title from MovieDetails table.
-- 2. Display the total of the BudgetUSD and BoxOfficeUSD assign the name TotalUSD from
-- MovieFinancials.
-- 3. Insert the new row with this data (11,The Incredible Hulk, Action, Louis Leterrier,2008) in
-- MovieDetails table.
-- 4. Set the value of the genre to 'Action' of 'Avengers:Endgame' movie from MovieDetails table.
-- 5. Delete the records with duration of 181 minutes from MovieRatingsDuration table.
-- 6. Add a new column 'Producer' into the MovieDetails table.
-- 7. Delete records of MovieFinancials table without removing its table structure.
-- 8. Retrive all the movies from MovieDetails table with title starting with 'The'.
-- 9. Retrive name of directors includes 'son' from MovieDetails table.
-- 10. Convert and display title of all movies in uppercase.
-- 11. Display the highest rating from the MovieRatingsDuration table.
-- 12. Calculate the years between current year and movies release year.
-- 13. Find the languages in which movies have an average rating of greater than 8.0. Display the language
-- and the average rating.
-- 14. Retrieve the minimum, maximum, and average movie duration for each language in the
-- MovieRatingsDuration table, but display only those languages where the average rating is greater
-- than 7.5.
-- 15. Find the titles of movies whose budget is higher than the average budget of all movies.(Do not use
-- JOINS)
-- 16. Find the titles of movies that have a box office revenue greater than the average box office revenue
-- of all movies.
-- 17. Create a view with Rating, Language and Country columns with no data and named it MovieReview.
-- 18. List all movies that have the same director but different genres, displaying the director’s name, both
-- movie titles, and their respective genres.
-- 19. Retrieve the title, director, and box office earnings for all movies that were released after 2010, along
-- with their ratings.
-- 20. List all directors and the number of movies they have directed, but only include directors who have
-- directed more than 1 movie.
```