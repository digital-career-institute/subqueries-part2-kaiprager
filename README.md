# Table 1: RepairTechnicians
This table will store information about the technicians who repair bicycles.

# Table 2: RepairJobs
This table will store information about the repair jobs performed by technicians.

## Please note that these table structures are quite basic, and you may need to adjust them based on specific requirements or additional details you want to capture in your database. Additionally, you might want to add primary and foreign key constraints or feel free to add primary and foreign key constraints .

## Task 1: Aggregate Functions and Subqueries
Task: Find the average number of years of experience for technicians who have performed repair jobs on bicycles costing more than $100. Include the technician's name and the average experience.

## Task 2: Combining Joins and Subqueries
Task: List the names of technicians who have not performed any repair jobs.

## Task 3: Using EXISTS and IN
Task: Find the technicians who have worked on repairs for bicycles owned by more than one person. Include the technician's name and the count of distinct bicycle owners.

## Task 4: Subqueries with GROUP BY and HAVING
Task: Identify technicians who have performed repairs on at least 5 bicycles and list the total number of repairs they have conducted. Include the technician's name and the total number of repairs.

## Task 5: Subqueries with AND, OR, and NOT
Task: List the technicians who have performed repairs on bicycles with a cost between $50 and $150 OR have more than 7 years of experience. Include the technician's name and the repair cost.

## Task 6: Combining EXISTS and Aggregate Functions
Task: Find technicians who have not performed any repairs on bicycles with a cost exceeding $200. Include the technician's name and the total number of such repairs.



CREATE TABLE RepairTechnicians (TechnicianID INT PRIMARY KEY, FirstName VARCHAR(50), LastName VARCHAR(50),
							   Email VARCHAR(100), Phone VARCHAR(15), Address VARCHAR(255),
							   Specialization VARCHAR(100), Experience INT);

INSERT INTO RepairTechnicians
	VALUES (1, 'Bob', 'Bobbins', 'bob@repairshop.net', '+69-56 987 907', 'Düsseldorf', 'Breaks', 2),
	(2, 'Sabine', 'Sunshine', 'sabine@repairshop.net', '+69-56 987 905', 'Köln', 'Frames', 8),
	(3, 'Vanessa', 'Vanity', 'vanessa@repairshop.net', '+69-56 987 904', 'Saarbrücken', 'Gears', 1),
	(4, 'Charlie', 'Brown', 'charlie@repairshop.net', '+69-56 987 903', 'Berlin', 'Frames', 5),
	(5, 'Dirk', 'Brown', 'dirk@repairshop.net', '+69-56 987 902', 'Bielefeld', 'Frames', 2),
	(6, 'Hansa', 'Plast', 'hansa@repairshop.net', '+69-56 987 901', 'Ham', 'Gears', 3),
	(7, 'Björn', 'Belau', 'bjoern@repairshop.net', '+69-56 987 900', 'Leipzig', 'Everything', 4),
	(8, 'Jane', 'Smith', 'jane@repairshop.net', '+69-56 987 908', 'Magdeburg', 'Breaks', 7),
	(9, 'Silke', 'SilkRoad', 'silke@repairshop.net', '+69-56 987 909', 'Breslau', 'Gears', 2);
	
SELECT * FROM RepairTechnicians;

CREATE TABLE RepairJobs (JobID INT PRIMARY KEY, TechnicianID INT, BicycleOwner VARCHAR(100), BicycleModel VARCHAR(50),
						RepairDate DATE, RepairType VARCHAR(50), Cost DECIMAL(10, 2), Notes TEXT,
						FOREIGN KEY(TechnicianID) REFERENCES RepairTechnicians(TechnicianID));
						
INSERT INTO RepairJobs VALUES
	(101, 2, 'Ralph Robbings', 'Speeder 567', '2022-09-24', 'Broken Frame', 346.56, 'Probably more'),
	(102, 4, 'Funda Ara', 'Speeder 623', '2024-12-02', 'Broken Gears', 32.95, 'Probably only needs readjustment'),
	(103, 6, 'Damian Dynamo', 'Speeder 888', '2023-03-09', 'Broken Tire', 42.25, 'Probably a glass splinter'),
	(104, 8, 'Renee Rock', 'Horsepower Bike 567', '2020-09-15', 'Broken Frame', 124.85, 'Probably a minor problem'),
	(105, 2, 'Desiree Desire', 'HotBike 007', '2023-11-22', 'Broken Everything', 1025.99, 'Better buy a new bike'),
	(106, 9, 'Aurora Morningsun', 'SuperBike 0815', '2021-02-24', 'Broken Breaks', 182.56, 'Probably more'),
	(107, 5, 'Sal Mineo', 'Racer 999', '2014-01-12', 'Broken Frame', 346.56, 'Probably smashed by an elephant'),
	(108, 7, 'Andrea Andres', 'Speeder 888', '2021-05-26', 'Broken Family', 66666.66, 'Probably broken due a secret affair with a bike repair guy. (I wonder who?)'),
	(109, 9, 'Josephine Josef', 'James Bond 007', '2022-06-30', 'Broken Bike', 602.88, 'Probably thrown out of the window'),
	(110, 2, 'August August', 'Agent Bike 02202', '2024-01-07', 'Broken Frame', 346.56, 'Probably thrown in a lake'),
	(111, 8, 'Bela Berlauch', 'SuperBike 0815', '2023-09-12', 'Broken Breaks', 182.56, 'Probably only minor readjustments'),
	(112, 2, 'Andre Alien', 'Speeder 567', '2022-07-10', 'Broken Gears', 32.95, 'You better run!'),
	(113, 2, 'Ralph Robbings', 'Speeder 567', '2022-09-24', 'Broken Frame', 346.56, 'MORE!'),
	(114, 2, 'Bambi Bino', 'Speeder 567', '2023-09-24', 'Broken Frame', 346.56, 'Probably more'),
	(115, 2, 'Douplo Duo', 'Speeder 567', '2023-10-24', 'Broken Frame', 346.56, 'Probably more'),
	(116, 2, 'Dua Lipa', 'Speeder 567', '2012-09-24', 'Broken Frame', 346.56, 'Too much music!');
	
SELECT * FROM RepairJobs;

SELECT t.FirstName, t.LastName, ROUND(AVG(t.experience), 2) AS AverageExperience FROM RepairTechnicians t
JOIN RepairJobs j ON t.TechnicianID = j.TechnicianID
WHERE j.cost > 100
GROUP BY t.TechnicianID;

SELECT t.FirstName, t.LastName FROM RepairTechnicians t
LEFT JOIN RepairJobs j ON t.TechnicianID = j.TechnicianID
WHERE j.TechnicianID IS NULL;

SELECT t.FirstName, t.LastName, COUNT(j.BicycleOwner) AS BikeOwnerCount FROM RepairTechnicians t
JOIN RepairJobs j ON t.TechnicianID = j.TechnicianID
WHERE EXISTS(SELECT 1 FROM RepairJobs j2
			WHERE j.BicycleModel = j2.BicycleModel AND t.TechnicianID = j2.TechnicianID
			AND NOT j.BicycleOwner = j2.BicycleOwner)
AND t.TechnicianID IN(SELECT j3.TechnicianID FROM RepairJobs j3
					 WHERE j.BicycleModel = j3.BicycleModel AND NOT j.BicycleOwner = j3.BicycleOwner)
GROUP BY t.TechnicianID;

SELECT t.FirstName, t.LastName, COUNT(j.JobID) AS TotalRepairs FROM RepairTechnicians t
JOIN RepairJobs j ON t.TechnicianID = j.TechnicianID
GROUP BY t.TechnicianID
HAVING COUNT(j.BicycleModel) >= 5;

SELECT t.FirstName, t.LastName, j.Cost FROM RepairTechnicians t
JOIN RepairJobs j ON t.TechnicianID = j.TechnicianID
WHERE j.Cost BETWEEN 50 and 150 OR t.Experience > 7;

SELECT t.FirstName, t.LastName, COUNT(j.JobID) AS TotalNonexpensiveRepairs FROM RepairTechnicians t
LEFT JOIN RepairJobs j ON t.TechnicianID = j.TechnicianID
WHERE NOT EXISTS(SELECT 1 FROM RepairJobs j2
				WHERE j2.TechnicianID = t.TechnicianID AND j2.Cost > 200)
GROUP BY t.TechnicianID;
