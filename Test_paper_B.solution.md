```sql
-- 1. Retrieve Unique Roles of Players.
SELECT 
    DISTINCT Player.player_role
From Player;
```
|player_role|
|:---------:|
|All-rounder|
|Batsman|
|Wicketkeeper|

```sql
-- 2. Calculate the winning percentage of each team.
SELECT
    Team.team_name,
    (Team.team_wins* 100 / Team.team_total_matches)  as [Win percentage]
FROM Team;
```
|team_name|Win percentage|
|:---------:|:---------:|
|Mumbai Indians	|60|
|Chennai Super Kings	|61|
|Royal Challengers Bangalore	|45|
|Kolkata Knight Riders	|52|
|Delhi Capitals	|47|
```sql
-- 3. Insert a new record to Stadium Table. (2, Wankhede Stadium, Mumbai,33000)
INSERT INTO Stadium VALUES
(2, 'Wankhede Stadium', 'Mumbai',33000);
```
```sql
-- 4. Update Team Coach Name of Team RCB to Ashish Nehra.
UPDATE Team
SET team_coach = 'Ashish Nehra'
WHERE team_name = 'Royal Challengers Bangalore';
```
```sql
-- 5. Delete All the Records of Player Shikhar Dhawan.
DELETE FROM Player
WHERE player_first_name = 'Shikhar'
  AND player_last_name = 'Dhawan';
```
```sql
-- 6. Change the size of stadium_name column from VARCHAR (100) to VARCHAR (25).
ALTER TABLE Stadium
ALTER COLUMN stadium_name VARCHAR(25);
```
```sql
-- 7. Remove Player Table.
BEGIN TRANSACTION 
DROP TABLE Player
ROLLBACK TRANSACTION;
```
```sql
-- 8. Display Top 30 Players Whose First Name Starts with Vowel.
SELECT 
    TOP 30 Player.player_first_name
FROM Player
WHERE Player.player_first_name LIKE '[a,e,i,o,u]%'
```
|player_first_name|
|:---------------:|
|Andre|
|Aaron|

```sql
-- 9. Display City Whose Stadium Name does not Ends with ‘M’.
SELECT
    Stadium.stadium_city
FROM Stadium
WHERE NOT Stadium.stadium_city LIKE '%M'
```
|stadium_city|
|:---------------:|
|Kolkata|
|Mumbai|
|Chennai|
|Bengaluru|
|Delhi|

```sql
-- 10. Generate Random Number between 0 to 100.
SELECT RAND()*100;
-- 11. Display 4th to 9th Character of ‘Virat Kohli’.
SELECT 
    SUBSTRING(Concat(Player.player_first_name,' ',Player.player_last_name),4,5) as [4th to 9th Character]
FROM Player
WHERE Player.player_first_name = 'Virat'
    AND Player.player_last_name = 'Kohli';
```
|4th to 9th Character|
|:------------------:|
|at Ko|
-- 12. Display The Day of week on 01-01-2005.
```sql
SELECT DAY('01-01-2005')
```
```sql
-- 13. Display City Wise Maximum Stadium Capacity.
SELECT Stadium.stadium_city, 
       MAX(Stadium.stadium_capacity) AS max_capacity
FROM Stadium
GROUP BY stadium_city;
```
|stadium_city|max_capacity|
|:----------:|:----------:|
|Bengaluru|	40000|
|Chennai|	50000|
|Delhi|	41000|
|Kolkata|	66000|
|Mumbai|	33000|

```sql
-- 14. Display City Whose Average Stadium Capacity is Above 20000.
SELECT Stadium.stadium_city, 
       AVG(Stadium.stadium_capacity) AS avg_capacity
FROM Stadium
GROUP BY Stadium.stadium_city
HAVING AVG(Stadium.stadium_capacity) > 20000;
```
|stadium_city|avg_capacity|
|:----------:|:----------:|
|Bengaluru	|40000|
|Chennai	|50000|
|Delhi	|41000|
|Kolkata	|66000|
|Mumbai	|33000|
```sql
-- 15. Display All Players Full Name, Jersey No and Role Who is Playing for Mumbai Indians. (Using Sub
--      query).
SELECT CONCAT(Player.player_first_name, ' ', Player.player_last_name) AS full_name,
       Player.player_jersey_number,
       Player.player_role
FROM Player
WHERE Player.team_id = (
    SELECT Team.team_id
    FROM Team
    WHERE Team.team_name = 'Mumbai Indians'
);
```
|full_name|player_jersey_number|player_role|
|:----------:|:----------:|:----------:|
|Rohit Sharma	|45	|Batsman|
|Ishan Kishan	|23	|Wicketkeeper|
```sql
-- 16. Display Team Name Having Home Stadium Capacity Over 50000. (Using Sub query ).
SELECT 
    Team.team_name
FROM Team
WHERE Team.home_stadium_id IN (
    SELECT 
        Stadium.stadium_id
    FROM Stadium
    WHERE Stadium.stadium_capacity > 50000
)
```
|team_name|
|:--------:|
|Kolkata Knight Riders|
```sql
-- 17. Create a View Players_Above_100_Matches of Players Who have Played More than 100 Matches.
CREATE VIEW Players_Above_100_Matches AS
SELECT *
FROM Player
WHERE player_matches_played > 100;
```
```sql
-- 18. Get the Player name, Team name, and their Jersey number who have played Between than 50 to
-- 100 matches.
SELECT 
    Player.player_first_name,
    Team.team_name,
    Player.player_jersey_number
FROM Player
JOIN Team
ON Team.team_id = Player.team_id
WHERE Player.player_matches_played <= 100 AND  
    Player.player_matches_played >= 50
```
|player_first_name|team_name|player_jersey_number|
|:--------------:|:--------------:|:--------------:|
|Vijay	Royal |Challengers Bangalore	|28|
|Aaron	Royal |Challengers Bangalore	|5|

```sql
-- 19. Produce Output Like: <PlayerFirstName> Plays For <TeamName> and Has Played <PlayerMatches>
--      matches. (In single column)
SELECT
    CONCAT(
        Player.player_first_name , 
        ' Plays For ', 
        Team.team_name , 
        ' and Has Played ' , 
        Player.player_matches_played, 
        ' matches '
    ) as Sentence
FROM Player
JOIN Team
ON Team.team_id = Player.team_id
```
|Sentence|
|:------:|
|Virat Plays For Royal Challengers Bangalore and Has Played 220 matches |
|Rohit Plays For Mumbai Indians and Has Played 230 matches |
|MS Plays For Chennai Super Kings and Has Played 250 matches |
|Andre Plays For Kolkata Knight Riders and Has Played 150 matches |
|David Plays For Delhi Capitals and Has Played 210 matches |
|Vijay Plays For Royal Challengers Bangalore and Has Played 75 matches |
|Aaron Plays For Royal Challengers Bangalore and Has Played 80 matches |
|Ishan Plays For Mumbai Indians and Has Played 120 matches |
|Ravindra Plays For Chennai Super Kings and Has Played 200 matches |

```sql
-- 20. Display Stadium Capacity of Team CSK.
SELECT
    Stadium.stadium_capacity
FROM Stadium
JOIN Team
ON Team.home_stadium_id = Stadium.stadium_id
WHERE Team.team_name = 'CSK'
```
|stadium_capacity|
|:--------------:|
|50000|