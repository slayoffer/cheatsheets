## Installing the latest version of Ansible

Most distributions have an older version of Ansible installed. This is usually fine except sometimes you may need to use features from the latest Ansible. Use the following commands to update Ansible to the latest version.

Check version

```
ansible --version
```

If it’s not the version you are looking for, check to see where it is installed

```
which ansible
```

If it lives somewhere like

```
/usr/bin/ansible
```

this is most likely due to your distribution installing it there.

Remove previous version

```
sudo apt remove ansible
```

Check to be sure it is removed

```
which ansible
```

You should see

```
ansible not found
```

check to see that you have `python3` and `pip`

```
python3 -m pip -V
```

You should see something like

```
pip 22.3.1 from /home/user/.local/lib/python3.8/site-packages/pip (python 3.8)
```

Install `pip` if the previous couldn’t find the `pip` module

```
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
```

Install ansible

```
python3 -m pip install --user ansible
```

Confirm your version with

```
ansible --version
```
> *Note: Most distributions include an “older” version of Ansible. If you want to install the latest version of Ansible, see [installing the latest version of ansible](https://docs.technotim.live/posts/ansible-automation/#installing-the-latest-version-of-ansible)*

```
hosts

[ubuntu]
server-01
server-02
192.168.0.100
192.168.0.1002
```

## commands

check ansible version

```
ansible --version
```

command with module

```
ansible -i ./inventory/hosts ubuntu -m ping --user someuser --ask-pass
```

command with playbook

```
ansible-playbook ./playbooks/apt.yml --user someuser --ask-pass --ask-become-pass -i ./inventory/hosts
```

## playbooks

```yml
# apt.yml

- hosts: "*"
  become: yes
  tasks:
    - name: apt
      apt:
        update_cache: yes
        upgrade: 'yes'

# qemu-guest-agent.yml

- name: install latest qemu-guest-agent
  hosts: "*"
  tasks:
    - name: install qemu-guest-agent
      apt:
        name: qemu-guest-agent
        state: present
        update_cache: true
      become: true
      
# zsh.yml

- name: install latest zsh on all hosts
  hosts: "*"
  tasks:
    - name: install zsh
      apt:
        name: zsh
        state: present
        update_cache: true
      become: true
      
# timezone.yml

- name: Set timezone and configure timesyncd
  hosts: "*"
  become: yes
  tasks:
  - name: set timezone
    shell: timedatectl set-timezone Asia/Vladivostok

  - name: Make sure timesyncd is stopped
    systemd:
      name: systemd-timesyncd.service
      state: stopped

  - name: Copy over the timesyncd config
    template: src=../templates/timesyncd.conf dest=/etc/systemd/timesyncd.conf

  - name: Make sure timesyncd is started
    systemd:
      name: systemd-timesyncd.service
      state: started

# timesyncd.conf

#  This file is part of systemd.
#
#  systemd is free software; you can redistribute it and/or modify it
#  under the terms of the GNU Lesser General Public License as published by
#  the Free Software Foundation; either version 2.1 of the License, or
#  (at your option) any later version.
#
# Entries in this file show the compile time defaults.
# You can change settings by editing this file.
# Defaults can be restored by simply deleting this file.
#
# See timesyncd.conf(5) for details.

[Time]
NTP=192.168.0.4
FallbackNTP=time.cloudflare.com
#RootDistanceMaxSec=5
#PollIntervalMinSec=32
#PollIntervalMaxSec=2048

# cloud template fix
- name: Add manage_etc_hosts string to /etc/cloud/cloud.cfg
  become: true
  lineinfile:
    path: /etc/cloud/cloud.cfg
    line: 'manage_etc_hosts: false'
    state: present
```

How to configure NTP server on VM:
- Install the systemd-timesyncd service or another NTP client, such as chrony or ntpd, if it's not already installed.
- Edit the NTP configuration file (e.g., /etc/systemd/timesyncd.conf for systemd-timesyncd) and set the desired NTP server(s) and other settings, as needed.
- Restart the NTP service to apply the changes.