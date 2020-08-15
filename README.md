# Hive-Cheatsheet
Quick reference and cheatsheet for commonly used Hive Commands.

***
## Creating a database - 
Note that Hive isn't a database, this just defines the schema and stores it in the METASTORE(Refer Hive Architecture) for retrieval later.
```sql
CREATE DATABASE IF NOT EXISTS db;
```
## Describing a database
```SQL
DESCRIBE DATABASE db;
```

## Displaying all databases
```SQL
SHOW DATABASES;
```

## Selecting a Database
```SQL
USE db;
```

## Creating Tables 
Tables are of 2 types in Hive - 
1. External - Hive is only responsible for metadata, this can be accessed by other applications aligned with HDFS security.
2. Internal - Hive is the sole owner of metadata and table's data, data can only be accessed using Hive.

* Dropping Internal Table - Both metadata and table data are losr
* Dropping External Table - Only metadata is lost.

By default, Hive creates Internal Table.
```sql
CREATE TABLE IF NOT EXISTS table1(col1 string, col2 array<string>, col3 string, col4 int) row format delimited fields terminated by ',' collection items terminated by ':' lines terminated by '\n' stored as parquet location '/user/rahul/table1'
```

To create and external table, use external keyword.
```sql
CREATE external TABLE IF NOT EXISTS table1(col1 string, col2 array<string>, col3 string, col4 int) row format delimited fields terminated by ',' collection items terminated by ':' lines terminated by '\n' stored as parquet location '/user/rahul/table1'
```


## Loading data in table from a source file

### 1. From HDFS
```SQL
load data inpath 'hdfs://user/rahul/' into table table1
```

### 2. From Local FS
```SQL
load data local inpath '/users/rahul/Desktop/file.txt' into table table1
```

## Loading data in table from another table
Task - Insert col1,col2,col3 from a table **emp_tab** into a new table **tab**
1. Create the table **tab**
```SQL
 CREATE TABLE tab(col1 int, col2, string, col3 string) stored as textfile;
```
2. Insert data from **emp_tab** to **tab**
```SQL
INSERT INTO TABLE tab select col1,col2,col3 from emp_tab;
```
3. Verify
```SQL
SELECT * FROM tab;
```

**Important Note - ```INTO ``` command will append data in existing table, to overwrite use ```INSERT OVERWRITE``` command.**

## Multi-insert in Hive
Task - From a table named **Employees**, we need to create seperate tables for Developers, Managers and HRs, based on **employee_type** attribute.

1. Create seperate tables for Developers, Managers and HRs.
```SQL
CREATE TABLE Developers(id int, name string, desg string)

CREATE TABLE Managers(id int,name string, desg string)

CREATE TABLE HRs(id int, name string, desg string)
```

2. Insert from **Employees** table to all above in one single command
```SQL
FROM Employees INSERT INTO TABLE Developers SELECT col1,col2,col3 WHERE col3='Developer' INSERT INTO TABLE Managers SELECT col1,col2,col3 WHERE col3='Manager' INSERT INTO TABLE HRs SELECT col1,col2,col3 WHERE col3='HR' 
```
