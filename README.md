# ChBackup
Linux rsync incremental backup of mysql and postgresql databases and local files in an external repository. It also can backup dokku repositories and databases.
Default daily and weekly backups.

## Prerequisites
All files must be installed in servers which contains files to be copied.
The processes work with cron, you can adjust the time if you want.
The connection between servers must be done with ssh public key auth.
Connection with remote server MUST be done with ssh public/private key.

For postgresql backup you need to give access to replicate and access for user postgres, like this
```
local   all             postgres        trust
local   replication     postgres        trust
```

## Installing
1. Copy etc files to your server. After you must have the following structure:
```
/etc/cron.daily/backup_daily
/etc/cron.weekly/backup.weekly
/etc/chbackup/backup.conf
/etc/chbackup/exclude-list.conf
/etc/logrotate.d/backup-log
```

2. Edit backup.conf file
```
USER=<user name in backup server>
SERVER=<IP or server name>
SERVER_DIR=<Remote directory to store files (for multiple servers)>
LOCAL_DIR=<Local directory to backup>
DO_MYSQL_BACKUP=1 # do mysql backup?
DO_PGSQL_BACKUP=0 # do postgresql backup?
DO_DOKKU_DB_BACKUP=0 # do dokku db backup?
LOGFILE=/var/log/backup.log
TOT_BKP=3 # amount of historic backups to keep on remote server
DELETE_MISSING=1 # remove locally deleted files from backup server
```

3. If mysql backup edit mysql.conf with local user/pass
[client]
user=
password=

4. Assign execution permissions to backup scripts and test
```
shell# chmod 755 /etc/cron.daily/backup_daily
shell# chmod 755 /etc/cron.weekly/backup.weekly
shell# /etc/cron.daily/backup_daily
```

5. Check log file
```
shell# tail -f /var/log/backup.log
```

6. BD permissions
BD must have permissions for full reading
```
grant select, lock tables, event, show view, process on *.* to backup@localhost;
```

