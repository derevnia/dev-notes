# Добавить статичный маршрут Astra Linux 1.7
```console
// Edit file
vi /etc/network/interfaces
// add route
iface eth0 inet static
        address 172.16.0.1/22
        gateway 172.16.0.2
        up ip route add 172.16.1.0/24 via 172.16.40.1
	up ip route add 172.16.3.0/22 via 172.16.40.1
// check 0 - GOOD/ 1 - BAD
ifup -a --no-act ; echo "status: $?"
```

## Apply & restart interface
```console
ifdown eth0 && sudo ifup eth0
```

# Выдать права пользователю root и его активация
```console
sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config && systemctl restart sshd
```
```console
gpasswd -a root astra-admin && pdpl-user -i 63 root && passwd
```


# Добавление репозиториев Astra Linux 1.8
```console
tee /etc/apt/sources.list.d/astra_1.8.list > /dev/null <<EOL
# Основной репозиторий, включающий актуальное оперативное или срочное обновление
deb https://dl.astralinux.ru/astra/stable/1.8_x86-64/main-repository/     1.8_x86-64 main contrib non-free non-free-firmware
# Расширенный репозиторий, соответствующий актуальному оперативному обновлению
deb https://dl.astralinux.ru/astra/stable/1.8_x86-64/extended-repository/ 1.8_x86-64 main contrib non-free non-free-firmware
EOL
```

# Закомментировать все стандартные репозитории Astra Linux
```console
sed -i '/^[^#]/ s/^/#/' /etc/apt/sources.list
```

# Обновление с 1.7 до 1.8
```console
apt-get dist-upgrade
```

# Установка postgres pro 16
```console
wget https://repo.postgrespro.ru/std-16/keys/pgpro-repo-add.sh && sh pgpro-repo-add.sh
/opt/pgpro/std-16/bin/pg-setup initdb && /opt/pgpro/std-16/bin/pg-setup service enable && /opt/pgpro/std-16/bin/pg-setup service start
ln -s /opt/pgpro/std-16/bin/pg_config /usr/bin/pg_config
# Дополнительные пакеты
apt-get install postgrespro-std-16-contrib
```

# osm
```console
cd /home/osm && wget https://planet.openstreetmap.org/pbf/planet-latest.osm.pbf
apt install debian-archive-keyring
apt install sudo screen locate libapache2-mod-tile renderd git tar unzip wget bzip2 apache2 lua5.1 mapnik-utils python3-mapnik python3-psycopg2 python3-yaml gdal-bin node-carto osm2pgsql net-tools curl
```

# postgis postgrepro 16
```console
wget https://postgis.net/stuff/postgis-3.4.3dev.tar.gz && tar -xvzf postgis-3.4.3dev.tar.gz && cd postgis-3.4.3dev
apt-get install postgrespro-std-16-dev build-essential libgeos-dev libproj-dev gdal-bin libgdal-dev
./configure --without-protobuf
```

## Setup osm db
```console
sudo -u postgres -i
createuser _renderd
createdb -E UTF8 -O _renderd gis
psql
\c gis
CREATE EXTENSION postgis;
CREATE EXTENSION hstore;
ALTER TABLE geometry_columns OWNER TO _renderd;
ALTER TABLE spatial_ref_sys OWNER TO _renderd;
exit
```

## Download openstreetmap-carto
```console
mkdir ~/src
cd ~/src
git clone https://github.com/gravitystorm/openstreetmap-carto
cd openstreetmap-carto
```

## import data
```console
osm2pgsql -U _renderd --slim -H /tmp -d gis --hstore --multi-geometry --number-processes 32 --tag-transform-script /home/osm/src/openstreetmap-carto-master/openstreetmap-carto.lua  --style /home/osm/src/openstreetmap-carto-master/openstreetmap-carto.style -C 200000 /home/osm/planet-latest.osm.pbf
```

## Generate xml
```console
carto project.mml > mapnik.xml
```

## Create indexes
```console
cd ~/src/openstreetmap-carto/
sudo -u _renderd psql -d gis -f indexes.sql
```

## Get additional data
```console
cd ~/src/openstreetmap-carto/
mkdir data
sudo chown _renderd data
sudo -u _renderd scripts/get-external-data.py
```

## Get fonts
```console
cd ~/src/openstreetmap-carto/
scripts/get-fonts.sh
```

## get renderd
```console
cd /etc/apache2/conf-available/
sudo wget https://raw.githubusercontent.com/openstreetmap/mod_tile/python-implementation/etc/apache2/renderd.conf
sudo a2enconf renderd
sudo systemctl reload apache2
mkdir /var/lib/mod_tile && chown _renderd /var/lib/mod_tile && /etc/init.d/renderd restart && /etc/init.d/apache2 restart
```

## osm render cli
```console
render_list -a -f -n 4 -m osm -z 0 -Z 14
```

## osm disable ttl
```console
touch /var/cache/renderd/tiles/planet-import-complete
chown _renderd:_renderd /var/cache/renderd/tiles/planet-import-complete 
```

## Дополнительные пакеты postgrespro
apt-get install postgrespro-std-16-contrib
```

# Fix error `/usr/bin/perl: error while loading shared libraries: libcrypt.so.1: cannot open shared object file: No such file or directory`
```console
apt -y install zstd # using gzip instead
cd $(mktemp -d) && apt -y download libcrypt1 && dpkg-deb -x libcrypt1_*.deb . && cp -ra lib/* /lib/ && apt -y --fix-broken install
```
