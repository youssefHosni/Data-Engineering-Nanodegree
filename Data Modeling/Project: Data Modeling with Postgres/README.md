# Project: Data Modeling with Postgres

This project models user activity data for a music streaming app called Sparkify to optimize queries for understanding what songs users are listening to by creating a **Postgres relational database** and **ETL pipeline** to build up **Fact and Dimension tables** and insert data into new tables.


## Introduction 

A startup called Sparkify wants to analyze the data they've been collecting on songs and user activity on their new music streaming app. The analytics team is particularly interested in understanding what songs users are listening to. Currently, they don't have an easy way to query their data, which resides in a directory of JSON logs on user activity on the app, as well as a directory with JSON metadata on the songs in their app.

They'd like a data engineer to create a Postgres database with tables designed to optimize queries on song play analysis, and bring you on the project. Your role is to create a database schema and ETL pipeline for this analysis. You'll be able to test your database and ETL pipeline by running queries given to you by the analytics team from Sparkify and compare your results with their expected results.


## Dataset 

### Song Dataset
The first dataset is a subset of real data from the [Million Song Dataset](http://millionsongdataset.com/). Each file is in JSON format and contains metadata about a song and the artist of that song. The files are partitioned by the first three letters of each song's track ID. For example, here are filepaths to two files in this dataset.

song_data/A/B/C/TRABCEI128F424C983.json
song_data/A/A/B/TRAABJL12903CDCF1A.json

### Log Dataset
The second dataset consists of log files in JSON format generated by this [event simulator](https://github.com/Interana/eventsim) based on the songs in the dataset above. These simulate activity logs from a music streaming app based on specified configurations.

The log files in the dataset you'll be working with are partitioned by year and month. For example, here are filepaths to two files in this dataset.

log_data/2018/11/2018-11-12-events.json
log_data/2018/11/2018-11-13-events.json

## Project Structure

```
Data Modeling with Postgres
|____data     # Dataset
| |____log_data
| | |____...
| |____song_data
| | |____...
|
|____notebook      # notebook for developing and testing ETL
| |____etl.ipynb     # developing ETL builder
| |____test.ipynb    # testing ETL builder
| |____cheat-sheet.pdf
|
|____src        # source code
| |____etl.py   # ETL builder
| |____sql_queries.py    # ETL query helper functions
| |____create_tables.py		    # database/table creation script
```


## ETL Pipeline
### etl.py
ETL pipeline builder

1. `process_data`
* Iterating dataset to apply `process_song_file` and `process_log_file` functions
2. `process_song_file`
* Process song dataset to insert record into _songs_ and _artists_ dimension table
3. `process_log_file`
* Process log file to insert record into _time_ and _users_ dimensio table and _songplays_ fact table

### create_tables.py
Creating Fact and Dimension table schema

1. `create_database`
2. `drop_tables`
3. `create_tables`

### sql_queries.py
Helper SQL query statements for `etl.py` and `create_tables.py`

1. `*_table_drop`
2. `*_table_create`
3. `*_table_insert`
4. `song_select`


## Database Schema
### Fact table
```
songplays
- songplay_id    PRIMARY KEY
- start_time     REFERENCES time (start_time)
- user_id        REFERENCES users (user_id)
- level
- song_id         REFERENCES songs (song_id)
- artist_id       REFERENCES artists (artist_id)
- session_id
- location
- user_agent
```

### Dimension table
```
users
- user_id   PRIMARY KEY
- first_name
- last_name
- gender
- level

songs
- song_id   PRIMARY KEY
- title
- artist_id
- year
- duration

artists
- artist_id   PRIMARY KEY
- name
- location
- latitude
- longitude

time
- start_time   PRIMARY KEY
- hour
- day
- week
- month
- year
- weekday
```
