### List and modify interfaces on host

```bash
ip link
```

### Check ips

```bash
ip addr
# or
ip a
```

### Set ip

```bash
ip addr add 192.168.1.10/24 dev eth0 
# only valid till reboot

# to persist changes set them it in /etc/network/interfaces
```

### Delete ip

```bash
ip addr del 1.1.1.2 dev eth2 # no mask needed (/24, etc)
```

### Up or down interface

```bash
sudo ip link set dev eth0 up
```



### Show existing routes

```bash
route
```

### Set a route

```bash
# to connect with 172.16.239.0/24 from 172.16.238.0 network via 172.16.238.10 host which can connect both
sudo ip route add 172.16.239.0/24 via 172.16.238.10

# for switch and router
# add 192.168.2.0/24 switch network on 192.168.1.0 switch network via 192.168.1.1 gateway of router
ip route add 192.168.2.0/24 via 192.168.1.1 # internal gateway

# similarly connect 192.168.1.0/24 switch via 192.168.2.1 gateway
ip route add 192.168.1.0/24 via 192.168.2.1

# add external website ip via 192.168.1.1 gateway of router for 192.168.1.0 network
ip route add 172.217.194.0/24 via 192.168.1.1 # for internet

# add all internet IPs
ip route add default via 192.168.2.1
# or
ip route add 0.0.0.0 via 192.168.2.1

# check
route
```

### Enable packets forwarding

```bash
cat /proc/sys/net/ipv4/ip_forward # by default is set to 0

# set to 1 to enable packets forwarding
echo 1 > /proc/sys/net/ipv4/ip_forward

# to keep this setting enabled after reboot enable same setting here
sudo vim /etc/sysctl.conf
```

### Troubleshoot

```bash
nslookup 192.168.2.5

ping 192.168.2.5

traceroute 192.168.2.5

netstat -an | grep 80 | grep -i LISTEN
```

