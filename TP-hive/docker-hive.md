# TP HIVE FOR DOCKER (QUICKSTART)

Apache Hive™ is a popular open-source data warehousing framework that allows users to query large datasets stored in distributed storage systems like Hadoop HDFS, Apache Ozone, Amazon S3, or Microsoft Azure Data Lake Storage (ADLS). It provides a SQL-like (called HiveQL) interface to query, analyze, and manage structured and semi-structured data.

With Hive, users can define tables and schemas, write queries in a familiar SQL-like language and perform various data analysis tasks such as filtering, aggregating, and joining data. Hive supports many popular data formats including CSV, JSON, Avro, ORC, and Parquet.

## Quickstart

Launch the HiveServer2 with an embedded Metastore.
This is lightweight and for a quick setup, it uses Derby as metastore db.

```
docker run -d -p 10000:10000 -p 10002:10002 --env SERVICE_NAME=hiveserver2 --name hive4 apache/hive:4.1.0
```

Launch Standalone Metastore
For a quick start, launch the Metastore with Derby,

```
docker run -d -p 9083:9083 --env SERVICE_NAME=metastore --name metastore-standalone apache/hive:4.1.0
```
Warning! Everything would be lost when the service is down! In order to save the Hive table's schema and data, start the container with an external Postgres and Volume to keep them.


## Usage
Accessing Beeline:
```
docker exec -it hive4 beeline -u 'jdbc:hive2://localhost:10000/'
```

You can use beeline as it is installed on your host machine beeline -u 'jdbc:hive2://localhost:10000/'

Accessing HiveServer2 Web UI:
Accessed on browser at http://localhost:10002/

Example Queries to execute in Beeline
```
show tables;
```

```
create table hive_example(a string, b int) partitioned by(c int);
```

```
alter table hive_example add partition(c=1);
```

```
insert into hive_example partition(c=1) values('a', 1), ('a', 2),('b',3);
```

```
select count(distinct a) from hive_example;
```

```
select sum(b) from hive_example;
```
