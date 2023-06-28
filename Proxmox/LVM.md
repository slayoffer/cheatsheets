### Format Disk

This will for sure remove all the data from the Disk, which I am sure about. My Disk name is sdb and the path is /dev/sdb so I will used below command to clean to create partition table

```
sgdisk -N 1 /dev/sdb
```

### Create Physical Volume and Volume Group within Disk

Now PV is Physical Volume and VG is Volume Group. I created both using the below commands in my disk named sdb

```
pvcreate --metadatasize 1024M -y -ff /dev/sdb1
vgcreate --metadatasize 1024M proxvg /dev/sdb1
```

### Create LVM Thin Pool

LVM (Logical Volume Manager) Think pool is the option that we need to create. Out of 900 GB I want to assign 400 GB to Thin pool. So I will use below commands to create the Thinpool

```
lvcreate -l 100%FREE --poolmetadatasize 1024M --chunksize 256 -T -n proxthin proxvg
lvcreate -n proxvz -V 400G proxvg/proxthin
```

### Create EXT4 File System

```
mkfs.ext4 /dev/proxvg/proxvz
```

### Mount new Logical Volume (LV)

Now mount the new Logical Volume to Proxmox Node so we will create a new directory

```
mkdir /media/vz
echo '/dev/proxvg/proxvz /media/vz ext4 defaults,errors=remount-ro 0 2' >> /etc/fstab
mount -a
lvs -a
```

### Now Add the Directory in the Web User

You can simply follow below screeshot, first click on Data Center, then Storage and then add directory.

![img](https://syncbricks.com/wp-content/uploads/2021/07/image-7.png)

The ID will be or any name you want and Directory path will be the one that we created on top.

ID : vz

Path : /media/vz

![img](https://syncbricks.com/wp-content/uploads/2021/07/image-8.png)

Add LVM Thin

Now you can create LVM Thin as per the below screenshot.

![img](https://syncbricks.com/wp-content/uploads/2021/07/image-9.png)

Use the below Information for in the box.

```
    id:proxthin
    Volume group:proxvg
    Thin Pool:proxthin
```