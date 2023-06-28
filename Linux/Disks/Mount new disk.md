To attach a disk to a system in Linux, you would generally need to partition it, format it, and then mount it.

Here are the basic steps:

1. Check the disk:
   
```bash
sudo lsblk -o NAME,SIZE,FSTYPE,TYPE,MOUNTPOINT
```
This command will list all the block devices (disks) along with their sizes, file system types, types and mount points.

2. Identify your disk. The new disk will likely show up as `/dev/vdb` or similar if it's a secondary drive. It will not have any FSTYPE or MOUNTPOINT.

3. Partition the disk if necessary. This may not be necessary if you want to use the whole disk for a single partition. To create a partition, you can use `fdisk` or `parted`. The following example uses `fdisk`:

```bash
sudo fdisk /dev/vdb
```
Then follow the prompts to create a new partition. If you want to use the entire disk for one partition, the simplest way is to press `n` for new partition, `p` for primary, `1` for the first partition, then hit enter to accept the default values for the first and last sector. Then, press `w` to write the partition table and exit.

4. Format the partition with the desired filesystem. This example uses `ext4`:

```bash
sudo mkfs.ext4 /dev/vdb1
```

5. Mount the partition:

```bash
sudo mkdir /mnt/mydisk
sudo mount /dev/vdb1 /mnt/mydisk
```

6. To ensure the partition is mounted automatically at boot, you need to add an entry in `/etc/fstab`. 

```bash
echo '/dev/vdb1 /mnt/mydisk ext4 defaults 0 0' | sudo tee -a /etc/fstab
```

Please replace `/dev/vdb1`, `/mnt/mydisk` and `ext4` with your actual partition, desired mount point and filesystem type respectively.

Keep in mind, these instructions involve operations that can destroy data, so be very careful and make sure you're operating on the correct device. Always make sure to have a backup of your data.