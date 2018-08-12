# Dnsmasq

#### Install
```bash
$ brew install dnsmasq
```

#### Configure for .test 
```bash
$ sudo mkdir -v /etc/resolver
$ sudo bash -c 'echo "nameserver 127.0.0.1" > /etc/resolver/test'
$ echo 'address=/.test/127.0.0.1' > /usr/local/etc/dnsmasq.conf
```

#### Make Dnsmasq auto-start on system up
```bash
$ sudo brew services start dnsmasq
```
