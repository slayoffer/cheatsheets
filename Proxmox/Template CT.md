### Basics

```bash
sudo apt clean

sudo apt autoremove

cd /etc/ssh

sudo rm ssh_host_*

sudo truncate -s 0 /etc/machine-id
```

### Regenerate host keys after creation of new CT

```bash
sudo rm /etc/ssh/ssh_host_*
sudo dpkg-reconfigure openssh-server
```

