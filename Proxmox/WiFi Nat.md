1. Plug the ethernet cable

2. Install Proxmox

3. Reboot and login with root

4. Update the Proxmox repository

```bash
# Disable the enterprise repository
vi /etc/apt/sources.list.d/pve-enterprise.list
# by commenting the line
#deb https://enterprise.proxmox.com/debian/pve bullseye pve-enterprise

# and enable the "no-subscription" repository by creating a new file
vi /etc/apt/sources.list.d/pve-no-subscription.list
# with
deb http://download.proxmox.com/debian/pve bullseye pve-no-subscription
```

### Update the System

```bash
apt update && apt upgrade
```

### Create a bridge for your VM and CT

The network address (10.0.0.1/16 here) must be different from the network address of your internet box.
Don't add anything to this file if you only use a wifi connection.

```bash
vi /etc/network/interfaces

# replace with
auto lo
iface lo inet loopback

auto vmbr0
iface vmbr0 inet static
     address 10.0.0.1/16
     bridge-ports none
     bridge-stp off
     bridge-fd 0
```

### Configure Nftables for NAT

`echo net.ipv4.ip_forward=1 > /etc/sysctl.d/routing.conf sysctl -p --system systemctl enable nftables.service vi /etc/nftables.conf`

```bash
#!/usr/sbin/nft -f
flush ruleset
table ip nat {
        chain postrouting {
                type nat hook postrouting priority 0; policy accept; masquerade
        }
}
systemctl start nftables.service
```

### Configure iwd

Iwd is much easier to configure and use than wpa_supplicant.
`apt install -y iwd systemctl --now enable iwd` (don't mind the messages)
`vi /etc/iwd/main.conf`

```bash
# add code
[General]
EnableNetworkConfiguration=true

# restart service
service iwd restart
```

### Connect to the wifi network

```bash
# check the wlan interface name
ip a

iwctl station wlan0 scan && iwctl station wlan0 get-networks

iwctl station wlan0 connect Keenetic-8201_5ghz
```

### Create a VM or CT and configure the network

Login to PVE at `https://127.0.0.1:8006`

Create a VM or CT and configure the IP address with 10.0.0.[100]/16 and gateway 10.0.0.1