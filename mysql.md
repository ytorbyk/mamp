# Intall MySQL

```bash
$ brew install mysql
```

#### Make mysql auto-start on login
```bash
$ brew services start mysql
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
# Default Homebrew MySQL server config
[mysqld]

# Only allow connections from localhost
bind-address = 127.0.0.1

collation_server=utf8_general_ci
character_set_server=utf8
init-connect='SET NAMES utf8'

max_allowed_packet      = 10G 
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
sql_mode=

#innodb_force_recovery = 1
[client]

[mysql]

[mysqldump]
quick
quote-names
max_allowed_packet = 10G
```
