# Projects[Coding Project.docx](https://github.com/avafaith/Projects/files/10053562/Coding.Project.docx)

Code for a database:
IF NOT EXISTS(SELECT * FROM sys.databases
	WHERE NAME = N'DisneyResorts2021')
	CREATE DATABASE DisneyResorts2021
GO
USE DisneyResorts2021
--
-- Alter the path so the script can find the CSV files 
--
DECLARE
@data_path NVARCHAR(256);
SELECT @data_path = 'C:\Users\aavsh\OneDrive\Desktop\New CSV Files\';


-- Delete existing tables
--
IF EXISTS(
	SELECT *
	FROM sys.tables
	WHERE NAME = N'Employee_Type'
       )
	DROP TABLE Employee_Type;
--
IF EXISTS(
	SELECT *
	FROM sys.tables
	WHERE NAME = N'Employee_Location'
       )
	DROP TABLE Employee_Location;

--
IF EXISTS(
	SELECT *
	FROM sys.tables
	WHERE NAME = N'Employee'
       )
	DROP TABLE Employee;
--
IF EXISTS(
	SELECT *
	FROM sys.tables
	WHERE NAME = N'Ride_Location'
       )
	DROP TABLE Ride_Location;
--

IF EXISTS(
	SELECT *
	FROM sys.tables
	WHERE NAME = N'Park'
       )
	DROP TABLE Park;
--
IF EXISTS(
	SELECT *
	FROM sys.tables
	WHERE NAME = N'Ride_Type'
       )
	DROP TABLE Ride_Type;
--
IF EXISTS(
	SELECT *
	FROM sys.tables
	WHERE NAME = N'Customer_Ride'
       )
	DROP TABLE Customer_Ride;
--
IF EXISTS(
	SELECT *
	FROM sys.tables
	WHERE NAME = N'Ticket_Type'
       )
	DROP TABLE Ticket_Type;
--
IF EXISTS(
	SELECT *
	FROM sys.tables
	WHERE NAME = N'Customer_Type'
       )
	DROP TABLE Customer_Type;
--

IF EXISTS(
	SELECT *
	FROM sys.tables
	WHERE NAME = N'Customer_Ticket'
       )
	DROP TABLE Customer_Ticket;
--
IF EXISTS(
	SELECT *
	FROM sys.tables
	WHERE NAME = N'Customer'
       )
	DROP TABLE Customer;
--
IF EXISTS(
	SELECT *
	FROM sys.tables
	WHERE NAME = N'Ride'
       )
	DROP TABLE Ride;
--
IF EXISTS(
	SELECT *
	FROM sys.tables
	WHERE NAME = N'Ticket'
       )
	DROP TABLE Ticket;


-- Create tables


CREATE TABLE Ticket
	(TicketID						INT CONSTRAINT pk_ticket_ticketid PRIMARY KEY,		
	Quantity						INT CONSTRAINT nn_ticket_quantity NOT NULL,
	Price							MONEY CONSTRAINT nn_ticket_price NOT NULL,
	
);

CREATE TABLE Ride
	(RideID							INT CONSTRAINT pk_ride_rideid PRIMARY KEY,			
	[Name]							NVARCHAR (50) CONSTRAINT nn_ride_name NOT NULL,
	MinHeight						NVARCHAR (10) CONSTRAINT nn_ride_minheight NOT NULL,
	MaxCapacity						NVARCHAR (2) CONSTRAINT nn_ride_maxcapacity NOT NULL,
	Duration						NVARCHAR (5) CONSTRAINT nn_ride_duration NOT NULL,
);

CREATE TABLE Customer
	(CustomerID						INT CONSTRAINT pk_customer_customerid PRIMARY KEY,
	RideID							INT CONSTRAINT fk_customer_rideid FOREIGN KEY
									REFERENCES Ride(RideID),
	FirstName						NVARCHAR (30) CONSTRAINT nn_customer_firstname NOT NULL,
	LastName						NVARCHAR (30) CONSTRAINT nn_customer_lastname NOT NULL,
	Gender							NCHAR(1) CONSTRAINT ck_customer_gender CHECK ((Gender = 'M') OR (Gender = 'F') OR (Gender = 'N')),			
	BirthDate						DATE CONSTRAINT nn_customer_birthdate NOT NULL,
	Height							NVARCHAR (10) CONSTRAINT nn_customer_height NOT NULL,
	Email							NVARCHAR(50) CONSTRAINT nn_customer_email NOT NULL,
	[State]							NVARCHAR (2) CONSTRAINT nn_customer_state NOT NULL,

);

CREATE TABLE Customer_Ticket
	(CustomerTicketID				NVARCHAR(10) CONSTRAINT pk_customerticket_customerticketid PRIMARY KEY,
	CustomerID						INT CONSTRAINT fk_customerticket_customerid FOREIGN KEY
									REFERENCES Customer(CustomerID),
	TicketID						INT CONSTRAINT fk_customerticket_ticket FOREIGN KEY
									REFERENCES Ticket(TicketID),
	[Date]							DATE CONSTRAINT nn_customerticket_date NOT NULL,
	Price							MONEY CONSTRAINT nn_customerticket_price NOT NULL,
	Quantity						INT CONSTRAINT nn_customerticket_quantity NOT NULL,
	Discount						FLOAT CONSTRAINT nn_customerticket_discount NULL,
	FastPass						NCHAR(1) CONSTRAINT ck_customerticket_fastpass CHECK ((FastPass = 'Y') OR (FastPass = 'N')),
	TicketQuantity					INT CONSTRAINT nn_customerticket_quantity NOT NULL,
	
);

CREATE TABLE Customer_Type
	(CustomerTypeID         		NVARCHAR (10) CONSTRAINT pk_customertype_customertypeID PRIMARY KEY,
	CustomerID						INT CONSTRAINT fk_customertype_customerid FOREIGN KEY
									REFERENCES Customer(CustomerID),
	CustomerType                 	NVARCHAR (10) CONSTRAINT nn_customertype_customertype NOT NULL,
	[Date]                   		DATE CONSTRAINT nn_customertype_date NOT NULL,

);


CREATE TABLE Ticket_Type
	(TicketTypeID					NVARCHAR (10) CONSTRAINT pk_tickettype_tickettypeid PRIMARY KEY,
	TicketID						INT CONSTRAINT fk_tickettype_ticketid FOREIGN KEY
									REFERENCES Ticket(TicketID),
	TicketType						NVARCHAR (30) CONSTRAINT nn_tickettype_tickettype NOT NULL,
);


CREATE TABLE Customer_Ride
	(CustomerRideID					NVARCHAR (10) CONSTRAINT pk_customerride_customerrideid PRIMARY KEY,
	CustomerID						INT CONSTRAINT fk_customerride_customerid FOREIGN KEY
									REFERENCES Customer(CustomerID),
	RideID							INT CONSTRAINT fk_customerride_rideid FOREIGN KEY
									REFERENCES Ride(RideID),
	[Date]							DATE CONSTRAINT nn_customerride_date NOT NULL,
	Quantity						INT CONSTRAINT nn_customerticket_quantity NOT NULL,
);


CREATE TABLE Ride_Type				
	(RideTypeID						NVARCHAR (10) CONSTRAINT pk_ridetype_ridetypeid PRIMARY KEY,	
	RideID							INT CONSTRAINT fk_ridetype_rideid FOREIGN KEY
									REFERENCES Ride(RideID),
	RideType						NVARCHAR (30) CONSTRAINT nn_ridetype_ridetype NOT NULL,
);

CREATE TABLE Park
	(ParkID							INT CONSTRAINT pk_park_parkid PRIMARY KEY,	
	[Name]							NVARCHAR (50) CONSTRAINT nn_park_name NOT NULL,
	Capacity						INT CONSTRAINT nn_park_capacity NOT NULL,
);

CREATE TABLE Ride_Location
	(RideLocationID					NVARCHAR (10) CONSTRAINT pk_ridelocation_ridelocationid PRIMARY KEY,
	RideID							INT CONSTRAINT fk_ridelocation_rideid FOREIGN KEY
									REFERENCES Ride(RideID),
	ParkID							INT CONSTRAINT fk_ridelocation_parkid FOREIGN KEY
									REFERENCES Park(ParkID)
);

CREATE TABLE Employee
	(EmployeeID					INT CONSTRAINT pk_employee_employeeid PRIMARY KEY,
	EmployeeLocationID			NVARCHAR (10) CONSTRAINT fk_employee_employeelocationid NOT NULL,
	FirstName					NVARCHAR (30) CONSTRAINT nn_employee_firstname NOT NULL,
	LastName					NVARCHAR (30) CONSTRAINT nn_employee_lastname NOT NULL,
	BirthDate					DATE CONSTRAINT nn_employee_birthdate NOT NULL,
	Gender						NCHAR(1) CONSTRAINT ck_employee_gender CHECK ((Gender = 'M') OR (Gender = 'F') OR (Gender = 'N')) NOT NULL,
	
);

CREATE TABLE Employee_Location
	(EmployeeLocationID			NVARCHAR (10) CONSTRAINT pk_employeelocation_employeelocationid PRIMARY KEY,	
	ParkID						INT CONSTRAINT fk_employeelocation_parkid FOREIGN KEY
								REFERENCES Park(ParkID),
	EmployeeID					INT CONSTRAINT fk_employeelocation_employeeid FOREIGN KEY
								REFERENCES Employee(EmployeeID),
	[Date]						DATE CONSTRAINT nn_employeelocation_date NOT NULL,
	
);


CREATE TABLE Employee_Type
	(EmployeeTypeID				NVARCHAR (10) CONSTRAINT pk_employeetype_employeetypeid PRIMARY KEY,
	EmployeeID					INT CONSTRAINT fk_employeetype_employeeid FOREIGN KEY
								REFERENCES Employee(EmployeeID),
	BeginDate					DATE CONSTRAINT nn_employeetype_begindate NOT NULL,
	Position					NVARCHAR (30) CONSTRAINT nn_employeetype_position NOT NULL,
	Commission					INT CONSTRAINT nn_employeetype_commission NOT NULL,
	
);

--
-- Load table data
--
EXECUTE (N'BULK INSERT Ticket FROM ''' + @data_path + N'Ticket.csv''
WITH (
	CHECK_CONSTRAINTS,
	CODEPAGE=''ACP'',
	DATAFILETYPE = ''char'',
	FIELDTERMINATOR= '','',
	ROWTERMINATOR = ''\n'',
	KEEPIDENTITY,
	TABLOCK
	);
');
--

EXECUTE (N'BULK INSERT Ride FROM ''' + @data_path + N'Ride.csv''
WITH (
	CHECK_CONSTRAINTS,
	CODEPAGE=''ACP'',
	DATAFILETYPE = ''char'',
	FIELDTERMINATOR= '','',
	ROWTERMINATOR = ''\n'',
	KEEPIDENTITY,
	TABLOCK
	);
');
--
EXECUTE (N'BULK INSERT Customer FROM ''' + @data_path + N'Customer.csv''
WITH (
	CHECK_CONSTRAINTS,
	CODEPAGE=''ACP'',
	DATAFILETYPE = ''char'',
	FIELDTERMINATOR= '','',
	ROWTERMINATOR = ''\n'',
	KEEPIDENTITY,
	TABLOCK
	);
');

--
EXECUTE (N'BULK INSERT Customer_Ticket FROM ''' + @data_path + N'Customer_Ticket.csv''
WITH (
	CHECK_CONSTRAINTS,
	CODEPAGE=''ACP'',
	DATAFILETYPE = ''char'',
	FIELDTERMINATOR= '','',
	ROWTERMINATOR = ''\n'',
	TABLOCK
	);
');
--
EXECUTE (N'BULK INSERT Customer_Type FROM ''' + @data_path + N'Customer_Type.csv''
WITH (
	CHECK_CONSTRAINTS,
	CODEPAGE=''ACP'',
	DATAFILETYPE = ''char'',
	FIELDTERMINATOR= '','',
	ROWTERMINATOR = ''\n'',
	KEEPIDENTITY,
	TABLOCK
	);
');

--
EXECUTE (N'BULK INSERT Ticket_Type FROM ''' + @data_path + N'Ticket_Type.csv''
WITH (
	CHECK_CONSTRAINTS,
	CODEPAGE=''ACP'',
	DATAFILETYPE = ''char'',
	FIELDTERMINATOR= '','',
	ROWTERMINATOR = ''\n'',
	KEEPIDENTITY,
	TABLOCK
	);
');
--
EXECUTE (N'BULK INSERT Customer_Ride FROM ''' + @data_path + N'Customer_Ride.csv''
WITH (
	CHECK_CONSTRAINTS,
	CODEPAGE=''ACP'',
	DATAFILETYPE = ''char'',
	FIELDTERMINATOR= '','',
	ROWTERMINATOR = ''\n'',
	KEEPIDENTITY,
	TABLOCK
	);
');
--
EXECUTE (N'BULK INSERT Ride_Type FROM ''' + @data_path + N'Ride_Type.csv''
WITH (
	CHECK_CONSTRAINTS,
	CODEPAGE=''ACP'',
	DATAFILETYPE = ''char'',
	FIELDTERMINATOR= '','',
	ROWTERMINATOR = ''\n'',
	KEEPIDENTITY,
	TABLOCK
	);
');

EXECUTE (N'BULK INSERT Park FROM ''' + @data_path + N'Park.csv''
WITH (
	CHECK_CONSTRAINTS,
	CODEPAGE=''ACP'',
	DATAFILETYPE = ''char'',
	FIELDTERMINATOR= '','',
	ROWTERMINATOR = ''\n'',
	KEEPIDENTITY,
	TABLOCK
	);
');
--
EXECUTE (N'BULK INSERT Ride_Location FROM ''' + @data_path + N'Ride_Location.csv''
WITH (
	CHECK_CONSTRAINTS,
	CODEPAGE=''ACP'',
	DATAFILETYPE = ''char'',
	FIELDTERMINATOR= '','',
	ROWTERMINATOR = ''\n'',
	KEEPIDENTITY,
	TABLOCK
	);
');
--
EXECUTE (N'BULK INSERT Employee FROM ''' + @data_path + N'Employee.csv''
WITH (
	CHECK_CONSTRAINTS,
	CODEPAGE=''ACP'',
	DATAFILETYPE = ''char'',
	FIELDTERMINATOR= '','',
	ROWTERMINATOR = ''\n'',
	KEEPIDENTITY,
	TABLOCK
	);
');
--
EXECUTE (N'BULK INSERT Employee_Location FROM ''' + @data_path + N'Employee_Location.csv''
WITH (
	CHECK_CONSTRAINTS,
	CODEPAGE=''ACP'',
	DATAFILETYPE = ''char'',
	FIELDTERMINATOR= '','',
	ROWTERMINATOR = ''\n'',
	KEEPIDENTITY,
	TABLOCK
	);
');
--
EXECUTE (N'BULK INSERT Employee_Type FROM ''' + @data_path + N'Employee_Type.csv''
WITH (
	CHECK_CONSTRAINTS,
	CODEPAGE=''ACP'',
	DATAFILETYPE = ''char'',
	FIELDTERMINATOR= '','',
	ROWTERMINATOR = ''\n'',
	KEEPIDENTITY,
	TABLOCK
	);
');





Code for a data mart:
-- DisneyResortsDM database developed and written by Ava Shapiro
-- Originally Written: October 2022 | Updated: Oct 2022
---------------------------------------------------------------
IF NOT EXISTS(SELECT * FROM sys.databases
	WHERE NAME = N'DisneyResortsDM')
	CREATE DATABASE DisneyResortsDM
GO
USE DisneyResortsDM
--
GO
--
IF EXISTS(
	SELECT *
	FROM sys.tables
	WHERE NAME = N'FactSales'
       )
	DROP TABLE FactSales;
--
IF EXISTS(
	SELECT *
	FROM sys.tables
	WHERE NAME = N'DimCustomer'
       )
	DROP TABLE DimCustomer;
--
IF EXISTS(
	SELECT *
	FROM sys.tables
	WHERE NAME = N'DimTicket'
       )
	DROP TABLE DimTicket;
--
IF EXISTS(
	SELECT *
	FROM sys.tables
	WHERE NAME = N'DimRide'
       )
	DROP TABLE DimRide;
--
IF EXISTS(
	SELECT *
	FROM sys.tables
	WHERE NAME = N'DimDate'
       )
	DROP TABLE DimDate;
--
-- Create tables
--
CREATE TABLE DimDate 
	(Date_SK INT CONSTRAINT pk_date_sk PRIMARY KEY, 
	Date DATE,
	Full_Date NCHAR(10), -- Date in MM-dd-yyyy format
	Day_Of_Month INT, -- Field will hold day number of Month
	Day_Name NVARCHAR(9), -- Contains name of the day, Sunday, Monday 
	Day_Of_Week INT, -- First Day Sunday=1 and Saturday=7
	Day_Of_Week_In_Month INT, -- 1st Monday or 2nd Monday in Month
	Day_Of_Week_In_Year INT,
	Day_Of_Quarter INT,
	Day_Of_Year INT,
	Week_Of_Month INT, -- Week Number of Month 
	Week_Of_Quarter INT, -- Week Number of the Quarter
	Week_Of_Year INT, -- Week Number of the Year
	Month INT, -- Number of the Month 1 to 12{}
	Month_Name NVARCHAR(9), -- January, February etc
	Month_Of_Quarter INT, -- Month Number belongs to Quarter
	Quarter NCHAR(2), 
	Quarter_Name NVARCHAR(9), -- First,Second...
	Year INT, -- Year value of Date stored in Row
	Year_Name NCHAR(7), -- CY 2017,CY 2018
	Month_Year NCHAR(10), -- Jan-2018,Feb-2018
	MM_YYYY INT,
	First_Day_Of_Month DATE,
	Last_Day_Of_Month DATE,
	First_Day_Of_Quarter DATE,
	Last_Day_Of_Quarter DATE,
	First_Day_Of_Year DATE,
	Last_Day_Of_Year DATE,
	Is_Holiday BIT, -- Flag 1=National Holiday, 0-No National Holiday
	Is_Weekday BIT, -- 0=Week End ,1=Week Day
	Holiday NVARCHAR(50), -- Name of Holiday in US
	Season NVARCHAR(10) -- Name of Season
	);
--
CREATE TABLE DimRide
	(Ride_SK			INT IDENTITY (1,1) NOT NULL CONSTRAINT pk_ride_sk PRIMARY KEY,
	Ride_AK				INT NOT NULL,
	Name				NVARCHAR(50) CONSTRAINT nn_name NOT NULL,
	Start_Date			DATETIME NOT NULL,
	End_Date			DATETIME NULL,
	Min_Height			NVARCHAR (10) CONSTRAINT nn_min_height NOT NULL, -- The minimum height of customers that are eligable to go on the rides
	Max_Capacity		NVARCHAR (2) CONSTRAINT nn_max_capacity NOT NULL, -- The number of seats avaiable on each ride
	Ride_Type			NVARCHAR(30) CONSTRAINT nn_ride_type NOT NULL, -- Adult ride, kids ride, water ride, etc.
	Duration			NVARCHAR (5) CONSTRAINT nn_duration NOT NULL, -- How long the ride runs for		
	);
--
CREATE TABLE DimTicket
	(Ticket_SK			INT IDENTITY (1,1) NOT NULL CONSTRAINT pk_ticket_sk PRIMARY KEY,
	Ticket_AK			INT NOT NULL,
	Ticket_Type			NVARCHAR(30) CONSTRAINT nn_ticket_type NOT NULL, --Type of ticket the customers purchased (one-day, two-day,etc.)
	Fast_Pass 			NCHAR(1) CONSTRAINT ck_fast_pass CHECK ((Fast_Pass = 'Y') OR (Fast_Pass = 'N')) NOT NULL, --Ticket that allows customers to skip ride lines
	[Date]				DATE  CONSTRAINT nn_date NOT NULL
	);
--
CREATE TABLE DimCustomer
	(Customer_SK		INT IDENTITY (1,1) NOT NULL CONSTRAINT pk_customer_sk PRIMARY KEY,
	Customer_AK			INT NOT NULL,
	Height				NVARCHAR (10) CONSTRAINT nn_height NOT NULL,
	Email				NVARCHAR(50) CONSTRAINT nn_email NOT NULL,
	Gender 				NCHAR(1) CONSTRAINT ck_gender CHECK ((Gender = 'M') OR (Gender = 'F') OR (Gender = 'N')) NOT NULL,
	[State] 			NVARCHAR(2) CONSTRAINT nn_consumer_state NOT NULL,
	Birth_Date			DATE CONSTRAINT nn_birth_date NOT NULL,
	Customer_Type		NVARCHAR(10) CONSTRAINT nn_customer_type NOT NULL, --Senior, Adult, Kid
	);
--
CREATE TABLE FactSales
	(Date_SK				INT NOT NULL,
	Ride_SK				INT NOT NULL,
	Ticket_SK			INT NOT NULL,
	Customer_SK			INT NOT NULL,
	Price				MONEY,
	Quantity			INT,
	CONSTRAINT pk_fact_sales PRIMARY KEY (Date_SK,Ride_SK,Ticket_SK, Customer_SK),
	CONSTRAINT fk_dim_date FOREIGN KEY (Date_SK) REFERENCES DimDate(Date_SK),
	CONSTRAINT fk_dim_ride FOREIGN KEY (Ride_SK) REFERENCES DimRide(Ride_SK),
	CONSTRAINT fk_dim_ticket FOREIGN KEY (Ticket_SK) REFERENCES DimTicket(Ticket_SK),
	CONSTRAINT fk_dim_customer FOREIGN KEY (Customer_SK) REFERENCES DimCustomer(Customer_SK)
	);
  
  
  Entire project of data mart/ETL/powerpivot:
  [INFO 3300 DisneyResorts.zip](https://github.com/avafaith/Projects/files/10053585/INFO.3300.DisneyResorts.zip)
