## TEST_PAPER-A SCHEMA & DATA
``` sql
CREATE DATABASE TestPaper_A;
USE TestPaper_A;
```
``` sql
CREATE TABLE Astronauts (
    astronaut_id INT IDENTITY(1,1) PRIMARY KEY,
    astronaut_name VARCHAR(100) NOT NULL,
    age INT,
    nationality VARCHAR(50),
    total_space_missions INT
);
```
``` sql
CREATE TABLE Missions (
    mission_id INT IDENTITY(1,1) PRIMARY KEY,
    mission_name VARCHAR(100) NOT NULL,
    launch_date DATE NOT NULL,
    duration_days INT ,
    mission_type VARCHAR(50)
);
```
``` sql
CREATE TABLE Spacecrafts (
    spacecraft_id INT IDENTITY(1,1) PRIMARY KEY,
    spacecraft_name VARCHAR(100) NOT NULL,
    capacity INT,
    manufacturer VARCHAR(100)
);
```
``` sql
CREATE TABLE Participation (
    participation_id INT IDENTITY(1,1) PRIMARY KEY,
    astronaut_id INT NOT NULL,
    mission_id INT NOT NULL,
    spacecraft_id INT NOT NULL,
    role VARCHAR(50),
    FOREIGN KEY (astronaut_id) REFERENCES Astronauts(astronaut_id),
    FOREIGN KEY (mission_id) REFERENCES Missions(mission_id),
    FOREIGN KEY (spacecraft_id) REFERENCES Spacecrafts(spacecraft_id)
);
```
``` sql
INSERT INTO Astronauts (astronaut_name, age, nationality, total_space_missions) VALUES
('Neil Armstrong', 39, 'USA', 2),
('Buzz Aldrin', 41, 'USA', 2),
('Yuri Gagarin', 34, 'Russia', 1),
('Valentina Tereshkova', 36, 'Russia', 1),
('Chris Hadfield', 55, 'Canada', 3),
('Samantha Cristoforetti', 44, 'Italy', 2),
('Koichi Wakata', 50, 'Japan', 4),
('Sunita Williams', 49, 'India-USA', 3),
('Elon Vega', 38, 'USA', 6),
('Rajesh Kumar', 42, 'India', 4);
```
``` sql
INSERT INTO Missions (mission_name, launch_date, duration_days, mission_type) VALUES
('Apollo 11', '1969-07-16', 8, 'Exploration'),
('Vostok 1', '1961-04-12', 1, 'Exploration'),
('Mars Research Alpha', '2023-03-15', 120, 'Research'),
('Expedition 35', '2013-03-13', 146, 'Research'),
('Expedition 46', '2015-12-15', 172, 'Research'),
('STS-118', '2007-08-08', 13, 'Repair'),
('Kibo Test Flight', '2008-03-11', 14, 'Research'),
('Jupiter Exploration', '2024-11-01', 365, 'Exploration'),
('Mars Exploration Beta', '2022-06-20', 200, 'Exploration'),
('Lunar Exploration Zeta', '2021-09-05', 45, 'Exploration');
```
``` sql
INSERT INTO Spacecrafts (spacecraft_name, capacity, manufacturer) VALUES
('Columbia', 7, 'NASA'),
('Soyuz TMA-08M', 3, 'Roscosmos'),
('Vostok 3KA', 1, 'Soviet Union'),
('Endeavour', 7, 'NASA'),
('Dragon', 7, 'SpaceX'),
('Kibo', 2, 'JAXA'),
('Falcon Heavy', 8, 'SpaceX');
```
``` sql
INSERT INTO Participation (astronaut_id, mission_id, spacecraft_id, role) VALUES
(1, 1, 1, 'Commander'),
(2, 1, 1, 'Lunar Module Pilot'),
(3, 2, 3, 'Pilot'),
(5, 3, 5, 'Commander'),
(8, 3, 5, 'Engineer'),
(9, 3, 5, 'Mission Specialist'),
(5, 4, 2, 'Commander'),
(6, 4, 2, 'Flight Engineer'),
(6, 5, 2, 'Flight Engineer'),
(8, 5, 5, 'Commander'),
(8, 6, 4, 'Engineer'),
(7, 7, 6, 'Mission Specialist'),
(9, 8, 7, 'Commander'),
(10, 8, 7, 'Engineer'),
(5, 9, 5, 'Commander'),
(9, 9, 5, 'Engineer'),
(1, 10, 4, 'Commander'),
(2, 10, 4, 'Engineer'),
(9, 10, 7, 'Commander');
```


