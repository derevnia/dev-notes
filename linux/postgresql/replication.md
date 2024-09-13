# Replication PostgreSQL

```console
su postgres
psql
CREATE ROLE replication_user WITH LOGIN REPLICATION PASSWORD 'secure_password';
GRANT SELECT ON ALL TABLES IN SCHEMA public TO replication_user;
```
