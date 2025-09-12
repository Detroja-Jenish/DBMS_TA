## TEST_PAPER-B SCHEMA & DATA
```sql
CREATE DATABASE TestPaper_B;
```
```sql
USE TestPaper_B;
```
```sql
-- Stadium Table
CREATE TABLE Stadium (
    stadium_id INT PRIMARY KEY,
    stadium_name VARCHAR(100),
    stadium_city VARCHAR(50),
    stadium_capacity INT
);
```
```sql
-- Team Table
CREATE TABLE Team (
    team_id INT PRIMARY KEY,
    team_name VARCHAR(50),
    team_coach VARCHAR(50),
    team_wins INT,
    team_total_matches INT,
    home_stadium_id INT,
    FOREIGN KEY (home_stadium_id) REFERENCES Stadium(stadium_id)
);
```
```sql
-- Player Table
CREATE TABLE Player (
    player_id INT PRIMARY KEY,
    player_first_name VARCHAR(50),
    player_last_name VARCHAR(50),
    team_id INT,
    player_role VARCHAR(20),
    player_jersey_number INT,
    player_matches_played INT,
    FOREIGN KEY (team_id) REFERENCES Team(team_id)
);

```
```sql

-- Insert into Stadium
INSERT INTO Stadium VALUES
(1, 'Eden Gardens', 'Kolkata', 66000),
(3, 'M. A. Chidambaram Stadium', 'Chennai', 50000),
(4, 'M. Chinnaswamy Stadium', 'Bengaluru', 40000),
(5, 'Arun Jaitley Stadium', 'Delhi', 41000);
```

```sql
-- Insert into Team
INSERT INTO Team VALUES
(2, 'CSK', 'Stephen Fleming', 130, 210, 3),
(3, 'Royal Challengers Bangalore', 'Sanjay Bangar', 90, 200, 4),
(4, 'Kolkata Knight Riders', 'Chandrakant Pandit', 100, 190, 1),
(5, 'Delhi Capitals', 'Ricky Ponting', 85, 180, 5);
```

```sql
-- Insert into Player
INSERT INTO Player VALUES
(1, 'Virat', 'Kohli', 3, 'Batsman', 18, 220),
(3, 'MS', 'Dhoni', 2, 'Wicketkeeper', 7, 250),
(4, 'Andre', 'Russell', 4, 'All-rounder', 12, 150),
(5, 'David', 'Warner', 5, 'Batsman', 31, 210),
(6, 'Shikhar', 'Dhawan', 5, 'Batsman', 25, 220),
(7, 'Vijay', 'Shankar', 3, 'All-rounder', 28, 75),
(8, 'Aaron', 'Finch', 3, 'Batsman', 5, 80),
(10, 'Ravindra', 'Jadeja', 2, 'All-rounder', 8, 200);
```

```sql

-- FOR 15th query
INSERT INTO Stadium VALUES
(2, 'MOTERA', 'AHMDADBAD', 13200);

INSERT INTO Team VALUE
(1, 'Mumbai Indians', 'Mahela Jayawardene', 120, 200, 2);

INSERT INTO Player VALUES
(2, 'Rohit', 'Sharma', 1, 'Batsman', 45, 230),
(9, 'Ishan', 'Kishan', 1, 'Wicketkeeper', 23, 120);
```

## Questions
```sql
-- 1. Retrieve Unique Roles of Players.
-- 2. Calculate the winning percentage of each team.
-- 3. Insert a new record to Stadium Table. (2, Wankhede Stadium, Mumbai,33000)
-- 4. Update Team Coach Name of Team RCB to Ashish Nehra.
-- 5. Delete All the Records of Player Shikhar Dhawan.
-- 6. Change the size of stadium_name column from VARCHAR (100) to VARCHAR (15).
-- 7. Remove Player Table.
-- 8. Display Top 30 Players Whose First Name Starts with Vowel.
-- 9. Display City Whose Stadium Name does not Ends with ‘M’.
-- 10. Generate Random Number between 0 to 100.
-- 11. Display 4th to 9th Character of ‘Virat Kohli’.
-- 12. Display The Day of week on 01-01-2005.
-- 13. Display City Wise Maximum Stadium Capacity.
-- 14. Display City Whose Average Stadium Capacity is Above 20000.
-- 15. Display All Players Full Name, Jersey No and Role Who is Playing for Mumbai Indians. (Using Sub
-- query).
-- 16. Display Team Name Having Home Stadium Capacity Over 50000. (Using Sub query ).
-- 17. Create a View Players_Above_100_Matches of Players Who have Played More than 100 Matches.
-- 18. Get the Player name, Team name, and their Jersey number who have played Between than 50 to
-- 100 matches.
-- 19. Produce Output Like: <PlayerFirstName> Plays For <TeamName> and Has Played <PlayerMatches>
-- matches. (In single column)
-- 20. Display Stadium Capacity of Team CSK.

```
