# DB Project

## Redis and PostgreSql

- `docker exec mobydock_postgres_1 createdb -U postgres mobydock`
- `docker exec mobydock_postgres_1 psql -U postgres -c "CREATE USER mobydock WITH PASSWORD 'yourpassword'; GRANT ALL PRIVILEGES ON DATABASE mobydoc to mobydock;"`
