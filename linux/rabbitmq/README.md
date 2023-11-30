# Install RabbitMQ CentOS 9

- copy [a relative link](rabbitmq.repo) to `/etc/yum.repos.d/rabbitmq.repo`
```cli
update-crypto-policies --set LEGACY # to avoid error with sha1
dnf install rabbitmq-server
update-crypto-policies --set DEFAULT # return back
rabbitmq-plugins enable rabbitmq_management # enabling ui plugin
systemctl enable --now rabbitmq-server
```
Default creadentials: guest/guest

## Add/Update user
```cli
rabbitmqctl add_user Admin YOUR_SUPER_SECRET_PASS
rabbitmqctl set_user_tags Admin administrator
```
