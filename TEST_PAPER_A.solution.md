```sql
USE TestPaper_A;
```
```sql
-- 1. Retrieve the distinct mission names where the mission lasted more than 30 days.
SELECT
    distinct Missions.mission_name
From Missions
WHERE Missions.duration_days > 30
```
```sql
-- 2. Retrieve the top 3 astronauts who participated in the most missions, ensuring no duplicates.
SELECT
    TOP 3
    Astronauts.astronaut_name,
    COUNT(Participation.participation_id) AS TOTAL_MISISONS
From Astronauts
JOIN Participation
ON Participation.astronaut_id = Astronauts.astronaut_id
GROUP By Astronauts.astronaut_name
ORDER BY TOTAL_MISISONS DESC
```
```sql
-- 3. Insert a new space mission called "Jupiter Exploration" that is scheduled to launch on '2024-11-01', lasting 365 days, 
--and classified as an exploration mission.
INSERT INTO Missions (mission_name, launch_date, duration_days, mission_type)
VALUES ('Jupiter Exploration', '2024-11-01', 365, 'Exploration');

```
```sql
-- 4. Update the total space missions count for astronaut with ID = 5, increasing it by 1.
UPDATE Astronauts
SET total_space_missions = total_space_missions + 1
WHERE astronaut_id = 5;
```
```sql
-- 5. Delete participation record for astronaut ID 3 in mission ID 2.
DELETE FROM Participation
WHERE astronaut_id = 3 AND mission_id = 2;
```
```sql
-- 6. Add a new column experience_level (VARCHAR(50)) to the Astronauts table to store the astronaut's experience level.
ALTER TABLE Astronauts
ADD experience_level VARCHAR(50)
```
```sql
-- 7. Clear all the data from the Participation table.(Truncate)
BEGIN TRANSACTION;
SELECT * FROM Participation
TRUNCATE TABLE Participation
SELECT * FROM Participation
ROLLBACK TRANSACTION;
```
```sql
-- 8. Retrieve all mission names where the mission type contains 'exploration'.
SELECT 
    Missions.mission_name
FROM Missions
WHERE Missions.mission_type='exploration'
```
```sql
-- 9. Retrieve all missions that contain the word "Mars" and lasted more than 100 days.
SELECT
    Missions.mission_id,
    Missions.mission_name,
    Missions.mission_type,
    Missions.duration_days,
    Missions.launch_date
FROM Missions
WHERE Missions.mission_name LIKE '%Mars%'
    AND Missions.duration_days > 100
```
```sql
-- 10. Retrieve the square root of the total number of missions for astronaut ID 5
SELECT
    Astronauts.astronaut_id,
    Astronauts.astronaut_name,
    COUNT(Participation.participation_id) Total_missions,
    SQRT(COUNT(Participation.participation_id)) Sqrt_Total_missions
From Astronauts
LEFT JOIN Participation
ON Participation.astronaut_id = Astronauts.astronaut_id
WHERE Astronauts.astronaut_id = 5
GROUP By Astronauts.astronaut_id,
    Astronauts.astronaut_name
```
```sql
-- 11. Retrieve the first 3 characters of each astronaut's name.
SELECT 
    SUBSTRING(astronaut_name, 1,3) AS first_three_chars
FROM Astronauts
```
```sql
-- 12. Retrieve the astronauts who participated in missions launched in the current year.
SELECT
     Astronauts.astronaut_name
FROM Astronauts
JOIN Participation
ON Participation.astronaut_id = Astronauts.astronaut_id
JOIN Missions
ON Missions.mission_id = Participation.mission_id
WHERE YEAR(GETDATE()) - YEAR(Missions.launch_date) > 2
```
```sql
-- 13. Count the number of astronauts from each nationality who have participated in more than 2 space missions.
SELECT
    Astronauts.nationality,
    COUNT(Astronauts.astronaut_name) as Total_missions
FROM Astronauts
JOIN Participation
ON Participation.astronaut_id = Astronauts.astronaut_id
GROUP BY Astronauts.nationality
HAVING COUNT(Participation.astronaut_id) > 2
```
```sql
-- 14. Retrieve the total number of missions and the average mission duration for each mission type,
-- but only include mission types that have been involved in more than 3 missions.
SELECT 
    Missions.mission_type,
    Count(Missions.mission_id) [total number of missions],
    AVG(Missions.duration_days) [ average mission duration]
FROM Missions
GROUP BY Missions.mission_type
HAVING COUNT(Missions.mission_id) > 3
```
```sql
-- 15. Find the number of missions commanded by astronauts for each nationality where more than 5 missions were commanded
SELECT 
    Astronauts.nationality,
    COUNT(DISTINCT Participation.mission_id) AS commanded_missions
FROM Participation
JOIN Astronauts  
ON Participation.astronaut_id = Astronauts.astronaut_id
WHERE Participation.role = 'Commander'
GROUP BY Astronauts.nationality
HAVING COUNT(DISTINCT Participation.mission_id) > 5;
```
```sql
-- 16. Retrieve the name of the spacecraft used in the mission "Apollo 11" (Use sub Query)
SELECT 
    distinct
    Spacecrafts.spacecraft_name, Missions.mission_name
FROM Spacecrafts
JOIN Participation
ON Participation.spacecraft_id = Spacecrafts.spacecraft_id
JOIN Missions
ON Missions.mission_id = Participation.mission_id
WHERE Missions.mission_name = 'Apollo 11'
```
```sql
-- 17. Create a view that shows all active missions (those that launched after 2020).
CREATE VIEW ActiveMissions  AS
SELECT 
    Missions.mission_id,
    Missions.mission_name,
    Missions.launch_date,
    Missions.duration_days,
    Missions.mission_type
FROM Missions
WHERE launch_date > '2020-12-31';
```
```sql
-- 18. List all astronauts and their respective spacecraft for each mission they participated in.
SELECT 
    Astronauts.astronaut_name,
    Spacecrafts.spacecraft_name,
    Missions.mission_name
From Astronauts
JOIN Participation
ON Participation.astronaut_id = Astronauts.astronaut_id
JOIN Spacecrafts
ON Participation.spacecraft_id = Spacecrafts.spacecraft_id
JOIN Missions
ON Participation.mission_id = Missions.mission_id
```
```sql
-- 19. Retrieve the names of astronauts 
-- who participated in missions using spacecrafts manufactured by "SpaceX", 
-- along with the names of those missions and the duration of each mission. Include only astronauts 
-- who have participated in more than 2 missions.
SELECT
    Astronauts.astronaut_name,
    Missions.mission_name,
    Missions.duration_days
FROM Astronauts
JOIN Participation
ON Participation.astronaut_id = Astronauts.astronaut_id
JOIN Spacecrafts
ON Participation.spacecraft_id = Spacecrafts.spacecraft_id
JOIN Missions
ON Participation.mission_id = Missions.mission_id
WHERE Spacecrafts.manufacturer = 'SpaceX'
     AND 2 < (
        SELECT COUNT(Participation.participation_id)
        FROM Astronauts Astronut_temp
        JOIN Participation
        ON Participation.astronaut_id = Astronauts.astronaut_id
        WHERE  Astronauts.astronaut_id = Astronut_temp.astronaut_id 
     )
```
```sql
-- 20. Retrieve the names of astronauts, the names of missions they participated in, the names of
-- spacecraft used in those missions, and the manufacturers of those spacecraft, for missions where
-- the mission duration is greater than the average duration of all missions conducted by astronauts
-- from the "USA".

SELECT 
    Astronauts.astronaut_name,
    Missions.mission_name,
    Spacecrafts.spacecraft_name,
    Spacecrafts.manufacturer
FROM Astronauts
JOIN Participation
ON Participation.astronaut_id = Astronauts.astronaut_id
JOIN Spacecrafts
ON Participation.spacecraft_id = Spacecrafts.spacecraft_id
JOIN Missions
ON Participation.mission_id = Missions.mission_id
WHERE Missions.duration_days > (
    SELECT 
        AVG(Missions_temp.duration_days)
    FROM Astronauts Astronauts_temp
    JOIN Participation Participation_temp
    ON Participation_temp.astronaut_id = Astronauts_temp.astronaut_id
    JOIN Missions Missions_temp
    ON Participation_temp.mission_id = Missions_temp.mission_id
    WHERE Astronauts.nationality = 'USA'
) 
```
