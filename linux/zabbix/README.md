# Install Zabbix 6 CentOS Stream 8

```console
rpm -Uvh https://repo.zabbix.com/zabbix/6.0/rhel/8/x86_64/zabbix-release-6.0-1.el8.noarch.rpm
dnf clean all
dnf -y install zabbix-agent
```


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

- Change permissions
```console
chown zabbix:zabbix /etc/zabbix/security/psk/zabbix_agentd.psk 
chmod 600 /etc/zabbix/security/psk/zabbix_agentd.psk
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

- if get error like this `psql: error: /var/lib/zabbix/postgresql/pgsql.wal.stat.sql: Permission denied`
switch off selinux in /etc/selinux/config and reboot

# Setup nginx
main instruction https://git.zabbix.com/projects/ZBX/repos/zabbix/browse/templates/app/nginx_agent

Setup [ngx_http_stub_status_module](https://nginx.ru/en/docs/http/ngx_http_stub_status_module.html).
Test availability of http_stub_status module with `nginx -V 2>&1 | grep -o with-http_stub_status_module`.

- Example configuration of Nginx:
```console
#redirect !https
server {
  server_name  _ localhost;

  listen 80 default_server;
  listen [::]:80 default_server;
  # Apache
  location /server-status {
    proxy_pass http://127.0.0.1:8080;
    allow 127.0.0.1;
    allow ::1;
    deny all;
  }
  # Nginx
  location /basic_status {
    stub_status;
    allow 127.0.0.1;
    allow ::1;
    deny all;
  }
  # Enable php-fpm status page
  location ~ ^/(status|ping)$ {
    allow 127.0.0.1;
    allow ::1;
    deny all;
    
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    fastcgi_index index.php;
    include fastcgi_params;
    fastcgi_pass unix:/run/php-fpm/www.sock;
  }
}
```

- change macros on zabbix server for this host
```text
{$NGINX.STUB_STATUS.HOST} with value 127.0.0.1
```

# Setup fail2ban
main instruction [zabbix](https://share.zabbix.com/cat-app/firewall/fail2ban) [github](https://github.com/hermanekt/zabbix-fail2ban-discovery-)

## Install fail2ban CentOS 9
```console
dnf install -y epel-release
dnf install -y fail2ban
systemctl enable --now fail2ban
```
## Install template on Zabbix server

## Create template for fail2ban on Zabbix agent
/etc/zabbix/zabbix_agentd.d/template_fail2ban.conf
```console
tee /etc/zabbix/zabbix_agentd.d/template_fail2ban.conf<<EOF  
UserParameter=fail2ban.status[*],fail2ban-client status '$1' | grep 'Currently banned:' | grep -E -o '[0-9]+'
UserParameter=fail2ban.discovery,fail2ban-client status | grep 'Jail list:' | sed -e 's/^.*:\W\+//' -e 's/\(\(\w\|-\)\+\)/{"{#JAIL}":"\1"}/g' -e 's/.*/{"data":[\0]}/'
EOF
```

## Install in file /etc/sudoers add string (after «root ALL=(ALL:ALL) ALL»):
```text
zabbix  ALL=(ALL) NOPASSWD: /usr/bin/fail2ban-client
```

## Restart Zabbix agent
```console
systemctl restart zabbix-agent
```

## Create triggers if you needed

## Selinux rules probably must be updated
```console
ausearch -c 'fail2ban-client' --raw | audit2allow -M my-fail2banclient
semodule -X 300 -i my-fail2banclient.pp
```

## Zabbix Agent 2
[source](https://github.com/hermanekt/zabbix-fail2ban-discovery-)

## Grant access to Fail2Ban
Fail2ban works only with root by default. We need to grant permission to Zabbix to access the Fail2ban by adding this 2 lines to /etc/sudoers:
```txt
zabbix ALL=NOPASSWD: /usr/bin/fail2ban-client status
zabbix ALL=NOPASSWD: /usr/bin/fail2ban-client status *
```
## Edit /etc/zabbix/zabbix_agent2.conf to avoid permission error
# Include configuration files for plugins
Include=/etc/zabbix/zabbix_agent2.d/plugins.d/*.conf

## Restart
```console
systemctl restart zabbix-agent2
```

## Check
```console
sudo -u zabbix zabbix_agent2 -c /etc/zabbix/zabbix_agent2.conf -t fail2ban.status['sshd']
```
