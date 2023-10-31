---
attachments: [automate_partitions.zip]
title: Section 6 - Partitioning
created: '2023-10-31T07:56:25.128Z'
modified: '2023-10-31T14:52:20.937Z'
---

# Section 6 - Partitioning

Partitioning refers to splitting of table into mutiple smaller tables for faster access.

## Vertical Partitioning vs Horizontal Partitioning

### Vertical Partitioning
Separating column(s) into mutliple tables.
Example: separating blob column to slow storage and keeping small columns in fast access memory.

### Horizontal Parititioning
- Separting rows into multiple tables.
- table can become very large (millions/billions of rows) which affects the performance or IO Operations.
Example: separating rows on base of limit like 200k in one table, next 200k in next table, and so on.

## Partitioning Types

### By Range
Pushing old data (date, ids) into slow memory

### By List
Move non Netflix Specials and non popular movies into slow memory

### By Hash
It is consistent hashing (done by Cassandra) by using Hash Functions to evenly distribute data among multiple tables.

## Partitioning vs Sharding
- Partitioning means dividing data in a single DB while Sharding means splitting big table into multiple tables across multiple database servers
- Partitioning is automatically managed by DB Client while sharding needs to be managed by us.
- Client is aware of the shard (the db which client is accessing)
- HP table name changes (or schema) in Paritioning but in Sharding, Everything is same. (only server changes)


## Create and Attach Parition table
```sql
create table grades_parts (id serial not null, g int not null) partition by range(g);
```

Note: We need to create each parition one by one. It is not automatic.

```sql
create table g0035 (like grades_parts including indexes);
create table g3560 (like grades_parts including indexes);
create table g6080 (like grades_parts including indexes);
create table g80100 (like grades_parts including indexes);
```

Now, we have to attach partitions in grades_parts table.

```sql
alter table grades_parts attach parition g0035 for values from (0) to (35);
alter table grades_parts attach parition g3560 for values from (35) to (60);
alter table grades_parts attach parition g6080 for values from (60) to (80);
alter table grades_parts attach parition g80100 for values from (80) to (100);
```

**Remember, we dont have any data here. :) We don't even have indexes as of now.**

## Populating Partitions
```sql
! -- Inserts data from grades_org
insert into grades_parts select * from grades_org;
select count(*) from grades_parts;
select max(g) from grades_parts;
select max(g) from g0035;
select count(*) from g0035;
```

## Creating Index
```sql
create index grades_parts_idx on grades_parts(g);
```

Database automatically creates index on all of the parititions automatically.


## Pros of partitioning
1. Improved Query perf when accessing single parititon
2. Sequential scan vs Scattered Index scan
3. Easy bulk loading (attach partition) - create new table and attach it. Will link all records of new table means bulk laoding.
4. Archiving old data into cheap storage

## Cons of Partitioning
1. Updating row is slow (moving one row to another partition)
2. Inefficient queries can scan all partitions


## Automate Partitioning Script
You can download it from [here](./../attachments/automate_partitions.zip)
