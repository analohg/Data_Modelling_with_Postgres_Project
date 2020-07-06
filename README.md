# Data_Modelling_with_Postgres_Project
## Purpose
Sparkify (an app for streaming music) have been collecting data of songs and user activity. Their current system isn't optimized to easily query data that resides on a directory of JSON logs for user activity (LOG DATA) and a directory of json metadata for songs (SONG DATA) in their app. This project focuses on creating a database with tables designed to optimize queries on song play analysis and I'm creating a database schema and and ETL pipeline process for this analysis.

## Database Schema Design
The Database was designed to contain one fact table (songplays) on music streaming activity. Four (4) dimension tables are created to complement this fact table and they all have keys on which they can be joined to the fact table(song plays) to easily query the entire database for specific information. The dimension tables include;

- user table
- song table
- artist table
- time table.
A doc explaining the tables and its content is provided below.

### Songplays Table
- Name: songplays
- Type: Fact Table

|Column	|Type	|Description|
| ----- | --- | --------- |
|songplay_id	|INTEGER	|The main identification of the table|
|start_time	|TIMESTAMP NOT NULL	|The timestamp of when the song play log happened|
|user_id	|INTEGER NOT NULL REFERENCES users (user_id)	|This is unique for each user and it refers to that user that triggered this song play log. It cannot be null, as any songplay must be trigerred by a user|
|level	|VARCHAR	|The level of the user that triggered this song play log|
|song_id	|VARCHAR REFERENCES songs (song_id)	|The identification of the song that was played|
|artist_id	|VARCHAR REFERENCES artists (artist_id)	|The identification of the artist of the song that was played.|
|session_id	|INTEGER NOT NULL	|The session_id of the user on the app|
|location	|VARCHAR	|The location where this song play log was triggered|
|user_agent	|VARCHAR	|The user agent of our app|

### Users Tables
- Name: users
- Type: Dimension Table


|Column	|Type	|Description|
| ----  | --- | --------- |
|user_id	|INTEGER PRIMARY KEY	|The main identification of an user|
|first_name	|VARCHAR NOT NULL	|First name of the user, can not be null. It is the basic information we have from the user|
|last_name	|VARCHAR NOT NULL	|Last name of the user.|
|gender	|CHAR(1)	|The gender is stated with just one character M (male) or F (female). Otherwise it can be stated as NULL|
|level	|VARCHAR NOT NULL	|The level stands for the user app plans (premium or free)|


### Songs Tables
- Name: songs
- Type: Dimension Table


|Column	|Type	|Description|
| ----- | --- | --------- |
|song_id	|VARCHAR PRIMARY KEY	|The main identification of a song|
|title	|VARCHAR NOT NULL	|The title of the song. It can not be null, as it is the basic information we have about a song.|
|artist_id	|VARCHAR NOT NULL REFERENCES artists (artist_id)	|The artist id, it can not be null as we don't have songs without an artist, and this field also references the artists table.|
|year	|INTEGER NOT NULL	|The year that this song was made|
|duration	|NUMERIC (15, 5) NOT NULL	|The duration of the song|


### Artists Table
- Name: artists
- Type: Dimension Table


|Column	|Type	|Description|
| ----- | --- | --------- |
|artist_id	|VARCHAR PRIMARY KEY|	The main identification of an artist|
|name	|VARCHAR NOT NULL	|The name of the artist|
|location	|VARCHAR	|The location where the artist are from|
|latitude	|NUMERIC	|The latitude of the location that the artist are from|
|longitude	|NUMERIC	|The longitude of the location that the artist are from|

### Time Table
- Name: time
- Type: Dimension Table


|Column	|Type	|Description|
| ----- | --- | --------- |
|start_time	|TIMESTAMP NOT NULL PRIMARY KEY	|The timestamp itself, serves as the main identification of this table|
|hour	|NUMERIC NOT NULL	|The hour from the timestamp|
|day	|NUMERIC NOT NULL	|The day of the month from the timestamp|
|week	|NUMERIC NOT NULL	|The week of the year from the timestamp|
|month	|NUMERIC NOT NULL	|The month of the year from the timestamp|
|year	|NUMERIC NOT NULL	|The year from the timestamp|
|weekday	|NUMERIC NOT NULL	|The week day from the timestamp|

### ETL Pipeline
The pipeline works in two steps.

First you create the PostgreSQL database by running the `create_tables` script

    python create_tables.py
Next, you parse the log files by running the `etl.py` script

    python etl.py
    
### Project File Structure
A list of files this project contains and thier descriptions is included below

- `sql_queries.py` - This file contains the SQL queries to create and drop tables in the database, it is meant to serve as a query repository to use throughout the ETL process
- `create_tables.py` - It's the file reponsible for creating the database schema structure into the PostgreSQL database
- `etl.py` - This file is responsible for the main ETL process
- `etl.ipynb` - The python notebook that was written to develop the logic behind the etl.py process
- `test.ipynb` - And finally this notebook contains queries to test if the ETL process was successful (or not) by confirming if the tables have been created.
