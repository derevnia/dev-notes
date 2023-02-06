# Install dnsmasq CentOS 8-9

```console
dnf install dnsmasq
touch /etc/dnsmasq.hosts
systemctl enable --now dnsmasq

# (optional) add `nameserver 127.0.0.1` to
vi /etc/resolv.conf
# add server to nmtui

dnsmasq --test # check config 
service dnsmasq restart # apply changes in dnsmasq.hosts
```

## Copy [dnsmasq.conf](dnsmasq.conf) to `/etc/dnsmasq.d/dnsmasq.conf`
