---
title: Postgres SQL Tips
attachments: [grades.sql, students.sql]
created: 2023-10-25T05:38:34.804Z
modified: 2023-10-31T09:23:21.551Z
---

# Postgres SQL Tips

### Quickly run a postgres db.
```sh
docker run --name pgtest -e POSTGRES_PASSWORD=pass@123 -d postgres
docker exec -it pgtest psql -U postgres
```

### Dummy Employees Data
```sql
CREATE TABLE public.employees (
    id int8 NOT NULL,
    name varchar(120) NOT NULL,
    salary int8 NOT NULL,
    CONSTRAINT emp_pk PRIMARY KEY (id)
);

WITH salary_list AS (
    SELECT '{1000, 2000, 5000}'::INT[] salary
)
INSERT INTO public.employees
(id, name, salary)
SELECT n, 'Employee ' || n as name, salary[1 + mod(n, array_length(salary, 1))]
FROM salary_list, generate_series(1, 1000000) as n

```

### Quick Table Info
```sql
\d table_name;
```

### Explain Command
Only gives postgres planning information only. Overview only.
- **cost**'s lower limit shows how long it took to query first record. 
- **cost**'s upper limit shows how long it took to complete the query.
- width - sum of all bytes of all columns


```sql
explain select * from employees;
```

Some good examples: 
1. Sorting - will show parallel seq scan and Sort part costing.
2. Only Primary key fetching - uses inline query to fetch from index only, optimized.


### Explain Analyze Command
It gives details regarding the flow in which sql query is executed

```sql
explain analyze select id from employees where id = 3000;
```

### Buffers
Used to check if query is served from buffer
```sql
explain (analyze, buffers) from employees where id = 3000;
```

### Table Sizes
```sql
select pg_relation_size(oid), relname from pg_class order by pg_relation_size(oid) desc;
```
Sizes are in bytes


### Some Trivia Points
1. Sometimes, Some part of actual query results can be served from the disk. (eg:  picking 60% from disk and 40% from cache considering only 40% were available in cache)
2. If heap fetches with buffers is 0, It means we didn't go to the disk and fetched from most probably Index only. 
3. With Only Index, we still see some read in the output. Reason: we still have to fetch index from the disk. :P
