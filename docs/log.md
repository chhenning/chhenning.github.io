# 2026-01-23

## postgres_fdw

tags: postgres, aws, rds


I have been using Postgres' [dblink_connect](https://www.postgresql.org/docs/current/contrib-dblink-connect.html) for the longest time.

Turns out there is possibly an even better way. Enter [postgres_fdw](https://www.postgresql.org/docs/current/postgres-fdw.html). It is an supported
extension even by AWS' RDS and it let's you query remote tables in normal sql.

An example:

```sql
CREATE EXTENSION postgres_fdw;

CREATE SERVER remote_pg
FOREIGN DATA WRAPPER postgres_fdw
OPTIONS (
    host 'other-db.abcdefg.us-east-1.rds.amazonaws.com',
    port '5432',
    dbname 'orders'
);

CREATE USER MAPPING FOR my_user
SERVER remote_pg
OPTIONS (
    user 'remote_user',
    password 'secret'
);

CREATE FOREIGN TABLE foreign_orders (
    id bigint,
    total numeric
)
SERVER remote_pg
OPTIONS (schema_name 'public', table_name 'orders');
```

# 2026-01-22

## Dynamic Programming Tutorial

tags: dynamic programming, recursion

It's always fun to revisit dynamic programming. This [tutorial](https://www.youtube.com/watch?v=66hDgWottdA) is very well made.


