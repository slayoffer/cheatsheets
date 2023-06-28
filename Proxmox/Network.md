### VM network config to access internet via NAT

```bash
sudo vim /etc/netplan/00-installer-config.yaml

network:
  version: 2
  renderer: networkd
  ethernets:
    ens18:
      dhcp4: false
      addresses: [ 10.0.0.5/16 ]
      nameservers:
        addresses: [ 8.8.8.8, 8.8.4.4 ]
      routes:
        - to: default
          via: 10.0.0.1
```

### Apply changes

```bash
# for /etc/netplan/00-installer-config.yaml
sudo netplan apply

sudo sysctl -p

# for /etc/network/interfaces
sudo /etc/init.d/networking restart
# or
sudo ip addr flush interface-name && sudo systemctl restart networking
```

### Ignore network device waiting

```bash
sudo systemctl edit systemd-networkd-wait-online.service

# add
[Service]
ExecStart=
ExecStart=/lib/systemd/systemd-networkd-wait-online --ignore=enp0s8 # dont wait for enp0s3
# or
[Service]
ExecStart=
ExecStart=/lib/systemd/systemd-networkd-wait-online --interface=enp0s3 # wait only for enp0s3

sudo systemctl daemon-reload

# or change Netplan config - set "optional: yes"
network:
    version: 2
    renderer: networkd
    ethernets:
        enp0s3:
            ..........
            optional: no
        enp0s8:
            ..........
            optional: yes
```



### PVE host NAT port forwarding

Check [[WiFi Nat]]

```bash
# to apply rules
sudo systemctl restart networking

vim /etc/network/interfaces

auto lo
iface lo inet loopback

auto vmbr0
iface vmbr0 inet static
     address 10.0.0.1/24
     bridge-ports none
     bridge-stp off
     bridge-fd 0

#     post-up echo 1 > /proc/sys/net/ipv4/ip_forward
#     post-up iptables -t nat -A POSTROUTING -s '10.0.0.1/24' -o vmbr0 -j MASQUERADE
#     post-down iptables -t nat -D POSTROUTING -s '10.0.0.1/24' -o vmbr0 -j MASQUERADE    
 
# Port forwarding
post-up iptables -t nat -A PREROUTING -i wlan0 -p tcp --dport 32400 -j DNAT --to 10.0.0.2:32400
post-up iptables -t nat -A PREROUTING -i wlan0 -p tcp --dport 22 -j DNAT --to 10.0.0.2:22
post-up iptables -t nat -A PREROUTING -i wlan0 -p tcp --dport 2223 -j DNAT --to 10.0.0.5:22
post-up iptables -t nat -A PREROUTING -i wlan0 -p tcp --dport 2224 -j DNAT --to 10.0.0.7:22
post-up iptables -t nat -A PREROUTING -i wlan0 -p tcp --dport 2225 -j DNAT --to 10.0.0.8:22
post-up iptables -t nat -A PREROUTING -i wlan0 -p tcp --dport 80 -j DNAT --to 10.0.0.5:80
post-up iptables -t nat -A PREROUTING -i wlan0 -p tcp --dport 443 -j DNAT --to 10.0.0.5:443

# post-up iptables -t nat -A PREROUTING -i wlan0 -p udp --dport 32400 -j DNAT --to 10.0.0.2:32400
# post-down iptables -t nat -D PREROUTING -i wlan0 -p tcp --dport 32400 -j DNAT --to 10.0.0.2:32400
# post-down iptables -t nat -D PREROUTING -i wlan0 -p udp --dport 32400 -j DNAT --to 10.0.0.2:32400
# post-up iptables -t nat -A POSTROUTING -i wlan0 -p tcp --sport 32400 -s 10.0.0.2 -j SNAT --to-source 95.154.71.237:32400
# post-up iptables -t nat -A POSTROUTING -i wlan0 -p udp --sport 32400 -s 10.0.0.2 -j SNAT --to-source 95.154.71.237:32400
# post-down iptables -t nat -D POSTROUTING -i wlan0 -p tcp --sport 32400 -s 10.0.0.2 -j SNAT --to-source 95.154.71.237:32400
# post-down iptables -t nat -D POSTROUTING -i wlan0 -p udp --sport 32400 -s 10.0.0.2 -j SNAT --to-source 95.154.71.237:32400

# actual configs

# hosts
vim /etc/hosts
127.0.0.1 localhost.localdomain localhost
192.168.1.38 pve1.devcraft.app pve

# interfaces
vim /etc/network/interfaces
auto lo
iface lo inet loopback

iface enp2s0 inet manual

auto vmbr0
iface vmbr0 inet static
        address 10.0.0.1/24
        bridge-ports none
        bridge-stp off
        bridge-fd 0

auto vmbr1
iface vmbr1 inet static
        address 10.10.10.0/24
        bridge-ports enp2s0
        bridge-stp off
        bridge-fd 0
#vm net

post-up iptables -t nat -A PREROUTING -i wlan0 -p tcp --dport 32400 -j DNAT --to 10.0.0.2:32400
post-up iptables -t nat -A PREROUTING -i wlan0 -p tcp --dport 2223 -j DNAT --to 10.0.0.6:22
post-up iptables -t nat -A PREROUTING -i wlan0 -p tcp --dport 8080 -j DNAT --to 10.0.0.6:80
```