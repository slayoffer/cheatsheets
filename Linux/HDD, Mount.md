### Check space

```bash
df -hP
# or MUCH BETTER
duf

sudo du -hsc * /

# or
sudo ncdu /

# or BETTER
sudo dust
```



### List disks and partitions

```bash
# list all storage devices and partitions
lsblk
# or
ls -l /dev/ | grep "^b"

# detailed view of storage devices and partitions
sudo fdisk -l
# or
cat /etc/fstab

fdisk –t # sets the file system for a partition

# check all mounted partitions and sizes
df -hP # -h for human readable

# list with filesystem type
df -hT # -T for fs type

# list without tmpfs partitions
df -hT -x tmpfs # -x for exclude 

# show inodes
df -i

# check disks dynamically (for USB drives, etc)
watch df -hT -x tmpfs

# check free space on virtual group disks
sudo vgdisplay
```

### Du - check folder size

```bash
# check file/folder sizes
sudo du
# or better
sudo du -sh /var/log/* # -s for size, -h for human readable
# with folder depth size
sudo du -h --max-depth 1 /home/slayo
# with kb size
sudo du -sk

# with folders total size
sudo du -hsc * # -c for total size count

# compare two folders sizes
sudo du -hs /home/slayo /etc

# compare two folders sizes and show total size
sudo du -hsc /home/slayo /etc

# for subdirs
sudo du -hsc /home/slayo/*

# with sort
sudo du -h -d 10 /var/log | sort -rh # -d глубина поиска, -r обратный порядок сортировки

# -i for index descriptors
# -m for mb size
# -a for subdirs view
# -T for fs type
```

### NCDU  - best check folder size

```bash
# need to use with sudo or will not scan all folders
sudo ncdu # will scan root folder (/)
# or
sudo ncdu /

# show only local file system without mounted devices
sudo ncdu -x /dir/to/start/look/from

# to delete folders/files press d
```

### All disks and devices location folder

```bash
ls -l /dev/
```

### Mount disk

```bash
# list all mounted devices, inlcuding system ones
mount

# list mounted sdb partitions
mount | grep /dev/sdb1
# or
df -hP | grep /dev/sdb1

sudo mount /dev/sda2 /media
# mounts sda2 to /media dir

# if disk is already in fstab then can only use its mount dir
sudo mount /mnt/nvme

# mount all not mounted disks from fstab
sudo mount -a

# mount with FS type
sudo mount /dev/sda2 /media -t ntfs # for NTFS disks

# permanent disks should be mounted into /mnt/disk-folder
# temporary disks (USBs, etc) should be mounted into /media/disk-folder
```

### Unmount disk

```bash
sudo umount /dev/sda2
# or
sudo umount /media
```

### Repair a corrupted file system 

```bash
fsck # will repair a corrupted file system
fsck /mbr # will repair a corrupted Master Boot Record
```

### Mock payload

```bash
# (mock payload)
sudo dd if=/dev/zero of=/mnt/payload bs=1M count=2048 # создаст в папке /mnt папку /payload и наполнит ее на 2 гига
```

### Create partition (new way)

```bash
sudo apt install gdisk

gdisk /dev/sdb

? # all options
n # create partition
# to set 500mb size
Select parition number = 1 (for vdd1)
Select default first sector = 2048
Type +500M when asked for last sector
Use default hex code = 8300
Finally type w to write to the partition table

# check
sudo fdisk -l /dev/sdb
```

### Create partition(old way)

```bash
# list all storage devices and partitions
lsblk

# detailed view of storage devices and partitions
sudo fdisk -l

# check all mounted partitions and sizes
df -h

# disk creation
sudo fdisk /dev/sda2
# then input p to show partitions
# input g to create GPT partition table (recommended)
# input n to create new partition
# change part. sizes or just hit enter twice
# put w for write and disk will be formatted

# reboot or use
partprobe
```

### Create filesystem

```bash
# disk formatting to ext4 format
sudo mkfs.ext4 /dev/sda2
# or with label
sudo mkfs.ext4 -n "disk label" /dev/sda2

# for exfat format
sudo apt install exfat-utils exfat-fuse # ubuntu

# disk formatting to exfat format
sudo mkfs.exfat /dev/sda2
```

### Get info about disk

```bash
sudo blkid /dev/sda2
```

### fstab integrity check

```bash
# this cmd shouldnt give any errors
sudo mount -a
```

### Automount

```bash
# BAD WAY!

# edit fstab
sudo nano /etc/fstab

# add line
/dev/nvme0n1p1 /mnt/nvme ext4 defaults 0 2

# the disk name may change after reboot so better to use UUID instead of /dev/nvme0n1p1

# RIGHT WAY!

# check the disk name
sudo fdisk -l

# mount the disk
sudo mount /dev/sda2 /media

# get the disk UUID
sudo blkid /dev/sda2

# add the disk for automount on bootup
sudo vim /etc/fstab

# add line:
UUID=B83A843F7A83PHPH /mnt/plexmedia ext4 defaults,auto,rw,nofail 0 1 
# or ext4 defaults 0 1, 
# 0 for dump, 
# 1-2-3 for filesystem integrity checking order, can be set to 0

# if dont want to automount, and mount only manually via mount -a cmd
UUID=B83A843F7A83PHPH /mnt/plexmedia ntfs defaults,noauto 0 1 # added noauto option
# add ro for read only in case needed. defaults has rw option set

# then unmount media and mount plexmedia
sudo umount /media/
sudo mount /mnt/plexmedia/
```

### Copy home folder to new disk

```bash
# make new disk
#Копируем файлы пользователей в папку /tmp. Если данных много, то сохраняем их на внешний носитель
cp –a /home /tmp
# Монтируем наш новый диск в каталог /home
mount /dev/xvdb1 /home/

#Возвращаем папки пользователей в /home
cp -a /tmp/home/* /home/

# Добавить в файл /etc/fstab следующую строку
/dev/xvdb1 /home xfs defaults 0 0
```

### hdparm - получение информации о HDD  

```bash
-g Отображает геометрию устройства (цилиндры, головки, сектора), размер
(в секторах) устройства.
-h Отображает краткую информацию об использовании (помощь).
-H Считывает температуру некоторых устройств (большинство Hitachi). Также
отображает предупреждение, если температура выше нормы.
-i Отображает идентификационную информацию от драйвера устройства.
-I Отображает идентификационную информацию прямо от устройства. Более
детально.
-t. Отображает скорость чтения с диска, без кэширования данных
-T Отображает скорость чтения напрямую из кэша Linux-буфера, без доступа к
диску

# no args
sudo hdparm /dev/sda

# Вывод информации о жестком диске
sudo hdparm -i /dev/sda

# Отображение краткой информации о диске, текущая температура
sudo hdparm -iH /dev/sdа

# Скорость чтения данных с диска
sudo hdparm -tT /dev/sda

```

### Apps

```bash
• gsmartcontrol - графическая программа проверки диска
• smartmontools - консольная программа для проверки и контроля диска
• parted - работа с таблицей разделов
• fdisk - разметка диска 
• blkid - UUID жесткого диска для файла /etc/fstab
• cat /proc/partitions - все зарегистрированные разделы
• mount - смонтированные файловые системы
• df - свободное пространство на дисках
```

