### SMB set up

```bash
# Using the Proxmox GUI, create an unprivileged container. I used the latest Ubuntu template. Most options can be left at their defaults. I switched my network settings to be DHCP instead of the default static. My resources were set to 1 CPU, 8 GB storage, 512 MB RAM. As this container is for serving shares, it shouldn’t really need much more in terms of resource allocation.

# Console into your container & run:

apt update && apt upgrade -y
apt install net-tools #Needed for ifconfig, etc.
apt install samba

# That should get you up-to-date on the software side of things. To confirm that Samba is installed, run: whereis samba the output should show you the installed locations, samba: /usr/sbin/samba /usr/lib/samba /etc/samba /usr/share/samba /usr/share/man/man7/samba.7.gz /usr/share/man/man8/samba.8.g.

# After installing the service, we will need to create a user that can manage the shares & enable it to use the service

adduser --system smbusr
smbpasswd -a smbusr

# Next, create a directory to share, eg: mkdir /mnt/Data/Share& own it as smbusr by running: chown -R smbusr /mnt/Data/Share .

# Now edit your SMB configuration by adding at the end your share details:
nano /etc/samba/smb.conf

[Share]
    comment = Share on the Data Pool
    path = /mnt/share
    read only = no
    writeable = yes
    browsable = yes
    force user = smbusr

# Last, restart the service & allow it through the firewall.
service smbd restart
ufw allow samba

# You will want to note down the SMB user’s ID for later (to enable mounting these shares on other containers) by running id -u smbusr. Mine was 110.

# Run ip a to find out the IP address of your SMB container.
```

