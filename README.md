CS 105 Persistent Data Management
Part 1
Part 1 of assignment, we will need to identify data requirements, perform normalization (up to 3NF), create MySQL tables, and insert sample data. Here's a step-by-step solution.

Step 1: Identify Data Requirements
Based on the provided information, we can identify the following entities:

# Train
# Route
# Schedule
# Staff (Driver, Co-Driver)
# Coach
# Booking
# TravelAgent
# Passenger
# Maintenance
Part 2
Normalize the Data (Up to 3NF)

We need to ensure that the data is in 1NF, 2NF, and 3NF: 1NF: Each attribute must contain only atomic values. Ensure there are no repeating groups. 2NF: Remove partial dependencies, meaning that non-prime attributes are dependent on the entire primary key. 3NF: Remove transitive dependencies, meaning that non-prime attributes should not depend on other non-prime attributes.

Entity Relationships::
A Train has multiple Coaches.
A Train operates on a Route.
A Route has a Schedule.
Staff (Driver, Co-Driver) is associated with a Train on a given Schedule.
Each Coach has a Schedule.
Bookings are made for a particular Route on a specific Schedule.
Travel Agents handle Bookings.
Passengers are associated with Bookings.
Maintenance is related to Coaches.
The InterCity Express Trains (IET) database is designed to support day-to-day operations, route management, and decision-making processes. The normalization process ensures data integrity, reduces redundancy, and improves overall database efficiency.

Entities::
Train: TrainID (Primary Key), TrainName

Route: RouteID (Primary Key), RouteName, Distance, TimeTaken, OperatingDays

Schedule: ScheduleID (Primary Key), DepartureTime, ArrivalTime, TrainID (Foreign Key referencing Train.TrainID), RouteID (Foreign Key referencing Route.RouteID)

Staff: StaffID (Primary Key), StaffType, StaffName, ContactNumber, CityOfResidence

Coach: CoachID (Primary Key), CoachNumber, Mileage, LastMaintenanceDate

Booking: BookingID (Primary Key), BookingDate, ConcessionType, ScheduleID (Foreign Key referencing Schedule.ScheduleID), TravelAgentID (Foreign Key referencing TravelAgent.TravelAgentID), PassengerID (Foreign Key referencing Passenger.PassengerID)

TravelAgent: TravelAgentID (Primary Key), AgentName, CommissionRate

Passenger: PassengerID (Primary Key), PassengerName, Age

Maintenance: MaintenanceID (Primary Key), MaintenanceType, ScheduleDate, CoachID (Foreign Key referencing Coach.CoachID)

Normalization Steps:
1. First Normal Form (1NF):
All tables have atomic values in each cell, ensuring there are no repeating groups or arrays.

2. Second Normal Form (2NF):
No changes were needed for 2NF, as all non-prime attributes are fully functionally dependent on the primary key.

3. Third Normal Form (3NF):
Schedule Table: Removed redundant attributes (TrainID, RouteID) to create a new table (TrainRoute).TrainRoute (TrainID, RouteID) establishes a relationship between Train and Route.

Booking Table: Removed redundant attributes (ScheduleID) to create a new table (ScheduleBooking).ScheduleBooking (ScheduleID, BookingID) establishes a relationship between Schedule and Booking.

The normalization process has resulted in a well-structured database with tables in 3NF, ensuring data consistency and minimizing redundancy. The relationships between entities have been established through foreign key references, facilitating efficient data retrieval and management.

Part 3
Create MySQL Tables :
-- Create Train Table

CREATE TABLE Train ( TrainID INT PRIMARY KEY, TrainName VARCHAR(255) );

-- Create Route Table

CREATE TABLE Route ( RouteID INT PRIMARY KEY, RouteName VARCHAR(255), Distance INT, TimeTaken VARCHAR(20), OperatingDays VARCHAR(20) );

-- Create Schedule Table

CREATE TABLE Schedule ( ScheduleID INT PRIMARY KEY, DepartureTime TIME, ArrivalTime TIME );

-- Create Staff Table

CREATE TABLE Staff ( StaffID VARCHAR(10) PRIMARY KEY, StaffType VARCHAR(20), StaffName VARCHAR(255), ContactNumber VARCHAR(15), CityOfResidence VARCHAR(255) );

-- Create Coach Table

CREATE TABLE Coach ( CoachID INT PRIMARY KEY, CoachNumber VARCHAR(20), Mileage INT, LastMaintenanceDate DATE );

-- Create Booking Table

CREATE TABLE Booking ( BookingID INT PRIMARY KEY, ScheduleID INT, TravelAgentID INT, PassengerID INT, BookingDate DATE, ConcessionID INT, FOREIGN KEY (ScheduleID) REFERENCES Schedule(ScheduleID), FOREIGN KEY (TravelAgentID) REFERENCES TravelAgent(TravelAgentID), FOREIGN KEY (PassengerID) REFERENCES Passenger(PassengerID), FOREIGN KEY (ConcessionID) REFERENCES Concession(ConcessionID) );

-- Create TravelAgent Table

CREATE TABLE TravelAgent ( TravelAgentID INT PRIMARY KEY, AgentName VARCHAR(255), CommissionRate DECIMAL(5, 2) );

-- Create Passenger Table

CREATE TABLE Passenger ( PassengerID INT PRIMARY KEY, PassengerName VARCHAR(255), Age INT );

-- Create Concession Table

CREATE TABLE Concession ( ConcessionID INT PRIMARY KEY, ConcessionType VARCHAR(10) UNIQUE, DiscountRate DECIMAL(5, 2) );

-- Create Maintenance Table

CREATE TABLE Maintenance ( MaintenanceID INT PRIMARY KEY, CoachID INT, MaintenanceType VARCHAR(50), ScheduleDate DATE );

CREATE TABLE Roster ( RosterID INT AUTO_INCREMENT PRIMARY KEY, StaffID VARCHAR(10) NOT NULL, Date DATE NOT NULL, Time TIME NOT NULL, Route VARCHAR(255) NOT NULL, Remark VARCHAR(255), CONSTRAINT fk_staff FOREIGN KEY (StaffID) REFERENCES Staff(StaffID) ON UPDATE CASCADE ON DELETE CASCADE );

-- Assuming you may need an index on the Date column for better query performance

CREATE INDEX idx_date ON Roster (Date);

Part 4
INSERT INTO Train (TrainID, TrainName) VALUES (1, 'Mumbai Central - Gandhinagar'), (2, 'New Delhi - Himachal'), (3, 'Secunderabad - Visakhapatnam'), (4, 'Mumbai - Sainagar Shirdi'), (5, 'Mumbai - Solapur'), (6, 'Bhopal - Delhi'), (7, 'Lonavala - Ajmer'), (8, 'Dharwad - Bengaluru'), (9, 'Bhopal - Indore'), (10, 'Mumbai - Goa');

INSERT INTO Route (RouteID, RouteName, Distance, TimeTaken, OperatingDays) VALUES (1, 'Mumbai Central - Gandhinagar', 548, '5 hours 40 minutes', '6 days a week except Sundays'), (2, 'New Delhi - Himachal', 412, '6 hours 10 minutes', '6 days a week except Thursdays'), (3, 'Secunderabad - Visakhapatnam', 500, '8 hours 30 minutes', 'Once a week (every Sunday)'), (4, 'Mumbai - Sainagar Shirdi', 248, '5 hours 20 minutes', '6 days a week except Tuesdays'), (5, 'Mumbai - Solapur', 400, '6 hours and 35 minutes', '6 days a week except Wednesdays'), (6, 'Bhopal - Delhi', 700, '7 hours and 45 minutes', '6 days a week except Saturdays'), (7, 'Lonavala - Ajmer', 1062, '10 hours 45 minutes', 'Once a week (every Saturday)'), (8, 'Dharwad - Bengaluru', 432, '5 hours', 'Monday, Wednesday, and Saturday'), (9, 'Bhopal - Indore', 246, '3 hours', 'Every day'), (10, 'Mumbai - Goa', 588, '6 hours', 'Every day');

INSERT INTO Roster (StaffID, Date, Time, Route, Remark) VALUES ('K0012', '2023-10-07', '06:00:00', 'Mumbai - Goa', 'Main Driver'), ('K0012', '2023-10-07', '13:00:00', 'Goa - Mumbai', 'Co-Driver'), ('K0012', '2023-10-08', '09:00:00', 'Mumbai - Goa', 'Main Driver'), ('K0012', '2023-10-08', '16:00:00', 'Goa - Mumbai', 'Main Driver'), ('K0012', '2023-10-09', '09:00:00', 'Mumbai - Goa', 'Co-Driver'), ('K0012', '2023-10-09', '16:00:00', 'Goa - Mumbai', 'Main Driver'), ('K0012', '2023-10-10', '06:00:00', 'Lonavala - Ajmer', 'Co-Driver'), ('K0012', '2023-10-10', '17:00:00', 'Ajmer - Lonavala', 'Main Driver'); ('K0012', '2023-10-11', '00:00:00', 'OFF', 'OFF'), ('K0012', '2023-10-12', '06:00:00', 'Mumbai - Goa', 'Main Driver'), ('K0012', '2023-10-12', '13:00:00', 'Goa - Mumbai', 'Co-Driver'), ('K0012', '2023-10-13', '09:00:00', 'Mumbai - Goa', 'Main Driver'), ('K0012', '2023-10-13', '16:00:00', 'Goa - Mumbai', 'Main Driver');

-- Inserting new routes

INSERT INTO Route (RouteID, RouteName, Distance, TimeTaken, OperatingDays, IntermediateStations, ArrivalTimes, DepartureTimes) VALUES (11, 'Chennai - Bengaluru', 350, '4 hours', 'Every day', 'Station1, Station2', '6:00 am, 8:00 am', '7:45 pm, 9:45 pm'), (12, 'Delhi - Pune', 1200, '16 hours', 'Every day', 'StationA, StationB, StationC', '8:00 am, 2:00 pm, 6:00 pm', '12:00 am, 6:00 am, 10:00 am');

-- Inserting new schedules

INSERT INTO Schedule (ScheduleID, DepartureTime, ArrivalTime, StartStation, EndStation, IntermediateStations) VALUES (11, '5:45 am', '12:10 pm', 'KSR Bengaluru', 'Dharwad', 'Station1, Station2'), (12, '1:15 pm', '7:45 pm', 'Dharwad', 'KSR Bengaluru', 'Station2, Station1');

-- Inserting coach information for the new routes

INSERT INTO Coach (CoachID, CoachNumber, Mileage, LastMaintenanceDate, StandbyActivated, RouteID) VALUES (101, 'C101', 6000, '2023-10-01', FALSE, 11), (102, 'C102', 4500, '2023-05-15', FALSE, 11), (103, 'C103', 8000, '2023-08-20', FALSE, 12), (104, 'C104', 3000, '2023-02-10', FALSE, 12);

Modified Tables:
Route Table:

Add columns for intermediate stations, arrival, and departure times.

ALTER TABLE Route ADD COLUMN IntermediateStations TEXT, ADD COLUMN ArrivalTimes TEXT, ADD COLUMN DepartureTimes TEXT;

Schedule Table:

Modify the table to include details about the start and end stations, and intermediate stations for each train.

ALTER TABLE Schedule ADD COLUMN StartStation VARCHAR(255), ADD COLUMN EndStation VARCHAR(255), ADD COLUMN IntermediateStations TEXT;

Coach Table:

Add columns for mileage, last maintenance date, and a flag indicating whether a standby coach is activated.

ALTER TABLE Coach ADD COLUMN Mileage INT, ADD COLUMN LastMaintenanceDate DATE, ADD COLUMN StandbyActivated BOOLEAN, ADD COLUMN RouteID INT, ADD FOREIGN KEY (RouteID) REFERENCES Route(RouteID);

New Tables:
Driver Table:

Create a table to store information about drivers, including their contact information and rest day.

CREATE TABLE Driver ( DriverID INT PRIMARY KEY, DriverName VARCHAR(255), ContactNumber VARCHAR(15), RestDay VARCHAR(10) );

RouteDriver Table:

Create a table to associate drivers with routes.

CREATE TABLE RouteDriver ( RouteID INT, DriverID INT, PRIMARY KEY (RouteID, DriverID), FOREIGN KEY (RouteID) REFERENCES Route(RouteID), FOREIGN KEY (DriverID) REFERENCES Driver(DriverID) );

Accommodation Table:

Create a table to manage accommodation details for drivers.

CREATE TABLE Accommodation ( DriverID INT PRIMARY KEY, AccommodationDetails TEXT, FOREIGN KEY (DriverID) REFERENCES Driver(DriverID) );

MaintenanceSchedule Table:

Create a table to schedule routine maintenance for coaches.

CREATE TABLE MaintenanceSchedule ( CoachID INT, MaintenanceType VARCHAR(50), ScheduleDate DATE, PRIMARY KEY (CoachID, MaintenanceType), FOREIGN KEY (CoachID) REFERENCES Coach(CoachID) );

Concession Table (modified):

Add a new column to store the age limit for concession eligibility.

ALTER TABLE Concession ADD COLUMN AgeLimit INT;

Booking Table (modified):

Add a column to track whether the booking is made by a travel agent or the general public.

ALTER TABLE Booking ADD COLUMN BookingType VARCHAR(20);

Passenger Information Table:

Create a table to store personalized passenger information.

CREATE TABLE PassengerInfo ( PassengerID INT PRIMARY KEY, PassengerName VARCHAR(255), Age INT, ContactNumber VARCHAR(15), Email VARCHAR(255) );

-- Inserting new routes
INSERT INTO Route (RouteID, RouteName, Distance, TimeTaken, OperatingDays, IntermediateStations, ArrivalTimes, DepartureTimes) VALUES (11, 'Chennai - Bengaluru', 350, '4 hours', 'Every day', 'Station1, Station2', '6:00 am, 8:00 am', '7:45 pm, 9:45 pm'), (12, 'Delhi - Pune', 1200, '16 hours', 'Every day', 'StationA, StationB, StationC', '8:00 am, 2:00 pm, 6:00 pm', '12:00 am, 6:00 am, 10:00 am');

-- Inserting new schedules
INSERT INTO Schedule (ScheduleID, DepartureTime, ArrivalTime, StartStation, EndStation, IntermediateStations) VALUES (11, '5:45 am', '12:10 pm', 'KSR Bengaluru', 'Dharwad', 'Station1, Station2'), (12, '1:15 pm', '7:45 pm', 'Dharwad', 'KSR Bengaluru', 'Station2, Station1');

-- Inserting coach information for the new routes
INSERT INTO Coach (CoachID, CoachNumber, Mileage, LastMaintenanceDate, StandbyActivated, RouteID) VALUES (101, 'C101', 6000, '2023-10-01', FALSE, 11), (102, 'C102', 4500, '2023-05-15', FALSE, 11), (103, 'C103', 8000, '2023-08-20', FALSE, 12), (104, 'C104', 3000, '2023-02-10', FALSE, 12);
