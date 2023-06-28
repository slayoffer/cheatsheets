### Install

```bash
sudo apt install lvm2
```



### Help

```bash
man lvcreate
# Check EXAMPLES section

# создать логический раздел с именем sales_data, типом linear, размером 50% от свободного места в виртуальной группе dataVG объёмом 4 ГБ
lvcreate -n sales_data --type=linear -l 50%FREE dataVG
# or for same result
lvcreate --type=linear -n sales_data -L 2G dataVG

# Аргументы -l используются для указания относительного свободного места и -L — конкретного объёма
```

### 1. Разметим физический диск

```bash
sudo fdisk /dev/vdb
Command (m for help): g
Command (m for help): n
Partition number (1-128, default 1):
First sector (2048-8388574, default 2048):
Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-8388574, default 8388574): +1G # 1st partition with 1gb
# Created a new partition 1 of type 'Linux filesystem' and of size 1 GiB.

Command (m for help): n       
Partition number (2-128, default 2):
First sector (2099200-8388574, default 2099200):
Last sector, +/-sectors or +/-size{K,M,G,T,P} (2099200-8388574, default 8388574): +1G # 2nd partition with 1gb
# Created a new partition 2 of type 'Linux filesystem' and of size 1 GiB.

Command (m for help): w
# The partition table has been altered.
# Calling ioctl() to re-read partition table.
# Syncing disks.

sudo partprobe
```

### 2. Создадим PV для двух разделов на новом диске

```bash
sudo pvcreate /dev/vdb1 /dev/vdb2

sudo pvs

# list physical volumes
sudo pvdisplay
```

### 3. Создадим виртуальную группу разделов lvm (VG)

```bash
sudo vgcreate my_vg /dev/vdb1 /dev/vdb2

sudo vgs

# vg details
sudo vgdisplay

# PE Size — это размер чанка. Чанк — это кусочек дискового пространства, в которые производится запись данных и из которых состоит lvm диск. Это блок, если проводить аналогию с файловой системой, или кластер, если проводить аналогию с файловыми системами Windows.
```

### 4. Создадим логический раздел (виртуальный раздел)

```bash
sudo lvcreate -L +100%FREE -n my_vol my_vg

# list logical volumes
sudo lvs        

# details of logical volume
sudo lvdisplay

# example
sudo lvcreate -L 1G -n vol1 caleston_vg # -L for linear volume
```

### 5. Format and mount disk

```bash
sudo mkfs.ext4 /dev/my_vg/my_vol

sudo mount -t ext4 /dev/my_vg/my_vol /mnt/

sudo vim /etc/fstab
# add 
/dev/mapper/my_vg-my_vol                  /mnt            ext4    defaults 1 2

sudo mount -a

# теперь запишем в директорию произвольные данные на весь размер диска (mock payload)
sudo dd if=/dev/zero of=/mnt/payload bs=1M count=2048

sudo df -h
```

### Change lvm size

```bash
# check free space
sudo vgs

# add extra phisycal disk if needed (vdb3)
sudo vgextend my_vg /dev/vdb3

# увеличим наш логический диск lvm
sudo lvextend -L +100%FREE my_vg/my_vol
# or
sudo lvresize -L +1G -n /dev/caleston_vg/vol1

sudo lsblk

sudo df -h # покажет старый размер lvm

# теперь необходимо расширить файловую систему
sudo resize2fs /dev/my_vg/my_vol

sudo df -h # покажет новый размер lvm
```

### Уменьшение раздела lvm

```bash
sudo df -h

# отмонтируем раздел. В момент, когда мы отмонтируем раздел, не стоит находиться в его папке
sudo umount /mnt

# проверим файловую систему на ошибки и уменьшим логический диск lvm на 500 МБ
sudo fsck.ext4 -f /dev/my_vg/my_vol

sudo lvreduce -L-500M /dev/my_vg/my_vol

sudo resize2fs -f /dev/my_vg/my_vol

sudo mount -a

sudo df -h
```

### Extra tricks (snapshot, move, remove)

```bash
# перемещение раздела (при переносе со старого на новый диск)
$ pvmove -n my_vol /dev/sda1 

# резервное копирование логического раздела (снэпшот)
$ lvcreate -s -n mysnapshot -L 5g my_vg/my_vol

# накат снэпшота
$ lvconvert --merge my_vol/mysnapshot

# удаление снэпшота
$ lvremove /dev/my_vg/mysnapshot
```

### Rename

```bash
# rename my_vg to log_vg
sudo vgrename my_vg log_vg

# rename logical volume my_vol to log_vol
sudo lvrename log_vg my_vol log_vol
```

