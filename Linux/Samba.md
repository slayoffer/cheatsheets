### Install

```bash
sudo apt install samba

sudo systemctl stop smbd
```

### Config

```bash
sudo mv /etc/samba/smb.conf /etc/samba/smb.conf.orig
sudo touch /etc/samba/smb.conf /etc/samba/smbshared.conf
sudo vim /etc/samba/smb.conf
# add
include = /etc/samba/smbshared.conf

sudo vim /etc/samba/smbshared.conf
# add
[Documents] 
path = /share/documents # create dir beforehand
force user = slayo
force group = slayo 
public = yes 
writable = no 
 
[Public] 
path = /share/public # create dir beforehand
force user = slayo 
force group = slayo 
create mask = 0664 
force create mode = 0664 
directory mask = 0777 
force directory mode = 0777 
public = yes 
writable = yes

sudo chown -R slayo:slayo /share
sudo systemctl start smbd

# on Windows press Win + r and type
\\192.168.1.38\Documents

# to access shares from ubuntu
sudo apt install smbclient cifs-utils
```

### Mount shared folder on other linux pc

```bash
sudo vim /etc/fstab
# add
//myserver/share/documents /mnt/documents cifs username=myuser,noauto 0 0
# noauto is better because reboot might take too long with automount of smb
sudo mount /mnt/documents
```

### Mount without fstab (CIFS)

```bash
udo mount -t cifs //myserver/Documents -o username=myuser /mnt/documents
```





### WinFile Sharing With Ext4 Partition

It is certainly possible. To my knowledge, Samba doesn't care what file system you're using, just so long as you can read it and mount it. If you setup a Samba share that points to a directory on your esata drive, windows machines will be able to view it without ever having to know that it's formatted ext4.

edit: To provide more information, modifying your `/etc/samba/smb.conf` is how you would go about creating a share for your esata drive.

As an example, here is a relevant entry in my smb.conf:

```
[raid]
   comment = 4TB Raid5
   path = /mnt/raid
   public = yes
   writable = yes
   create mask = 0777
   directory mask = 0777
   force user = nobody
   force group = nogroup
```

That will create a share named *raid* that points to the directory */mnt/raid*. It doesn't require a username/password, and it's writable.

After making those changes, use `sudo service smbd restart` to restart the samba server.