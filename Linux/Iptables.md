### Install

```bash
sudo apt install iptables
```

### Enable IP forwarding

```bash
sudo vim /etc/sysctl.conf
# find and enable
net.ipv4.ip_forward=1
```

### Show firewall rules

```bash
# list default rules
sudo iptables -L

# list nat rules
sudo iptables --table nat --list 

# with line numbers
sudo iptables -n -L -v --line-numbers 
```

### Clear table

```bash
sudo iptables --table nat --flush
```

### Delete rules

```bash
sudo iptables --table nat --delete PREROUTING 1
# or
sudo iptables -D INPUT -p tcp --dport 1022 -j ACCEPT
```

### Find port in Iptables rules

```bash
sudo iptables -L | grep 22
```

### Allow port

```bash
sudo iptables -I INPUT 3 -s 0.0.0.0/0 -i eth0 -p tcp --dport 443 -j ACCEPT 

# Где атрибуты:
-I INPUT 3 — для того чтобы вставить новое правило 3-й строкой в список
-s — указать, откуда будут идти запросы адреса и маски
-i — через какой сетевой интерфейс
-p и --dport — описывает протокол и порт
-j — описывает цель правила

# allow incoming ssh port connections from 172.16.238.187
iptables -A INPUT -p tcp -s 172.16.238.187 --dport 22 -j ACCEPT
#
-A Add Rule
-p Protocol
-s Source
-d Destination
--dport Destination port
-j Action to take

# reject all other ssh connections
sudo iptables -A INPUT -p tcp --dport 22 -j DROP

# reject ALL incoming connections
sudo iptables -A INPUT -j DROP

# allow outgoing access to port
sudo iptables -A OUTPUT -p tcp -d 172.16.238.11 --dport 5432 -j ACCEPT

# close internet access
sudo iptables -A OUTPUT -p tcp --dport 443 -j DROP
sudo iptables -A OUTPUT -p tcp --dport 80 -j DROP

# add rule to the very top of rules
sudo iptables -I OUTPUT -p tcp -d 172.16.238.100 --dport 443 -j ACCEPT
```

### Delete rule

```bash
iptables -D INPUT 1 # delete rule at position 1
```

### Append rule

```bash
iptables -A OUTPUT -p tcp --dport 443 -j DROP
```

### Redirect ports

```bash
# redirect from 80 to 8080 port on target machine (do it on the target machine itself)
iptables --table nat --append PREROUTING --protocol tcp --dport 80 jump REDIRECT --to 8080
```

### IP and port forward

```bash
# if request goes to 192.168.254.47:80 forward it to 192.168.254.10:8080 (do it on 192.168.254.47 machine)
sudo iptables --table nat --append PREROUTING --protocol tcp --destination 192.168.254.47 --dport 80 --jump DNAT --to-destination 192.168.254.10:8080

# IMPORTANT: need to get reply back from target machine
sudo iptables --table nat -append POSTROUTING --protocol tcp --destination 192.168.254.10 --dport 8080 --jump SNAT --to-source 192.168.254.47
```



### Examples

```bash
# Расставьте правильный порядок команд, чтобы пользователи могли подключиться в Nginx, работающему на 80 порту из сети 172.1.50.0/24, но с учётом того, что правила изначально пусты.

# Шаг 1
iptables -I INPUT -s 0.0.0.0/0 -i eth0 -p tcp --dport 88 -j ACCEPT # for TCP port

# Шаг 2
iptables -I INPUT 2 -s 172.1.60.0/24 -i eth0 -p tcp --dport 80 -j ACCEPT

# Шаг 3
iptables -I INPUT 3 -i eth0 -p tcp --dport 80 -j DROP

# Шаг 4
iptables -I INPUT 3 -s 172.1.50.0/24 -i eth0 -p tcp --dport 80 -j ACCEPT

# Выберите команду, которая запретит подключаться извне группе пользователей с ip (5.3.177.173, 173.3.177.5, 177.3.5.173) на порт 5432, на любой интерфейс, по протоколу tcp, но не затронет других пользователей.
sudo iptables -I INPUT 1 -s 5.3.177.173/32,173.3.177.5/32,173.3.177.5/32,177.3.5.173/32 -p tcp --dport 5432 -j DROP
#
sudo iptables -I INPUT -s 0.0.0.0/0 -i eth0 -p tcp --dport 443 -j ACCEPT
#
sudo iptables -I INPUT -s 0.0.0.0/0 -i eth0 -p tcp --dport 8080 -j DROP
```

### Ports forwarding

```bash
# Port forwarding
post-up iptables -t nat -A PREROUTING -i wlan0 -p tcp --dport 32400 -j DNAT --to 10.0.0.2:32400
post-up iptables -t nat -A PREROUTING -i wlan0 -p tcp --dport 5432 -j DNAT --to 10.0.0.2:5432
post-up iptables -t nat -A PREROUTING -i wlan0 -p tcp --dport 2223 -j DNAT --to 10.0.0.5:22
post-up iptables -t nat -A PREROUTING -i wlan0 -p tcp --dport 2224 -j DNAT --to 10.0.0.7:22
post-up iptables -t nat -A PREROUTING -i wlan0 -p tcp --dport 2225 -j DNAT --to 10.0.0.8:22
post-up iptables -t nat -A PREROUTING -i wlan0 -p tcp --dport 80 -j DNAT --to 10.0.0.5:80
post-up iptables -t nat -A PREROUTING -i wlan0 -p tcp --dport 443 -j DNAT --to 10.0.0.5:443
```

### Load balancing without proxy server

```bash
# load balance connections to port 80/81 from 82, ip address is 192.168.254.10 
sudo iptables --table nat --append PREROUTING --destination 192.168.254.10 --protocol tcp --dport 82 --match statistic --mode nth --every 3 --packet 0 --jump DNAT --to-destination 192.168.254.10:80

sudo iptables --table nat --append PREROUTING --destination 192.168.254.10 --protocol tcp --dport 82 --match statistic --mode nth --every 2 --packet 0 --jump DNAT --to-destination 192.168.254.10:81

# then add SNAT to get packets reply back
sudo iptables --table nat --append POSTROUTING --destination 192.168.254.10 --protocol tcp --dport 81 --jump SNAT --to-source 192.168.254.10
sudo iptables --table nat --append POSTROUTING --destination 192.168.254.10 --protocol tcp --dport 82 --jump SNAT --to-source 192.168.254.10
```

### Round Robin load balance

```bash
# first packet will go to 3333
sudo iptables --table nat --append PREROUTING --destination 192.168.254.47 --protocol tcp --dport 80 --match statistic --mode nth --every 3 --packet 0 --jump DNAT --to-destination 192.168.254.10:1111
# second packet will go to 2222
sudo iptables --table nat --append PREROUTING --destination 192.168.254.47 --protocol tcp --dport 80 --match statistic --mode nth --every 2 --packet 0 --jump DNAT --to-destination 192.168.254.10:2222
# thrid packet will go to 1111
sudo iptables --table nat --append PREROUTING --destination 192.168.254.47 --protocol tcp --dport 80  --jump DNAT --to-destination 192.168.254.10:3333

# then add SNAT to get packets reply back
sudo iptables --table nat --append POSTROUTING --destination 192.168.254.10 --protocol tcp --dport 3333 --jump SNAT --to-source 192.168.254.47

sudo iptables --table nat --append POSTROUTING --destination 192.168.254.10 --protocol tcp --dport 2222 --jump SNAT --to-source 192.168.254.47

sudo iptables --table nat --append POSTROUTING --destination 192.168.254.10 --protocol tcp --dport 1111 --jump SNAT --to-source 192.168.254.47
```

### Random load balance

```bash
sudo iptables --table nat --append PREROUTING --destination 192.168.254.47 --protocol tcp --dport 80 --match statistic --mode random —probability 0.33 --jump DNAT --to-destination 192.168.254.10:1111

sudo iptables --table nat --append PREROUTING --destination 192.168.254.47 --protocol tcp --dport 80 --match statistic --mode random —probability 0.5 --jump DNAT --to-destination 192.168.254.10:2222

sudo iptables --table nat --append PREROUTING --destination 192.168.254.47 --protocol tcp --dport 80  --jump DNAT --to-destination 192.168.254.10:3333

# then add SNAT to get packets reply back
sudo iptables --table nat --append POSTROUTING --destination 192.168.254.10 --protocol tcp --dport 3333 --jump SNAT --to-source 192.168.254.47

sudo iptables --table nat --append POSTROUTING --destination 192.168.254.10 --protocol tcp --dport 2222 --jump SNAT --to-source 192.168.254.47

sudo iptables --table nat --append POSTROUTING --destination 192.168.254.10 --protocol tcp --dport 1111 --jump SNAT --to-source 192.168.254.47
```

