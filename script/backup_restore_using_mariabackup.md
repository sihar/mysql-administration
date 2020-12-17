# Backup and Restore using Mariabackup
  

## Backup
Pastikan target directory ada, create folder jika belum ada
```
$ sudo mkdir /var/mariadb/backup
```
  
    
Backup database menggunakan mariabackup
```
$ sudo mariabackup --backup --target-dir=/var/mariadb/backup/ --user=root --password=123456
  
...
  
201217 09:16:56 Backup created in directory '/var/mariadb/backup/'
201217 09:16:56 [00] Writing backup-my.cnf
201217 09:16:56 [00]        ...done
201217 09:16:56 [00] Writing xtrabackup_info
201217 09:16:56 [00]        ...done
mariabackup: Transaction log of lsn (305321776) to (305321776) was copied.
201217 09:16:56 completed OK!  
```
  
  
Prepare backup untuk restoration
```
$ sudo mariabackup --prepare --target-dir=/var/mariadb/backup/
mariabackup based on MariaDB server 10.1.48-MariaDB Linux (x86_64)
mariabackup: cd to /var/mariadb/backup/
mariabackup: This target seems to be not prepared yet.
mariabackup: xtrabackup_logfile detected: size=2097152, start_lsn=(305321776)
  

....
mariabackup: starting shutdown with innodb_fast_shutdown = 1
2020-12-17  9:19:26 140296755062528 [Note] InnoDB: FTS optimize thread exiting.
2020-12-17  9:19:26 140297112471808 [Note] InnoDB: Starting shutdown...
2020-12-17  9:19:26 140296729884416 [Note] InnoDB: Dumping buffer pool(s) not yet started
2020-12-17  9:19:27 140297112471808 [Note] InnoDB: Waiting for page_cleaner to finish flushing of buffer pool
2020-12-17  9:19:28 140297112471808 [Warning] InnoDB: Some resources were not cleaned up in shutdown: threads 0, events 33, os_mutexes 32, os_fast_mutexes 65
2020-12-17  9:19:28 140297112471808 [Note] InnoDB: Shutdown completed; log sequence number 305322006
201217 09:19:28 completed OK!
```
    
  
Copy target directory ke remote server
```
$ scp -r /var/mariadb/backup/ remote-user@remote-ip-address:/remote-folder/
```
  

## Restore
Persiapan sebelum restore di server tujuan
```
$ sudo systemctl stop mariadb
$ sudo mv mysql/ mysql2
$ sudo do mkdir mysql
$ sudo chown -R mysql:mysql mysql
```
  

Jalankan perintah mariabackup --copy-back dengan target directory sama dengan remote folder sewaktu menjalankan perintah scp sebelumnya
```
$ sudo mariabackup --copy-back --target-dir=/path/target/directory
  


...
201217 08:55:37 [01] Copying ./xtrabackup_info to /var/lib/mysql/xtrabackup_info
201217 08:55:37 [01]        ...done
201217 08:55:37 completed OK!
```
  

Sesuaikan ownership folder /var/lib/mysql dan start service mysql
```
$ sudo chown -R mysql:mysql /var/lib/mysql
$ sudo systemctl start mariadb
```
  
  
### Referensi
[mariadb.com](https://mariadb.com/kb/en/full-backup-and-restore-with-mariabackup/)