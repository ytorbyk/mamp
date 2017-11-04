## Cron configuration

Create `~/Support/cron.sh` file

```bash
$ env EDITOR=nano crontab -e
```
Add line and save
```bash
* * * * * /usr/bin/bash /Users/<your-user>/Support/cron.sh
```

Run to ensue it's saved correctly
```bash
crontab -l
```

Now the file is hit each minute, if we need to add cron for any magento instance then add command into the file.

#### Magento 1
```bash
/usr/local/bin/php /Users/<your-user>/<magento-root>/cron.php > /dev/null

# or with log file. make sure log folder exists
/usr/local/bin/php /Users/<your-user>/<magento-root>/cron.php > /Users/<your-user>/<magento-root>/var/log/cron.log
```
#### Magento 2
```bash
/usr/local/bin/php /Users/<your-user>/<magento2-root>/bin/magento cron:run > /dev/null

# or with log file. make sure log folder exists
/usr/local/bin/php /Users/<your-user>/<magento2-root>/bin/magento cron:run > /Users/<your-user>/<magento2-root>/var/log/cron.log
```
