## PHP Installation

```bash
$ brew install php@5.6
$ brew install php@7.0
$ brew install php@7.1
$ brew install php
```

#### Create the file for each php version /usr/local/etc/php/`<php-version>`/conf.d/z-performance.ini and the next into
```ini
; Add TIMEZONE string for replacing with machine timezone
date.timezone = "Europe/Kiev"

; Max execution time per request
max_execution_time = 18000

; Max memory per instance
memory_limit = 4G

; The maximum size of an uploaded file.
upload_max_filesize = 128M

; Sets max size of post data allowed. This setting also affects file upload. To upload large files, this value must be larger than upload_max_filesize
post_max_size = 128M

session.auto_start = off
session.gc_probability = 0
suhosin.session.cryptua = off

; Disable garbage collector
zend.enable_gc = off

; Transparent output compression using the zlib library
zlib.output_compression = On

; Use Smtp for sending emails
sendmail_path = /Users/<username>/Support/smtp_catcher.php
```

#### Install PHP SMTP catcher
```bash
$ curl -L https://gist.github.com/ytorbyk/9868298c8671a493737a53184854f0b9/raw > $HOME/Support/smtp_catcher.php
$ chmod +x $HOME/Support/smtp_catcher.php
```

#### Install PHP Switcher Script
```bash
$ curl -L https://gist.github.com/w00fz/142b6b19750ea6979137b963df959d11/raw > $HOME/bin/sphp
$ chmod +x $HOME/bin/sphp
```
#### Switching php
```bash
# pass 7.2 or 7.1 or 7.0 or 5.6 
$ sphp 7.0
```

#### Install xDebug extension
```bash
# Run next command for php 5.6
$ sphp 5.6
$ pecl install xdebug-2.5.5

#Run next command for php 7.0, 7.1 and 7.2
$ sphp <version>
$ pecl install xdebug
```

#### Install XDebug toggle
```bash
curl -L https://raw.githubusercontent.com/w00fz/xdebug-osx/master/xdebug-toggle > $HOME/bin/xdebug
chmod +x $HOME/bin/xdebug

# Usage
xdebug                            # outputs the current status
xdebug on                         # enables xdebug
xdebug off                        # disables xdebug
xdebug on|off --no-server-restart # toggles xdebug without restarting apache or php-fpm
```
#### Configure XDebug for each php version

Remove `zend_extension="xdebug.so"` line from the beginning of the /usr/local/etc/php/`<php-version>`/php.ini file

```ini
# Create the file /usr/local/etc/php/<php-version>/conf.d/ext-xdebug.ini and add the next into it
[xdebug]
zend_extension="xdebug.so"
xdebug.remote_enable=1
xdebug.remote_host=localhost
xdebug.remote_handler=dbgp
xdebug.remote_port=9000
xdebug.remote_autostart=0
```

#### Install ionCube

Download the file https://downloads.ioncube.com/loader_downloads/ioncube_loaders_dar_x86-64.tar.gz and unpack it
```bash
# Open the folder in terminal
# Run the command for each php version
sphp <version>
cp ioncube_loader_dar_<version>.so $(pecl config-get ext_dir)/ioncube.so
```
```ini
# Create the file /usr/local/etc/php/<php-version>/conf.d/ext-ioncube.ini and add the next into it
[ioncube]
zend_extension="ioncube.so"
```

#### Install ionCube toggle
```bash
curl -L https://raw.githubusercontent.com/ytorbyk/ioncube-toggle-osx/master/ioncube-toggle > $HOME/bin/ioncube
chmod +x $HOME/bin/ioncube

# Usage
ioncube                            # outputs the current status
ioncube on                         # enables ionCube
ioncube off                        # disables ionCube
ioncube on|off --no-server-restart # toggles ionCube without restarting apache or php-fpm
```
