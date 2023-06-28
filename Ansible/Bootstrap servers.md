### Install

```bash
# for ubuntu
sudo apt install ansible

# for CentOS
sudo yum install python3-pip
pip3 install ansible
```

### Process

```bash
# 1) Install Ansible on a central server or workstation

# 2) Create a user for Ansible on each machine you want to manage configuration on

# 3) Create the same user on your central server or your local machine

# 4) Set up the Ansible user on the server so that it can connect to clients via SSH without a password

# 5) Configure sudo on the client machines so that the Ansible user can execute commands with sudo with no password

# Assuming you named your Ansible user ansible, create the following file:
/etc/sudoers.d/ansible

# Inside that file, place the following text:
ansible ALL=(ALL) NOPASSWD: ALL

# Next, we need to ensure that the file is owned by root:
sudo chown root:root /etc/sudoers.d/ansible

# Finally, we need to adjust the permissions of the file:
sudo chmod 440 /etc/sudoers.d/ansible

# Go ahead and test this out. On the server, switch to the ansible user:
sudo su - ansible

# Then, to test this out, use SSH to execute a command on a remote machine:
ssh 192.168.1.123 sudo ls /etc
```

### Check and Send Motd (Message of the day) for worker server test

```bash
# ping
ansible all -m ping

# create it inside ansible dir
echo "Hello World" > motd

tasks:
- name: copy SSH motd
	ansible.builtin.copy
		src: motd
		dest: /etc/motd
```

