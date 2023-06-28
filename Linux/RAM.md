### Check free RAM

```bash
free

# or human readable
free -m # -m for megabytes, -g for gb
```

### Swappiness

```bash
# check
sudo sysctl vm.swappiness # 60 is default
# or
cat /proc/sys/vm/swappiness

# the lower the number the less likely swap is going to be used, but 0 wont disable swap usage (server will try not to use it). 60 is the best value

# set temporarily
sudo sysctl vm.swappiness=30 # will change back after reboot

# set permanently
sudo vim /etc/sysctl.conf
# add line
vm.swappiness = 30
```

### Swap file

```bash
# create
sudo fallocate -l 4g /swapfile # fallocate can create file of particular size

sudo chmod 0600 /swapfile

# convert file to swap
sudo mkswap /swapfile

# swap file is declated in /etc/fstab
/swapfile none swap sw 0 0

# mount & activate file after creation
sudo swapon -a

# deactivate
sydo swapoff -a

# K8S needs swap to be deactivated
```

## A simple way to Monitor memory every 2 seconds:

“watch -n X” repeats a command every X seconds and highlights the differences.

```bash
watch -n 2-d '/bin/free -m'
```

## how all current string values in the ram

A practical way to see the strings we have stored in the RAM.

```bash
sudo dd if=/dev/mem | cat | strings
```
