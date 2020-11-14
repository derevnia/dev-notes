# davfs2

Install
```console
[root@localhost]# yum update
[root@localhost]# yum install epel-release
[root@localhost]# yum install davfs2
```

config
add to file
**https://example.com/remote.php/webdav/ LOGIN PASSWORD**
```console
nano /etc/davfs2/secrets
```

dir
```console
mkdir /mnt/example.com
```

mount
```console
mount -t davfs https://example.com/remote.php/webdav/ /mnt/example.com
```

unmount
```console
umount /mnt/example.com
```

add to fstab

```console
https://example.com/remote.php/webdav/ /mnt/example.com davfs   defaults,_netdev    0   0
```

```console
nano /etc/fstab
```

