### Install

```bash
# check install
dpkg --get-selections | grep cloud-init

# install
sudo apt install cloud-init
```

### Edit config

```bash
sudo cp cloud.cfg cloud.cfg.bak
sudo vim /etc/cloud/cloud.cfg 
```

### Regenerate ssh host keys and machine id (before template)

```bash
cd /etc/ssh

ls

sudo rm ssh_host_*

cat /etc/machine-id

sudo truncate -s 0 /etc/machine-id

ls -l /var/lib/dbus/machine-id 
# if its link then all good, if not then create symlink
sudo ln -s /etc/machine-id /var/lib/dbus/machine-id
```

### Regenerate host keys on new VM (if needed)

```bash
# EASY WAY - do right after vm install from template
sudo rm /etc/ssh/ssh_host_*
sudo dpkg-reconfigure openssh-server

# HARD WAY - FYI
vim regenerate_ssh_host_keys.service

# insert code for reset host service file
[Unit]
Description=Regenerate SSH host keys
Before=ssh.service
ConditionFileIsExecutable=/usr/bin/ssh-keygen
 
[Service]
Type=oneshot
ExecStartPre=-/bin/dd if=/dev/hwrng of=/dev/urandom count=1 bs=4096
ExecStartPre=-/bin/sh -c "/bin/rm -f -v /etc/ssh/ssh_host_*_key*"
ExecStart=/usr/bin/ssh-keygen -A -v
ExecStartPost=/bin/systemctl disable regenerate_ssh_host_keys
 
[Install]
WantedBy=multi-user.target

# change permissions
sudo chown root:root regenerate_ssh_host_keys.service

# move file
sudo mv regenerate_ssh_host_keys.service /etc/systemd/system/

# reload system daemons
sudo systemctl daemon-reload

# start the service
sudo systemctl enable regenerate_ssh_host_keys.service
```

### Clean server (before template)

```bash
sudo apt clean

sudo apt autoremove

sudo poweroff
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