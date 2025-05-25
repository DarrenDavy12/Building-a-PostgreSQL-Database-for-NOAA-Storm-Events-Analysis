# Building-a-PostgreSQL-Database-for-NOAA-Storm-Events-Analysis




## Project Overview
This project builds a PostgreSQL database to store and analyze the NOAA Storm Events Database, which contains records of severe weather events in the United States. The project demonstrates key data engineering skills, including database design, data ingestion with Python, SQL querying, and query optimization.


## Dataset
The dataset is sourced from the NOAA Storm Events Database. It includes details on storm events (e.g., type, dates, damages) and related location data.







## Setup Instructions


### 1. Install PostgreSQL: Download and install PostgreSQL from postgresql.org.



### 2. Create Database: Run CREATE DATABASE storm_events_db; in psql or pgAdmin.

![Image](https://github.com/user-attachments/assets/b38a1c0a-23c6-4982-b468-47bcada1d387)



### 3.Install Python Libraries:

`pip install pandas psycopg2-binary`



### 4. Download Data: Get the CSV files from NOAA's FTP page.

### 5. Run Scripts:

  - Execute create_tables.sql to set up the database schema.

  - Run load_data.py to load the CSV data into PostgreSQL.

  - Use analytical_queries.sql to explore the data.

  - Apply optimize.sql for performance improvements.



## Sample Queries


### Top 10 states with most storm events:

`SELECT state, COUNT(*) as total_events
FROM locations
GROUP BY state
ORDER BY total_events DESC
LIMIT 10;`



### Total damage by event type:

`SELECT event_type, SUM(damage_property + damage_crops) as total_damage
FROM storms
GROUP BY event_type
ORDER BY total_damage DESC;`
