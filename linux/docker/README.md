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
curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
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