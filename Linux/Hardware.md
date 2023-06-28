### lshw: Get the Hardware Details

```bash
# all hardware
sudo lshw
# short list
lshw -short

# or specifically network adapter
lshw -C network
```

### Plug-in (usb) devices

```bash
udevadm info --query=path --name=/dev/sda5

# listen to plug in devices
udevadm monitor
```

### PCI devices

```bash
# list PCIs
lspci
```

### Block (disk) devices

```bash
# list disks
lsblk
# or
lsblk -a
# or
fdisk -l

# MAJ:MIN
# Major is for device type
# Minor is to differentiate same device parts from each other
```

### CPU devices

```bash
lscpu
# or
cat /proc/cpuinfo
```

### Memory

```bash
lsmem
# or short
lsmem --summary
# or
free -m # for megabytes
free -k # for kilobytes
free -g # for gigabytes
```

## Monitoring CPU speed

The default is every 2 seconds.

```bash
watch grep \"cpu MHz\" /proc/cpuinfo
```
