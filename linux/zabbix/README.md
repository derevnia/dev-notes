# Encryption setting (PSK) Zabbix Agent

- Settings `/etc/zabbix/zabbix_agentd.conf`
```console
TLSConnect=psk
TLSAccept=psk
TLSPSKIdentity=zabbix-agent-host001
TLSPSKFile=/etc/zabbix/security/psk/zabbix_agentd.psk
```

- Create dir for certs and keys
```console
mkdir -p /etc/zabbix/security/psk
```

- Generate psk
```console
openssl rand -hex 32 2>&1 | tee /etc/zabbix/security/psk/zabbix_agentd.psk
```

- Change settings on zabbix server on Encryption tab 
Connections to host: PSK
Connections from host: PSK
PSK identity: zabbix-agent-host001
PSK: get string from zabbix_agentd.psk

- restart zabbix-agent
```console
service zabbix-agent restart
```
