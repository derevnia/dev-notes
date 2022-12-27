# Add SSH for deploy

## Local machine

generate `id_ed25519.pub` and `id_ed25519` whenever you want

```console
ssh-keygen -t ed25519 -C "GitLab SSH key"
```

## Remote machine

1. Create if not exist file `/home/user/.ssh/authorized_keys` with attributes `600` and ownership `user`

```console
touch /home/user/.ssh/authorized_keys
chmod 600 /home/user/.ssh/authorized_keys
chown user:user /home/user/.ssh/authorized_keys
```
2. Paste your `id_ed25519.pub` content in file of `authorized_keys`

## GitLab add

Add variable `SSH_PRIVATE_KEY` in `Settings->CI/CD->Variables` with content of your `id_ed25519`
