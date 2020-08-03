# Set Login Path

## MySQL config editor
```
mysql_config_editor set --login-path=your_label_name --host=your_host_address --user=your_username --password
```

## Login path example
```
mysql_config_editor set --login-path=local --host=127.0.0.1 --user=test --password
mysqldump.exe --login-path=local --no-create-db "your_database_name"  > your_sql_file.sql
```