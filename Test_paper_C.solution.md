```sql
-- 1. Retrive first five distinct movies along with their title from MovieDetails table.
SELECT
    DISTINCT TOP 5 
    MovieDetails.Title
FROM MovieDetails
```
|Title|
|:----:|
|Avengers: Endgame|
|Gladiator|
|Inception|
|Interstellar|
|Parasite|
```sql
-- 2. Display the total of the BudgetUSD and BoxOfficeUSD assign the name TotalUSD from
-- MovieFinancials.
SELECT 
    (BudgetUSD + BoxOfficeUSD) AS TotalUSD
FROM MovieFinancials;
```
|TotalUSD|
|:-------:|
|53000000.00|
|251000000.00|
|989000000.00|
|3154000000.00|
|269400000.00|
|1190000000.00|
|149000000.00|
|563000000.00|
|842000000.00|
|599000000.00|
```sql
-- 3. Insert the new row with this data (11,The Incredible Hulk, Action, Louis Leterrier,2008) in
-- MovieDetails table.
INSERT INTO MovieDetails (MovieID, Title, Genre, Director, ReleaseYear)
VALUES (11, 'The Incredible Hulk', 'Action', 'Louis Leterrier', 2008);
```
```sql
-- 4. Set the value of the genre to 'Action' of 'Avengers:Endgame' movie from MovieDetails table.
UPDATE MovieDetails
SET MovieDetails.Genre = 'Action'
WHERE MovieDetails.Title = 'Avengers: Endgame';
```
```sql
-- 5. Delete the records with duration of 181 minutes from MovieRatingsDuration table.
BEGIN TRANSACTION
DELETE FROM MovieRatingsDuration
WHERE MovieRatingsDuration.DurationMin = 181;
ROLLBACK TRANSACTION
```
```sql
-- 6. Add a new column 'Producer' into the MovieDetails table.
ALTER TABLE MovieDetails
ADD Producer VARCHAR(100);
```
```sql
-- 7. Delete records of MovieFinancials table without removing its table structure.
BEGIN TRANSACTION
DELETE FROM MovieFinancials
ROLLBACK TRANSACTION
```
```sql
-- 8. Retrive all the movies from MovieDetails table with title starting with 'The'.
SELECT 
    MovieDetails.MovieID,
    MovieDetails.Title,
    MovieDetails.Genre,
    MovieDetails.Director,
    MovieDetails.ReleaseYear,
    MovieDetails.Producer
FROM MovieDetails
WHERE Title LIKE 'The%';
```
|MovieID| Title| Genre| Director| ReleaseYear| Producer|
|:-----:|:----:|:----:|:-------:|:----------:| :------:|
|1	|The Shawshank Redemption|	Drama|	Frank Darabont	|1994	|NULL|
|2	|The Godfather	|Crime|	Francis Ford Coppola	|1972	|NULL|
|6	|The Dark Knight|	Action|	Christopher Nolan	|2008	|NULL|
|7	|The Prestige	|Drama|	Christopher Nolan	|2006	|NULL|
|11	|The Incredible Hulk	|Action|	Louis Leterrier	|2008	|NULL|
```sql
-- 9. Retrive name of directors includes 'son' from MovieDetails table.
SELECT 
    DISTINCT MovieDetails.Director
FROM MovieDetails
WHERE MovieDetails.Director LIKE '%son%';
```
```sql
-- 10. Convert and display title of all movies in uppercase.
SELECT 
    UPPER(Title) AS UppercaseTitle
FROM MovieDetails;
```
|UppercaseTitle|
|:------------:|
|THE SHAWSHANK REDEMPTION|
|THE GODFATHER|
|INCEPTION|
|AVENGERS: ENDGAME|
|PARASITE|
|THE DARK KNIGHT|
|THE PRESTIGE|
|GLADIATOR|
|INTERSTELLAR|
|THOR|
|THE INCREDIBLE HULK|
```sql
-- 11. Display the highest rating from the MovieRatingsDuration table.
SELECT 
    MAX(MovieRatingsDuration.Rating) AS HighestRating
FROM MovieRatingsDuration;
```
|HighestRating|
|:-----------:|
|9.3|
```sql
-- 12. Calculate the years between current year and movies release year.
SELECT 
    MovieDetails.Title,
    (YEAR(GETDATE()) - MovieDetails.ReleaseYear) AS YearsSinceRelease
FROM MovieDetails;
```
|Title|YearsSinceRelease|
|:---:|:---------------:|
|The Shawshank Redemption	|31|
|The Godfather	|53|
|Inception	|15|
|Avengers: Endgame	|6|
|Parasite	|6|
|The Dark Knight	|17|
|The Prestige	|19|
|Gladiator	|25|
|Interstellar	|11|
|Thor	|14|
|The Incredible Hulk	|17|
```sql
-- 13. Find the languages in which movies have an average rating of greater than 8.0. Display the language
-- and the average rating.
SELECT 
    MovieRatingsDuration.Language,
    AVG(MovieRatingsDuration.Rating) AS AvgRating
FROM MovieRatingsDuration
GROUP BY Language
HAVING AVG(MovieRatingsDuration.Rating) > 8.0;
```
|Language|AvgRating|
|:------:|:-------:|
|English|8.588888|
|Korean|8.600000|
```sql
-- 14. Retrieve the minimum, maximum, and average movie duration for each language in the
-- MovieRatingsDuration table, but display only those languages where the average rating is greater
-- than 7.5.
SELECT 
    MovieRatingsDuration.Language,
    MIN(MovieRatingsDuration.DurationMin) AS MinDuration,
    MAX(MovieRatingsDuration.DurationMin) AS MaxDuration,
    AVG(MovieRatingsDuration.DurationMin) AS AvgDuration
FROM MovieRatingsDuration
GROUP BY MovieRatingsDuration.Language
HAVING AVG(MovieRatingsDuration.Rating) > 7.5;
```
|Language|MinDuration|MaxDuration|AvgDuration|
|:--------:|:--------:|:--------:|:--------:|
|English|115|181|151|
|Korean|132|132|132|
```sql
-- 15. Find the titles of movies whose budget is higher than the average budget of all movies.(Do not use
-- JOINS)
SELECT 
    MovieDetails.Title
FROM MovieDetails
WHERE MovieDetails.MovieID IN (
    SELECT 
        MovieFinancials.MovieID
    FROM MovieFinancials
    WHERE MovieFinancials.BudgetUSD > (
        SELECT AVG(MovieFinancialsTemp.BudgetUSD) FROM MovieFinancials MovieFinancialsTemp
    )
);
```
|Title|
|:---:|
|Inception|
|Avengers: Endgame|
|The Dark Knight|
|Interstellar|
|Thor|
```sql
-- 16. Find the titles of movies that have a box office revenue greater than the average box office revenue
-- of all movies.
SELECT 
    MovieDetails.Title
FROM MovieDetails
WHERE MovieDetails.MovieID IN (
    SELECT 
        MovieFinancials.MovieID
    FROM MovieFinancials
    WHERE MovieFinancials.BoxOfficeUSD > (SELECT AVG(MovieFinancialsTemp.BoxOfficeUSD) FROM MovieFinancials MovieFinancialsTemp)
);
```
|Title|
|:---:|
|Inception|
|Avengers: Endgame|
|The Dark Knight|

```sql
-- 17. Create a view with Rating, Language and Country columns with no data and named it MovieReview.
CREATE VIEW MovieReview AS
SELECT 
    MovieRatingsDuration.Rating,
    MovieRatingsDuration.Language,
    MovieRatingsDuration.Country
FROM MovieRatingsDuration
WHERE 1 = 0;
```
|Rating|Language|Country|
|:----:|:------:|:-----:|
```sql
-- 18. List all movies that have the same director but different genres, displaying the director’s name, both
-- movie titles, and their respective genres.
SELECT 
    md1.Director,
    md1.Title AS MovieTitle1,
    md1.Genre AS Genre1,
    md2.Title AS MovieTitle2,
    md2.Genre AS Genre2
FROM MovieDetails md1
JOIN MovieDetails md2 
    ON md1.Director = md2.Director 
    AND md1.Genre <> md2.Genre
    AND md1.MovieID <> md2.MovieID;
```
|Director| MovieTitle1 |Genre1 |MovieTitle2 |Genre2|
|:------:|:-----------:|:-----:|:----------:|:----:|
|Christopher Nolan|Inception|Sci-Fi|The Dark Knight|Action|
|Christopher Nolan|Inception|Sci-Fi|The Prestige|Drama|
|Christopher Nolan|The Dark Knight|Action|Inception|Sci-Fi|
|Christopher Nolan|The Dark Knight|Action|The Prestige|Drama|
|Christopher Nolan|The Dark Knight|Action|Interstellar|Sci-Fi|
|Christopher Nolan|The Prestige|Drama|Inception|Sci-Fi|
|Christopher Nolan|The Prestige|Drama|The Dark Knight|Action|
|Christopher Nolan|The Prestige|Drama|Interstellar|Sci-Fi|
|Christopher Nolan|Interstellar|Sci-Fi|The Dark Knight|Action|
|Christopher Nolan|Interstellar|Sci-Fi|The Prestige|Drama|
```sql
-- 19. Retrieve the title, director, and box office earnings for all movies that were released after 2010, along
-- with their ratings.
SELECT 
    MovieDetails.Title,
    MovieDetails.Director,
    MovieFinancials.BoxOfficeUSD,
    MovieRatingsDuration.Rating
FROM MovieDetails 
JOIN MovieFinancials  
ON MovieDetails.MovieID = MovieFinancials.MovieID
JOIN MovieRatingsDuration  
ON MovieDetails.MovieID = MovieRatingsDuration.MovieID
WHERE MovieDetails.ReleaseYear > 2010;
```
|Title| Director| BoxOfficeUSD| Rating|
|:----:|:-------:|:----------:|:------:|
|Avengers: Endgame|Anthony Russo|2798000000.00|8.4|
|Parasite|Bong Joon-ho|258000000.00|8.6|
|Interstellar|Christopher Nolan|677000000.00|8.6|
|Thor|Kenneth Branagh|449000000.00|7.0|
```sql
-- 20. List all directors and the number of movies they have directed, but only include directors who have
-- directed more than 1 movie.
SELECT 
    MovieDetails.Director,
    COUNT(*) AS MovieCount
FROM MovieDetails
GROUP BY MovieDetails.Director
HAVING COUNT(*) > 1;
```
|Director|MovieCount|
|:-------:|:-------:|
|Christopher Nolan|4|