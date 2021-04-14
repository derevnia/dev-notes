# Encryption setting (PSK) Zabbix Agent

- Settings `/etc/zabbix/zabbix_agentd.conf`
```console
TLSConnect=psk
TLSAccept=psk
TLSPSKIdentity=zabbix-agent-host001
TLSPSKFile=/etc/zabbix/security/psk/zabbix_agentd.psk
```

- Create dir for certs and keys
```console
mkdir -p /etc/zabbix/security/psk
```

- Generate psk
```console
openssl rand -hex 32 2>&1 | tee /etc/zabbix/security/psk/zabbix_agentd.psk
```

- Change settings on zabbix server on Encryption tab 
Connections to host: PSK
Connections from host: PSK
PSK identity: zabbix-agent-host001
PSK: get string from zabbix_agentd.psk

- restart zabbix-agent
```console
service zabbix-agent restart
```

# Setup Postgresql
main instruction https://www.zabbix.com/ru/integrations/postgresql or https://git.zabbix.com/projects/ZBX/repos/zabbix/browse/templates/db/postgresql

1. Install Zabbix agent and create a read-only `zbx_monitor` user with proper access to your PostgreSQL server.

    For PostgreSQL version 10 and above:

    ```sql
    CREATE USER zbx_monitor WITH PASSWORD '<PASSWORD>' INHERIT;
    GRANT pg_monitor TO zbx_monitor;
    ```

    For PostgreSQL version 9.6 and below:

    ```sql
    CREATE USER zbx_monitor WITH PASSWORD '<PASSWORD>';
    GRANT SELECT ON pg_stat_database TO zbx_monitor;

    -- To collect WAL metrics, the user must have a `superuser` role.
    ALTER USER zbx_monitor WITH SUPERUSER;
    ```

2. Copy `postgresql/` to Zabbix agent home directory `/var/lib/zabbix/`. The `postgresql/` directory contains the files needed to obtain metrics from PostgreSQL.

3. Copy `template_db_postgresql.conf` to Zabbix agent configuration directory `/etc/zabbix/zabbix_agentd.d/` and restart Zabbix agent service.

4. Edit `pg_hba.conf` to allow connections from Zabbix agent https://www.postgresql.org/docs/current/auth-pg-hba-conf.html.

    Add rows (for example):

    ```bash
    host all zbx_monitor 127.0.0.1/32 trust
    host all zbx_monitor 0.0.0.0/0 md5
    host all zbx_monitor ::0/0 md5
    ```

- if everything works but status of postgresql is down
```console
ln -s /usr/pgsql-12/bin/pg_isready /usr/bin/pg_isready
```

# Setup nginx
main instruction https://git.zabbix.com/projects/ZBX/repos/zabbix/browse/templates/app/nginx_agent

Setup [ngx_http_stub_status_module](https://nginx.ru/en/docs/http/ngx_http_stub_status_module.html).
Test availability of http_stub_status module with `nginx -V 2>&1 | grep -o with-http_stub_status_module`.

- Example configuration of Nginx:
```text
location = /basic_status {
    stub_status;
    allow 127.0.0.1;
    allow ::1;
    deny all;
}
```

- change macros on zabbix server for this host
```text
{$NGINX.STUB_STATUS.HOST} with value 127.0.0.1
```
