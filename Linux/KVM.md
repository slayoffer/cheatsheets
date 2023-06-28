### Check whether system supports VM

```bash
egrep -c '(vmx|svm)' /proc/cpuinfo # should be 1 and above if supports
```

### KVM images dir

```bash
/var/lib/libvirt/images
```

### Install

```bash
sudo apt install bridge-utils libvirt-clients libvirt-daemon-system qemu-system-x86
sudo systemctl stop libvirtd

# check for groups kvm and libvirt, if not present
sudo groupadd kvm
sudo groupadd libvirt
# add your user to these groups
sudo usermod -aG kvm slayo
sudo usermod -aG libvirt slayo

# set images dir for group permissions
sudo chown :kvm /var/lib/libvirt/images
sudo chmod g+rw /var/lib/libvirt/images

sudo systemctl start libvirtd
sudo systemctl status libvirtd

# install vm GUI if not installed (may not be possible to install on headless OS)
sudo apt install ssh-askpass virt-manager
```

### Configure virt manager

```bash
# run
virt-manager

# set up KVM host in File > Add Connection

# add ISO storage pool
Name: ISO
Target Path: /var/lib/libvirt/images/ISO

# set ISO dir for group permissions
sudo chown :kvm /var/lib/libvirt/images/ISO
sudo chmod g+rw /var/lib/libvirt/images/ISO
```

### Use via CLI

```bash
# show running VMs
virsh list
# for all
virsh list --all

# cmds
virsh start vm-name
virsh shutdown vm-name
virsh suspend vm-name
virsh resume vm-name
virsh destroy vm-name # deletes VM
virsh undefine vm-name # deletes VM but not the files

# virsh can also create VMs, etc
```

