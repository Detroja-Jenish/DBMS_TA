```sql
-- 1. Display the top 3 percentages books ordered by title in descending
SELECT 
    TOP 3
    Books.title,
    (Books.available_copies * 100.0 / SUM(Books.available_copies) OVER()) AS percentage
FROM Books
ORDER BY Books.title DESC;
```
|title|percentage|
|:------:|:------:|
|The Shining|5.660377358490|
|The House of the Spirits|3.773584905660|
T|he Alchemist|18.867924528301|

```sql
-- 2. Display a distinct list of genres
SELECT DISTINCT 
    Books.genre
FROM Books;
```
|genre|
|:----:|
|Adventure|
|Drama|
|Dystopian|
|Fantasy|
|Fiction|
|Horror|
|Magical Realism|
|Mystery|
|Poetry|

```sql
-- 3. Insert a new book into the books table
INSERT INTO Books (Books.book_id, Books.title, Books.author_id, Books.genre, Books.publish_year, Books.available_copies)
VALUES (13, 'The Adventures of Sherlock Holmes', 2, 'Mystery', 1892, 5);
```

```sql
-- 4. Update the number of available copies to 10 for a book whose book_id is available
UPDATE Books
SET Books.available_copies = 10
WHERE Books.book_id = 3;
```

```sql
-- 5. Delete a member from the members table whose member_id is 4
BEGIN TRANSACTION;
DELETE FROM Members
WHERE Members.member_id = 4;
ROLLBACK TRANSACTION;
```

```sql
-- 6. Add a new column language varchar(20) to the books table
ALTER TABLE Books
ADD [language] VARCHAR(20);
```

```sql
-- 7. Truncate all data from the loans table
BEGIN TRANSACTION;
TRUNCATE TABLE Loans;
ROLLBACK TRANSACTION;
```

```sql
-- 8. Find books whose title starts with ‘H’ and ends with ‘L’
SELECT *
FROM Books
WHERE Books.title LIKE 'H%L';
```
|book_id|title|author_id|genre|publish_year|available_copies|language|
|:-------:|:-------:|:-------:|:-------:|:-------:|:-------:|:-------:|
|1|Himalayan Trail|3|Adventure|1990|3|NULL|
|2|Harry and the Crystal|1|Fiction|2005|2|NULL|
|5|Hotel Imperial|3|Drama|2001|1|NULL|
|11|Hail to the Rebel|7|Fiction|1955|8|NULL|
|12|Howl|5|Poetry|1956|2|NULL|

```sql
-- 9. Find authors whose name does not end with a vowel
SELECT 
    Authors.author_id,
    Authors.name,
    Authors.birth_year,
    Authors.nationality
FROM Authors
WHERE Authors.name NOT LIKE '%[a,e,i,o,u]'
```

|author_id|name|birth_year|nationality|
|:-------:|:-------:|:-------:|:-------:|
|3|Haruki Murakam|1949|Japanese|
|5|Stephen King|1947|American|
|6|J.K. Rowling|1965|British|
|7|George Orwell|1903|British|

```sql
-- 10. Find length of ‘Manish Pandey’
SELECT LEN('Manish Pandey') AS name_length;
```
|name_length|
|:---------:|
|13|

```sql
-- 11. Calculate your age in years
SELECT DATEDIFF(YEAR, '2004-11-17', GETDATE()) AS age_in_years;
```
|age_in_years|
|:---------:|
|21|

```sql
-- 12. Display the total number of books by genre
SELECT 
    Books.genre,
    COUNT(Books.book_id) AS total_books
FROM Books
GROUP BY Books.genre;
```
|genre|total_books|
|:------:|:------:|
|Adventure|1|
|Drama|1|
|Dystopian|1|
|Fantasy|1|
|Fiction|3|
|Horror|2|
|Magical Realism|1|
|Mystery|2|
|Poetry|1|

```sql
-- 13. Display the minimum, maximum, and average number of available copies 
--     for each genre whose book_id is available
SELECT 
    Books.genre,
    MIN(Books.available_copies) AS min_copies,
    MAX(Books.available_copies) AS max_copies,
    AVG(Books.available_copies) AS avg_copies
FROM Books
GROUP BY Books.genre;
```
|genre|min_copies|max_copies|avg_copies|
|:-------:|:-------:|:-------:|:-------:|
|Adventure|3|3|3|
|Drama|1|1|1|
|Dystopian|7|7|7|
|Fantasy|6|6|6|
|Fiction|2|10|6|
|Horror|3|4|3|
|Magical Realism|2|2|2|
|Mystery|5|10|7|
|Poetry|2|2|2|

```sql
-- 14. Display the title of books where the author was born before 1970.(Using Sub query)
SELECT
    Books.title
FROM Books
WHERE Books.author_id IN (
    SELECT 
        Authors.author_id
    FROM Authors
    WHERE Authors.birth_year < 1970
)
```
|title|
|:---:|
|Himalayan Trail|
|Harry and the Crystal|
|The Adventures of Sherlock Holmes|
|The Alchemist|
|Hotel Imperial|
|1984|
|It|
|Harry Potter and the Philosopher Stone|
|The House of the Spirits|
|The Shining|
|Hail to the Rebel|
|Howl|
|The Adventures of Sherlock Holmes|

```sql
-- 15. Create a view View_Member whose membership date is not available from members table
CREATE VIEW View_Member AS
SELECT 
    Members.member_id,
    Members.name,
    Members.email,
    Members.phone_number,
    Members.membership_date
FROM Members
WHERE Members.membership_date IS NULL;
```

```sql
-- 16. Find the title of books that have been borrowed the most (top 1) and the corresponding author name
SELECT 
    Books.title,
    Authors.name AS author_name
FROM Books
JOIN Authors ON Books.author_id = Authors.author_id
WHERE Books.book_id = (
    SELECT
        TOP 1 Loans.book_id
    FROM Loans
    GROUP BY Loans.book_id
    ORDER BY COUNT(Loans.book_id) DESC
);
```
|title|author_name|
|:------:|:------:|
|Himalayan Trail|Haruki Murakam|

```sql
-- 17. Display the loan_id, member name, title, loan_date whose member name is 'Raj'
SELECT 
    Loans.loan_id,
    Members.name AS member_name,
    Books.title,
    Loans.loan_date
FROM Loans
JOIN Members
ON Loans.member_id = Members.member_id
JOIN Books
ON Loans.book_id = Books.book_id
WHERE Members.name = 'Raj';
```
|loan_id|member_name|title|loan_date|
|:-------:|:-------:|:-------:|:-------:|
|1|Raj	Himalayan Trail|2024-01|15|
|3|Raj	The Alchemist|2024-05|01|
|7|Raj	It|2023-05|15|

```sql
-- 18. List the titles of books that have been borrowed by members who registered before 2020.(using
-- Sub query)
SELECT
    DISTINCT Books.title
FROM Books
JOIN Loans ON Loans.book_id = Books.book_id
WHERE Loans.member_id IN (
    SELECT 
        Members.member_id
    FROM Members
    WHERE YEAR(Members.membership_date) < 2020
);
```
|title|
|:----:|
|Harry and the Crystal|
|Himalayan Trail|
|It|
|The Adventures of Sherlock Holmes|
|The Alchemist|
|The House of the Spirits|
|The Shining|

```sql
-- 19. Display the total number of books borrowed by each member
SELECT 
    Members.name AS member_name,
    COUNT(Loans.book_id) AS total_books_borrowed
FROM Members
LEFT JOIN Loans 
ON Members.member_id = Loans.member_id
GROUP BY Members.name;
```

|member_name|total_books_borrowed|
|:---------:|:------------------:|
|Alice|1|
|John Doe|0|
|Manish Pandey|4|
|Michael Clarke|3|
|Raj|3|
|Sarah Connor|1|

```sql
-- 20. Display the title of books that have not been borrowed by any members
SELECT 
    Books.title
FROM Books
LEFT JOIN Loans 
ON Books.book_id = Loans.book_id
WHERE Loans.book_id IS NULL;
```
|title|
|:----:|
|Hotel Imperial|
|1984|
|Hail to the Rebel|
|Howl|
|The Adventures of Sherlock Holmes|