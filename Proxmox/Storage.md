### Pass disk to VM

```bash
# get disk ID on PVE root
ls -n /dev/disk/by-id/

# pass disk to VM (100)
/sbin/qm set 200 -virtio2 /dev/disk/by-id/ata-Samsung_SSD_860_EVO_1TB_S3YBNB0M504569W
```

### Resize disk in VM

```bash
# ONLY for LVM partitions !!! If simple disk partition is used (part) then no need to do anything, just resize disk in PVE GUI and reboot VM

# for LVM first resize HDD in PVE GUI and then confirm increase in disk space (1TB in this case)
lsblk
# will show
NAME                      MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda                         8:0    0    1T  0 disk
├─sda1                      8:1    0    1M  0 part
├─sda2                      8:2    0    1G  0 part /boot
└─sda3                      8:3    0    1T  0 part
  └─ubuntu--vg-ubuntu--lv 253:0    0   31G  0 lvm  /

# Resize the partition
sudo parted

# will show
GNU Parted 3.3
Using /dev/sda
Welcome to GNU Parted! Type 'help' to view a list of commands.

# do
(parted) resizepart 3 100%
(parted) quit

# Extend the logical volume now
sudo lvextend -r -l +100%FREE /dev/mapper/ubuntu--vg-ubuntu--lv

# will show
  Size of logical volume ubuntu-vg/ubuntu-lv changed from <31.00 GiB (7935 extents) to 1.03 TiB (270079 extents).
  Logical volume ubuntu-vg/ubuntu-lv successfully resized.
resize2fs 1.45.5 (07-Jan-2020)
Filesystem at /dev/mapper/ubuntu--vg-ubuntu--lv is mounted on /; on-line resizing required
old_desc_blocks = 4, new_desc_blocks = 132
The filesystem on /dev/mapper/ubuntu--vg-ubuntu--lv is now 276560896 (4k) blocks long.

# Resize the physical volume (may or may not need)
sudo pvresize /dev/sda3

# will show
Physical volume "/dev/sda3" changed
1 physical volume(s) resized or updated / 0 physical volume(s) not resized

# Confirm resize complete
lsblk

# will show
NAME                      MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda                         8:0    0    1T  0 disk
├─sda1                      8:1    0    1M  0 part
├─sda2                      8:2    0    1G  0 part /boot
└─sda3                      8:3    0    1T  0 part
  └─ubuntu--vg-ubuntu--lv 253:0    0    1T  0 lvm  /
```

### Mount disk via PVE GUI and pass to LXC

```bash
# add folder in Proxmox GUI

# edit LXC config file
vim /etc/pve/lxc/110.conf

# add line with folder path
mp0: /mnt/pve/usb,mp=/mnt/video

# reboot PVE

# check folder on LXC
ls /mnt/video/

# config contents
arch: amd64
cores: 1
features: nesting=1
hostname: SMB
memory: 512
mp0: /mnt/media_ssd,mp=/mnt/share
net0: name=eth0,bridge=vmbr0,firewall=1,gw=10.0.0.1,hwaddr=46:3A:51:5F:A1:CE,ip=10.0.0.3/24,type=veth
onboot: 1
ostype: ubuntu
rootfs: local-lvm:vm-111-disk-0,size=8G
startup: order=1
swap: 512
unprivileged: 1
```

### Mount disk via PVE CLI

```bash
mkdir /mnt/media_ssd

mount /dev/sdb1 /mnt/media_ssd

# go to Datacenter - Storage - Add - Folder
# add folder with path /mnt/media_ssd
```