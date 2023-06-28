### Regenerate host keys

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