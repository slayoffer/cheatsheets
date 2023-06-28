### Mount file storage to the VM

1. Connect to the VM via SSH

2. Run this command:

```bash
sudo mount -t virtiofs vfs1 /mnt/backups/
```

3. To check that your file store is mounted, run the command:

```bash
df -T
```

   Result:

```bash
Filesystem        Type         1K-blocks    Used Available Use% Mounted on
udev              devtmpfs        988600       0    988600   0% /dev
tmpfs             tmpfs           203524     780    202744   1% /run
/dev/vda2         ext4          13354932 1909060  10861420  15% /
tmpfs             tmpfs          1017604       0   1017604   0% /dev/shm
tmpfs             tmpfs             5120       0      5120   0% /run/lock
tmpfs             tmpfs          1017604       0   1017604   0% /sys/fs/cgroup
tmpfs             tmpfs           203520       0    203520   0% /run/user/1000
filesystem        virtiofs      66774660       0  66774660   0% /mnt/vfs1
```

4. In order for file storage to be mounted every time the VM is started, add a line to the `/etc/fstab` file in the following format:

```bash
sudo vim /etc/fstab

# add
vfs1  /mnt/backups/ virtiofs    rw    0   0
```