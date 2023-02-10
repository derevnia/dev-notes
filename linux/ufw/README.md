# Commands for ubuntu ufw

## Block ip if ufw doesn't block
```console
sudo vi /etc/ufw/before.rules
# Block openvpn 
-A ufw-before-input -s 10.8.0.1/24 -j DROP
```

## Common commands
```console
# allow 22/tcp
ufw allow ssh
# allow 80/tcp
ufw allow http
# allow 443/tcp
ufw allow https
# deny http 
ufw deny http

# show rules
ufw status numbered
# delete rule
ufw delete 2
# enable
ufw enable
# disable
ufw disable
# reset to default
ufw reset
```
