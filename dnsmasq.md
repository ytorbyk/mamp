# Dnsmasq

#### Install
```bash
$ brew install dnsmasq
```

#### Configure for .dev 
```bash
$ sudo mkdir -v /etc/resolver
$ sudo bash -c 'echo "nameserver 127.0.0.1" > /etc/resolver/dev'
$ echo 'address=/.dev/127.0.0.1' > /usr/local/etc/dnsmasq.conf
```

#### Make Dnsmasq auto-start on sstem up
```bash
$ sudo brew services start dnsmasq
```
