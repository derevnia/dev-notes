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


# Добавление репозиториев Astra Linux 1.8
```console
tee /etc/apt/sources.list.d/astra_1.8.list > /dev/null <<EOL
# Основной репозиторий, включающий актуальное оперативное или срочное обновление
deb https://dl.astralinux.ru/astra/stable/1.8_x86-64/main-repository/     1.8_x86-64 main contrib non-free non-free-firmware
# Расширенный репозиторий, соответствующий актуальному оперативному обновлению
deb https://dl.astralinux.ru/astra/stable/1.8_x86-64/extended-repository/ 1.8_x86-64 main contrib non-free non-free-firmware
EOL
```

# Закомментировать все стандартные репозитории Astra Linux
```console
sed -i '/^[^#]/ s/^/#/' /etc/apt/sources.list
```

# Обновление с 1.7 до 1.8
```console
apt-get dist-upgrade
```

# Fix error `/usr/bin/perl: error while loading shared libraries: libcrypt.so.1: cannot open shared object file: No such file or directory`
```console
apt -y install zstd # using gzip instead
cd $(mktemp -d) && apt -y download libcrypt1 && dpkg-deb -x libcrypt1_*.deb . && cp -ra lib/* /lib/ && apt -y --fix-broken install
```
