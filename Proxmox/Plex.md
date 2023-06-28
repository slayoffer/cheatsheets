### SMB set up

```bash
apt install smbclient

smbclient -U smart -L //192.168.1.87

# for Win PC
mount -t cifs -o username=smart,password=wassup //192.168.1.87/e /mnt/share/

# for SMB CT
mount -t cifs -o uid=110,username=smbusr,password=wassup //10.0.0.3/share /mnt/share

vim /etc/fstab

//192.168.0.194/Share /mnt/Share cifs _netdev,x-systemd.automount,x-systemd.requires=network-online.target,uid=110,username=smbusr,password=smbpass,iocharset=utf8 0 0
# This line tells fstab that there is a mount & provides all the connection details for the mount. Saving the password in the fstab isn’t recommended, you’ll want to do something like:
vim /.credentials/smbcredentials
# Add to this file:
username=smbusr
password=wassup
chmod 600 /.credentials/smbcredentials
# Replace the username & password parameters from the command above with:
credentials=/root/.credentials/smbcredentials

vim /etc/fstab

# for Win PC
//192.168.1.87/e /mnt/windows_share cifs _netdev,x-systemd.automount,x-systemd.requires=network-online.target,username=smart,password=wassup,iocharset=utf8 0 0

# for SMB CT
//10.0.0.3/share /mnt/share cifs _netdev,x-systemd.automount,x-systemd.requires=network-online.target,uid=110,credentials=/.credentials/smbcredentials,iocharset=utf8 0 0

systemctl daemon-reload

# add cron to perform auto-mount on reboot
crontab -e

# add
@reboot mount -a
```

