# Replication PostgreSQL

```console
su postgres
psql
CREATE ROLE replication_user WITH LOGIN REPLICATION PASSWORD 'secure_password';
GRANT SELECT ON ALL TABLES IN SCHEMA public TO replication_user;
```

## Edit `pg_hba.conf`

```console
host    replication     replication_user     your_ip/32        md5
```

## Apply changes

```console
SELECT pg_reload_conf();
# or
pg_ctl reload
```
