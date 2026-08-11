# Airline Management System
A comprehensive database management system for airline operations built with Python and MySQL. This project implements a full-featured airline management system with capabilities for managing employees, airports, flights, passengers, baggage, and airline operations.

##  Table of Contents

- [Features](#features)
- [Database Schema](#database-schema)
- [Project Structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Usage](#usage)
- [Functionality](#functionality)
- [Database Operations](#database-operations)
- [Technologies Used](#technologies-used)

##  Features

### Employee Management
- Hire new employees (pilots, cabin crew, ground staff, managers, technicians)
- Fire employees with cascade handling
- Promote employees with salary updates
- View employee details including phone numbers
- Track employee hierarchies with manager relationships
- Calculate average salaries by airport

### Airport Operations
- Add new airports with city and country information
- Delete airports with proper constraint handling
- View all airlines operating at a specific airport
- Get flight statistics by airline at each airport
- Track employees working at specific airports

### Passenger Services
- Manage passenger records with flight assignments
- First-class and business-class passenger categories
- Calculate total baggage weight per passenger on flights
- Search passengers by name with partial matching
- Track passenger phone numbers

### Flight Management
- Create and manage flight records
- Track source and destination airports
- Assign pilots and airplanes to flights
- Record arrival and departure times
- Link flights to operating airlines

### Baggage Handling
- Register baggage items with weight tracking
- Associate baggage with passengers
- Delete baggage records with proper validation

### Airline & Airport Relations
- Manage airline registrations
- Track which airlines operate at which airports
- Delete airlines with cascade effects
- View comprehensive airline statistics

## 🗄️ Database Schema

The system uses a normalized relational database with the following main entities:

### Core Tables
- **Airport**: Airport details (name, city, country)
- **Airline**: Airline information (ID, name)
- **Flight_Employee**: Employee records with job types and salary information
- **Airplane**: Aircraft details (ID, model, capacity, flying hours)
- **Flight**: Flight records linking airports, airplanes, and pilots
- **Passenger**: Passenger information and flight assignments

### Relationship Tables
- **Operates**: Many-to-many relationship between airports and airlines
- **Employee_Phone**: Employee contact information
- **Passenger_Phone**: Passenger contact information
- **Baggage**: Baggage items linked to passengers

### Specialized Tables
- **First_Class_Passenger**: Premium passenger amenities
- **First_Class_Services**: Services available to first-class passengers
- **Business_Class_Passenger**: Business class passenger perks
- **Business_Class_Services**: Business class service offerings
- **Dependents**: Employee dependent information

## Project Structure

```
Airline/
├── main.py                 # Main application entry point with menu system
├── db_connection.py        # Database connection handler using PyMySQL
├── employee.py            # Employee management operations
├── airport.py             # Airport operations and queries
├── airline.py             # Airline management functions
├── passenger.py           # Passenger operations and searches
├── flight.py              # Flight management functions
├── airplane.py            # Airplane operations
├── baggage.py             # Baggage management
├── operates.py            # Airport-Airline relationship management
├── airline.sql            # Database schema and sample data
├── ER_Diagram.drawio      # Entity-Relationship diagram
└── relational_model.drawio # Relational schema diagram
```

##  Prerequisites

- Python 3.x
- MySQL Server (5.7 or higher)
- PyMySQL library

##  Installation

1. **Clone or download the project**
   ```bash
   cd /path/to/Airline
   ```

2. **Install required Python packages**
   ```bash
   pip install pymysql
   ```

3. **Set up the MySQL database**
   ```bash
   mysql -u root -p < airline.sql
   ```
   This will create the `AIRLINE` database and populate it with sample data.

4. **Configure database credentials**
   - The application prompts for MySQL username and password at runtime
   - Ensure your MySQL user has privileges on the `AIRLINE` database

## Usage

Run the application:
```bash
python main.py
```

### Login
- Enter your MySQL username
- Enter your MySQL password
- Upon successful connection, you'll see the main menu

### Main Menu Options

```
1.  Hire an Employee
2.  Fire an Employee
3.  Promote Employee
4.  Add Airport
5.  Delete Airport
6.  Get details of all employees working at a specific airport
7.  Get airlines operating at a specific airport
8.  Get average salary of all employees at a specific airport
9.  Get number of flights operated by each airline at a specific airport
10. Get total baggage weight for each passenger on a specific flight
11. Search Passenger by Name
12. Delete Airline
13. Delete Baggage
14. Logout
```

## 🔍 Functionality

### Employee Operations

**Hire Employee** (`hireAnEmployee`)
- Collects employee details: ID, name, job type, salary, birthdate
- Assigns employee to an airline
- Optional manager assignment
- Validates data and handles constraints

**Fire Employee** (`fireAnEmployee`)
- Deletes employee records by employee ID
- Handles foreign key constraints
- Rollback on failure

**Promote Employee** (`promoteEmployee`)
- Updates employee salary
- Validates employee existence
- Commits changes to database

### Airport Operations

**Add Airport** (`addAirport`)
- Creates new airport records
- Requires airport name, city, and country
- Primary key validation

**Delete Airport** (`deleteAirport`)
- Displays existing airports for selection
- Validates airport existence before deletion
- Handles cascade deletions

**Get Employees by Airport** (`getEmployeesByAirport`)
- Joins multiple tables (Flight_Employee, Airline, Operates)
- Displays all employees working at the selected airport
- Shows employee details and phone numbers

**Get Airlines by Airport** (`getAirlinesByAirport`)
- Lists all airlines operating at a specific airport
- Uses the Operates relationship table
- Displays airline ID and name

**Get Average Salary by Airport** (`getAverageSalaryByAirport`)
- Calculates average employee salary per airport
- Aggregates data across airline relationships
- Formatted output with two decimal places

**Get Flights by Airline at Airport** (`getFlightsByAirlineAtAirport`)
- Complex query joining Flight, Flight_Employee, Airline, and Operates tables
- Counts flights per airline at a specific airport
- Groups results by airline name

### Passenger Operations

**Search Passenger by Name** (`searchPassengerByName`)
- Partial name matching using SQL LIKE operator
- Searches across first, middle, and last names
- Displays passenger ID, full name, birthdate, and flight

**Get Total Baggage Weight** (`getTotalBaggageWeightByPassenger`)
- Calculates total baggage weight per passenger on a specific flight
- Joins Passenger and Baggage tables
- Groups by passenger and sums weights

### Airline & Baggage Operations

**Delete Airline** (`deleteAirline`)
- Displays all existing airlines
- Validates airline existence
- Handles cascade deletions for related records

**Delete Baggage** (`deleteBaggage`)
- Shows all baggage records
- Requires both baggage ID and passenger ID
- Validates ownership before deletion

## Database Operations

### Key SQL Features Used

1. **Complex Joins**: Multi-table joins for comprehensive data retrieval
2. **Aggregate Functions**: COUNT(), SUM(), AVG() for statistics
3. **GROUP BY**: Grouping results for analytical queries
4. **Foreign Keys**: Maintaining referential integrity
5. **Cascade Operations**: Automatic handling of related record deletions
6. **Parameterized Queries**: SQL injection prevention
7. **Transaction Management**: COMMIT and ROLLBACK for data consistency

### Sample Complex Query
```sql
SELECT Airline.Airline_name, COUNT(Flight.Flight_code) AS Flight_Count
FROM Flight
JOIN Flight_Employee ON Flight.Pilot = Flight_Employee.Emp_id
JOIN Airline ON Flight_Employee.Works_For = Airline.Airline_ID
JOIN Operates ON Airline.Airline_ID = Operates.AirlineID
WHERE Operates.AirportID = %s
GROUP BY Airline.Airline_name;
```

##  Technologies Used

- **Python 3.x**: Core programming language
- **PyMySQL**: MySQL database connector for Python
- **MySQL**: Relational database management system
- **SQL**: Data definition and manipulation
- **Draw.io**: ER diagram and relational model visualization

##  Security Features

- Parameterized queries to prevent SQL injection
- Password masking during login (using `getpass`)
- Transaction rollback on errors
- Input validation and error handling

##  Sample Data

The system comes pre-populated with:
- 12 airports across major cities worldwide
- 10 airlines
- 19 employees (pilots, cabin crew, ground staff, managers, technicians)
- 10 airplanes with various models and capacities
- 22 flights with schedules
- 22 passengers with various classes
- 10 baggage items
- 10 operates relationships

## Key Design Decisions

1. **Modular Architecture**: Separate files for each entity type for maintainability
2. **User-Friendly Interface**: Interactive menus with clear prompts
3. **Data Validation**: Checks for existence before operations
4. **Error Handling**: Try-catch blocks with rollback mechanisms
5. **Normalized Database**: Follows 3NF to minimize redundancy
6. **Flexible Queries**: Uses parameterized queries for security and flexibility

##  Error Handling

The system includes comprehensive error handling:
- Database connection failures
- Invalid input data
- Foreign key constraint violations
- Transaction failures with automatic rollback
- User-friendly error messages

##  Future Enhancements

Potential improvements for the system:
- Web-based interface
- Real-time flight tracking
- Booking system integration
- Reporting and analytics dashboard
- Multi-user authentication with roles
- Flight scheduling optimization
- Revenue management system

## Author

Ved Maurya  
Semester 3 Course Project  


