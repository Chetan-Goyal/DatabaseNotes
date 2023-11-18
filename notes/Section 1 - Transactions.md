---
title: Section 1 - Transactions
attachments: [transactions_problems.png]
created: 2023-10-20T06:37:16.714Z
modified: 2023-11-17T15:24:34.381Z
---

# Section 1 - Transactions

## Read Uncommitted
Any change in any transaction (whether committed or not) can be observed in other transactions in between.

## Read Commited
Only commited changes from other transactions are visible in our transaction.

## Repeatable Read
Even if data is changed, our transaction will consider the same data even when it is read again. (ONly in case of Postgresql, other shows different data)
Important Point (Non Postgres) : As name suggests, It allows repeatable read means row level locking on already read data. So, if some data is inserted, it will be visible in the results as lock is not present there.

## Serializable
All transactions are serialiaised. means one after the other like a queue.


**Note:** _If Dependency/Conflict found, It will rollback the remaining transaction which we try to apply. Eg- A & B are related. A applied. When B is being applied, it will throw error and rollback._

## Snapshot
Every transaction has it's own copy of data and works on it. Similar to Serializable but the changes of transaction A will not be visible in transaction B.


![problems visible in each transaction type](../attachments/transactions_problems.png)

Official Postgresql transaction comparison: https://www.postgresql.org/docs/current/transaction-iso.html#MVCC-ISOLEVEL-TABLE

Note:
Some long failed transactions can create dead rows, which can alter the performance of the queries. So, when vacuum runs, it cleans those things.




## **Example:**
There is a hospital which must have atleast 1 doctor at all time. If a doctor is trying to sign out, what will be the best method?


Answer:
1. Serialisable Transaction.
2. Select for update query.

> The SELECT FOR UPDATE statement is used to order transactions by controlling concurrent access to one or more rows of a table. It works by locking the rows returned by a selection query, such that other transactions trying to access those rows are forced to wait for the transaction that locked the rows to finish.
