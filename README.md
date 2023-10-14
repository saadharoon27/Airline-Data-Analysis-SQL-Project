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
- [Analysis](#analysis)
- [Conclusion](#conclusion)

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
  ![image](https://github.com/saadharoon27/Airline-Data-Analysis-SQL-Project/assets/147087623/ef850b11-6e8d-4c1f-93df-23ad77c119b8)


  - Calculating the **average charges for each aircraft with different fare conditions**
    ```python
    # Query the database to calculate the average charges for each aircraft with different fare conditions
    df = pd.read_sql_query("""
        SELECT fare_conditions, aircraft_code, AVG(amount) 
        FROM ticket_flights
        JOIN flights ON ticket_flights.flight_id = flights.flight_id
        GROUP BY aircraft_code, fare_conditions
        """, connection)
    
    # Display the result
    df
    ```
    _**Output**_
    ```
           fare_conditions   aircraft_code    avg(amount)
    0         Business           319         113550.557703
    1          Economy           319         38311.402347
    2         Business           321         34435.662664
    3          Economy           321         11534.974764
    4         Business           733         41865.626175
    5          Economy           733         13985.152000
    6         Business           763         82839.842866
    7          Economy           763         27594.721829
    8         Business           773         57779.909435
    9          Comfort           773         32740.552889
    10         Economy           773         19265.225693
    11         Economy           CN1          6568.552345
    12         Economy           CR2         13207.661102
    13        Business           SU9         33487.849829
    14         Economy           SU9         11220.183400
    ```

  - ***Visualising The Extracted Data***
    ```python
    sns.barplot(data = df, x = 'aircraft_code', y = 'avg(amount)', hue = 'fare_conditions')
    ```
    ![image](https://github.com/saadharoon27/Airline-Data-Analysis-SQL-Project/assets/147087623/6575ffd4-b528-4e4f-878b-e76164191e65)

_**Analysing Occupancy Rate**_
- For each aircraft, _**calculating the total revenue per year**_ and _**the average revenue per ticket**_
  ```python
  # Query the database to calculate the total revenue per year and the average revenue per ticket for each aircraft
  pd.read_sql_query("""
      SELECT aircraft_code, ticket_count, total_revenue, total_revenue/ticket_count AS avg_revenue_per_ticket 
      FROM (
          SELECT aircraft_code, COUNT(*) AS ticket_count, SUM(amount) AS total_revenue 
          FROM ticket_flights
          JOIN flights ON ticket_flights.flight_id = flights.flight_id 
          GROUP BY aircraft_code
      )
      """, connection)
    ```
   ***Output***
  ```
    aircraft_code  ticket_count  total_revenue  avg_revenue_per_ticket
  0           319         52853     2706163100                   51201
  1           321        107129     1638164100                   15291
  2           733         86102     1426552100                   16568
  3           763        124774     4371277100                   35033
  4           773        144376     3431205500                   23765
  5           CN1         14672       96373800                    6568
  6           CR2        150122     1982760500                   13207
  7           SU9        365698     5114484700                   13985
  ```
- **Calculating The Average Occupancy Per Aircraft**
  ```python
    # Query to calculate the occupancy rate for each aircraft
  occupancy_rate = pd.read_sql_query("""
      SELECT a.aircraft_code, AVG(a.seats_count) AS booked_seats, b.num_seats, 
             AVG(a.seats_count)/b.num_seats AS occupancy_rate
      FROM (
          SELECT aircraft_code, flights.flight_id, COUNT(*) AS seats_count
          FROM boarding_passes
          INNER JOIN flights
          ON boarding_passes.flight_id = flights.flight_id
          GROUP BY aircraft_code, flights.flight_id
      ) AS a
      INNER JOIN (
          SELECT aircraft_code, COUNT(*) as num_seats
          FROM seats
          GROUP BY aircraft_code
      ) AS b
      ON a.aircraft_code = b.aircraft_code
      GROUP BY a.aircraft_code
      """, connection)
  
  # Output: DataFrame displaying the aircraft_code, booked_seats, num_seats, and occupancy_rate
  occupancy_rate
  ```
   ***Output***
  ```
      aircraft_code  booked_seats  num_seats  occupancy_rate
  0           319     53.583181        116        0.461924
  1           321     88.809231        170        0.522407
  2           733     80.255462        130        0.617350
  3           763    113.937294        222        0.513231
  4           773    264.925806        402        0.659019
  5           CN1      6.004431         12        0.500369
  6           CR2     21.482847         50        0.429657
  7           SU9     56.812113         97        0.585692
  ```
- Calculating By How Much **The Total Annual Turnover** Could Increase By Giving All Aircraft A **_10%_ Higher Occupancy Rate**.
  ```python
    # Adding a column to occupancy_rate DataFrame with an increased occupancy rate
    occupancy_rate['inc occupancy rate'] = occupancy_rate['occupancy_rate'] + occupancy_rate['occupancy_rate'] * 0.1
  
    # Setting the float format to a string (disabling scientific notation)
    pd.set_option("display.float_format", str)
    ```
    ```python
    total_revenue = pd.read_sql_query("""SELECT aircraft_code, SUM(amount) AS total_revenue FROM ticket_flights
                          JOIN flights ON ticket_flights.flight_id = flights.flight_id
                          GROUP BY aircraft_code""", connection)
  total_revenue
  
  occupancy_rate['inc Total Annual Turnover'] = (total_revenue['total_revenue']/occupancy_rate['occupancy_rate'])*occupancy_rate['inc occupancy rate']
  occupancy_rate
  ```
  ```python
    # Query to calculate the total revenue for each aircraft
  total_revenue = pd.read_sql_query("""
      SELECT aircraft_code, SUM(amount) AS total_revenue
      FROM ticket_flights
      JOIN flights ON ticket_flights.flight_id = flights.flight_id
      GROUP BY aircraft_code
      """, connection)
  
  # Display the total revenue DataFrame
  total_revenue
  
  # Calculate and add a new column 'inc Total Annual Turnover' in the 'occupancy_rate' DataFrame
  occupancy_rate['inc Total Annual Turnover'] = (total_revenue['total_revenue'] / occupancy_rate['occupancy_rate']) * occupancy_rate['inc occupancy rate']
  
  # Display the updated 'occupancy_rate' DataFrame
  occupancy_rate
  ```
  ```
      aircraft_code       booked_seats  num_seats      occupancy_rate  \
  0           319  53.58318098720292        116 0.46192397402761143   
  1           321  88.80923076923077        170  0.5224072398190045   
  2           733  80.25546218487395        130   0.617349709114415   
  3           763 113.93729372937294        222  0.5132310528350132   
  4           773  264.9258064516129        402   0.659019419033863   
  5           CN1  6.004431314623338         12  0.5003692762186115   
  6           CR2  21.48284690220174         50 0.42965693804403476   
  7           SU9  56.81211267605634         97  0.5856918832583128   
  
     inc occupancy rate  inc Total Annual Turnover  
  0  0.5081163714303726               2976779410.0  
  1   0.574647963800905               1801980510.0  
  2  0.6790846800258565         1569207310.0000002  
  3  0.5645541581185146               4808404810.0  
  4  0.7249213609372492               3774326050.0  
  5  0.5504062038404727         106011180.00000001  
  6  0.4726226318484382               2181036550.0  
  7   0.644261071584144          5625933169.999999
  ```
## Analysis
The analysis of data provides insights into the number of planes with more than **100 seats**, how the number of tickets booked and total amount earned changed over time, and the *average fare* for each aircraft with different fare conditions. These findings will be useful in developing strategies to increase *occupancy rates* and optimize pricing for each aircraft. **Table 1** shows the aircraft with more than **100 seats** and the actual count of the seats.<br>


***Table 1***
| **Aircraft Code** | **Number Of Seats** |
|---------------|-----------------|
| 319           | 116             |
| 320           | 140             |
| 321           | 170             |
| 733           | 130             |
| 763           | 222             |
| 773           | 402             |

In order to gain a deeper understanding of the trend of ticket bookings and revenue earned through those bookings, we have utilized a line chart visualization. Upon analysis of the chart, we observe that the number of tickets booked exhibits a gradual increase from **June 22nd to July 7th**, followed by a relatively stable pattern from *July 8th until August*, with a noticeable peak in ticket bookings where the highest number of tickets were booked on a single day. It is important to note that the *revenue earned* by the company from these bookings is closely tied to the number of tickets booked. Therefore, we can see a similar trend in the *total revenue* earned by the company throughout the analyzed time period. These findings suggest that further exploration of the factors contributing to the peak in ticket bookings may be beneficial for increasing overall revenue and optimizing operational strategies.<br>

_**Figure 1**_
![image](https://github.com/saadharoon27/Airline-Data-Analysis-SQL-Project/assets/147087623/adecd6b4-1f0c-4a12-9029-aea86e157acf)

_**Figure 2**_
  ![image](https://github.com/saadharoon27/Airline-Data-Analysis-SQL-Project/assets/147087623/77921a66-c81b-48ec-b01a-b530a449ee24)

_**Figure 3**_<br>
![image](https://github.com/saadharoon27/Airline-Data-Analysis-SQL-Project/assets/147087623/6575ffd4-b528-4e4f-878b-e76164191e65)

We were able to generate a bar graph to graphically compare the data after we completed the computations for the *average costs* associated with different fare conditions for each aircraft. The graph **Figure 3** shows data for three types of fares: *business*, *economy*, and *comfort*. It is worth mentioning that the *comfort class* is available on only one aircraft, the **773**. The **CN1** and **CR2** planes, on the other hand, only provide the *economy class*. When different pricing circumstances within each aircraft are compared, the charges for *business class* are consistently greater than those for *economy class*. This trend may be seen across all planes, regardless of fare conditions. <br>

_**Analysing Occupancy Rate**_ <br>
Airlines must thoroughly analyze their *revenue streams* in order to maximize *profitability*. The *overall income per year* and *average revenue per ticket* for each aircraft are important metrics to consider. Airlines may use this information to determine which *aircraft types* and *itineraries* generate the most *income* and alter their operations appropriately. This research can also assist in identifying potential for *pricing optimization* and allocating resources to more *profitable routes*. The below **Figure 4** shows the *total revenue*, *total tickets*, and *average revenue* made per ticket for each *aircraft*. The aircraft with the highest *total revenue* is **SU9**, and from **Figure 3**, it can be seen that the price of the *business class* and *economy class* is the lowest in this aircraft. This can be the reason that most of the people bought this aircraft ticket as its cost is less compared to others. The aircraft with the least *total revenue* is **CN1**, and the possible reason behind this is it only offers *economy class* with very *least price*, and it might be because of its poor conditions or *less facilities*.
<br>

***Figure 4***
|  **SR**  | **aircraft_code** | **total_count** | **ticket_revenue** | **avg_revenue_per_ticket** |
| --- | ------------- | ----------- | -------------- | ------------------------ |
|  0  |      319      |    52853    |   2706163100   |          51201           |
|  1  |      321      |   107129    |   1638164100   |          15291           |
|  2  |      733      |    86102    |   1426552100   |          16568           |
|  3  |      763      |   124774    |   4371277100   |          35033           |
|  4  |      773      |   144376    |   3431205500   |          23765           |
|  5  |      CN1      |    14672    |    96373800    |          6568            |
|  6  |      CR2      |   150122    |   1982760500   |          13207           |
|  7  |      SU9      |   365698    |   5114484700   |          13985           |

**The average occupancy per aircraft** is another critical number to consider. Airlines may measure how successfully they fill their seats and discover chances to boost **the occupancy rate** by using this metric. Higher **occupancy rates** can help airlines increase revenue and profitability while lowering operational expenses associated with vacant seats. **Pricing strategy**, **airline schedules**, and **customer satisfaction** are all factors that might influence **occupancy rates**. The below **Figure 5** shows the **average booked seats** from the total number of seats for each aircraft. The **occupancy rate** is calculated by dividing the **booked seats** by the total number of seats. Higher **occupancy rate** means the aircraft seats are more booked and only a few seats are left unbooked.<br>

***Figure 5***
| **SR**  | **aircraft_code** | **booked_seats** | **num_seats** | **occupancy_rate** |
|---|--------------|--------------|-----------|----------------|
| 0 | 319          | 53.583181    | 116       | 0.461924       |
| 1 | 321          | 88.809231    | 170       | 0.522407       |
| 2 | 733          | 80.255462    | 130       | 0.617350       |
| 3 | 763          | 113.937294   | 222       | 0.513231       |
| 4 | 773          | 264.925806   | 402       | 0.659019       |
| 5 | CN1          | 6.004431     | 12        | 0.500369       |
| 6 | CR2          | 21.482847    | 50        | 0.429657       |
| 7 | SU9          | 56.812113    | 97        | 0.585692       |

Airlines can assess how much their *total yearly turnover* could improve by providing all aircraft a *10% higher occupancy rate* to further examine the *possible benefits* of raising occupancy rates. This research can assist airlines in determining the *financial impact* of boosting occupancy rates and if it is a *realistic strategy*. Airlines may enhance occupancy rates and revenue while delivering greater value and service to consumers by optimizing *pricing tactics* and other operational considerations. The below figure shows how the *total revenue increased* after increasing the *occupancy rate by 10%* and it gives the result that it will *increase gradually* so airlines should be more focused on the *pricing strategies*.

## Conclusion
To summarize, analyzing revenue data such as *total revenue per year*, *average revenue per ticket*, and *average occupancy per aircraft* is critical for airlines seeking to maximize profitability. Airlines can find areas for improvement and modify their *pricing* and *route plans* as a result of assessing these indicators. A *greater occupancy rate* is one important feature that can enhance profitability since it allows airlines to maximize revenue while minimizing costs associated with vacant seats. The airline should revise the *price* for each aircraft as the *lower price* and *high price* is also the factor that people are not buying tickets from those aircrafts. They should decide the *reasonable price* according to the *condition* and *facility* of the aircraft and it should not be very *cheap* or *high*.

Furthermore, boosting occupancy rates should not come at the price of consumer *happiness* or *safety*. Airlines must strike a balance between the necessity for *profit* and the significance of delivering *high-quality service* and upholding *safety regulations*. Airlines may achieve long-term success in a highly competitive business by adopting a *data-driven strategy* to *revenue analysis* and *optimization*.
