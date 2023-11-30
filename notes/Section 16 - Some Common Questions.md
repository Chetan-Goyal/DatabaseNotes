---
title: Section 16 - Some Common Questions
created: 2023-11-18T16:14:16.310Z
modified: 2023-11-20T05:10:52.832Z
---

# Section 16 - Some Common Questions

<details>
<summary>What is the Unit of Cost in Postgres</summary>
<markdown>
It is some system defined unit which gives us rough estimation to how much work DB had to do in order to execute our query. It must not be confused with milliseconds.
</markdown>
</details>

<details>
<summary>Snapshot and Repeatable Read Differences</summary>
<markdown>
Snapshot Gives the copy of db to the transactions and they can perform their operations on it. It will give error in case of a conflict but logical errors can occur since all transactions will have of snapshot of the changes.

However in Repeatable Read, It is a bit similar but it adds lock to all the rows which are read and in case of insertions, those changes (phantom read) will be reflected in the read queries. (Postgres avoid Phantom Reads)

Differences
Insertions will appear out of nowhere in Repeatable Read whereas Snapshot will remain intact. 
</markdown>
</details>


<details>
<summary>I have Index, still db doing full db scan</summary>
<markdown>
In case of less no of records or query returning most/many of the records, db can skip the index completely to avoid 2 hops and instead directly accessing the table b+tree.
</markdown>
</details>

<details>
<summary>Why DB read pages not rows?</summary>
<markdown>
Due to unequal sizes of the rows, exact row address is stored in the metadata of the page and page address is stored in the heap. That's why, Pages are fetched in order to get the metadata and the address of the required row and then row is fetched.
</markdown>
</details>



<details>
<summary><markdown>Why use serializable isolation level when we have `Select for Update`?</markdown></summary>
<markdown>
`Select for update` is pesimistic approach and gives us better control but we can have a chance of leaving deadlocks.
</markdown>
</details>


<details>
<summary>Why updating touches all indexes?</summary>
<markdown>
Updating means creation of new row and updating references of old row to now point to this newly created row with latest data.
In case there is no space in a page and row needed to be created in a sepatate page then the indexes must be notified about the new location of the newly created row.
</markdown>
</details>


<details>
<summary>Postgres vs MySQL</summary>
<markdown>
- Postgres has all indexes as secondary indexes (including primary key) and all these indexes point to the row id.

- MySQL has primary key as the primary index and all secondary indexes points to this primary key index.
</markdown>
</details>


