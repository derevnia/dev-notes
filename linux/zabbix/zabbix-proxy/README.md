# Install zabbix-proxy 6 on Ubuntu 20.04

## Install repos postgresql
```console
sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
```

## Install repos postgresql
```console
wget http://repo.zabbix.com/zabbix/6.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.0-1+ubuntu$(lsb_release -rs)_all.deb
```

## Install software
```console
apt -y update
apt -y install postgresql-14 zabbix-proxy-pgsql zabbix-sql-scripts
systemctl enable --now postgresql
```

## Edit settings and setup zabbix database
```console
su postgres
createuser zabbix --interactive --pwprompt
psql
create database zabbix_proxy with owner zabbix;
GRANT ALL PRIVILEGES ON DATABASE zabbix_proxy to zabbix;
cat /usr/share/doc/zabbix-sql-scripts/postgresql/proxy.sql | psql -U zabbix -d zabbix_proxy
\q
exit
```

## Edit zabbix proxy settings
```console
vi /etc/zabbix/zabbix_proxy.conf
```
## Create psk settings (optional)
```console
mkdir -p /etc/zabbix/security/psk
openssl rand -hex 32 2>&1 | tee /etc/zabbix/security/psk/zabbix_proxy.psk
chown zabbix:zabbix /etc/zabbix/security/psk/zabbix_proxy.psk 
chmod 600 /etc/zabbix/security/psk/zabbix_proxy.psk
```

## Start zabbix-proxy service
```console
systemctl enable --now postgresql
```
