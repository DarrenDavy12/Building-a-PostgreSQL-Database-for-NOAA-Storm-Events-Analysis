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

#### Wrote three python scripts for the three tables created in vscode with errors included. 

    - storms table
![Image](https://github.com/user-attachments/assets/0670b822-ddd5-4d59-bae3-cbdea8c80811)

![Image](https://github.com/user-attachments/assets/70b3dc83-2c83-416e-a4a3-d97d067172b5)


    - locations table
![Image](https://github.com/user-attachments/assets/68c8de7a-6fe0-4934-b7fd-1dab690205f5)

![Image](https://github.com/user-attachments/assets/795a3644-6e8d-41ae-bf46-0035bb79c0e6)


--- 

    - fatalities table 
![Image](https://github.com/user-attachments/assets/0b745c1e-38d5-4367-8226-5c6b8b9bc157)



### 5. Verifying postgreSQL setup  

First I logged into psql (sql shell) and ran these commands: 

        `\1 -- listed databases inside postgres`
        
        `\dt -- listed tables inside of storm_events_db` 
        
        `SELECT * FROM storms LIMIT 5; -- selected the first five rows from the details(storms) table`  


![Image](https://github.com/user-attachments/assets/7b9472ac-add4-468e-b5f5-73d02d001858)



### 6. Ran Scripts:

#### - Execute create_tables.sql to set up the database schema.

#### - Ran load_data.py for all three table with their individual scripts to load the CSV data into PostgreSQL.



##### - storm table
    
![Image](https://github.com/user-attachments/assets/f85176a2-9d5f-48be-8c02-d5cee9138f69)

##### - ran a query in pgAdmin for the storms table

![Image](https://github.com/user-attachments/assets/e78bc99d-ea8c-4af1-ae16-5ffb0128be2c)




##### - locations table
    
![Image](https://github.com/user-attachments/assets/9bef7480-7af7-4f5e-938b-b4a8c721f251)

##### - ran a query in pgAdmin for the locations table





##### - fatalities table

![Image](https://github.com/user-attachments/assets/0a1a23b5-4522-4a3f-b860-8e42aa849bb0)

##### - ran a query in pgAdmin for the fatalities table

![Image](https://github.com/user-attachments/assets/b08a0412-0a4d-4a3b-aef9-0392bc8e5d7d)


---

#### Next was querying the 'storm' table in which I ran into an error, these are steps I took (below). 

### Troubleshooting error:

#### - Checked error logfile and saw that a duplicate of EVENT_ID column as a unique key was being added to the already created EVENT_ID.
    
![Image](https://github.com/user-attachments/assets/53e75223-099f-4797-b04f-565e2129c0d8)


#### - So I had to run the command in psql:
        `TRUNCATE TABLE storms RESTART IDENTITY;` -- removes all rows from the storms table
    
![Image](https://github.com/user-attachments/assets/c0e2bc32-d1c1-49bb-878a-be92feb83994)


#### - Running the 'TRUNCATE' command resolved the issue and I verified the data by running these sql commands inside pgAdmin:
    
        `SELECT COUNT(*) FROM storms;  -- Check total rows`
        `SELECT * FROM storms LIMIT 5;  -- View sample data`

![Image](https://github.com/user-attachments/assets/c35bf40b-8753-45fd-8933-d8c34b3cc4ef)

---


![Image](https://github.com/user-attachments/assets/1a3ce51e-f24a-4a2d-bd54-142bcbd820f8)



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




### Number of storms per year:

        `SELECT EXTRACT(YEAR FROM start_date) as year, COUNT(*) as total_storms
        FROM storms
        GROUP BY year`
        ORDER BY year;



#### I created index's, for faster date-based queries and joins in locations. 

![Image](https://github.com/user-attachments/assets/8eb3b9b7-9af0-455d-b9d7-c428e5378a10)



