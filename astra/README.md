# Добавить статичный маршрут Astra Linux 1.7
```console
// Edit file
vi /etc/network/interfaces
// add route
iface eth0 inet static
        address 172.16.0.1/22
        gateway 172.16.0.2
// check 0 - GOOD/ 1 - BAD
ifup -a --no-act ; echo "status: $?"
```

# Выдать права пользователю root и его активация
```console
sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config && systemctl restart sshd
```
```console
gpasswd -a root astra-admin && pdpl-user -i 63 root && passwd
```
