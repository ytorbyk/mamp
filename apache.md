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

Open `/usr/local/etc/httpd/httpd.conf` file in any text editor and add the next line to the end of file

```ini
Include /Users/<username>/Support/httpd.conf
```

Create the file and put next content into
```ini
Listen 80
ServerName localhost

User <username>
Group staff

LoadModule rewrite_module lib/httpd/modules/mod_rewrite.so
LoadModule vhost_alias_module lib/httpd/modules/mod_vhost_alias.so
LoadModule socache_shmcb_module lib/httpd/modules/mod_socache_shmcb.so
LoadModule ssl_module lib/httpd/modules/mod_ssl.so

LoadModule proxy_module lib/httpd/modules/mod_proxy.so
LoadModule proxy_http_module lib/httpd/modules/mod_proxy_http.so


<IfModule dir_module>
    DirectoryIndex index.php index.html
</IfModule>

<FilesMatch \.php$>
    SetHandler application/x-httpd-php
</FilesMatch>

<Directory />
    AllowOverride All
    Require all granted
</Directory>

ErrorLog "/Users/<username>/Support/apache-log/apache-error.log"
<IfModule log_config_module>
    LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\"" combined
    LogFormat "%h %l %u %t \"%r\" %>s %b" common

    <IfModule logio_module>
      LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\" %I %O" combinedio
    </IfModule>

    CustomLog "/Users/<username>/Support/apache-log/apache-access.log" common
</IfModule>

Listen 443
SSLCipherSuite HIGH:MEDIUM:!MD5:!RC4
SSLProxyCipherSuite HIGH:MEDIUM:!MD5:!RC4
SSLHonorCipherOrder on
SSLProtocol all -SSLv3
SSLProxyProtocol all -SSLv3
SSLPassPhraseDialog  builtin
SSLSessionCache        "shmcb:/usr/local/var/run/apache2/ssl_scache(512000)"
SSLSessionCacheTimeout  300

Include /Users/<username>/Support/apache-vhosts/localhost.conf
Include /Users/<username>/Support/apache-vhosts/*.conf
```

#### Generage ssl certificates
```bash
$ cd $HOME/Support/apache-ssl
$ openssl req -x509 -nodes -days 3650 -newkey rsa:2048 -keyout server.key -out server.crt
```

#### Configure default virtual host.
Create /Users/<username>/Support/apache-vhosts/localhost.conf file and put the next into it

```ini
<VirtualHost *:80>
    DocumentRoot "/Users/<username>/Support/localhost"
    
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
    
    ErrorLog "/Users/<username>/Support/apache-log/localhost-ssl-error.log"
    CustomLog "/Users/<username>/Support/apache-log/localhost-ssl-access.log" common
    
    SSLEngine on
    SSLCertificateFile "/Users/<your-user>/Support/apache-ssl/server.crt"
    SSLCertificateKeyFile "/Users/<your-user>/Support/apache-ssl/server.key"
</VirtualHost>
```

#### Use next template for config.
```ini
#
# <site-domain>.test
#
<VirtualHost *:80>
    DocumentRoot "/Users/<username>/<sites-folder>/<site-folder>"
    
    ServerName <site-domain>.test
    ServerAlias <site-alias-1>.test <site-alias-2>.test
    
    ErrorLog "/Users/<username>/Support/apache-log/<site-domain>-error.log"
    CustomLog "/Users/<username>/Support/apache-log/<site-domain>-access.log" common
</VirtualHost>

<VirtualHost *:443>
    DocumentRoot "/Users/<username>/<sites-folder>/<site-folder>"
    
    ServerName <site-domain>.test
    ServerAlias <site-alias-1>.test <site-alias-2>.test
    
    ErrorLog "/Users/<username>/Support/apache-log/<site-domain>-error.log"
    CustomLog "/Users/<username>/Support/apache-log/<site-domain>-access.log" common
    
    SSLEngine on
    SSLCertificateFile "/Users/<username>/Support/apache-ssl/server.crt"
    SSLCertificateKeyFile "/Users/<username>/Support/apache-ssl/server.key"
</VirtualHost>
```

#### Restart Apache after add|edit|remove vhost

