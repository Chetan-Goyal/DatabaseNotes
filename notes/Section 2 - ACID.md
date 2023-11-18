---
title: Section 2 - ACID
created: 2023-10-20T05:21:41.641Z
modified: 2023-10-20T06:57:34.704Z
---

# Section 2 - ACID

## ACID

### A - Atomicity
Refers to the transaction being treated as a single unit (query) instead of collection of queries

### C - Consistency
Data should remain consistent (in sync with table) and has predefined / predicatable effect.

### I - Isolation
Transaction must be independent even when they are happening parallely.

### D - Durability
In case of system failure, data must remain intact for successfully applied transactions.



## Phantom Reads (Important Topic)
If there is a transaction A, and some change is applied from outside. It will get reflected in transaction A which will result in inconsistent results breaking C+I of ACID.

#### Special Case
Postgres is the only db which avoids phantom reads in repeatable read and serialisable. All other db only avoids phantom reads in serialisation transactions.


## Eventual Consistency

#### Consistency in Data
Usage of foreign keys and linking to avoid inconsistent data which usually happens when deleting a record but not deleting data of same user in other tables.

#### Consistency in Reads
3 shards of db.. Data added in 1 (master maybe/ region specific), but read immediately from 3.. It is still not propagated to 3 resulting in inconsistent data (old data).
