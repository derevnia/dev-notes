# davfs2

#### Install
```console
yum update
yum install epel-release
yum install davfs2
```

#### config
> https://example.com/remote.php/webdav/ LOGIN PASSWORD

```console
nano /etc/davfs2/secrets
```

#### dir
```console
mkdir /mnt/example.com
```

#### mount
```console
mount -t davfs https://example.com/remote.php/webdav/ /mnt/example.com
```

#### unmount
```console
umount /mnt/example.com
```

#### fstab

> https://example.com/remote.php/webdav/ /mnt/example.com davfs   defaults,_netdev    0   0*

```console
nano /etc/fstab
```

