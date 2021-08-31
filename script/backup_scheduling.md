# Backup with shell script

create .my.cnf in your home folder
```
nano .my.cnf
```

paste this script and customize according to your need
```
[mysqldump]
host=your_ip_server
user=your_username
password=your_password
```

create filename with extension .sh
```
#!/bin/bash

targetdir=/path/to/backup/folder
today=$(date +%Y%m%d)

## send log to syslog
exec 1> >(logger -s -t $(basename $0)) 2>&1
echo $today

mkdir -p $targetdir/$today

## remove folder older than 30 days
find $targetdir/* -mtime +30 -exec rm -rf {} \;

## backup databasename
start_time="$(date -u +%s)"
mysqldump -h ip_address --single-transaction --skip-lock-tables -u username databasename | gzip > $targetdir/$today/databasename.sql.gz
end_time="$(date -u +%s)"
elapsed="$(($end_time-$start_time))"
echo "databasename : $elapsed seconds"

```

add your shell script in crontab
```
0 2 * * * /bin/bash /path/to/backupscript/file
```