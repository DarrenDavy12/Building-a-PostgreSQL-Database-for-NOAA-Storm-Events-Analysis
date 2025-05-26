# Building-a-PostgreSQL-Database-for-NOAA-Storm-Events-Analysis




## Project Overview
This project builds a PostgreSQL database to store and analyze the NOAA Storm Events Database, which contains records of severe weather events in the United States. The project demonstrates key data engineering skills, including database design, data ingestion with Python, SQL querying, and query optimization.


## Dataset
The dataset is sourced from the NOAA Storm Events Database. It includes details on storm events (e.g., type, dates, damages) and related location data.







## Setup Instructions


### 1. Created Database within pgAdmin4 (postgreSQL) : Run CREATE DATABASE storm_events_db; in psql or pgAdmin.

![Image](https://github.com/user-attachments/assets/98d8af75-2444-4110-9002-74eb59027f33)

--- 

### 2. Created tables in the 'storm_events_db' database by running: 

#### storms table 


`CREATE TABLE storms (
    storm_id SERIAL PRIMARY KEY,
    event_type VARCHAR(50),
    start_date DATE,
    end_date DATE,
    damage_property NUMERIC,
    damage_crops NUMERIC
);`


#### locations table

`CREATE TABLE locations (
    location_id SERIAL PRIMARY KEY,
    storm_id INTEGER REFERENCES storms(storm_id),
    state VARCHAR(50),
    county VARCHAR(100),
    latitude NUMERIC,
    longitude NUMERIC
);`

#### fatalities table

`CREATE TABLE fatalities (
    fatality_id SERIAL PRIMARY KEY,
    storm_id INTEGER REFERENCES storms(storm_id),
    number_of_fatalities INTEGER
);`


![Image](https://github.com/user-attachments/assets/ba48bd8f-4cc7-4597-b1c5-48fa1ad7c11b)


---

### 3.Installed Python Libraries:

Ran the command: `pip install pandas psycopg2-binary` inside VScode terminal. 

![Image](https://github.com/user-attachments/assets/54dc9a2c-f8e4-4295-b778-ca18d770037f)


---

### 4. Data Ingestion: Downloaded CSV files from NOAA's FTP page.

#### Wrote python script in vscode with errors included. 

![Image](https://github.com/user-attachments/assets/0670b822-ddd5-4d59-bae3-cbdea8c80811)

![Image](https://github.com/user-attachments/assets/70b3dc83-2c83-416e-a4a3-d97d067172b5)

--- 


### 5. Verifying postgreSQL setup  

First I logged into psql (sql shell) and ran these commands: 

`\1 -- listed databases inside postgres`

`\dt -- listed tables inside of storm_events_db` 

`SELECT * FROM storms LIMIT 5; -- selected the first five rows from the details(storms) table`  


![Image](https://github.com/user-attachments/assets/7b9472ac-add4-468e-b5f5-73d02d001858)



### 4. Ran Scripts:

  - Execute create_tables.sql to set up the database schema.

  - Run load_data.py to load the CSV data into PostgreSQL.

  - Use analytical_queries.sql to explore the data.

  - Apply optimize.sql for performance improvements.


--- 

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
