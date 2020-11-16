# install postgresql 12 (CentOS 7)

### repo
```console
yum -y install https://download.postgresql.org/pub/repos/yum/reporpms/EL-7-x86_64/pgdg-redhat-repo-latest.noarch.rpm
```

### install
```console
yum -y install epel-release yum-utils
yum-config-manager --enable pgdg12
yum install postgresql12-server postgresql12
```
### init
```console
/usr/pgsql-12/bin/postgresql-12-setup initdb
```

### autostart
```console
systemctl enable --now postgresql-12
```

### start
```console
systemctl start postgresql-12
```

# migrate PostgreSQL 12 to PostgreSQL 13 with new installation

### repo
```console
yum -y install https://download.postgresql.org/pub/repos/yum/reporpms/EL-7-x86_64/pgdg-redhat-repo-latest.noarch.rpm
```

### install
```console
yum -y install epel-release yum-utils
yum-config-manager --enable pgdg13
yum install postgresql13-server postgresql13
```
### init
```console
/usr/pgsql-13/bin/postgresql-13-setup initdb
```

### autostart
```console
systemctl enable --now postgresql-13
```

### settings
check settings and copy if you need from 12 to 13
```console
diff /var/lib/pgsql/12/data/postgresql.conf /var/lib/pgsql/13/data/postgresql.conf
diff /var/lib/pgsql/12/data/pg_hba.conf /var/lib/pgsql/13/data/pg_hba.conf
```

### extensions
when migrate you can get this error for example pg_trgm extension if used install

>ERROR: could not open extension control file ".../extension/pg_trgm.control": No such file or directory"

```console
yum install postgresql13-contrib
```

```sql
# if you want install new extension do not forget to create it
CREATE EXTENSION pg_trgm;
```

### check before migrate
```console
su postgres
cd /tmp #error could not open log file "pg_upgrade_internal.log": Permission denied
```

```console
/usr/pgsql-13/bin/pg_upgrade \
  --old-datadir=/var/lib/pgsql/12/data \
  --new-datadir=/var/lib/pgsql/13/data \
  --old-bindir=/usr/pgsql-12/bin \
  --new-bindir=/usr/pgsql-13/bin \
  --check
```

### stop services
```console
systemctl stop postgresql-12
systemctl stop postgresql-13
```

### migrate

```console
su postgres
cd /tmp
/usr/pgsql-13/bin/pg_upgrade \
  --old-datadir=/var/lib/pgsql/12/data \
  --new-datadir=/var/lib/pgsql/13/data \
  --old-bindir=/usr/pgsql-12/bin \
  --new-bindir=/usr/pgsql-13/bin 
```

### check after migrate
```console
./analyze_new_cluster.sh
```

### start
```console
systemctl start postgresql-13
```

### remove old
```console
systemctl disable postgresql12
yum autoremove postgresql12
su postgres
cd /tmp
./delete_old_cluster.sh
```

