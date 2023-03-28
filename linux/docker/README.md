# Install Docker Centos 8

## Installing
```console
dnf config-manager --add-repo=https://download.docker.com/linux/centos/docker-ce.repo
dnf install docker-ce docker-ce-cli containerd.io
systemctl enable --now docker
```

## Network
```console
firewall-cmd --zone=public --add-masquerade --permanent
firewall-cmd --reload
```

## Install docker-compose
```console
curl -L "https://github.com/docker/compose/releases/download/v2.6.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```

## Checking (optional)
```console
docker run hello-world
docker ps -a
docker-compose --version
```

## Privilegies
```console
usermod -aG docker user
```

## Set proxy for docker

### Create file
```console
mkdir /etc/systemd/system/docker.service.d
touch /etc/systemd/system/docker.service.d/http-proxy.conf
```

### Add to `http-proxy.conf`
```txt
[Service]
Environment="HTTP_PROXY=http://username:password@proxy_url:proxy_port"
Environment="HTTPS_PROXY=http://username:password@proxy_url:proxy_port"
Environment="NO_PROXY=127.0.0.1,localhost"
```

### Reload daemon && restart docker for new Environment && check new Environment
```console
sudo systemctl daemon-reload
sudo systemctl restart docker
sudo systemctl show docker --property Environment
```
