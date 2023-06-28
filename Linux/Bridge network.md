### Set

```bash
# backup up and then open network config
sudo vim /etc/netplan/00-installer-config.yaml

# add
network:
  ethernets:
    enp0s3:
      dhcp4: false
  bridges: 
    br0: 
      interfaces: [enp0s3] 
      dhcp4: true 
      parameters: 
        stp: false 
        forward-delay: 0
        
# apply
sudo netplan apply # DONT DO VIA SSH, just reboot
```

