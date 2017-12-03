## Apache Installation

#### Stop built in apache
```bash
$ sudo apachectl stop
$ sudo launchctl unload -w /System/Library/LaunchDaemons/org.apache.httpd.plist 2>/dev/null
```

#### Install brew apache
```bash
brew install httpd
```

#### Make apache auto-start on system up
```bash
$ sudo brew services start httpd
```

#### start|stop|restart commands
```bash
$ sudo apachectl start
$ sudo apachectl stop
$ sudo apachectl -k restart
```


#### Apache Configuration

Create next folders:
- `~/Support/apache-log`
- `~/Support/apache-ssl`
- `~/Support/apache-vhosts`
- `~/Support/localhost`

Open `/usr/local/etc/httpd/httpd.conf` file in any text editor

#### Replace
```ini
Listen 8080
-
Listen 80
```
```ini
DocumentRoot "/usr/local/var/www/htdocs"
<Directory "/usr/local/var/www/htdocs">
-
DocumentRoot "/Users/<your-user>/Support/localhost"
<Directory "/Users/<your-user>/Support/localhost">
```
```ini
# in the same <Directory> block
AllowOverride None
-
AllowOverride All
```
```ini
User www-data
Group www-data
-
User <your-user>
Group staff
```
```ini
#ServerName www.example.com:8080
-
ServerName localhost
```
```ini
# after all php verstions are installed
LoadModule php5_module    /usr/local/opt/php54/libexec/apache2/libphp7.so
LoadModule php5_module    /usr/local/opt/php55/libexec/apache2/libphp7.so
LoadModule php5_module    /usr/local/opt/php56/libexec/apache2/libphp5.so
LoadModule php7_module    /usr/local/opt/php70/libexec/apache2/libphp7.so
LoadModule php7_module    /usr/local/opt/php71/libexec/apache2/libphp7.so
-
# Brew PHP LoadModule for `sphp` switcher
LoadModule php5_module /usr/local/lib/libphp5.so
#LoadModule php7_module /usr/local/lib/libphp7.so
```
```ini
<IfModule dir_module>
    DirectoryIndex index.html
</IfModule>
-
<IfModule dir_module>
    DirectoryIndex index.php index.html
</IfModule>

<FilesMatch \.php$>
    SetHandler application/x-httpd-php
</FilesMatch>
```
```ini
<Directory />
    AllowOverride none
    Require all denied
</Directory>
-
<Directory />
    AllowOverride All
    Require all granted
</Directory>
```
```ini
ErrorLog "/usr/local/var/log/apache2/error_log"
-
ErrorLog "/Users/<your-user>/Support/apache-log/error.log"
```
```ini
CustomLog "/usr/local/var/log/apache2/access_log" common
-
CustomLog "/Users/<your-user>/Support/apache-log/access.log" common
```

#### Uncomment
```
LoadModule rewrite_module lib/httpd/modules/mod_rewrite.so
LoadModule vhost_alias_module lib/httpd/modules/mod_vhost_alias.so
LoadModule socache_shmcb_module lib/httpd/modules/mod_socache_shmcb.so
LoadModule ssl_module lib/httpd/modules/mod_ssl.so
```

#### Generage ssl certificates
```bash
$ cd /Users/<your-user>/Support/apache-ssl
$ openssl req -x509 -nodes -days 3650 -newkey rsa:2048 -keyout server.key -out server.crt
```

#### Configure default virtual host
```ini
# uncomment
Include /usr/local/etc/httpd/extra/httpd-vhosts.conf

# Add line. Place all your vhosts configs into the folder. At least one config MUST be present in the folder
Include /Users/<your-user>/Support/apache-vhosts/*.conf
```

#### Open in any text editor and replace the content with
```ini
<VirtualHost *:80>
    DocumentRoot "/Users/<your-user>/Support/localhost"
    
    ServerName localhost
    
    ErrorLog "/Users/<your-user>/Support/apache-log/localhost-error.log"
    CustomLog "/Users/<your-user>/Support/apache-log/localhost-access.log" common
</VirtualHost>

Listen 443
SSLCipherSuite HIGH:MEDIUM:!MD5:!RC4
SSLProxyCipherSuite HIGH:MEDIUM:!MD5:!RC4
SSLHonorCipherOrder on
SSLProtocol all -SSLv3
SSLProxyProtocol all -SSLv3
SSLPassPhraseDialog  builtin
SSLSessionCache        "shmcb:/usr/local/var/run/apache2/ssl_scache(512000)"
SSLSessionCacheTimeout  300

<VirtualHost *:443>
    DocumentRoot "/Users/<your-user>/Support/localhost"
    
    ServerName localhost
    
    ErrorLog "/Users/<your-user>/Support/apache-log/localhost-ssl-error.log"
    CustomLog "/Users/<your-user>/Support/apache-log/localhost-ssl-access.log" common
    
    SSLEngine on
    SSLCertificateFile "/Users/<your-user>/Support/apache-ssl/server.crt"
    SSLCertificateKeyFile "/Users/<your-user>/Support/apache-ssl/server.key"
</VirtualHost>
```

#### Use next template for config.
```ini
#
# m2ce.dev
#
<VirtualHost *:80>
    DocumentRoot "/Users/<your-user>/<sites-folder>/m2ce"
    
    ServerName m2ce.dev
    ServerAlias alias-m2ce.dev alias-2-m2ce.dev
    
    ErrorLog "/Users/<your-user>/Support/apache-log/mce-error.log"
    CustomLog "/Users/<your-user>/Support/apache-log/mce-access.log" common
</VirtualHost>

<VirtualHost *:443>
    DocumentRoot "/Users/<your-user>/<sites-folder>/m2ce"
    
    ServerName m2ce.dev
    ServerAlias alias-m2ce.dev alias-2-m2ce.dev
    
    ErrorLog "/Users/<your-user>/Support/apache-log/mce-ssl-error.log"
    CustomLog "/Users/<your-user>/Support/apache-log/mce-ssl-access.log" common
    
    SSLEngine on
    SSLCertificateFile "/Users/<your-user>/Support/apache-ssl/server.crt"
    SSLCertificateKeyFile "/Users/<your-user>/Support/apache-ssl/server.key"
</VirtualHost>
```

#### Restart Apache after add|edit|remove vhost

