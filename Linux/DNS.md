### DNS config file

```bash
sudo resolvectl status

sudo cat /etc/resolv.conf # symlink for /run/systemd/resolve/resolv.conf - DONT EDIT ORIGINAL!

# google DNS
8.8.8.8
8.8.4.4

# local network DNS
nameserver 192.168.1.38
nameserver 10.9.8.1
nameserver 10.9.8.2 

# to search for partial domain names add
search devcraft.app home.devcraft.app
# after that can ping pve and it will ping pve.devcraft.app or pve.home.devcraft.app if finds such host in hosts records
```

### Flush DNS

```bash
sudo resolvectl flush-caches
```

### Change order of DNS check

```bash
sudo vim /etc/nsswitch.conf

# change
...
hosts:  files dns # files for /etc/hosts, dns for /etc/resolv.conf
...
# to
...
hosts:  dns files # files for /etc/hosts, dns for /etc/resolv.conf
...
```

### Query hostname (dig, nslookup)

```bash
# only works with DNS config, not /etc/hosts
nslookup www.google.com
# or
dig www.google.com

# with ip
dig @10.9.8.1 server.db.yandex.ru
```

