# install pm2 to another dir CentOS 9

```console
mkdir /opt/pm2daemon
chown user:user /opt/pm2daemon
dnf module install nodejs:18 # or
dnf module install nodejs:20 # or
npm i -g pm2
pm2 install pm2-logrotate
pm2 startup
```

add config for every user `/home/user/.bashrc`
```console
# PM2 environment
export PM2_HOME=/opt/pm2daemon
```

```console
su user
pm2 startup
usermod -a -G pm2 user
chmod 664 /opt/pm2daemon/pm2.log
```
# With proxy issue
```console
npm config set proxy http://login:password@172.16.0.1:80
```
# optional for history config
```console
# Files sizes
HISTSIZE=500000
HISTFILESIZE=10000000
# Save immediately
export PROMPT_COMMAND="history -a; history -c; history -r; $PROMPT_COMMAND"
# Add time for every command
export HISTTIMEFORMAT='%F %T '
```

# example pm2 start services
```console
pm2 start ecosystem.config.js --env production --only Webscoket
```
