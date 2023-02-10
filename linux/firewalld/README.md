# Centos add rules for ssh ip restriction

```console
# Add new zone
firewall-cmd --new-zone=sshd-access --permanent
# Add allowed ip
firewall-cmd --zone=sshd-access --add-source=127.0.0.1/32 --permanent
# Add allowed port
firewall-cmd --zone=sshd-access --add-port=22/tcp --permanent
# Reload rules
firewall-cmd --reload
# Show rules
firewall-cmd --list-all --zone=sshd-access
# Remove default ssh port
firewall-cmd --permanent --zone=public --remove-service=ssh
# Reload rules
firewall-cmd --reload
# Show all rules
firewall-cmd --list-all-zones
# Show all active zones
firewall-cmd --get-active-zones
```
## add service
```console
# show all rules
firewall-cmd --list-all
# allow 80/tcp
firewall-cmd --permanent --zone=public --add-service=http
# allow 443/tcp
firewall-cmd --permanent --zone=public --add-service=https
# apply new config
firewall-cmd --reload
```
## Block ip for example OpenVPN
```console
firewall-cmd --permanent --add-rich-rule="rule family='ipv4' source address='10.8.0.1/24' reject"
firewall-cmd --reload
```
