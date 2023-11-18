---
title: Section 14 - DB Security
created: 2023-11-15T18:51:07.418Z
modified: 2023-11-18T16:08:40.589Z
---

# Section 14 - DB Security

1. Enable TLS in Postgres Database, add certificates
2. Postgres connects via TCP 3 way handshake and then exchanges query, data.
3. It is best to have separate User Pool for db connections for different type of CRUD Operations. For example: read users can't edit the data. and in case of sql injection, Hackers will not get edit privileges.
