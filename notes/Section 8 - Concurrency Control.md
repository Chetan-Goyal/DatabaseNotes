---
title: Section 8 - Concurrency Control
created: 2023-11-03T03:34:44.101Z
modified: 2023-11-06T03:15:00.238Z
---

# Section 8 - Concurrency Control

## Exclusive Locks
- Prevents others from reading or writing to a resource while a transaction is modifying it.
- Example: Money Transactions

## Shared Locks
- Allows multiple transactions to read a resource concurrently.
- Example: Mainly used for generating reports with consistent data.

## Dead Locks
- If A depends on B, B depends on C, and C depends on A. then it will be stuck here forever.
- Handled by DBs most of the times.

Example: 
```sql
shell 1 : begin transaction;
shell 2 : begin transaction;
shell 1 : insert into test values (20);
shell 2 : insert into test values (21);
shell 2 : insert into test values (20);
```
Above scenario will throw dead lock error since trying to insert same unique value.
Although, rolling previous transaction will result in success of new transaction modifying same value.

## Two Phase Locking (Double Booking problem)

```sql
shell 1 : begin transaction;
shell 2 : begin transaction;

shell 1 : select * from seats where user_id=14; ! -- Returns not booked status
shell 2 : select * from seats where user_id=14; ! -- Returns not booked status

shell 1 : update seats set booked=1, name=Chetan where user_id=14; ! -- Books to Chetan
shell 2 : update seats set booked=1, name=NoOne where user_id=14; ! -- Hangs here but executes when shell 1 txn is commited

shell 1: commit;
shell 2: commit;
```
Here, seat is booked to Chetan but later updated to NoOne, This is double booking problem.

### Solutions

#### 1. Exclusive Lock
Execute select condition with `for update` for Exclusive Lock.
```sql
shell 1 : select * from seats where user_id=14 for update;
shell 2 : select * from seats where user_id=14 for update; ! -- Is paused here, until shell 1 is commmited
```

## Pagination via Offset
Pagination via offset is very slow since db fetches all rows upto required rows and then remove the offset no of rows..

Instead follow something like doing pagination on the basis of last record id.

It also fixes the issue of insert in between the pagination.

## Database Connection Pooling
It is good to have a pool of connections (multiple connections) and sending request randomly to one connection to reduce the load and improve performance.
