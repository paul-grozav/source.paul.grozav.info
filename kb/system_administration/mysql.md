---
layout: page
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
