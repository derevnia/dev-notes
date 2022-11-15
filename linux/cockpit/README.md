```console
# install
dnf install cockpit
# change port (optional)
systemctl edit cockpit.socket
# paste this
[Socket]
ListenStream=

# apply settings if (optional)
systemctl daemon-reload

# restart
systemctl restart cockpit.socket

# disable selinux (optional)
setenforce 0
vi /etc/selinux/config

# install storage
dnf install cockpit-storaged
```
