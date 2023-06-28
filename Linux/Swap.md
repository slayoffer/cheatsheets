### Reset swap

Check the current swap usage:

```
free -h
```

This command will show you the total, used, and free memory, including swap.

Turn off swap:

```
sudo swapoff -a
```

This command will disable swap on all swap devices. The system will move all data from swap to the main memory (RAM). Make sure you have enough free RAM before running this command to avoid running out of memory.

Clear the swap cache by dropping caches:

```
sudo sysctl vm.drop_caches=3
```

This command will clear the page cache, dentries, and inodes caches from memory. The value `3` is a combination of `1` (page cache) and `2` (dentries and inodes).

Turn on swap:

```
sudo swapon -a
```

This command will re-enable swap on all swap devices.

Check the swap usage again:

```
free -h
```

You should now see that the used swap space has been reduced or cleared.

### Increase swap

```bash
sudo swapon --show

free -h

df -h

sudo fallocate -l 1G /swapfile

ls -lh /swapfile

sudo chmod 600 /swapfile

sudo mkswap /swapfile

sudo swapon /swapfile

sudo swapon --show

free -h

sudo cp /etc/fstab /etc/fstab.bak

echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab

```

### Adjusting the Swappiness Property

The `swappiness` parameter configures how often your system swaps data out of RAM to the swap space. This is a value between 0 and 100 that represents a percentage.

With values close to zero, the kernel will not swap data to the disk unless absolutely necessary. Remember, interactions with the swap file are “expensive” in that they take a lot longer than interactions with RAM and they can cause a significant reduction in performance. Telling the system not to rely on the swap much will generally make your system faster.

Values that are closer to 100 will try to put more data into swap in an effort to keep more RAM space free. Depending on your applications’ memory profile or what you are using your server for, this might be better in some cases.

We can see the current swappiness value by typing:

```
cat /proc/sys/vm/swappiness
```

For a Desktop, a swappiness setting of 60 is not a bad value. For a server, you might want to move it closer to 0.

We can set the swappiness to a different value by using the `sysctl` command.

For instance, to set the swappiness to 10, we could type:

```
sudo sysctl vm.swappiness=10
```

```
Outputvm.swappiness = 10
```

This setting will persist until the next reboot. We can set this value automatically at restart by adding the line to our `/etc/sysctl.conf` file:

```
sudo nano /etc/sysctl.conf
```

At the bottom, you can add:

/etc/sysctl.conf

```
vm.swappiness=10
```

Save and close the file when you are finished.

### Adjusting the Cache Pressure Setting

Another related value that you might want to modify is the `vfs_cache_pressure`. This setting configures how much the system will choose to cache _inode_ and _dentry_ information over other data.

Basically, this is access data about the filesystem. This is generally very costly to look up and very frequently requested, so it’s an excellent thing for your system to cache. You can see the current value by querying the `proc` filesystem again:

```
cat /proc/sys/vm/vfs_cache_pressure
```

```
Output100
```

As it is currently configured, our system removes inode information from the cache too quickly. We can set this to a more conservative setting like 50 by typing:

```
sudo sysctl vm.vfs_cache_pressure=50
```

```
Outputvm.vfs_cache_pressure = 50
```

Again, this is only valid for our current session. We can change that by adding it to our configuration file like we did with our swappiness setting:

```
sudo nano /etc/sysctl.conf
```

At the bottom, add the line that specifies your new value:

/etc/sysctl.conf

```
vm.vfs_cache_pressure=50
```

Save and close the file when you are finished.

### Disable swap

Before actually disabling swap space, first, you need to [visualize your memory load degree](https://www.tecmint.com/find-linux-processes-memory-ram-cpu-usage/) and then identify the partition that holds the swap area, by issuing the below [free command](https://www.tecmint.com/check-memory-usage-in-linux/).

```bash
free -h 
```

Look for the Swap space used size. If the used size is **0B** or close to **0** bytes, it can be assumed that swap space is not used intensively and can be safely disabled.

Next, issue following the **blkid command**, look for `TYPE="swap"` line in order to identify the swap partition, as shown in the below screenshot.

```bash
blkid 
```

Again, issue the following [lsblk command](https://www.tecmint.com/commands-to-collect-system-and-hardware-information-in-linux/) to search and identify the `[SWAP]` partition as shown in the below screenshot.

```bash
lsblk
```

After you’ve identified the swap partition or file, execute the below command to deactivate the swap area.

```bash
swapoff /dev/mapper/centos-swap  
```

Or disable all swaps from **/proc/swaps**, which provides a snapshot of the swap file name.

```bash
swapoff -a 
```

Run [free command](https://www.tecmint.com/check-memory-usage-in-linux/) in order to check if the swap area has been disabled.

```bash
free -h
```

In order to permanently disable swap space in Linux, open **/etc/fstab** file, search for the swap line and comment on the entire line by adding a `#` (hashtag) sign in front of the line, as shown in the below screenshot.

```bash
vi /etc/fstab
```

Afterward, **reboot** the system in order to apply the new swap setting or issue `mount -a` command in some cases might do the trick.

```bash
mount -a
```

After the system reboot, issuing the commands presented at the beginning of this tutorial should reflect that the swap area has been completely and permanently disabled in your system.

```bash
free -h
blkid 
lsblk 
```