---
title: Section 7 - Sharding
created: '2023-11-02T04:15:16.884Z'
modified: '2023-11-02T05:09:17.128Z'
---

# Section 7 - Sharding
Splitting rows into multiple databases where each deployed at different location/server etc.

**Very complex, should be avoided by all means.**


## Consistent Hashing
Specific input should also point to specific server everytime.
For example: Input1 should always be pointed at some server, 5432.. Our hash function can't give different values for same input.


## Demo

### Creating Seed Sql file
```sh
vim init.sql
```
Contents:
```sql
CREATE TABLE URL_TABLE (id serial NOT NULL PRIMARY KEY, URL text, URL_ID character(5));
```

### Creating Docker File
```sh
vim Dockerfile
```

Contents:
```
FROM postgres
COPY init.sql /docker-entrypoint-initdb.d
```
Above Commands marks the inital sql file for the database.


```sh
docker build -t pgshard .
```

### Firing up 3 shards
```sql
docker run --name pgshard1 -p 5432:5432 -d pgshard
docker run --name pgshard2 -p 5433:5432 -d pgshard
docker run --name pgshard3 -p 5434:5432 -d pgshard
```



### Approaching Problem
Sharding shouldn;t be implemented directly. Most complicated thing..


Index -> Partitioning -> Replication (Read / Write separate) -> Advanced Replication (Multiple Read / Multiple Write dbs) -> Sharding



## Problems with Sharding
- No ACID (Transactions, Rollbacks etc)
- Almost impossible to change data division in shards without downtime
