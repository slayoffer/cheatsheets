### Install

```bash
sudo snap install lxd

sudo usermod -aG lxd slayo

# initialize install
lxd init # most default anasers are fine
```

### Run CT

```bash
lsc launch ubuntu:22.04 mycontainer
```

### Get into CT

```bash
lxc exec mycontainer bash

# or with user
lxc exec mycontainer -- su --login ubuntu # login as user ubuntu with sudo rights

# to get out
Ctrl + d
# or
exit
```

### Popular CMDs

```bash
# list the containers
lxc list

# Start a container
lxc start <container>

# Stop a container
lxc stop <container>

# Remove a container
lxc delete <container>

# List the downloaded images
lxc image list

# Remove an image
lxc image delete <image_name>
```

### Set CT auto boot

```bash
lxc config set mycontainer boot.autostart 1
```

### Create network profile and give CT public network access

```bash
# NEEDS bridge (br01) set up in NETPLAN beforehand!

lxc profile create external

lxc profile edit external

# replace text
description: External access profile
devices:
  eth0:
  name: eth0
  nictype: bridged
  parent: br0
  type: nic

# run CT
lxc launch ubuntu:22.04 mynewcontainer -p default -p external

# to add network to already running CT
lxc profile add mycontainer external
```

