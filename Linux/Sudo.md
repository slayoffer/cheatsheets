### Если вы потеряли доступ к `sudo`, не расстраивайтесь — это более чем нормально. Чтобы вернуть доступ, вам нужно:

```bash
1. Запустить два сеанса `ssh`.
2. В первом сеансе запустить `echo $$`.
3. Во втором сеансе запустить `pkttyagent --process <PID ПРОЦЕССА С ПРЕДЫДУЩЕГО ШАГА>`.
4. Вернуться в первый терминал и запустить `pkexec visudo`.
```

### Sudo

```bash
# make user sudo
sudo gpasswd -a username sudo
# or
sudo usermod -aG sudo username

# check user privileges and allowed cmds
sudo -l

# rerun last command with sudo
sudo !!

# check sudo group
sudo cat /etc/sudoers
# or
sudo visudo
# look for "Allow members of group sudo to execute any command %sudo ALL=(ALL:ALL) ALL"

root ALL=(ALL:ALL) ALL # for user
%sudo  ALL=(ALL) NOPASSWD: ALL # for group
# First ALL means that root can use sudo from any terminal
# Second ALL means - root can impersonate any user
# Third ALL - can impersonate any group
# Fourth ALL - can use any commands

charlie ALL=(ALL:ALL) /sbin/reboot,/sbin/shutdown # user charlie can only exec reboot and shutdown cmds

charlie ubuntu=(ALL:ALL) /usr/bin/apt # can only use terminal on ubuntu server and can only use apt cmds

charlie ubuntu= /usr/bin/apt # cant impersonate users or groups

charlie ubuntu=(dscully:admins) ALL # can impersonate user dscully and group admins

# in case errors in sudoers file PRESS:
- e to edit again
- q to save changes
- x to revert changes

# if syntax error, it will ask "WHAT NOW?"" when try to exit
# need to type e (for edit) and fix syntax errors
```

### Much safer option than edit etc/sudoers file

```bash
# if need to give all the group sudo permissions (slayo needs to have sudo rights)

# go to sudoers list file
cd /etc/sudoers.d/
# make copy of sudoer file
cp slayo slayogroup

# edit the copy (replace with new user or group)
vim slayogroup # replace slayo with %slayogroup or username in the file
```

### Prevent su users from switching to other users

```bash
# To prevent switching users by sudo su -, you need to disable root ability to su to any user
sudo vim /etc/pam.d/su 
# and comment this line:
auth       sufficient pam_rootok.so # this line allows root to su without passwords.

# Or can restrict sudo usage to a limited set of commands which dont include su and force user-switching via sudo -i -u/sudo -s -u, and enable the targetpw option in sudoers
```

### Aliases

```bash
# add to sudoers file e.g. /etc/sudoers.d/developers

# Command alias
Cmnd_Alias	START_GREETING    = /bin/systemctl start greeting , \
				    /bin/systemctl start greeting.service
Cmnd_Alias	STOP_GREETING     = /bin/systemctl stop greeting , \
				    /bin/systemctl stop greeting.service
Cmnd_Alias	RESTART_GREETING  = /bin/systemctl restart greeting , \
				    /bin/systemctl restart greeting.service

# Host Alias
Host_Alias      LOCAL_VM = {{ hostvars[inventory_hostname]['ansible_default_ipv4']['address'] }}
# User specification
%developers LOCAL_VM = (root) NOPASSWD: START_GREETING, STOP_GREETING, \
	    	       RESTART_GREETING, \
		       sudoedit {{ greeting_application_file }} # lets to edit this file and makes auto copy of it

# check
sudoedit /opt/website.py
```



### Logs

```bash
sudo journalctl -e /usr/bin/sudo
# or
sudo nano /home/slayo/.bash_history # this will show all CMDs
# or
sudo grep sudo /var/log/auth.log

# find users who tried elevated cmds without sudo rights
grep "user NOT in sudoers" /var/log/auth.log
```

