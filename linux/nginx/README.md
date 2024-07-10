# Install nginx from official repos CentOS 8

### Create repo file
```console
cat >> /etc/yum.repos.d/nginx.repo << 'EOL'
[nginx]
name=nginx repo
baseurl=https://nginx.org/packages/centos/$releasever/$basearch/
gpgcheck=0
enabled=1
EOL
```

### Install, start, enable autostart and add firewall rules

```console
dnf module disable nginx:*
```
```console
dnf module enable nginx:1.20
```
```console
dnf -y install nginx
```
```console
firewall-cmd --permanent --add-service=http
```
```console
firewall-cmd --permanent --add-service=https
```
```console
mkdir -p /etc/pki/tls/ && openssl dhparam -out /etc/pki/tls/dhparam.pem 4096
```
```console
systemctl enable --now nginx
```
