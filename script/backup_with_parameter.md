# Backup with parameter

## Using --single-transaction flag
mysqldump will let mysqldump read the database in the current state at the time of the transaction, making for a consistent data dump
```
mysqldump --single-transaction --skip-lock-tables -u root -p nama_database > nama_file.sql
```