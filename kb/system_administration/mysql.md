---
layout: page
ptitle: MySQL
---

- slow queries are a good place to start investigating high CPU usage
- https://mariadb.com/kb/en/library/optimization-and-tuning/
- https://www.percona.com/blog/2014/01/24/mysql-server-memory-usage-2/
`mysql> SHOW ENGINE INNODB STATUS;`

## 1. Memory usage
#### 1.1. Per server (Global)
###### 1.1.1. `key_buffer_size`
###### 1.1.2. `query_cache_size`
###### 1.1.3. `table_open_cache`
###### 1.1.4. `tmp_table_size`
###### 1.1.5. `innodb_buffer_pool_size`
###### 1.1.6. `innodb_buffer_pool_bytes_data`
###### 1.1.7. `innodb_log_buffer_size`
###### 1.1.8. `log_tc_size`
###### 1.1.9. `max_heap_table_size` * number of tables
#### 1.2. Per each connection/thread
The number of currently open connections is visible in this variable: `threads_connected`. Each thread can allocate the following:
###### 1.2.1. `read_buffer_size`
###### 1.2.2. `sort_buffer_size`
###### 1.2.3. `myisam_sort_buffer_size`
###### 1.1.4. `innodb_sort_buffer_size`
###### 1.2.5. `read_rnd_buffer_size`
###### 1.2.6. `join_buffer_size`
###### 1.2.7. `thread_stack`
###### 1.2.8. `binlog_cache_size`
###### 1.2.10. `bulk_insert_buffer_size`
###### 1.2.11. `max_sort_length`
#### 1.3. In memory tables
```sql
SELECT CONCAT(table_schema, ".", table_name), data_length, index_length FROM INFORMATION_SCHEMA.TABLES WHERE engine = 'MEMORY' and table_schema <> "information_schema";
SELECT SUM(data_length), SUM(index_length) FROM INFORMATION_SCHEMA.TABLES WHERE engine = 'MEMORY' and table_schema <> "information_schema";
```

## 2. User management
```sql
-- 2.1. Add a new user:
mysql> CREATE USER 'finley'@'localhost' IDENTIFIED BY 'some_pass';

-- 2.2. Deleting an existing user:
mysql> DROP USER 'finley'@'localhost';

-- 2.3. Updating the password of an existing user:
mysql> SET PASSWORD FOR 'finley'@'localhost' = PASSWORD('auth_string');
```

## 3. Dump
```bash
# 3.1. A Database:
mysqldump -h db1 -ppass databaseName

# 3.2. A table from a database:
mysqldump -h db1 -ppass databaseName tableName

# 3.3. Copy table by dumping between two hosts:
mysqldump -h db1 -ppass1 dbname tableX | mysql -h db2 -ppass2 dbname
```

## 4. Other
```sql
-- Repair table
repair table tableX;

-- Create a new table using the same structure as another table:
create table users_backup like users;

-- Show currently running queries:
show processlist;

-- Engine statistics: List of databases and size of each in MiB
select table_schema as database_name, round(sum(data_length + index_length) / 1024 / 1024, 2) as size_mib from information_schema.tables group by table_schema order by size_mib desc;

-- Get current GTID position from binlog file & pos:
select variable_value into @binlog_file from information_schema.global_status where variable_name="binlog_snapshot_file";
-- select @binlog_file;
select variable_value into @position from information_schema.global_status where variable_name="binlog_snapshot_position";
-- select @position;
select binlog_gtid_pos(@binlog_file, @position);
```