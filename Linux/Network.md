### To check IP address, etc

```bash
ifconfig
# or newer command
ip addr show
# or
ip a
# or better
ip -4 -br addr # basic info only

# check public ip
dig -4 TXT +short o-o.myaddr.l.google.com @ns1.google.com
```

### Ping

```bash
ping 192.168.12.40
# or
ping slayo-server
# -c for hops count
ping -c 6 192.168.1.87 
```

### Change hostname

```bash
# change hostname in this file
sudo vim /etc/hostname
# or
sudo hostnamectl set-hostname devcraft.app
# Delete the old name and setup new name.

# Next Edit the /etc/hosts file: 
sudo vim /etc/hosts
# add ip of the needed server, name and shortname for it
127.0.0.1 localhost
127.0.1.1 devcraft.app devcraft

# Reboot the system to changes take effect: 
sudo reboot
```

### Change DNS

```bash
vim /etc/resolv.conf
```

### Check iptables rules

```bash
iptables-save
```

### Show all network connections

```bash
ip link
```



### Check all used ports

```bash
sudo ss -tlpn
```

### Trace route (check how many hops it takes to reach the server)

```bash
tracert www.ya.ru
# or with package loss
sudo mtr
```

### Show open TCP ports

```bash
sudo netstat -antp
# or better
sudo ss -tunlp

# or BEST way to check open ports
sudo nmap localhost # need to install nmap first
sudo nmap 192.168.40.12
sudo nmap -F slayo-server # -F for fast scan

# check for ports and services + versions
sudo nmap -sV 192.168.40.12 # -sV for services + versions

# check specific ports which process is using
sudo ps -ef | grep apache2
sudo netstat -antp | grep <apache2PID>

```

### Connect to TCP port

```bash
telnet 192.168.40.12 3306 # server 192.168.40.12, port 3306
```

### Check port 80

```bash
ss -tunlp | grep 80
```

### Connect WiFi

```bash
apt install -y iwd
systemctl --now enable iwd
vim /etc/iwd/main.conf

#
[General]
EnableNetworkConfiguration=true
#

adduser slayo

# reboot and login with user

ip a # (check the wlan interface name ; "wlan0" here)
iwctl station wlan0 scan && iwctl station wlan0 get-networks
iwctl station wlan0 connect Keenetic-8201_5ghz
```

### Turn on/off network interface

```bash
# ONLY DO LOCALLY!
ip link set wlan0 down
ip link set wlan0 up
```



### Show DNS records of domain

```bash
sudo dig ya.ru
# or
sudo nslookup ya.ru
```

### Show gateways

```bash
sudo route -n
```

### Show MAC addresses

```bash
arp
```

### Check how many active network interfaces pc has

```bash
ip a | grep -v LOOPBACK | grep -ic mtu 
# -v ignore LOOPBACK, -i case insensitive, -c count
```

### lshw: Get the Hardware Details

```bash
lshw -C network
```

### To change wifi network

```bash
sudo vim /etc/netplan/00-installer-config-wifi.yaml
sudo netplan apply
# change SSID to new and reboot
```



### Homelab settings

```bash
# lan
\\10.0.0.2\media

sudo ip ad add 10.0.0.2 dev enp2s0

# or
sudo ifconfig enp2s0 up

sudo ifconfig enp2s0 10.0.0.2

sudo ifconfig enp2s0 netmask 255.255.255.0

sudo route add default gw 10.0.0.1 enp2s0

# wifi
iwconfig

nmcli device wifi connect Keenetic-8201_5ghz password smart1234 name 5ghz

iwlist wlo1 scan
iwlist wlx784476c48ea0 scan


nmcli d wifi connect Keenetic-8201_5ghz password smart1234

wpa_passphrase Keenetic-8201_5ghz smart1234 | sudo tee /etc/wpa_supplicant.conf
wpa_passphrase Keenetic-8201 smart1234 | sudo tee /etc/wpa_supplicant.conf

sudo wpa_supplicant -c /etc/wpa_supplicant.conf -i wlo1

sudo wpa_supplicant -c /etc/wpa_supplicant.conf -i wlo1

sudo dhclient -r wlo1

sudo ifconfig wlo1 down

iw reg get

sudo iwlist scan

sudo iw wlo1 connect wsiit

nmcli connection show

nmcli connection modify Keenetic-8201_5ghz connection.autoconnect no

# network settings
/etc/rc.local
/etc/network/interfaces
/etc/NetworkManager/
/etc/netplan
```
## ip

Утилита управляет интерфейсами, сетевыми устройствами и туннелями. Всем сетевым стеком linux. Например: 

```
ip addr
```

Покажет список всех сетевых адресов. Тоже самое делает утилита `ifconfig` без параметров. 

```
ip route
```

Покажет таблицу роутинга. Тоже самое сделает команда `route`. 

```
ip link set eth0 up
```

Поднимает интерфейс eth0. Это можно также сделать с помощью `ifconfig`. 

Так зачем она нужна если все ее команды можно выполнить с помощью других утилит? Дело в том, что во многие компактные дистрибутивы (например *Alpine* или *openwrt*) не используют `ifconfig`, `route` и другие “большие” утилит. Там оставляют только `ip`. Вот с помощью нее и придется выполнять все сетевые задачи. 

## nc

Полное имя **netcat** - на него она тоже откликается. Это утилита, позволяет слушать и выполнять TCP и UDP соединении. 

И так что может netcat? 

Открываем одно окно терминала и делаем там `nc -l 12345` это будет наш сервер. Отрываем другое окно и пишем в нем `nc 127.0.0.1 12345` — это наш клиент. Печатаем там “Hello!” нажимаем enter. В первом окне видим эту фразу. Ура! Мы написали однонаправленный чат.

Эта утилита пригодиться чтобы проверить жив ли сервер и слушает ли от вообще на этом порту? С помощью `nc` можно переслать файл и даже просканировать порты. Http сервер еще можно написать.

## socat 

Это труба, которая позволяет соединить два сокета между собой. Mysql слушает только на local socket а мы хотим на его ходить по tcp. Выглядит это так: `socat TCP-LISTEN:3307,reuseaddr,fork UNIX-CONNECT:/var/lib/mysql/mysql.sock` 

Конектор к netcat из предыдущего примера: `socat - TCP4:127.0.0.1:80` 

Если вам нужно перебросить сокет в другое место присмотритесь к socat он вам скорее всего поможет. 