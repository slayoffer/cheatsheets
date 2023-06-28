### Client install

```bash
apt-cache policy nfs-common
# if not installed
sudo apt install nfs-common
```

### Mount

```bash
sudo mount -t nfs 178.154.212.255:/var/www/images /mnt/images 
sudo mount -t nfs 158.160.40.92:/mnt/pets /mnt/nfs_yandex

# with NFS version
sudo mount -t nfs -o vers=3 /dir

# for automount
sudo vim /etc/fstab
# add
178.154.212.255:/var/www/images /mnt/images  nfs auto,nofail,noatime,nolock,intr,tcp,actimeo=1800 0 0
# but better use AutoFS insted of fstab (check below)
```

### Unmount

```bash
sudo umount /mnt/nfs 

# in case error 'device is busy' cd out of nfs dir path, and if doesnt help then
sudo fuser -m -v nfs NFS /mnt/nfs
sudo fuser -k NFS /mnt/nfs 
sudo umount /mnt/nfs
```

### Show shared NSF server dirs on client server

```bash
sudo showmount -e 178.154.212.255 # -e for --exports
```

### NFS server setup

```bash
apt install nfs-kernel-server portmap
cat /etc/exports
sudo mv /etc/exports /etc/exports.orig
sudo vim /etc/exports

# add
/exports *(ro,fsid=0,no_subtree_check) # sets /exports dir as NFS root folder and no need to type /exports when connecting to folders inside it (can just type address as 192.168.1.38:/backup)
/exports/backup 192.168.1.0/255.255.255.0(rw,no_subtree_check) 
/exports/documents 192.168.1.0/255.255.255.0(ro,no_subtree_check) 
/exports/public 192.168.1.0/255.255.255.0(rw,no_subtree_check) 
# can also add no_root_squash for root user to be treated as root on other server (useless)

# set permissions
sudo vim /etc/idmapd.conf
# edit
Domain = localdomain # should be same domain name on all servers

# Yandex example
/mnt/pets 95.154.71.237(rw,sync,no_subtree_check) # allow for one ip
/mnt/pets *(rw,no_root_squash,no_subtree_check) # allow for all ips
/srv/data 10.9.8.1(rw) 10.9.0.0/255.255.0.0(ro) # allow for one ip and one subnet

# to reload nfs after changing config
sudo exportfs -a 
# or
sudo systemctl restart nfs-kernel-server
# or manual export
sudo exportfs -o 10.61.35.201:/software/repos
```

### Advanced server config

```bash
sudo vim /etc/nfs.conf
```

### AutoFS (auto mount remote NFS dirs)

```bash
sudo apt install autofs
sudo vim /etc/auto.master
# insert in the end
/mnt/nfs /etc/auto.nfs --ghost --timeout=60 # --ghost to create folders which are not present, every dir thats set in /etc/auto.nfs file will be used by this /etc/auto.master config, timeout will unmount idle nfs shares in 60 secs

sudo nano /etc/auto.nfs
# insert lines in the end
backup -fstype=nfs4,rw 10.10.10.222:/exports/backup # /backup dir will be auto created under /mnt/nfs dir
documents -fstype=nfs4,rw 10.10.10.222:/exports/documents # can set your own r/o or r/w permissions on dirs (will ignore nfs server rules for r/w for example and set it to r/o), but cant set r/w if server sets are r/o

sudo systemctl restart autofs
df -h # at first will not show mounted dirs (ghost mode)
ls -l /mnt/nfs # any command request to nfs dir will mount it
df -h # after ls it will show mounted dirs but unmount them in 60 secs if idle
```

