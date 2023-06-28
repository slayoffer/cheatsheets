### Install

```bash
# Download Ubuntu (replace with the url of the one you chose from above)
wget https://cloud-images.ubuntu.com/jammy/current/jammy-server-cloudimg-amd64.img
# Create a new virtual machine
qm create 8000 --memory 2048 --core 2 --name ubuntu-cloud --net0 virtio,bridge=vmbr0
# Import the downloaded Ubuntu disk to local-lvm storage
qm importdisk 8000 focal-server-cloudimg-amd64.img local
# Attach the new disk to the vm as a scsi drive on the scsi controller
qm set 8000 --scsihw virtio-scsi-pci --scsi0 local:8000/vm-8000-disk-0.raw
# Add cloud init drive
qm set 8000 --ide2 local:cloudinit
# Make the cloud init drive bootable and restrict BIOS to boot from disk only
qm set 8000 --boot c --bootdisk scsi0
# Add serial console
qm set 8000 --serial0 socket --vga serial0
# DO NOT START YOUR VM
# Create template
qm template 8000

# Now, configure hardware and cloud init, then create a template and clone. If you want to expand your hard drive you can on this base image before creating a template or after you clone a new machine. I prefer to expand the hard drive after I clone a new machine based on need.

# Clone template.
qm clone 8000 135 --name yoshi --full

```

## Troubleshooting

If you need to reset your machine-id

```bash
sudo rm -f /etc/machine-id
sudo rm -f /var/lib/dbus/machine-id
```

Then shut it down and do not boot it up. A new id will be generated the next time it boots. If it does not you can run:

```bash
sudo systemd-machine-id-setup
```

### Purge

```bash
sudo cloud-init clean
sudo rm -rf /var/lib/cloud/instances

# and do this too, but only if you plan to make template out of this server instance (on Proxmox, etc)
sudo truncate -s 0 /etc/machine-id
sudo rm /var/lib/dbus/machine-id
sudo ln -s /etc/machine-id /var/lib/dbus/machine-id
ls -l /var/lib/dbus/machine-id





```