# Basic linux commands (CentOS 7)

Copy file from another machine
```console
scp root@10.0.0.1:/backup/common/backup.tar.gz /tmp
```

Extract tar.gz
```console
tar -zxvf /tmp/backup.tar.gz
```

Extract gz
```console
gunzip backup.gz 
```

remove folder with files **⚠️be careful⚠️**
```console
rm -rfv /var/www/html/var/
```

copy files from folder `source` to `destination` folder
```console
cp -R /source/* /destination
```

create absolute symlink
```console
ln -s /souce/full/path/folder /link/full/path/new_folder
```
create relative symlink
```console
cd /full/path/folder 
ln -s ../path/source link_folder
```

Add time and increase file size `.bash_history` and save commands immediately edit file `.bashrc` for every user
```console
# Files sizes
HISTSIZE=500000
HISTFILESIZE=10000000
# Save immediately
export PROMPT_COMMAND="history -a; history -c; history -r; $PROMPT_COMMAND"
# Add time for every command
export HISTTIMEFORMAT='%F %T '
```
