## PHP Installation

```bash
$ brew install php54 --with-httpd
$ brew unlink php54

$ brew install php55 --with-httpd
$ brew unlink php55

$ brew install php56 --with-httpd
$ brew unlink php56

$ brew install php70 --with-httpd
$ brew unlink php70

$ brew install php71 --with-httpd
$ brew unlink php71
```

#### Open for each php version /usr/local/etc/php/<php-version>/php.ini and edit
```ini
memory_limit = 4096M
max_execution_time = 18000
zlib.output_compression = On
post_max_size = 8M
upload_max_filesize = 8M
date.timezone = Europe/Kiev
sendmail_path = /Users/<your-user>/Support/smtp_catcher.php
```

#### Install PHP SMTP catcher
```bash
$ curl -L https://gist.github.com/9868298c8671a493737a53184854f0b9.git > /Users/<your-user>/Support/smtp_catcher.php
$ chmod +x /Users/<your-user>/Support/smtp_catcher.php
```

#### Install PHP Switcher Script
```bash
$ curl -L https://gist.github.com/w00fz/142b6b19750ea6979137b963df959d11/raw > /Users/<your-user>/bin/sphp
$ chmod +x /Users/<your-user>/bin/sphp
```
#### Switching php
```bash
# pass 71 or 70 or 56 or 55 or 54
$ sphp 70
```

#### Install php extension
```bash
# Run next commanda for each php version
$ sphp 70
$ brew install php70-opcache php70-apcu php70-xdebug --build-from-source
```

#### Install XDebug toggle
```bash
curl -L https://raw.githubusercontent.com/w00fz/xdebug-osx/master/xdebug-toggle > /Users/<your-user>/bin/xdebug
chmod +x /Users/<your-user>/bin/xdebug

# Usage
xdebug                            # outputs the current status
xdebug on                         # enables xdebug
xdebug off                        # disables xdebug
xdebug on|off --no-server-restart # toggles xdebug without restarting apache or php-fpm
```
#### Configure XDebug
```ini
# open /usr/local/etc/php/7.0/conf.d/ext-xdebug.ini
[xdebug]
zend_extension="/usr/local/opt/php70-xdebug/xdebug.so"
xdebug.remote_enable=1
xdebug.remote_host=localhost
xdebug.remote_handler=dbgp
xdebug.remote_port=9000
```

#### Install ionCube toggle
```bash
curl -L https://raw.githubusercontent.com/ytorbyk/ioncube-toggle-osx/master/ioncube-toggle > /Users/<your-user>/bin/ioncube
chmod +x /Users/<your-user>/bin/ioncube

# Usage
ioncube                            # outputs the current status
ioncube on                         # enables ionCube
ioncube off                        # disables ionCube
ioncube on|off --no-server-restart # toggles ionCube without restarting apache or php-fpm
```
