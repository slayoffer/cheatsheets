### Start/stop Firewall via CLI

```bash
pve-firewall stop

systemctl pve-firewall disable

iptables-save

# reboot

# or
vim /etc/pve/firewall/cluster.fw
# set ENABLE to 0
```