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
firewall-cmd --permanent --zone=public --remove-ssh=ssh
# Reload rules
firewall-cmd --reload
# Show all rules
firewall-cmd --list-all-zones
```
