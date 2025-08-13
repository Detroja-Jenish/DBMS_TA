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
|mission_name|
|:----------------:|
|Expedition 35|
|Expedition 46|
|Jupiter Exploration|
|Lunar Exploration Zeta|
|Mars Exploration Beta|
|Mars Research Alpha|
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
|astronaut_name|TOTAL_MISISONS|
|:----------------|----------------:|
|Elon Vega|4|
|Chris Hadfield|3|
|Sunita Williams|3|
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
|mission_name|
|:----------------:|
|Apollo 11|
|Vostok 1|
|Jupiter Exploration|
|Mars Exploration Beta|
|Lunar Exploration Zeta|
|Jupiter Exploration|
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
|mission_id|mission_name|mission_type|duration_days|launch_date|
|:----------------|:----------------:|:----------------:|:----------------:|----------------:|
|3|Mars Research Alpha|Research|120|2023-03-15|
|9|Mars Exploration Beta|Exploration|200|2022-06-20|
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
|mission_id|astronaut_name|Total_missions|Sqrt_Total_missions|
|:----------------|:----------------:|:----------------:|----------------:|
|5|Chris Hadfield|3|1.7320508075688772|

```sql
-- 11. Retrieve the first 3 characters of each astronaut's name.
SELECT 
    SUBSTRING(astronaut_name, 1,3) AS first_three_chars
FROM Astronauts
```
|first_three_chars|
|:---------------:|
|Nei|
|Buz|
|Yur|
|Val|
|Chr|
|Sam|
|Koi|
|Sun|
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
|astronaut_name|
|:---------------:|
|Neil Armstrong|
|Buzz Aldrin|
|Yuri Gagarin|
|Chris Hadfield|
|Samantha Cristoforetti|
|Samantha Cristoforetti|
|Sunita Williams|
|Sunita Williams|
|Koichi Wakata|
|Chris Hadfield|
|Elon Vega|
|Neil Armstrong|
|Buzz Aldrin|
|Elon Vega|
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
|nationality|Total_missions|
|:---------------|---------------:|
|Canada|3|
|India-USA|3|
|USA|8|
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
|mission_type|total number of missions|average mission duration|
|:---------------|:---------------:|---------------:|
|Exploration|6|164|
|Research|4|113|
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
|nationality|commanded_missions|
|:--|--:
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
|spacecraft_name|mission_name|
|:--------------|-----------:|
|Columbia|Apollo 11|
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
|astronaut_name|spacecraft_name|mission_name|
|:------------:|:-----------------:|:-----------:|
|Neil Armstrong	|Columbia	|Apollo 11|
|Buzz Aldrin	|Columbia	|Apollo 11|
|Yuri Gagarin	|Vostok 3KA	|Vostok 1|
|Chris Hadfield|	Dragon|	Mars Research Alpha|
|Sunita Williams|	Dragon	|Mars Research Alpha|
|Elon Vega|	Dragon|	Mars Research Alpha|
|Chris Hadfield	|Soyuz TMA-08M	|Expedition 35|
|Samantha Cristoforetti	|Soyuz TMA-08M|	Expedition 35|
|Samantha Cristoforetti	|Soyuz TMA-08M|	Expedition 46|
|Sunita Williams	|Dragon	|Expedition 46|
|Sunita Williams	|Endeavour|	STS-118|
|Koichi Wakata	|Kibo|	Kibo Test Flight|
|Elon Vega	|Falcon Heavy|	Jupiter Exploration|
|Rajesh Kumar	|Falcon Heavy|	Jupiter Exploration|
|Chris Hadfield|	Dragon|	Mars Exploration Beta|
|Elon Vega	|Dragon|	Mars Exploration Beta|
|Neil Armstrong|	Endeavour|	Lunar Exploration Zeta|
|Buzz Aldrin|	Endeavour|	Lunar Exploration Zeta|
|Elon Vega|	Falcon Heavy|	Lunar Exploration Zeta|
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
|astronaut_name|mission_name|duration_days|
|:------------:|:-----------------:|:-----------:|
|Chris Hadfield|	Mars Research Alpha	|120|
|Sunita Williams|	Mars Research Alpha	|120|
|Elon Vega|	Mars Research Alpha	|120|
|Sunita Williams|	Expedition 46	|172|
|Elon Vega|	Jupiter Exploration	|365|
|Chris Hadfield|	Mars Exploration Beta	|200|
|Elon Vega|	Mars Exploration Beta	|200|
|Elon Vega|	Lunar Exploration Zeta	|45|
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
|astronaut_name|mission_name|spacecraft_name|manufacturer|
|:------------:|:-----------------:|:-----------:|:-----------:|
|Elon Vega|	Jupiter Exploration|	Falcon Heavy	|SpaceX|
|Elon Vega|	Mars Exploration Beta	|Dragon	|SpaceX|
