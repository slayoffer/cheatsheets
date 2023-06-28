### Implement btrfs on server

```bash
# prepare ssd or hdd

# check storage device name
lsblk

# install btrfs utils if not installed
# make btrfs volume
mkfs.btrfs /dev/sdc

# make dir for mount
mkdir /mnt/btrfs-data

# mount
mount /dev/sdc /mnt/btrfs-data

# check
df -h 
# df, du, etc are inaccurate with btrfs!
# better use
sudo btrfs filesystem df /mnt/btrfs-data

# check if its mounted
mount

# create subvolume
btrfs subvolume create /mnt/btrfs-data/myvol-1

# check subvolume
sudo btrfs subvolume list /mnt/btrfs-data/
sudo btrfs filesystem df /mnt/btrfs-data
```



### Check file system usage

```bash
sudo btrfs filesystem usage /home
```

### Create snapshot

```bash
sudo btrfs subvolume snapshot /home /home/home-snapshot-1 # snapshot of /home folder
# check created snapshot
sudo btrfs subvolume list /home
```

### Restore snapshot

```bash
# check snapshot subvolume ID
sudo btrfs subvolume list /home

# create fstab backup
sudo cp /etc/fstab /etc/fstab.bak

# change subvolume of /home to snapshot subvolume via ID
sudo vim /etc/fstab
# change subvol=<folder> to subvolid=<ID>
```

## For snapshots can also use advanced tools like Snapper and TimeShift