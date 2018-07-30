# chbackup
Linux rsync backup of mysql databases and local files in an external repository.

# configuration
All files must be installed in servers which contains files to be copied.
The processes work with cron, you can adjust the time if you want.
The connection between servers must be done with ssh public key auth.

Steps:
1. Copy etc files to your server. After you must have the following structure:
/etc/cron.daily/backup_daily
/etc/cron.weekly/backup.weekly
/etc/chbackup/backup.conf
/etc/chbackup/exclude-list.conf

2. Edit backup.conf file
USER=<user name in backup server>
SERVER=<IP or server name>
SERVER_DIR=<Remote directory to store files (for multiple servers)>
LOCAL_DIR=<Local directory to be backup>
MYSQL_USER=<Local user of mysql>
MYSQL_PASS=<Local mysql password>
LOGFILE=/var/log/backup.log
TOT_BKP=3 # amount of historic backups to keep on remote server

3. Assign execution permissions to backup scripts and test
shell# chmod 755 /etc/cron.daily/backup_daily
shell# chmod 755 /etc/cron.weekly/backup.weekly
shell# /etc/cron.daily/backup_daily

4. Check log file
shell# tail -f /var/log/backup.log


