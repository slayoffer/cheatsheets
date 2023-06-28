### Help

```bash
# VM manager help
man qm
```

### Create VM with Cloud image

```bash
# download Cloud image from https://cloud-images.ubuntu.com/
wget https://cloud-images.ubuntu.com/jammy/current/jammy-server-cloudimg-amd64.img

# create base VM
qm create 400 --memory 1024 --name ubuntu-cloud --net0 virtio,bridge=vmbr0

# import Cloud image
qm importdisk 400 jammy-server-cloudimg-amd64.img local-lvm

# set hardware
qm set 400 --scsihw virtio-scsi-pci --scsi0 local-lvm:vm-400-disk-0

# add cloudinit drive
qm set 400 --ide2 local-lvm:cloudinit

# Make the cloud init drive bootable and restrict BIOS to boot from disk only
qm set 400 --boot c --bootdisk scsi0

# Add serial console
qm set 400 --serial0 socket --vga serial0

# DO NOT START YOUR VM

# Now, configure hardware and cloud init, then create a template and clone. If you want to expand your hard drive you can on this base image before creating a template or after you clone a new machine. I prefer to expand the hard drive after I clone a new machine based on need.

# Create template.
qm template 400

# Clone template.
qm clone 8000 135 --name yoshi --full

# Troubleshooting

# If you need to reset your machine-id
sudo rm -f /etc/machine-id
sudo rm -f /var/lib/dbus/machine-id

# Then shut it down and do not boot it up. A new id will be generated the next time it boots. If it does not you can run:
sudo systemd-machine-id-setup

# to increase image default disk space (+ 2GB)
qemu-img resize jammy-server-cloudimg-amd64.img +2G
```



### List VMs

```bash
qm list
```

### Start/Stop/Restart VM

```bash
qm start 100 # 100 is VM id

qm shutdown 100

qm reboot 100
```

### Reset VM

```bash
qm reset 100 # abrupt reboot

qm stop 100 # abrupt shutdown
```

### Start at boot

```bash
qm set --onboot 0 100 # 0 for no
```

### VM config

```bash
# list all config settings
qm config 100

# example
qm config 100 | grep cores
qm set --cores 2 100
qm config 100 | grep memory
qm set --memory 2048 100
```

