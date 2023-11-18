---
title: Section 12 - Database Cursors
created: 2023-11-15T04:12:24.065Z
modified: 2023-11-15T06:30:09.406Z
---

# Section 12 - Database Cursors

## Database Cursors
Database Cursors are like bookmark. Gib me a ref to a command and I will use it to fetch data next time.

```sql
declare c cursor for select id from grades where g between 90 and 100;
fetch c;
fetch last c;
```

### Pros
- Saves Memory
- App can control as per it's capability
- can be closed at any time
- paging, but difficult due to stateful
- PL/SQL

### Cons
- Stateful, cursors can't be shared between multiple connections while doing horizontal scaling. Can be done via proxies but difficult. IF done, it is amazing!! :D
- Can block execution of indexes due to this long running "transaction-like" cursor.


## Server Side vs Client Side DB Cursors
Server - DB
Client - App
### Server Side Cursor
leaves everything on the server but needs some extra round trips to fetch all. 
Usage: Only name needs to be given for server side cursor, everything else remains same. 
Should be avoided.

#### Pros
1. Client is free, server get's the overhead.

#### Cons
1. hundreds of clients using cursor, server have to manage a lot of data/states.
2. Leaking cursor will be worst.

**Usecase:** for opening million rows connection once and not putting overhead on clients.

### Client Side Cursor
pulls everything to the client, so faster due to less round trips.

#### Pros
1. Client gets the overhead of memory allocation, server doesn't care about any state

#### Cons
1. Lot of data req means high internet bandwidth (sometimes limit crossed)
2. Client may have less resources to manage that huge amount of data

**Usecase:** for opening small cursors like pagination for personalised recommendations, can do some stateful pagination management via proxy and offload extra load from server.

## Inserting Million Rows using Client Side Cursor :D
in python, create cursor use that cursor to insert data one by one using for loop, then commit the cursor .. finally close.. 

This will save the planning repetition (by postgres) in every req, prepare var only once, but execute multiple times.

**BEST PERFORMANCE TO BULK INSERT IS TO USE POSTGRES COPY**
