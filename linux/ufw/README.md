# Commands for ubuntu ufw

## Block ip if ufw doesn't block
```console
sudo vi /etc/ufw/before.rules
# Block openvpn 
-A ufw-before-input -s 10.8.0.1/24 -j DROP
```
