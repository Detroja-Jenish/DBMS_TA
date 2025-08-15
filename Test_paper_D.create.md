## TEST_PAPER-C SCHEMA & DATA
```sql
CREATE DATABASE TestPaper_D;
```
```sql
USE TestPaper_D;
```

```sql
-- 1. Authors Table
CREATE TABLE Authors (
    author_id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    birth_year INT,
    nationality VARCHAR(50)
);
```

```sql
-- 2. Books Table
CREATE TABLE Books (
    book_id INT PRIMARY KEY,
    title VARCHAR(200) NOT NULL,
    author_id INT,
    genre VARCHAR(50),
    publish_year INT,
    available_copies INT,
    FOREIGN KEY (author_id) REFERENCES Authors(author_id)
);
```

```sql
-- 3. Members Table
CREATE TABLE Members (
    member_id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100),
    phone_number VARCHAR(20),
    membership_date DATE
);
```
```sql
-- 4. Loans Table
CREATE TABLE Loans (
    loan_id INT PRIMARY KEY,
    book_id INT,
    member_id INT,
    loan_date DATE,
    return_date DATE,
    FOREIGN KEY (book_id) REFERENCES Books(book_id),
    FOREIGN KEY (member_id) REFERENCES Members(member_id)
);
```
```sql
INSERT INTO Authors (author_id, name, birth_year, nationality) VALUES
(1, 'Agatha Christie', 1890, 'British'),
(2, 'Arthur Conan Doyle', 1859, 'British'),
(3, 'Haruki Murakam', 1949, 'Japanese'), -- ends with consonant for Q9
(4, 'Paulo Coelho', 1947, 'Brazilian'),
(5, 'Stephen King', 1947, 'American'),
(6, 'J.K. Rowling', 1965, 'British'),
(7, 'George Orwell', 1903, 'British'),
(8, 'Isabel Allende', 1942, 'Chilean');
```
```sql
INSERT INTO Books (book_id, title, author_id, genre, publish_year, available_copies) VALUES
(1, 'Himalayan Trail', 3, 'Adventure', 1990, 3),
(2, 'Harry and the Crystal', 1, 'Fiction', 2005, 2),
(3, 'The Adventures of Sherlock Holmes', 2, 'Mystery', 1892, 5),
(4, 'The Alchemist', 4, 'Fiction', 1988, 10),
(5, 'Hotel Imperial', 3, 'Drama', 2001, 1),
(6, '1984', 7, 'Dystopian', 1949, 7),
(7, 'It', 5, 'Horror', 1986, 4),
(8, 'Harry Potter and the Philosopher Stone', 6, 'Fantasy', 1997, 6),
(9, 'The House of the Spirits', 8, 'Magical Realism', 1982, 2),
(10, 'The Shining', 5, 'Horror', 1977, 3),
(11, 'Hail to the Rebel', 7, 'Fiction', 1955, 8),
(12, 'Howl', 5, 'Poetry', 1956, 2);
```
```sql
INSERT INTO Members (member_id, name, email, phone_number, membership_date) VALUES
(1, 'Raj', 'raj@example.com', '1234567890', '2019-05-10'),
(2, 'Manish Pandey', 'manish@example.com', '9876543210', '2018-03-15'),
(3, 'Alice', 'alice@example.com', '5556667777', '2021-01-10'),
(4, 'John Doe', 'john@example.com', '4445556666', '2022-06-20'),
(5, 'Sarah Connor', 'sarah@example.com', '5551112222', NULL), -- NULL for Q15
(6, 'Michael Clarke', 'michael@example.com', '3332221111', '2017-11-25');
```
```sql
INSERT INTO Loans (loan_id, book_id, member_id, loan_date, return_date) VALUES
(1, 1, 1, '2024-01-15', '2024-02-15'),
(2, 3, 2, '2024-03-10', '2024-04-10'),
(3, 4, 1, '2024-05-01', '2024-06-01'),
(4, 2, 2, '2023-08-20', '2023-09-20'),
(5, 3, 3, '2024-07-10', '2024-08-10'),
(6, 7, 6, '2023-01-12', '2023-02-12'),
(7, 7, 1, '2023-05-15', '2023-06-15'),
(8, 8, 5, '2024-04-04', '2024-05-04'),
(9, 9, 6, '2022-12-01', '2023-01-01'),
(10, 10, 2, '2024-02-02', '2024-03-02'),
(11, 1, 2, '2023-03-15', '2023-04-15'),
(12, 4, 6, '2024-06-15', '2024-07-15');
```

## Question
```sql
-- 1. Display the top 3 percentages books order by title in descending.
-- 2. Display a distinct list of genres.
-- 3. Insert a new book into the books table. ('The Adventures of Sherlock Holmes', 2, 'Mystery', 1892,
-- 5)
-- 4. Update the number of available copies is 10 for a book whose book_id is available.
-- 5. Delete a member from the members table whose member_id is 4.
-- 6. Add a new column language varchar(20) to the books table.
-- 7. Truncate all data from the loans table. (Using Truncate)
-- 8. Find books whose title starts with ‘H’ and end with ‘L’.
-- 9. Find authors whose name does not ends with vowel.
-- 10. Find Lenth of ‘Manish Pandey’
-- 11. Calculate your age in year.
-- 12. Display the total number of books by genre.
-- 13. Display the minimum, maximum, and average number of available copies for each genre whose
-- book_id is available.
-- 14. Display the title of books where the author was born before 1970.(Using Sub query)
-- 15. Create a view View_Member whose membership date is not available from members table
-- 16. Find the title of books that have been borrowed the most (the top 1 book) and the corresponding
-- author name (Using sub Query)
-- 17. Display the loan_id ,member name ,title ,loan date whose member name is 'Raj'.
-- 18. List the titles of books that have been borrowed by members who registered before 2020.(using
-- Sub query)
-- 19. Display the total number of books borrowed by each member.
-- 20. Display the title of books that have not been borrowed by any members.
```