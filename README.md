![banner](Assets/Banner.jpg)

# Airline-Data-Analysis-SQL-Project
***A company operating a diverse aircraft fleet faces profitability challenges due to environmental regulations, rising costs, and a tight labor market. They aim to analyze their database to increase occupancy rates and enhance seat profitability.***

## Author
- [@saadharoon27](https://github.com/saadharoon27)

## Table of Contents
- [Project Overview](#project-overview)
- [Project Focus](#project-focus)
- [Approach](#approach)
- [About The Dataset](#about-the-dataset)
- [Code](#code)

## Project Overview
**A company** operates a diverse fleet of aircraft ranging from small business jets to medium–sized machines. They have been providing high-quality air transportation services to their clients for several years, and their primary focus is to ensure a safe, comfortable, convenient journey for our passengers. However, they are currently facing challenges due to several factors such as **stricter environmental regulations, higher flight taxes, increased interest rates, rising fuel prices**, and a tight labour market resulting in increased labour costs. As a result, the company’s **profitability** is under pressure, and they are seeking ways to address this issue. To tackle this challenge, they are looking to conduct an analysis of their database to find ways to **increase their occupancy rate**, which can help boost the **average profit earned per seat**.<br>
<br>
**Challenges**<br>
- **Stricter environmental regulations**: The demand on the airline industry to decrease its carbon footprint is growing, which has resulted in more stringent environmental laws that raise operating costs and restrict expansion potential.
- **Higher flight taxes**: To solve environmental issues and increase money, governments all around the world are taxiing aircraft more heavily, which raises the cost of flying and decreases demand.
- **Tight labour market resulting in increased labour costs**: The lack of trained people in the aviation sector has increased labour costs and increased turnover rates.

## Project Focus
- **Increase occupancy rate**: By increasing the _occupancy rate_, we can boost the _average profit_ earned per seat and mitigate the impact of the challenges we're facing.
- **Improve pricing strategy**: We need to develop a _pricing strategy_ that takes into account the changing market conditions and _customer preferences_ to attract and retain customers.

## Approach
***This project is done using a combination of Python and SQL***

## About The Dataset
[**Airlines Dataset**](https://www.kaggle.com/datasets/saadharoon27/airlines-dataset)<br>

**Tables:** <br>

**Table: aircrafts_data**
  | **Column Name**     | **Description**                         |
  |-----------------    |-------------------------------------|
  | **aircraft_code**   | Code for the aircraft (3 characters) |
  | **model**           | Aircraft model in JSON format       |
  | **range**           | The range of the aircraft in integer format |

**Table: airports_data**
  | **Column Name**     | **Description**                        |
  |-----------------    |------------------------------------|
  | **airport_code**    | Code for the airport (3 characters) |
  | **airport_name**    | Name of the airport in JSON format  |
  | **city**            | City where the airport is located in JSON format |
  | **coordinates**     | Geographic coordinates of the airport (point) |
  | **timezone**        | Timezone of the airport (text)     |

**Table: boarding_passes**
| **Column Name**     | **Description**                             |
|-----------------    |-----------------------------------------|
| **ticket_no**       | Ticket number (13 characters)            |
| **flight_id**       | ID of the flight (integer)              |
| **boarding_no**     | Boarding number (integer)               |
| **seat_no**         | Seat number (character varying, 4 characters) |

**Table: bookings**
| **Column Name**     | **Description**                         |
|-----------------    |-------------------------------------|
| **book_ref**        | Booking reference (6 characters)    |
| **book_date**       | Booking date with timestamp and time zone |
| **total_amount**    | Total booking amount in numeric format (10,2) |

**Table: flights**
| **Column Name**     | **Description**                              |
|-----------------    |------------------------------------------|
| **flight_id**       | Flight ID (integer)                      |
| **flight_no**       | Flight number (6 characters)             |
| **scheduled_departure** | Scheduled departure time with timestamp and time zone |
| **scheduled_arrival**   | Scheduled arrival time with timestamp and time zone |
| **departure_airport**   | Departure airport code (3 characters)     |
| **arrival_airport**     | Arrival airport code (3 characters)       |
| **status**             | Flight status (character varying, 20 characters) |
| **aircraft_code**      | Aircraft code (3 characters)              |
| **actual_departure**   | Actual departure time with timestamp and time zone |
| **actual_arrival**     | Actual arrival time with timestamp and time zone |

**Table: seats**
| **Column Name**     | **Description**                         |
|-----------------    |-------------------------------------|
| **aircraft_code**   | Aircraft code (3 characters)        |
| **seat_no**         | Seat number (character varying, 4 characters) |
| **fare_conditions** | Fare conditions (character varying, 10 characters) |

**Table: ticket_flights**
| **Column Name**     | **Description**                         |
|-----------------    |-------------------------------------|
| **ticket_no**       | Ticket number (13 characters)       |
| **flight_id**       | ID of the flight (integer)          |
| **fare_conditions** | Fare conditions (character varying, 10 characters) |
| **amount**          | Ticket amount in numeric format (10,2) |

**Table: tickets**
| **Column Name**     | **Description**                         |
|-----------------    |-------------------------------------|
| **ticket_no**       | Ticket number (13 characters)       |
| **book_ref**        | Booking reference (6 characters)    |
| **passenger_id**    | Passenger ID (character varying, 20 characters) |

## Code
***Import necessary libraries***
```python
# Import necessary libraries
import sqlite3          # Library for working with SQLite databases
import pandas as pd     # Data manipulation and analysis library
import matplotlib.pyplot as plt  # Data visualization library
import warnings         # To suppress warnings
import seaborn as sns    # Data visualization library with a high-level interface

# Suppress warnings for cleaner output
warnings.filterwarnings('ignore')
```

***Connecting Database***
```python
# Establish a connection to the 'travel.sqlite' SQLite database
import sqlite3
connection = sqlite3.connect('travel.sqlite')

# Create a connection to interact with the database
cursor = connection.cursor()
```

```python
# Execute SQL query to retrieve table names
cursor.execute("""select name from sqlite_master where type = 'table';""")
print('List of tables present in the database')

# Fetch and store table names in a list
table_list = [table[0] for table in cursor.fetchall()]

# Display the list of table names
table_list
```
_**Output:**_
```
List of tables present in the database
['aircrafts_data', 'airports_data', 'boarding_passes', 'bookings', 'flights', 'seats', 'ticket_flights', 'tickets']
```

_**Data Tables Exploration**_

```python
# Execute SQL query to retrieve all data from the 'aircrafts_data' table
aircrafts_data = pd.read_sql_query('SELECT * FROM aircrafts_data', connection)

# Display the contents of the 'aircrafts_data' table
aircrafts_data
```
```
  aircraft_code                                            model  range
0          773  {"en": "Boeing 777-300", "ru": "Боинг 777-300"}  11100
1          763  {"en": "Boeing 767-300", "ru": "Боинг 767-300"}   7900
2          SU9  {"en": "Sukhoi Superjet-100", "ru": "Сухой Суп...   3000
3          320  {"en": "Airbus A320-200", "ru": "Аэробус A320-...   5700
4          321  {"en": "Airbus A321-200", "ru": "Аэробус A321-...   5600
5          319  {"en": "Airbus A319-100", "ru": "Аэробус A319-...   6700
6          733  {"en": "Boeing 737-300", "ru": "Боинг 737-300"}   4200
7          CN1  {"en": "Cessna 208 Caravan", "ru": "Сессна 208...   1200
8          CR2  {"en": "Bombardier CRJ-200", "ru": "Бомбардье ...   2700
```

```python
# Execute SQL query to retrieve all data from the 'airports_data' table
airports_data = pd.read_sql_query('SELECT * FROM airports_data', connection)

# Display the contents of the 'airports_data' table
airports_data
```
```
   airport_code                              airport_name  \
0          YKS                        {"en": "Yakutsk Airport", "ru": "Якутск"}   
1          MJZ                        {"en": "Mirny Airport", "ru": "Мирный"}   
2          KHV        {"en": "Khabarovsk-Novy Airport", "ru": "Хабаровск"}   
3          PKC                  {"en": "Yelizovo Airport", "ru": "Елизово"}   
4          UUS  {"en": "Yuzhno-Sakhalinsk Airport", "ru": "Хом...   
..          ...                                      ...   
99         MMK                    {"en": "Murmansk Airport", "ru": "Мурманск"}   
100        ABA                      {"en": "Abakan Airport", "ru": "Абакан"}   
101        BAX                    {"en": "Barnaul Airport", "ru": "Барнаул"}   
102        AAQ        {"en": "Anapa Vityazevo Airport", "ru": "Витяз...   
103        CNN                      {"en": "Chulman Airport", "ru": "Чульман"}   

                                        city                            coordinates        timezone  
0                      {"en": "Yakutsk", "ru": "Якутск"}  (129.77099609375,62.0932998657226562)      Asia/Yakutsk  
1                    {"en": "Mirnyj", "ru": "Мирный"}   (114.03900146484375,62.534698486328125)      Asia/Yakutsk  
2                {"en": "Khabarovsk", "ru": "Хабаровск"}  (135.18800354004,48.5279998779300001)  Asia/Vladivostok  
3    {"en": "Petropavlovsk", "ru": "Петропавловск-К...  (158.453994750976562,53.1679000854492188)  Asia/Kamchatka  
4    {"en": "Yuzhno-Sakhalinsk", "ru": "Южно-Сахали...  (142.718002319335938,46.8886985778808594)  Asia/Sakhalin  
..                                           ...                                   ...               ...  
99                   {"en": "Murmansk", "ru": "Мурманск"}  (32.7508010864257812,68.7817001342773438)    Europe/Moscow  
100                     {"en": "Abakan", "ru": "Абакан"}  (91.3850021362304688,53.7400016784667969)  Asia/Krasnoyarsk  
101                    {"en": "Barnaul", "ru": "Барнаул"}  (83.5384979248046875,53.363800048828125)  Asia/Krasnoyarsk  
102                      {"en": "Anapa", "ru": "Анапа"}  (37.3473014831539984,45.002101898192997)    Europe/Moscow  
103                {"en": "Neryungri", "ru": "Нерюнгри"}  (124.914001464839998,56.9138984680179973)      Asia/Yakutsk  
104 rows × 5 columns
```

```python
# Execute SQL query to retrieve all data from the 'boarding_passes' table
boarding_passes = pd.read_sql_query('SELECT * FROM boarding_passes', connection)

# Display the contents of the 'boarding_passes' table
boarding_passes
```
```
       ticket_no  flight_id  boarding_no seat_no
0  0005435212351      30625            1      2D
1  0005435212386      30625            2      3G
2  0005435212381      30625            3      4H
3  0005432211370      30625            4      5D
4  0005435212357      30625            5     11A
...
579681  0005434302871      19945           85     20F
579682  0005432892791      19945           86     21C
579683  0005434302869      19945           87     20E
579684  0005432802476      19945           88     21F
579685  0005432802482      19945           89     21E
579686 rows × 4 columns
```
```python
# Execute SQL query to retrieve all data from the 'bookings' table
bookings = pd.read_sql_query('SELECT * FROM bookings', connection)

# Display the contents of the 'bookings' table
bookings
```
```
     book_ref                 book_date  total_amount
0     00000F  2017-07-05 03:12:00+03        265700
1     000012  2017-07-14 09:02:00+03         37900
2     000068  2017-08-15 14:27:00+03         18100
3     000181  2017-08-10 13:28:00+03        131800
4     0002D8  2017-08-07 21:40:00+03         23600
...
262783  FFFEF3  2017-07-17 07:23:00+03         56000
262784  FFFF2C  2017-08-08 05:55:00+03         10800
262785  FFFF43  2017-07-20 20:42:00+03         78500
262786  FFFFA8  2017-08-08 04:45:00+03         28800
262787  FFFFF7  2017-07-01 22:12:00+03         73600
262788 rows × 3 columns
```

```python
# Execute SQL query to retrieve all data from the 'flights' table
flights = pd.read_sql_query('SELECT * FROM flights', connection)

# Display the contents of the 'flights' table
flights
```

```
    flight_id flight_no     scheduled_departure       scheduled_arrival departure_airport arrival_airport      status aircraft_code      actual_departure        actual_arrival
0      1185     PG0134  2017-09-10 09:50:00+03  2017-09-10 14:55:00+03              DME            BTK  Scheduled           319                    \N                      \N
1      3979     PG0052  2017-08-25 14:50:00+03  2017-08-25 17:35:00+03              VKO            HMA  Scheduled           CR2                    \N                      \N
2      4739     PG0561  2017-09-05 12:30:00+03  2017-09-05 14:15:00+03              VKO            AER  Scheduled           763                    \N                      \N
3      5502     PG0529  2017-09-12 09:50:00+03  2017-09-12 11:20:00+03              SVO            UFA  Scheduled           763                    \N                      \N
4      6938     PG0461  2017-09-04 12:25:00+03  2017-09-04 13:20:00+03              SVO            ULV  Scheduled           SU9                    \N                      \N
...
33116  33117     PG0063  2017-08-02 19:25:00+03  2017-08-02 20:10:00+03              SKX            SVO  Arrived           CR2  2017-08-02 19:25:00+03  2017-08-02 20:10:00+03
33117  33118     PG0063  2017-07-28 19:25:00+03  2017-07-28 20:10:00+03              SKX            SVO  Arrived           CR2  2017-07-28 19:30:00+03  2017-07-28 20:15:00+03
33118  33119     PG0063  2017-09-08 19:25:00+03  2017-09-08 20:10:00+03              SKX            SVO  Scheduled           CR2                    \N                      \N
33119  33120     PG0063  2017-08-01 19:25:00+03  2017-08-01 20:10:00+03              SKX            SVO  Arrived           CR2  2017-08-01 19:26:00+03  2017-08-01 20:12:00+03
33120  33121     PG0063  2017-08-26 19:25:00+03  2017-08-26 20:10:00+03              SKX            SVO  Scheduled           CR2                    \N                      \N
33121 rows × 10 columns
```

```python
# Execute SQL query to retrieve all data from the 'seats' table
seats = pd.read_sql_query('SELECT * FROM seats', connection)

# Display the contents of the 'seats' table
seats
```

```
    aircraft_code seat_no fare_conditions
0            319     2A         Business
1            319     2C         Business
2            319     2D         Business
3            319     2F         Business
4            319     3A         Business
...
1334         773     48H        Economy
1335         773     48K        Economy
1336         773     49A        Economy
1337         773     49C        Economy
1338         773     49D        Economy
1339 rows × 3 columns
```

```python
# Execute SQL query to retrieve all data from the 'ticket_flights' table
ticket_flights = pd.read_sql_query('SELECT * FROM ticket_flights', connection)

# Display the contents of the 'ticket_flights' table
ticket_flights
```

```
         ticket_no  flight_id fare_conditions   amount
0    0005432159776      30625        Business  42100.0
1    0005435212351      30625        Business  42100.0
2    0005435212386      30625        Business  42100.0
3    0005435212381      30625        Business  42100.0
4    0005432211370      30625        Business  42100.0
...
1045721  0005435097522      32094        Economy   5200.0
1045722  0005435097521      32094        Economy   5200.0
1045723  0005435104384      32094        Economy   5200.0
1045724  0005435104352      32094        Economy   5200.0
1045725  0005435104389      32094        Economy   5200.0
1045726 rows × 4 columns
```
```python
# Execute SQL query to retrieve all data from the 'tickets' table
tickets = pd.read_sql_query('SELECT * FROM tickets', connection)

# Display the contents of the 'tickets' table
tickets
```
```
         ticket_no book_ref    passenger_id
0     0005432000987    06B046     8149 604011
1     0005432000988    06B046     8499 420203
2     0005432000989    E170C3     1011 752484
3     0005432000990    E170C3     4849 400049
4     0005432000991    F313DD     6615 976589
...
366728  0005435999869    D730BA     0474 690760
366729  0005435999870    D730BA     6535 751108
366730  0005435999871    A1AD46     1596 156448
366731  0005435999872    7B6A53     9374 822707
366732  0005435999873    7B6A53     7380 075822
366733 rows × 3 columns
```

**_Checking Datatypes Of Every Column Of Every Table_**
```python
# Loop through the list of table names
for table in table_list:
    print('\nTable:', table)
    
    # Execute a PRAGMA query to extract table information
    column_info = connection.execute('PRAGMA table_info({})'.format(table))
    
    # Loop through the columns and print their names and data types
    for column in column_info.fetchall():
        print(column[1:3])
```
***Output***
```
table: aircrafts_data
('aircraft_code', 'character(3)')
('model', 'jsonb')
('range', 'integer')

table: airports_data
('airport_code', 'character(3)')
('airport_name', 'jsonb')
('city', 'jsonb')
('coordinates', 'point')
('timezone', 'text')

table: boarding_passes
('ticket_no', 'character(13)')
('flight_id', 'integer')
('boarding_no', 'integer')
('seat_no', 'character varying(4)')

table: bookings
('book_ref', 'character(6)')
('book_date', 'timestamp with time zone')
('total_amount', 'numeric(10,2)')

table: flights
('flight_id', 'integer')
('flight_no', 'character(6)')
('scheduled_departure', 'timestamp with time zone')
('scheduled_arrival', 'timestamp with time zone')
('departure_airport', 'character(3)')
('arrival_airport', 'character(3)')
('status', 'character varying(20)')
('aircraft_code', 'character(3)')
('actual_departure', 'timestamp with time zone')
('actual_arrival', 'timestamp with time zone')

table: seats
('aircraft_code', 'character(3)')
('seat_no', 'character varying(4)')
('fare_conditions', 'character varying(10)')

table: ticket_flights
('ticket_no', 'character(13)')
('flight_id', 'integer')
('fare_conditions', 'character varying(10)')
('amount', 'numeric(10,2)')

table: tickets
('ticket_no', 'character(13)')
('book_ref', 'character(6)')
('passenger_id', 'character varying(20)')
```
_**Checking Missing Values For Every Table**_
```python
# Loop through the list of table names
for table in table_list:
    print('\nTable:', table)
    
    # Retrieve the table data and store it in a DataFrame
    df_table = pd.read_sql_query(f"SELECT * FROM {table}", connection)
    
    # Calculate and print the number of missing values for each column in the table
    print(df_table.isnull().sum())
```
***Output***
```
table: aircrafts_data
aircraft_code    0
model            0
range            0
dtype: int64

table: airports_data
airport_code    0
airport_name    0
city            0
coordinates     0
timezone        0
dtype: int64

table: boarding_passes
ticket_no      0
flight_id      0
boarding_no    0
seat_no        0
dtype: int64

table: bookings
book_ref        0
book_date       0
total_amount    0
dtype: int64

table: flights
flight_id              0
flight_no              0
scheduled_departure    0
scheduled_arrival      0
departure_airport      0
arrival_airport        0
status                 0
aircraft_code          0
actual_departure       0
actual_arrival         0
dtype: int64

table: seats
aircraft_code      0
seat_no            0
fare_conditions    0
dtype: int64

table: ticket_flights
ticket_no          0
flight_id          0
fare_conditions    0
amount             0
dtype: int64

table: tickets
ticket_no       0
book_ref        0
passenger_id    0
dtype: int64
```

***Basic Analysis***
- How many planes have more than _**100 seats**_?
  ```python
  # Query to count the number of seats for each aircraft with more than 100 seats
  pd.read_sql_query("""
    SELECT aircraft_code, COUNT(*) AS num_seats
    FROM seats
    GROUP BY aircraft_code
    HAVING num_seats > 100
    """, connection)
  ```
  ***Output***
  ```
          aircraft_code  num_seats
  0           319          116
  1           320          140
  2           321          170
  3           733          130
  4           763          222
  5           773          402
  ```
- How the many _**number of tickets booked**_ and _**total amount**_ earned changed with the time.
  ```python
  # Execute an SQL query to retrieve data from 'tickets' table and join it with 'bookings' table
  tickets_query = """
  SELECT *
  FROM tickets
  INNER JOIN bookings ON tickets.book_ref = bookings.book_ref
  """
  
  # Use pandas to execute the SQL query and retrieve the results
  tickets = pd.read_sql_query(tickets_query, connection)
  
  # Convert 'book_date' column to a datetime object
  tickets['book_date'] = pd.to_datetime(tickets['book_date'])
  
  # Extract the date part from 'book_date' and create a new 'date' column
  tickets['date'] = tickets['book_date'].dt.date
  
  # Group the data by 'date' and count the number of occurrences
  line = tickets.groupby('date')[['date']].count()
  ```
- Plotting A _**Line Graph To Visualize The Trend Of Number Of Booking**_
  ```python
  # Create a figure for the plot with specified dimensions
  plt.figure(figsize=(18, 6))
  
  # Create a line plot using the date index and 'date' column data
  # Add markers on data points in the shape of '^' for visibility
  plt.plot(line.index, line['date'], marker='^')
  
  # Set the x-axis label
  plt.xlabel('Date', fontsize=20)
  
  # Set the y-axis label
  plt.ylabel('Tickets', fontsize=20)
  
  # Add a grid to the plot with gridlines in blue color
  plt.grid('b')
  
  # Display the plot
  plt.show()
  ```
  ***Output***
  ![image](https://github.com/saadharoon27/Airline-Data-Analysis-SQL-Project/assets/147087623/adecd6b4-1f0c-4a12-9029-aea86e157acf)

 - Plotting **A Line Graph To Visualize The Trend Of Total Amount _(Revenue)_**
   ```python
   # Read data from the 'bookings' table in the database
   bookings = pd.read_sql_query("""SELECT * FROM bookings""", connection)
    
   # Convert the 'book_date' column to a datetime format
   bookings['book_date'] = pd.to_datetime(bookings['book_date'])
    
   # Extract the date portion of the 'book_date' and create a new 'date' column
   bookings['date'] = bookings['book_date'].dt.date
    
   # Group the data by 'date' and calculate the sum of 'total_amount' for each date
   line2 = bookings.groupby('date')[['total_amount']].sum()
    
   # Create a figure for the plot with specified dimensions
   plt.figure(figsize=(18, 6))
    
   # Create a line plot using the date index and 'total_amount' column data
   # Add markers on data points in the shape of '^' for visibility
   plt.plot(line2.index, line2['total_amount'], marker='^')
    
   # Set the x-axis label
   plt.xlabel('Date', fontsize=20)
    
   # Set the y-axis label
   plt.ylabel('Total Amount Earned', fontsize=20)
    
   # Add a grid to the plot with gridlines in cyan color
   plt.grid('c')
    
   # Display the plot
   plt.show()
   ```
   ***Output***
  ![image](https://github.com/saadharoon27/Airline-Data-Analysis-SQL-Project/assets/147087623/d06975cd-a887-4df1-aa0e-feb0ad27b3af)

  - Calculate the average charges for each aircraft with different fare conditions
