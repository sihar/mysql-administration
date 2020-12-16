# Restore

```
mysql -u nama_user -p nama_database --max_allowed_packet=1GB < nama_sql_file
```
Parameter --max_allowed_packet disesuaikan dengan ukuran file sql yang mau direstore

## Catatan
Beberapa variable dalam konfiruasi mysql perlu disesuaikan untuk restore file sql yang berukuran besar.  
  

Spesifikasi vm
```
CPU 4 core
RAM 8GB
```
  

Contoh konfigurasi mysql
```
[mysqld]
max_connections=2

# set dengan ukuran 70% - 80% RAM yang tersedia
innodb_buffer_pool_size = 2G
innodb_log_buffer_size = 1G

# (innodb_log_file_size * 2) / innodb_buffer_pool_size = 25%
innodb_log_file_size = 256M

# default 4, ubah menjadi 8
innodb_write_io_threads = 8

innodb_flush_log_at_trx_commit = 0

# ukuran innodb_buffer_pool_size/1G
innodb_buffer_pool_instances=2

# disable doublewrite ketika import sql
innodb_doublewrite=0

# default 200 untuk low end ssd
innodb_io_capacity=800

max_allowed_packet=1GB
skip-name-resolve=1
query_cache_size=0
query_cache_size=0

# set key_buffer_size jika masih menggunakan engine MyISAM
# key_buffer_size=2G
```