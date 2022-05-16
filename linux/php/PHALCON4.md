# install phalcon 4 with php 7.4 Centos Stream 8
```console
dnf install -y epel-release
dnf -y install dnf-utils
dnf install http://rpms.remirepo.net/enterprise/remi-release-8.rpm
dnf module install php:remi-7.4
dnf install -y php-curl php-gettext php-json php-mbstring php-pgsql php-fileinfo php-openssl php-opcache php-phalcon
```
