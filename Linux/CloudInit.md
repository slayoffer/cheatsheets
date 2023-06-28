### Purge CI configs from server

```bash
sudo cloud-init clean
sudo rm -rf /var/lib/cloud/instances

# and do this too, but only if you plan to make template out of this server instance (on Proxmox, etc)
sudo truncate -s 0 /etc/machine-id
sudo rm /var/lib/dbus/machine-id
sudo ln -s /etc/machine-id /var/lib/dbus/machine-id
ls -l /var/lib/dbus/machine-id





```