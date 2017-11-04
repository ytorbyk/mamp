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
