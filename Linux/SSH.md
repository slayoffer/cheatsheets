### Check SSH

```bash
which ssh
# to check ssh daemon process (server component of SSH)
which sshd
```

### Install SSH

```bash
apt install openssh-client
systemctl enable ssh # to autoload on boot
```

### WARNING: UNPROTECTED PRIVATE KEY FILE

```bash
sudo chmod 600 ~/.ssh/rubius_rsa_key
sudo chmod 600 ~/.ssh/id_rsa.pub

# If you are getting another error:
# This means that the permissions on that file are also set incorrectly, and can be adjusted with this:
sudo chmod 644 ~/.ssh/known_hosts

# Finally, you may need to adjust the directory permissions as well:
sudo chmod 755 ~/.ssh
```



### **Generate new ssh key in the ~/.ssh/ folder of the server**

```bash
# simplest way which will generate RSA key (not the most secure)
ssh-keygen

# with filename
ssh-keygen -t rsa -f ~/.ssh/ssh_key

# for most secure way
ssh-keygen -t ed25519
ssh-keygen -t ed25519 -C "email or username or server name or whatever" # -C for comment

# for old systems which dont support ed25519
ssh-keygen -b 4096
# or
ssh-keygen -t rsa -b 4096 -C "email"

-t — тип ключа
-b — длина ключа в битах (по умолчанию 3072 для RSA)
-f — путь к файлу ключа (по умолчанию ~/.ssh/id_rsa)
-C — комментарий к ключу (по умолчанию username@hostname)
-P — пароль для доступа к ключу 
```

### Change SSH private key password

```bash
# should work
ssh-keygen -p -f ~/.ssh/id_dsa

# may work
openssl rsa -des3 -in private_key.pem -out private_key_encrypted.pem
```

### Protect key

```bash
# to get rid of "WARNING: UNPROTECTED PRIVATE KEY FILE!"
chmod 600 ~/.ssh/id_rsa
```

### **Connect server **

```bash
ssh -i ~/.ssh/id_rsa user@server
# with port
ssh -p 2222 -i ~/.ssh/id_rsa user@server
# with user
ssh -l slayo hostname
```

### **Adding an SSH key to the SSH Agent**

```bash
# start the ssh-agent in the background
eval `ssh-agent -s`
# or
eval "$(ssh-agent)"
# then it shows Agent PID
# then add key to agent
ssh-add ~/.ssh/id_rsa

# then check if the agent successfully stored the key by listing all identities
ssh-add -l
```

### Copy public SSH key to remote server

```bash
# copy default id_rsa.pub key
ssh-copy-id devops@web03
# or
ssh-copy-id -i ~/.ssh/proxmox_id_ed25519.pub -p 2222 root@192.168.1.38

# or copy content of pub key and go to server user's .shh folder
mkdir .ssh
cd .ssh
sudo vim authorized_keys # paste pub key here

# or old way
$ scp $HOME/.ssh/id_rsa.pub nixcraft@server1.cyberciti.biz:~/.ssh/authorized_keys # warning this will overwrite existing file on the remote box

# or
ssh serveruser@servername "cat ~/.ssh/id_dsa.pub >> ~/.ssh/authorized_keys"
```

### Disable root & password login and protect server

```shell
sudo vim /etc/ssh/sshd_config
# change PermitRootLogin and PasswordAuthentication to no
# can also change port 22 to 2222 to protect server from hacker bots
sudo systemctl restart sshd
```

### Disable password for user

```bash
# get in sudoers file
sudo visudo
# then add
slayo ALL=(ALL:ALL) NOPASSWD: ALL
```

### **To issue SSH keys under the user you need to change ownership of the .ssh folder from root to user**

```bash
sudo chown -R slayo:slayo /home/slayo
```

### Config

```bash
sudo vim /etc/ssh/sshd_config
```

### SSH config file example

```bash
Host slayo
  Hostname 192.168.1.38
  Port 2222
  User slayo
  IgnoreUnknown UseKeychain # IgnoreUnknown is for Linux to work, as it only works in Mac OS
  AddKeysToAgent yes
  IdentityFile ~/.ssh/ubuntu_id_rsa
 
Host devops
  Hostname 192.168.1.38
  Port 2222
  User devops
  IgnoreUnknown UseKeychain
  AddKeysToAgent yes
  IdentityFile ~/.ssh/ubuntu_id_ed25519

Host yandex
  Hostname 158.160.40.92
  Port 22
  User student
  IgnoreUnknown UseKeychain
  AddKeysToAgent yes
  IdentityFile ~/.ssh/yandexvm_id_ed25519
  # student user password: 50cent4life
  
  # Для подключения к разным хостам можно задавать общие настройки:
StrictHostKeyChecking no
UserKnownHostsFile=/dev/null
#RequestTTY force
#LogLevel=quiet 

# Уникальные настройки для хоста gitlab-runner:
Host gitlab-runner
  HostName 10.20.30.40
  User ya
  ForwardAgent yes
  IdentityFile ~/.ssh/id_rsa
  ProxyJump jump 
  
# Настройки для всех хостов, кроме gitlab-runner:
Host * !gitlab-runner
  ControlMaster auto
  ControlPath ~/.ssh/sockets/%r@%h-%p
  ControlPersist 600
  StrictHostKeyChecking no
  UserKnownHostsFile=/dev/null
  ServerAliveInterval 30
  ServerAliveCountMax 25
  ForwardAgent yes
  
sudo systemctl restart ssh
```

### Troubleshooting SSH

```bash
ssh -v slayo@server # -v for verbose - detailed log of logging in

# check .ssh folder permissions
# it should be drwx------ (700) - only accessible by the user

# check auth logs (dynamically)
journalctl -fu ssh # -u for unit (service), -f for follow (dynamic)

# old way
tail -f /var/log/auth.log # -f for follow
```

### Protection tips

```bash
# 1. Stop and disable sshd service in case of long absence from server

# 2. Change the SSH listen port from 22 to 2222 to protect it from bots
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak

sudo systemctl restart sshd

# 3. Disable root ssh access
sudo vim /etc/ssh/sshd_config
PermitRootLogin no

# 4. Disable password auth
sudo vim /etc/ssh/sshd_config
PasswordAuthentication no

# 5. Restrict access to personal IP via firewall

# 6. Hardware SSH key (usb token)
```

### If config doesnt apply

```bash
sudo apt purge openssh-server
sudo rm -rf /etc/ssh # may be necessary if using apt remove
sudo apt install openssh-server
```

### Azure Devops SSH auth failed issue

```bash
# open
sudo vim /etc/ssh/sshd_config
# add
PubkeyAcceptedKeyTypes=+ssh-rsa
```

