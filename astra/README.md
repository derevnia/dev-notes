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
