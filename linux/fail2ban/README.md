# Basic installation fail2ban on CentOS 8 and Ubuntu 20.04

## CentOS 8
```console
dnf install epel-release fail2ban -y
```

## Ubuntu 20.04
```console
apt-get install fail2ban
```

## Create basic config
```console
tee /etc/fail2ban/jail.local<<EOF  
[sshd]
enabled = true
maxretry = 3
EOF
```

## Enable autostart and start deamon
```console
systemctl enable --now fail2ban
```

# Unban ip
```console
fail2ban-client -i
status sshd
set sshd unbanip 127.0.0.1
```
