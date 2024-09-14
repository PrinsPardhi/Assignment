
## Part 1
Part 1 of assignment, we will need to identify data requirements, perform normalization (up to 3NF), create MySQL tables, and insert sample data. Here's a step-by-step solution.

## Step 1: Identify Data Requirements

Based on the provided information, we can identify the following entities:

#### # CITY
#### # STATION
#### # TRAIN
#### # INTERMEIDATE STATION
#### # DRIVER
#### # MAINTENANCE SCHEDULE
#### # STAND BY TRAIN
#### # PASSENGER
#### # TRAVEL AGENT
#### # BOOKING
#### # SERVICE DAY
#### # TRAIN SERVICE DAY
#### # ROUTE
#### # SCHEDULE
#### # DRIVER ASSIGNMENT



## Step 2
Normalize the Data (Up to 3NF)

We need to ensure that the data is in 1NF, 2NF, and 3NF:
1NF: Each attribute must contain only atomic values. Ensure there are no repeating groups.
2NF: Remove partial dependencies, meaning that non-prime attributes are dependent on the entire primary key.
3NF: Remove transitive dependencies, meaning that non-prime attributes should not depend on other non-prime attributes.

### Entity Relationships Diagram::


![er](https://github.com/PrinsPardhi/Assignment/assets/73771547/2e9d88f7-e25d-4c55-9c30-8c4540609b37)



## Step 3

### Create MySQL Tables :

-- 1 City Table
CREATE TABLE City (
    CityID INT PRIMARY KEY,
    CityName VARCHAR(255)
);

-- 2 Station Table
CREATE TABLE Station (
    StationID INT PRIMARY KEY,
    StationName VARCHAR(255),
    CityID INT,
    FOREIGN KEY (CityID) REFERENCES City(CityID) ON DELETE CASCADE ON UPDATE CASCADE
);

-- 3 Train Table
CREATE TABLE Train (
    TrainID INT PRIMARY KEY,
    TrainName VARCHAR(255),
    Facilities VARCHAR(255),
    OperatingDays VARCHAR(255),
    TotalCoaches INT,
    Distance INT,
    CONSTRAINT UC_Train UNIQUE (TrainName) -- Added a unique constraint for TrainName
);

--  4 IntermediateStation Table
CREATE TABLE IntermediateStation (
    IntermediateStationID INT PRIMARY KEY,
    StationID INT,
    TrainID INT,
    ArrivalTime TIME,
    DepartureTime TIME,
    FOREIGN KEY (StationID) REFERENCES Station(StationID) ON DELETE CASCADE ON UPDATE CASCADE,
    FOREIGN KEY (TrainID) REFERENCES Train(TrainID) ON DELETE CASCADE ON UPDATE CASCADE
);

-- 5 Driver Table
CREATE TABLE Driver (
    DriverID VARCHAR(10) PRIMARY KEY,
    FirstName VARCHAR(255),
    LastName VARCHAR(255),
    ContactNumber VARCHAR(15),
    CityID INT,
    RestDay VARCHAR(255),
    AccommodationRequired BOOLEAN,
    FOREIGN KEY (CityID) REFERENCES City(CityID) ON DELETE CASCADE ON UPDATE CASCADE
);

-- 6 MaintenanceSchedule Table
CREATE TABLE MaintenanceSchedule (
    MaintenanceID INT PRIMARY KEY,
    TrainID INT,
    LastMaintenanceDate DATE,
    Mileage INT,
    MaintenanceType VARCHAR(50),
    FOREIGN KEY (TrainID) REFERENCES Train(TrainID) ON DELETE CASCADE ON UPDATE CASCADE
);

-- 7 StandByTrain Table
CREATE TABLE StandByTrain (
    StandByTrainID INT PRIMARY KEY,
    TrainID INT,
    DriverID VARCHAR(10),
    ActivationTime DATETIME,
    FOREIGN KEY (TrainID) REFERENCES Train(TrainID) ON DELETE CASCADE ON UPDATE CASCADE,
    FOREIGN KEY (DriverID) REFERENCES Driver(DriverID) ON DELETE CASCADE ON UPDATE CASCADE
);

-- 8 Passenger Table
CREATE TABLE Passenger (
    PassengerID INT PRIMARY KEY,
    FirstName VARCHAR(255),
    LastName VARCHAR(255),
    Age INT,
    ConcessionType VARCHAR(20)
);

-- 9 TravelAgent Table
CREATE TABLE TravelAgent (
    TravelAgentID INT PRIMARY KEY,
    AgentName VARCHAR(255),
    CommissionRate DECIMAL(5, 2)
);

-- 10 Booking Table
CREATE TABLE Booking (
    BookingID INT PRIMARY KEY,
    PassengerID INT,
    TrainID INT,
    BookingDate DATE,
    TravelAgentID INT,
    TravelDate DATE,
    SeatNumber INT,
    Amount DECIMAL(10, 2),
    FOREIGN KEY (PassengerID) REFERENCES Passenger(PassengerID) ON DELETE CASCADE ON UPDATE CASCADE,
    FOREIGN KEY (TrainID) REFERENCES Train(TrainID) ON DELETE CASCADE ON UPDATE CASCADE,
    FOREIGN KEY (TravelAgentID) REFERENCES TravelAgent(TravelAgentID) ON DELETE CASCADE ON UPDATE CASCADE
);

-- 11 ServiceDays Table
CREATE TABLE ServiceDays (
    ServiceDayID INT PRIMARY KEY,
    Day VARCHAR(255)
);

-- 12 TrainServiceDays Table
CREATE TABLE TrainServiceDays (
    TrainID INT,
    ServiceDayID INT,
    PRIMARY KEY (TrainID, ServiceDayID),
    FOREIGN KEY (TrainID) REFERENCES Train(TrainID) ON DELETE CASCADE ON UPDATE CASCADE,
    FOREIGN KEY (ServiceDayID) REFERENCES ServiceDays(ServiceDayID) ON DELETE CASCADE ON UPDATE CASCADE
);

-- 13 Route Table
CREATE TABLE Route (
    RouteID INT PRIMARY KEY,
    StartStationID INT,
    EndStationID INT,
    Distance INT,
    TimeTaken VARCHAR(255),
    FOREIGN KEY (StartStationID) REFERENCES Station(StationID) ON DELETE CASCADE ON UPDATE CASCADE,
    FOREIGN KEY (EndStationID) REFERENCES Station(StationID) ON DELETE CASCADE ON UPDATE CASCADE
);

-- 14 Schedule Table
CREATE TABLE Schedule (
    ScheduleID INT PRIMARY KEY,
    TrainID INT,
    RouteID INT,
    DepartureTime TIME,
    OperatingDays VARCHAR(255),
    FOREIGN KEY (TrainID) REFERENCES Train(TrainID) ON DELETE CASCADE ON UPDATE CASCADE,
    FOREIGN KEY (RouteID) REFERENCES Route(RouteID) ON DELETE CASCADE ON UPDATE CASCADE
);

-- 15 DriverAssignment Table
CREATE TABLE DriverAssignment (
    DriverID VARCHAR(10),
    ScheduleID INT,
    MainDriver BOOLEAN,
    CoDriver BOOLEAN,
    Remark VARCHAR(255),
    PRIMARY KEY (DriverID, ScheduleID),
    FOREIGN KEY (DriverID) REFERENCES Driver(DriverID) ON DELETE CASCADE ON UPDATE CASCADE,
    FOREIGN KEY (ScheduleID) REFERENCES Schedule(ScheduleID) ON DELETE CASCADE ON UPDATE CASCADE
);

--16 CREATE TABLE LateArrivals ( 
  LateArrivalID INT AUTO_INCREMENT PRIMARY KEY,
    TrainID INT,
    ArrivalTime DATETIME,
    ScheduledArrivalTime DATETIME,
    DelayMinutes INT,
    LateReason VARCHAR(255),
    LateDate DATE,
    FOREIGN KEY (TrainID) REFERENCES Train(TrainID) ON DELETE CASCADE ON UPDATE CASCADE
);




## Step 4

### Insert Values Into Tables :

-- City Table 
INSERT INTO City (CityID, CityName) 
VALUES (1, 'Mumbai'), (2, 'New Delhi'), (3, 'Secunderabad'), (4, 'Bhopal'), (5, 'Lonavala'), (6, 'Dharwad'), (7, 'Pune'), (8, 'Ajmer'), (9, 'Delhi'), (10, 'Chennai');

-- Station Table 
INSERT INTO Station (StationID, StationName, CityID) 
VALUES (1, 'Mumbai Central', 1), (2, 'Gandhinagar', 1), (3, 'New Delhi', 2), (4, 'Himachal', 2), (5, 'Secunderabad', 3), (6, 'Visakhapatnam', 3), (7, 'Sainagar Shirdi', 1), (8, 'Solapur', 1), (9, 'Bhopal', 4), (10, 'Delhi', 2);

-- IntermediateStation Table
 INSERT INTO IntermediateStation (IntermediateStationID, StationID, TrainID, ArrivalTime, DepartureTime) 
VALUES (1, 1, 1, '05:45:00', '12:10:00'), (2, 2, 1, '13:15:00', '19:45:00'), (3, 3, 2, '09:00:00', '15:10:00'), (4, 4, 2, '16:10:00', '22:20:00'), (5, 5, 3, '08:30:00', NULL), (6, 6, 3, '17:00:00', NULL), (7, 7, 4, '05:20:00', '10:40:00'), (8, 8, 4, '06:35:00', '13:00:00'), (9, 9, 5, '07:45:00', '15:30:00'), (10, 10, 5, '08:00:00', '16:00:00');

-- Driver Table 
INSERT INTO Driver (DriverID, FirstName, LastName, ContactNumber, CityID, RestDay, AccommodationRequired) 
VALUES ('K0012', 'Ketan', 'Vibhuti', '9812131415', 7, 'Sunday', TRUE), ('S0023', 'Sneha', 'Gupta', '9876543210', 4, 'Saturday', FALSE), ('R0034', 'Rajesh', 'Kumar', '9998887776', 1, 'Thursday', TRUE), ('P0045', 'Prerna', 'Singh', '8765432109', 6, 'Wednesday', FALSE), ('A0056', 'Alok', 'Yadav', '8887776665', 3, 'Monday', TRUE), ('N0067', 'Nisha', 'Sharma', '8765432101', 9, 'Friday', FALSE), ('M0078', 'Manoj', 'Verma', '9876543211', 10, 'Tuesday', TRUE), ('V0089', 'Vikas', 'Goyal', '9812345678', 5, 'Sunday', FALSE), ('G0090', 'Gauri', 'Gupta', '9999999999', 2, 'Saturday', TRUE), ('L0101', 'Lalit', 'Mishra', '7777777777', 8, 'Tuesday', FALSE);

-- MaintenanceSchedule Table 
INSERT INTO MaintenanceSchedule (MaintenanceID, TrainID, LastMaintenanceDate, Mileage, MaintenanceType) 
VALUES (1, 1, '2023-11-01', 5500, 'Routine'), (2, 2, '2023-10-15', 4000, 'Routine'), (3, 3, '2023-10-20', 3000, 'Routine'), (4, 4, '2023-11-05', 6000, 'Routine'), (5, 5, '2023-10-10', 5500, 'Routine'), (6, 6, '2023-10-25', 4500, 'Routine'), (7, 7, '2023-11-10', 9000, 'Routine'), (8, 8, '2023-10-30', 3500, 'Routine'), (9, 9, '2023-11-15', 2000, 'Routine'), (10, 10, '2023-11-20', 7000, 'Routine');

-- StandByTrain Table 
INSERT INTO StandByTrain (StandByTrainID, TrainID, DriverID, ActivationTime)
 VALUES (1, 1, 'K0012', '2023-11-01 05:30:00'), (2, 2, 'S0023', '2023-10-15 08:45:00'), (3, 3, 'R0034', '2023-10-20 07:45:00'), (4, 4, 'P0045', '2023-11-05 15:45:00'), (5, 5, 'A0056', '2023-10-10 06:30:00'), (6, 6, 'N0067', '2023-10-25 16:00:00'), (7, 7, 'M0078', '2023-11-10 10:30:00'), (8, 8, 'V0089', '2023-10-30 04:30:00'), (9, 9, 'G0090', '2023-11-15 08:15:00'), (10, 10, 'L0101', '2023-11-20 05:45:00');
 --Passenger Table
INSERT INTO Passenger (PassengerID, FirstName, LastName, Age, ConcessionType) 

VALUES (1, 'Amit', 'Sharma', 30, 'None'), (2, 'Priya', 'Patel', 25, 'Child'), (3, 'Raj', 'Kumar', 45, 'Senior'), (4, 'Anjali', 'Desai', 22, 'None'), (5, 'Suresh', 'Verma', 60, 'Senior'),(6, 'Aisha', 'Singh', 18, 'None'), (7, 'Vikram', 'Yadav', 35, 'None'), (8, 'Pooja', 'Gupta', 28, 'None'), (9, 'Ravi', 'Chopra', 50, 'Senior'), (10, 'Neha', 'Reddy', 40, 'None');

--TravelAgent Table

 INSERT INTO TravelAgent (TravelAgentID, AgentName, CommissionRate)

 VALUES (1, 'TravelHub', 10.00), (2, 'GlobalTours', 12.50), (3, 'EpicTravels', 8.00), (4, 'CityExplorers', 15.00), (5, 'StarVoyages', 10.50), (6, 'SunshineHolidays', 11.00), (7, 'DreamJourneys', 9.50), (8, 'AdventureQuest', 14.00), (9, 'VacationPalace', 12.00), (10, 'JourneyMasters', 13.00);
-- Booking Table 

INSERT INTO Booking (BookingID, PassengerID, TrainID, BookingDate, TravelAgentID, TravelDate, SeatNumber, Amount)

 VALUES (1, 1, 1, '2023-11-01', 1, '2023-11-15', 101, 120.00), (2, 2, 2, '2023-10-15', 2, '2023-10-30', 201, 150.00), (3, 3, 3, '2023-10-20', 3, '2023-11-05', 301, 180.00), (4, 4, 4, '2023-11-05', 4, '2023-11-20', 401, 200.00), (5, 5, 5, '2023-10-10', 5, '2023-10-25', 501, 220.00), (6, 6, 6, '2023-10-25', 6, '2023-11-10', 601, 250.00), (7, 7, 7, '2023-11-10', 7, '2023-10-30', 701, 180.00), (8, 8, 8, '2023-10-30', 8, '2023-11-15', 801, 200.00), (9, 9, 9, '2023-11-15', 9, '2023-11-01', 901, 210.00), (10, 10, 10, '2023-11-20', 10, '2023-11-13', 1001, 230.00);

--Serviceday Table

INSERT INTO ServiceDays (ServiceDayID, Day)

 VALUES (1, 'Monday'), (2, 'Tuesday'), (3, 'Wednesday'), (4, 'Thursday'), (5, 'Friday'), (6, 'Saturday'),
(7, 'Sunday');

-- TrainServiceDays Table 

INSERT INTO TrainServiceDays (TrainID, ServiceDayID) 

VALUES (1, 1), (1, 2),(1, 3), (2, 4), (2, 5), (3, 6), (4, 7), (5, 1), (5, 2), (5, 3), (6, 4), (6, 5), (7, 6), (8, 7), (9, 1), (9, 2), (9, 3), (10, 4), (10, 5), (10, 6);

-- Route Table

 INSERT INTO Route (RouteID, StartStationID, EndStationID, Distance, TimeTaken)

 VALUES (1, 1, 2, 548, '5 hours 40 minutes'), (2, 3, 4, 412, '6 hours 10 minutes'), (3, 5, 6, 500, '8 hours 30 minutes'), (4, 7, 8, 248, '5 hours 20 minutes'), (5, 1, 8, 400, '6 hours 35 minutes'), (6, 9, 10, 588, '6 hours'), (7, 2, 7, 1062, '10 hours 45 minutes'), (8, 4, 5, 700, '7 hours 45 minutes'), (9, 10, 1, 432, '5 hours'), (10, 6, 9, 246, '3 hours');

-- Schedule Table

INSERT INTO Schedule (ScheduleID, TrainID, RouteID, DepartureTime, OperatingDays)

 VALUES (1, 1, 1, '05:45:00', 'Mon,Tue,Wed,Thu,Fri'), (2, 1, 1, '13:15:00', 'Sat,Sun'), (3, 2, 2, '09:00:00', 'Mon,Wed,Fri,Sat'), (4, 2, 2, '16:10:00', 'Sun,Tue'), (5, 3, 3, '08:30:00', 'Sun'), (6, 3, 3, '17:00:00', 'Mon'), (7, 4, 4, '05:20:00', 'Mon,Thu,Fri,Sat,Sun'), (8, 4, 4, '06:35:00', 'Tue'), (9, 5, 5, '07:45:00', 'Mon,Thu,Fri,Sat,Sun'), (10, 5, 5, '08:00:00', 'Tue,Wed,Sat,Sun');

-- DriverAssignment Table

 INSERT INTO DriverAssignment (DriverID, ScheduleID, MainDriver, CoDriver, Remark)

 VALUES ('K0012', 1, TRUE, FALSE, 'Main Driver'), ('K0012', 2, FALSE, TRUE, 'Co-Driver'), ('S0023', 3, TRUE, FALSE, 'Main Driver'), ('S0023', 4, TRUE, FALSE, 'Main Driver'), ('R0034', 5, FALSE, TRUE, 'Co-Driver'), ('R0034', 6, TRUE, FALSE, 'Main Driver'), ('P0045', 7, FALSE, TRUE, 'Co-Driver'), ('P0045', 8, TRUE, FALSE, 'Main Driver'), ('A0056', 9, TRUE, FALSE, 'Main Driver'), ('A0056', 10, FALSE, TRUE, 'Co-Driver');

--LateArrivals Table

INSERT INTO LateArrivals (LateArrivalID, TrainID, ArrivalTime, ScheduledArrivalTime, DelayMinutes, LateReason, LateDate)

VALUES  (1, 1, '2023-11-30 08:15:00', '2023-11-30 08:00:00', 15, 'Signal Issue', '2023-11-30');


## Part 2

## #SET A (Prins Pardhi)

Q.1 
SELECT DISTINCT T.*
FROM Train T
JOIN Schedule S ON T.TrainID = S.TrainID
JOIN Route R ON S.RouteID = R.RouteID
JOIN MaintenanceSchedule MS ON T.TrainID = MS.TrainID
WHERE 
    (R.StartStationID = (SELECT StationID FROM Station WHERE StationName = 'Goa'))
    AND
    (R.EndStationID = (SELECT StationID FROM Station WHERE StationName = 'Mumbai'))
    AND
    (
        R.StartStationID = (SELECT StationID FROM Station WHERE StationName = 'Ajmer')
        AND
        R.EndStationID = (SELECT StationID FROM Station WHERE StationName = 'Lonavala')
    )
    AND
    (
        MS.MaintenanceType = 'Routine'
        AND
        MS.LastMaintenanceDate <= '2023-11-30'
        AND
        (
            SELECT COUNT(*) FROM MaintenanceSchedule
            WHERE TrainID = T.TrainID AND MaintenanceType = 'Routine'
            GROUP BY TrainID
        ) >= T.TotalCoaches / 2
    );




Q.2 
 SELECT
    R.RouteID,
    R.StartStationID,
    R.EndStationID,
    R.Distance,
    R.TimeTaken,
    COUNT(B.BookingID) AS TotalSeatsSold,
    SUM(CASE WHEN P.Age <= 12 THEN 1 ELSE 0 END) AS ChildrenSeats,
    SUM(CASE WHEN P.Age > 12 AND P.Age <= 65 THEN 1 ELSE 0 END) AS AdultSeats,
    SUM(CASE WHEN P.Age > 65 THEN 1 ELSE 0 END) AS SeniorCitizenSeats
FROM
    Route R
JOIN
    Schedule S ON R.RouteID = S.RouteID
JOIN
    Train T ON S.TrainID = T.TrainID
JOIN
    Booking B ON T.TrainID = B.TrainID
JOIN
    Passenger P ON B.PassengerID = P.PassengerID
WHERE
    MONTH(B.TravelDate) = 10 AND YEAR(B.TravelDate) = 2023
GROUP BY
    R.RouteID,
    R.StartStationID,
    R.EndStationID,
    R.Distance,
    R.TimeTaken
ORDER BY
    TotalSeatsSold DESC;

![image](https://github.com/PrinsPardhi/Assignment/assets/73771547/d40c7911-4e16-48bc-b0b4-de1f74f99717)

 


Q.3 
SELECT
    TA.TravelAgentID,
    TA.AgentName,
    TA.CommissionRate,
    COUNT(B.BookingID) AS ConfirmedBookings
FROM
    TravelAgent TA
JOIN
    Booking B ON TA.TravelAgentID = B.TravelAgentID
WHERE
    MONTH(B.BookingDate) = 9 AND YEAR(B.BookingDate) = 2023
GROUP BY
    TA.TravelAgentID,
    TA.AgentName,
    TA.CommissionRate
HAVING
    ConfirmedBookings > 10
ORDER BY
    ConfirmedBookings DESC;



Q.4
   SELECT
    R.RouteID,
    R.StartStationID,
    R.EndStationID,
    R.Distance,
    R.TimeTaken,
    COUNT(P.PassengerID) AS SeniorCitizenTravels
FROM
    Route R
JOIN
    Schedule S ON R.RouteID = S.RouteID
JOIN
    Train T ON S.TrainID = T.TrainID
JOIN
    Booking B ON T.TrainID = B.TrainID
JOIN
    Passenger P ON B.PassengerID = P.PassengerID
WHERE
    P.Age > 65
GROUP BY
    R.RouteID,
    R.StartStationID,
    R.EndStationID,
    R.Distance,
    R.TimeTaken
ORDER BY
    SeniorCitizenTravels DESC
LIMIT 1;



Q.5 
SELECT
    R.RouteID,
    R.StartStationID,
    R.EndStationID,
    R.Distance,
    R.TimeTaken
FROM
    Route R
WHERE
    NOT EXISTS (
        SELECT 1
        FROM Schedule Sch
        JOIN Train T ON Sch.TrainID = T.TrainID
        LEFT JOIN LateArrivals LA ON T.TrainID = LA.TrainID
        WHERE
            Sch.RouteID = R.RouteID
            AND LA.LateArrivalID IS NOT NULL
    );

![image](https://github.com/PrinsPardhi/Assignment/assets/73771547/4e7c5131-4ef2-4ccf-9771-f2851b54efe5)






Thank You!!
