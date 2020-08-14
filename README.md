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
1. External - Hive is only responsible for metadata.
2. Internal - Hive is the sole owner of metadata and table's data.

* Dropping Internal Table - Both metadata and table data are losr
* Dropping External Table - Only metadata is lost.

By default, Hive creates Internal Table.
```sql
CREATE TABLE IF NOT EXISTS table1(col1 string, col2 array<string>, col3 string, col4 int) row format delimited fields terminated by ',' collection items terminated by ':' lines terminated by '\n' stored as parquet location '/user/rahul/table1'

```

## Loading data in table

### 1. From HDFS
```SQL
load data inpath 'hdfs://user/rahul/' into table table1
```

### 2. From Local FS
```SQL
load data local inpath '/users/rahul/Desktop/file.txt' into table table1
```

