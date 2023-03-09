# Create custom service for systemd

```console
touch /etc/systemd/system/custom.service
```

## Example file
```txt
[Unit]
Description=Start DBGp Proxy for php Xdebug
After=network.target

[Service]
Type=simple
ExecStart=/root/dbgpProxy -s 127.0.0.1:9003 -i 127.0.0.1:9000 >> /var/log/dbgp_proxy.log
TimeoutStartSec=0
SyslogIdentifier=dbgp_proxy

Restart=always
RestartSec=120

[Install]
WantedBy=default.target
```

## Start service
```console
systemctl enable --now allure
```

## Reload config if required
```console
systemctl daemon-reload
```
