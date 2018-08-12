# Intall MySQL

```bash
$ brew install mysql@5.7
```

#### Make mysql auto-start on login
```bash
$ brew services start mysql@5.7
```

#### start|stop|restart commands
```bash
$ mysql.server status
$ mysql.server start
$ mysql.server stop
$ mysql.server restart
```

#### Configure mysql security
```bash
# start mysql server if it's not started yet
$ mysql_secure_installation
# Answer the questions and fill them in as is appropriate for your environment.
```

Open `/usr/local/etc/my.cnf` file and replace with
```ini
[client]
user=root
password=<root-password>

[mysqld]
sql_mode="NO_AUTO_VALUE_ON_ZERO"
bind-address = 127.0.0.1

collation_server=utf8_general_ci
character_set_server=utf8
init-connect='SET NAMES utf8'

max_allowed_packet      = 1073741824
innodb_strict_mode      = off
query_cache_limit       = 64M
query_cache_size        = 512M
query_cache_type        = 1
table_open_cache        = 100
wait_timeout            = 28800
tmp_table_size          = 1G
max_heap_table_size     = 1G
innodb_buffer_pool_size = 1500M
innodb_log_buffer_size  = 128M
innodb_lock_wait_timeout= 200
log-queries-not-using-indexes
long_query_time         = 2
skip-external-locking

#innodb_force_recovery = 1

[mysqldump]
quick
quote-names
max_allowed_packet = 1073741824
```
