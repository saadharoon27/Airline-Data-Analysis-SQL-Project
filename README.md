![banner](Assets/Banner.jpg)

# Airline-Data-Analysis-SQL-Project
***A company operating a diverse aircraft fleet faces profitability challenges due to environmental regulations, rising costs, and a tight labor market. They aim to analyze their database to increase occupancy rates and enhance seat profitability.***

## Author
- [@saadharoon27](https://github.com/saadharoon27)

## Table of Contents
- [Project Overview](#project-overview)
- [Project Focus](#project-focus)
- [About The Dataset](#about-the-dataset)

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
[**Airlines Dataset**](https://www.kaggle.com/datasets/saadharoon27/airlines-dataset)
<br>
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
