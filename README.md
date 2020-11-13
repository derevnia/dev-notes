# New server CentOS 7 install commdands

```console
nginx+apache+mariadb+postgresql+git+zabbix
```

##### update (if it possible)
```console
yum update
```

##### Zabbix (agent)
```console
yum install zabbix-agent 
firewall-cmd --permanent --zone=public --add-service=zabbix-agent 
firewall-cmd --reload 
systemctl restart zabbix-agent
```

##### nginx
```console
yum install nginx 
systemctl enable nginx
```
##### nginx (nginx repo)
```console
tee /etc/yum.repos.d/nginx.repo<<EOF  
[nginx-stable]
name=nginx stable repo
baseurl=http://nginx.org/packages/centos/$releasever/$basearch/
gpgcheck=1
enabled=1
gpgkey=https://nginx.org/keys/nginx_signing.key
module_hotfixes=true

[nginx-mainline]
name=nginx mainline repo
baseurl=http://nginx.org/packages/mainline/centos/$releasever/$basearch/
gpgcheck=1
enabled=0
gpgkey=https://nginx.org/keys/nginx_signing.key
module_hotfixes=true
EOF
```

##### apache + itk
```console
yum install httpd 
yum install httpd-itk 
systemctl enable httpd
```
##### User IP from nginx to apache (REMOTE_ADDR)
create file /etc/httpd/conf.d/mod_remotip.conf
```console
RemoteIPHeader X-Real-IP
RemoteIPInternalProxy 127.0.0.1
```
##### open ports for nginx
```console
firewall-cmd --list-all 
firewall-cmd --permanent --add-service=http 
firewall-cmd --permanent --add-service=https 
firewall-cmd --reload
```
##### start nginx proxy + apache
change apache port to 8080 
configs
/etc/httpd/conf/httpd.conf 
/etc/nginx/nginx.conf

```console
service nginx start 
service httpd start
```
##### MariaDB
add repo
```console
tee /etc/yum.repos.d/MariaDB.repo<<EOF  
[mariadb]
name = MariaDB 
baseurl = http://yum.mariadb.org/10.4/centos7-amd64 
gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB 
gpgcheck=1 
EOF
```
install
```console
yum makecache fast
yum -y install MariaDB-server MariaDB-client
mysql -V
systemctl enable --now mariadb 
mysql_secure_installation
```
settings
```console
CREATE DATABASE IF NOT EXISTS NEW_DATABASE_NAME; 
CREATE USER 'NEW_USER_FOR_DATABASE'@'localhost' IDENTIFIED BY 'NEW_PASSWORD'; 
GRANT ALL PRIVILEGES ON NEW_DATABASE_NAME.* TO 'NEW_USER_FOR_DATABASE'@'localhost'; 
FLUSH PRIVILEGES;
```

##### postgresql 12
```console
yum update postgresql-12
yum -y install epel-release yum-utils
yum-config-manager --enable pgdg12
yum install postgresql12-server postgresql12
/usr/pgsql-12/bin/postgresql-12-setup initdb
systemctl enable --now postgresql-12
systemctl status postgresql-12
```

##### php
```console
yum install epel-release 
yum install yum-utils 
yum install http://rpms.remirepo.net/enterprise/remi-release-7.rpm 
yum-config-manager --enable remi-php74 
yum install php 
yum install php-xdebug php-calendar php-dom php-ftp php-iconv php-mbstring php-phar php-sockets php-xml php-mysqli php-wddx php-mysql php-bcmath php-ctype php-exif php-gd php-json php-mysqlnd php-simplexml php-sqlite3 php-xmlwriter php-pdo_mysql php-xmlreader php-bz2 php-curl php-fileinfo php-gettext php-ldap php-pdo php-soap php-tokenizer php-xsl php-pdo_sqlite php-zip php-opcache
```
###### phalcon4 (remirepo)
```console
yum install php-phalcon4
```

##### add user and usergroup
```console
adduser www-root 
passwd www-root 
groupadd www-root 
usermod -a -G www-root,apache www-root
```
##### New folder for website
```console
mkdir /var/www/html/newsite.com 
chown -R www-root:www-root /var/www/html/newsite.com/
```
##### Disable selinux
/etc/selinux/config
for permanent (not recommend) change to SELINUX=disabled and reboot
```console
reboot
```
##### nmap and hostname
```console
yum install nmap
nmap -v -sU newsite.com -p 80 
nmap -v -sT newsite.com -p 80 
hostname
```
edit file /etc/hosts
add ip hostname site_name
10.0.0.1 newsite.com.local newsite.com

##### git
add repo
```console
cat > /etc/yum.repos.d/WANdisco-git.repo << EOF 
[WANdisco-git] 
name=WANdisco Git 
baseurl=http://opensource.wandisco.com/rhel/\$releasever/git/\$basearch 
gpgcheck=1 
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-WANdisco 
EOF
```
install
```console
rpm --import http://opensource.wandisco.com/RPM-GPG-KEY-WANdisco 
yum install git 
git status
```