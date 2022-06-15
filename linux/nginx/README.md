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
dnf module enable nginx:1.20
dnf -y install nginx

firewall-cmd --permanent --add-service=http
firewall-cmd --permanent --add-service=https

systemctl enable --now nginx
```
