# Profiling

Example request `https://localhost/api/datetime?XDEBUG_TRIGGER=StartProfileForMe`

Config `/etc/php.d/15-xdebug.ini`

```console
xdebug.log = "/var/log/php/xdebug.log"
;xdebug.mode=debug
xdebug.mode = profile
xdebug.start_with_request = trigger
xdebug.output_dir = "/var/log/php"
xdebug.trigger_value=StartProfileForMe
xdebug.log_level = 10
```
