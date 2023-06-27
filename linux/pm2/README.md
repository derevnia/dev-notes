# install pm2 to another dir CentOS 9

```console
mkdir /opt/pm2daemon
chown user:user /opt/pm2daemon
dnf module install nodejs:18
npm i -g pm2
pm2 install pm2-logrotate
```

add config for every user `/home/user/.bashrc`
```console
# PM2 environment
export PM2_HOME=/opt/pm2daemon
```
