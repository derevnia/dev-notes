# Resize Swap to 8GB
```console
swapoff -a
```
```console
dd if=/dev/zero of=/swapfile bs=1M count=8192
```
```console
chmod 0600 /swapfile && mkswap /swapfile && swapon /swapfile
```

# Add to fstab `vi /etc/fstab`
```console
/swapfile none swap sw 0 0
```
