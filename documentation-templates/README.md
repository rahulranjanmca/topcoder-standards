
# VA Online Memorial - Data import & sync 
Deployment Guide

### Description

## Prerequisites
1. Node 8.x
  1. NPM
1. PostgreSQL 10.1


## Local Deployment
Initialize you database with ddl.sql

```bash
npm install
npm start

```

## Production Build and Installation
Initialize you database with ddl.sql
```bash
npm install
npm start
```

### Manual Deployment Notes
1.	Run the ddl.sql script PostgreSQL. It will create all the DB schemas

2.	Download the Submission.zip and extract it in your folder. I will call your folder as <BASE_FOLDER>

3.	Got to <BASE_FOLDER>/Sumission/app/config.json

  a.	MAX_NO_OF_CSV_LINE_ONE_TIME- max no of lines to process at a time- Right now its 50000, You can increase or decrease depending upon hardware.
  b.	FILE_STARTING_INDEX- If you are running second time, and only want to update from  last the states index from this number. Index is the sequence in which url is found in data.json. Default 0.
  c.	FILE_STARTING_INDEX- If you are running second time, and only want to update last the states index from this number. Index is the sequence in which url is found in data.json. Default -1. Means process all.
  d.	DATABASE_USER- PostgreSQL  Database User
  e.	DATABASE_NAME-  PostgreSQL  Database Name
  f.	DATABASE_PASSWORD-   PostgreSQL  Database Password
  g.	DATABASE_HOST-  PostgreSQL  Database Host
  h.	DATABASE_PORT- PostgreSQL  Database Port
  i.	TEST_MODE- true/false, If test mode, and if data is already in /data/ folder, it will not download and processes the earlier file. But with production mode, it will always download. Default false. Have included data.json in the app folder, as in test mode it doesn’t download data.json but it reads from local.
  j.	VETERAN_UNIQUE_KEY_COLUMN_INDEXES-  Comma separated indexes which should be considered as unique key consideration for veteran details. 
  k.	CEMENTRY_UNIQUE_KEY_COLUMN_INDEXES- Comma separated indexes which should be considered as unique key consideration for cemetery. 

4.	Open Console or bash command from app folder
5.	Npm install
6.	Npm start
7.	Now you can check database periodically to see the records being inserted. Download might take some time.


## Running Tests
  No test case righ now

## Notes
Some Consideration
  a.	 Node will never be out of memory, As we are reading line by line and no of lines can be reduced.
  b.	This app, downloads all the file into data folder and processes from there. But it keeps data.json in memory for processing and let this go once we have all the urls from it. You can check the progress of download in data folder as well.
  c.	Once the database rows will be greater that 1 or 1.5 million, database might be little slow
  d.	Probably Primary key and foreign key can be removed to make the database faster. But as a complete schema details, I am putting all the foreign key and primary key. Am testing with this only.
  e.	You cannot remove unique key constraint. This is the base for primary key. And my queries needs it be unique. 
  f.	One and only one query will be fired for one operation.  So lets take, we have 55000 records and max rows to process are 10000,       then a total of 6 queries will be fired for save. And same number of queries will be fired for getting and updating war, branch,      rank etc.
  g.	This is designed in such a way, we keep unique key, and foreign key. In future if the think to change the primary key base, just update the config.json and empty the table and run the program. If you don’t want to empty the tables, update the table unique key with the way I am generating in program like underscore separated and then run the program.
