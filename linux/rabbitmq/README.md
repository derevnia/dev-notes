# Install RabbitMQ CentOS 9

- copy [a relative link](rabbitmq-centos9.repo) to `/etc/yum.repos.d/rabbitmq.repo`
```cli
dnf -y install rabbitmq-server
rabbitmq-plugins enable rabbitmq_management # enabling ui plugin
systemctl enable --now rabbitmq-server
```
Default credentials: guest/guest

# Install RabbitMQ CentOS 8
- copy [a relative link](rabbitmq-centos8.repo) to `/etc/yum.repos.d/rabbitmq.repo`
```cli
update-crypto-policies --set LEGACY # to avoid error with sha1
dnf -y install rabbitmq-server
update-crypto-policies --set DEFAULT # return back
rabbitmq-plugins enable rabbitmq_management # enabling ui plugin
systemctl enable --now rabbitmq-server
```
Default credentials: guest/guest

## Add/Update user
```cli
rabbitmqctl add_user Admin YOUR_SUPER_SECRET_PASS
rabbitmqctl set_user_tags Admin administrator
```
