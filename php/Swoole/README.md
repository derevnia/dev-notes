# install swoole (CentOS 8)

### install dependencies
(if u need curl install curl-devel)
```console
dnf install php-devel php-pear curl-devel
```

### install swoole
(everywhere yes instead mysqlnd)
```console
pecl install swoole
```

### enable
(create file /etc/php.d/99-swoole.ini with content)
```console
# manually installed from https://www.swoole.co.uk/docs/get-started/installation
extension=swoole
```

### restart apache
(create file /etc/php.d/99-swoole.ini with content)
```console
httpd -t
service httpd restart
```
