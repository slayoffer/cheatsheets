### Handy examples

```bash
sudo adduser --system --shell /bin/bash -U 10001 --no-create-home opensearch
sudo groupadd opensearch
sudo usermod -aG opensearch opensearch
mkdir -p /home/opensearch
sudo chown -R opensearch /home/opensearch
```

### To check username

```bash
whoami
```
### Switch user

```bash
# switch to root user
su – root
# or
sudo -i
# or
su -
# or
sudo -s

# switch to another user - can only be done via root without password
su - slayo

# to switch to any user without password reset
sudo su - username
```

### Logout user

```bash
exit
```

### Last logged in users list

```bash
last

# show bad login attemtps
sudo lastb -adF # -a for hostname, -d for dns & ip. -F for times

# show and follow logging stats list
sudo tail -f /var/log/auth.log # Ubuntu, -f for follow (live stats update)
```

### Check existence of user or group

```bash
getent passwd slayo

getent group slayo
```

### Check user permissions

```bash
id
# or
sudo -l
# or
groups
# or
umask # default output is 002

# To calculate the default directory creation permission you need to subtract this default output number from 0777. Once you do that you get 0775 so it means when you create a directory, by default it will have 0772 permission

# Similarly, to calculate the default file creation permission you need to subtract the umask value from 0666. So it will be 0666-0002 = 0664. It means whenever you will create a file, by default it will have 0664 permission.
```

### Current users list

```bash
# show logged in users
who

# user's detailed info and processes list
w
# or
yum install lsof -y
lsof -u slayo 

# check users folders
ls -l /home

# all users list including system ones
cat /etc/passwd

# count all users
cat /etc/passwd | wc -l # wc for word count
# or
getent passwd | wc -l

# list only usernames
awk -F':' '{ print $1}' /etc/passwd
# or
cut -d : -f 1 /etc/passwd
# or
getent passwd
# or
getent passwd | awk -F: '{ print $1}'
# or
getent passwd | cut -d: -f1
# or
compgen -u
# or one name
getent passwd | grep slayo
```

### Run cmd as other user

```bash
sudo apt update -u slayo # run apt update as user slayo (impersonation)
```

### Run cmd as other group

```bash
sg admin ls
```

### Send message to user

```bash
write <username>
```

### Delete user

```bash
sudo userdel slayo # home dir will stay

sudo userdel -r slayo # will delete with home dir

# if forgot to remove the user folder
sudo rm -r /home/username
```

### Add new user

```bash
# UBUNTU ONLY - will auto create folders, etc
sudo adduser slayo 

# create user and join group
sudo adduser slayo --group devops

# create user and prevent from logging in (for system users)
sudo adduser slayo -s /bin/false

# for other linux distros
sudo useradd slayo

# can use with options
sudo useradd -d /home/jdoe -m jdoe # -d for home dir, -m for the home dir to be created
# or just
sudo useradd -m jdoe

# /etc/skel is a dir contents of which is copied to every new user home dir by default
# can store files there to automatically add them to new users /home dirs (welcome letter, etc)

# with multiple options
useradd -u 1009 -g 1009 -d /home/robert -s /bin/bash -c "Mercury Project member" bob
```

### Create system user for cron jobs, etc

```bash
sudo useradd -r sysuser # -r for system user tag (will assign UID < 1000, will not show on the login screen)
# or
sudo useradd --system sysuser
# without home dir
sudo useradd --system --no-create-home sysuser
```

### Passwords hash file (stores passwords in hashed format)

```bash
sudo cat /etc/shadow

# format
USERNAME:PASSWORD:LASTCHANGE:MINAGE:MAXAGE:INACTIVE:EXPDATE
```

### Add to group (usermod)

```bash
# add user as admin to sudoers group
sudo usermod -aG sudo slayo # -a for append (will replace all groups without this flag), -G for secondary group modify (if want to change primary group then put -g flag instead of -G)

# add user to other group
sudo usermod -aG docker slayo # -aG for add to group

# add to several groups
sudo usermod -aG admin,sudo slayo

# add via gpasswd
sudo gpasswd -a slayo devops # add slayo to devops

# can also add users via VIM
vim /etc/passwd # add users to the group name, coma separated
```

### Give user full ownership to dir

```bash
# give user ansible and gorup devops ownership of devopsdir dir
chown ansible:devops /opt/devopsdir

# change ownership without pointing group
sudo chown -R jay: Downloads/ # will change group to default user's group (jay)

# can be done recursive BUT DANGEROUS AS WILL CHANGE ALL SUBFOLDERS
$ chown -R ansible:devops /opt/devopsdir

# change ownership of Downloads folder to user batman
sudo chown -R batman Download/
```

### Change group of the file/dir

```bash
sudo chgrp sales myfile.txt # sets file group to sales group
sudo chgrp sales -R /mydir # with subdirs
```



### Take/give permission (symbolic method)

```bash
# give user, group and others exec rights
chmod +x file

# take away exec permission (-x) from Others group (o)
chmod o-x /opt/devopsdir

# take away read permission (-r) from Others group (o)
chmod o-r /opt/devopsdir

# give write permission (+w) to the user group (g)
chmod g+w /opt/devopsdir

# grant read access to all for file
chmod ugo+r file

# deny write and exec to others for dir
chmod o-wx dir

# Numeric method
# 4 for read
# 2 for write
# 1 for execute

# read and write for user, read for group, nothing for others
chmod 640 myfile

# full access to dir for user and group, no access to others
chmod 770 /opt/webdata

# full access to dir for user and group, no access to others - RECURSIVE (with sub dirs)
chmod -R 770 /opt/webdata

# make rights only for subdirs without main dir
chmod 600 Downloads/*

# safe way to change sub files rights without changing permissions on dir
find /path/to/dir/ -type f -exec chmod 644 {} \; # finds all the files (-type f) in dir and executes (-exec) chmod on them

#options
-R # recursive
-v # verbose (will print out results)
--reference # reference another file for its mode
```

### Check user permissions, groups, etc

```bash 
id brad

#
id
```

### Show groups

```bash
groups # current user

groups slayo

# show all the existing groups
cat /etc/group
# show group names
cut -d : -f 1 /etc/group

# format
NAME:PASSWORD:GID:MEMBERS
```

### Create group

```bash
groupadd devops

# with custom GID
groupadd -g 1011 devops

# create system group
groupadd --system devops
```

### Delete group

```bash
groupdel devops
```

### Remove user from group

```bash
sudo gpasswd -d slayo devops # remove slayo from devops
```

### Lock/unlock user account

```bash
sudo passwd -l slayo # -l for lock, but doesnt lock in case user has SSH access (need to remove his SSH key)
#or
sudo usermod -L slayo

# unlock user
sudo passwd -u slayo # -u for unlock
# or
sudo usermod -U slayo
```

### Lock root user log in access

```bash
sudo passwd -l root
```

### Change users home dir and name

```bash
# change jdoe home dir to /home/...
sudo usermod -d /home/jsmith jdoe -m # -m for --move-home arg (move contents of old home dir)

# change jdoe name from jdoe to jsmith
sudo usermod -l jsmith jdoe 
```

### Set/reset password

```bash
sudo passwd slayo
# reset can be done only via root user

sudo passwd -S slayo # to check when the password was changed last time

sudo passwd # set password for current user
```

### Ser very strong password

```bash
sudo apt install whois pwgen

pass=`pwgen --secure --capitalize --numerals --symbols 121`

echo $pass | mkpasswd --stdin --method=sha-512; echo $pass
# will show two lines, one for hash another for password itself
# copy hashed password to Ansible, etc
```



### Check/set account/password expiration date

```bash
# check account, password expiration dates, etc
sudo chage -l slayo

# set account exp date (will block user access after the date)
sudo chage -E 2022-12-01 slayo # -E for expiration date
# or
sudo usermod slayo -e 2022-12-31

# set immediate password reset upon login (0 days)
sudo chage -d 0 slayo 

# set password reset in 90 days
sudo chage -M 90 slayo

# remove expire date
sudo chage -M -1 slayo

# set the minimum (-m) number of days before new password change is available
sudo chage -m 5 slayo
```

### Set password policy (PAM)

```bash
sudo apt install libpam-cracklib # install PAM module

sudo vim /etc/pam.d/common-password # set policies in this file

# set to remember last 99 passwords and dont let choose them again
password required pam_pwhistory.so
remember=99 use_authok

# difok3 section sets how many chars have to be different at least
# obscure option prevents from using simple passwords
```

### SUID (euid add)

```bash
# it will allow to create files under root instead of user name

mkdir -p /tmp/test3 && cd /tmp/test3
cp /usr/bin/id .
sudo chown root.makky ./id
sudo chmod u+s,a+rx id
ls -la id # root is owner, not makky
-rwsr-xr-x 1 root makky 43560 Jun  7 13:26 id 
./id # euid added
uid=1000(makky) gid=1001(makky) euid=0(root) groups=1001(makky),10(wheel),48(docker),304(vboxusers),443(sudo)

# example
# У passwd по дефолту установлен SUID:
ls -la /bin/passwd
-rws--x--x 1 root root 60384 May 16 01:28 /bin/passwd 

# Когда пользователь меняет свой пароль командой /usr/bin/passwd, RUID пользователя остаётся прежним, а EUID станет root. Таким образом, процесс passwd может произвести запись в файлы /etc/passwd и /etc/shadow.
# Также passwd проверяет ruid пользователя, иначе вы могли бы поменять пароль любого пользователя в системе без каких-либо прав.
```

### SGID (egid add)

```bash
# SGID аналогичен SUID, но устанавливаются права группы владельца файла. Если SGID назначен директории, то создаваемые в директории объекты получат идентификатор группы владельца. То есть новые файлы и директории, создаваемые в директории с установленным SGID, будут наследовать права от директории-родителя.

# Рассмотрим пример:
$ mkdir -p /tmp/test4 && cd /tmp/test4
$ cp /usr/bin/id .
$ chmod g+s,a+rx id
$ ls -la id

-rwxr-sr-x 1 makky makky 43560 Jun  7 13:26 id 

# Числовой аналог g+s,a+rx — 2555.

$ ./id
uid=1000(makky) gid=1001(makky) egid=0(root) groups=0(root),10(wheel),48(docker),304(vboxusers),443(sudo),1001(makky)

# Аналогично SUID появился egid (Effective Group ID). То есть процесс выполнится с правами текущего пользователя и группой владельца.
$ cp /usr/bin/touch .
$ ls -la touch
-rwxr-xr-x 1 makky makky 96936 Sep 19 14:02 touch
$ sudo chown root.root touch
$ sudo chmod g+s,a+rx touch
$ ls -la touch
-rwxr-sr-x 1 root root 96936 Sep 19 14:02 touch
$ ./touch 123
$ ls -la 123
-rw-r--r-- 1 makky root 0 Sep 19 14:02 123 
```

### Sticky Bit

```bash
# sticky bit запрещает удаление файла при наличии достаточных прав, если вы не его владелец. Это правило не распространяется для root. Типичный пример использования /tmp или /var/tmp

# Назначается sticky bit chmod +t или в числовом выражении 1777

chmod +t [path_to_directory] 

# Sticky bit выглядит как буква t в конце первой колонки вывода ls -l. Если не задан eXecution bit для всех (others), то будет T.
```

### Access List (acl)

```bash
#Каждый файл в любой файловой системе будет иметь владельца, группу и права. Когда нескольким пользователям нужен доступ к одному и тому же файлу, а принадлежат они к разным группам, нам помогут расширенные права доступа или Access List (acl).

cat /boot/config* | grep _ACL

# Если знак + есть в списке прав, это значит, что у файла есть расширенные права
ls -l
-rw-rw-r--+ 1 makky makky 0 Jun  7 10:30 1
-rw-r--r--  1 makky makky 0 Jun  7 10:30 2

# Для просмотра дополнительных разрешений используется команда getfacl
getfacl file1
# file: file1
# owner: makky
# group: makky
user::rw-
group::r--
group:makky:rw- # ACL
group:max:rw- # ACL
mask::rw-
other::r-- 

# example
mkdir -p /tmp/test5 && touch /tmp/test5/file1
setfacl -m u:max:rw /tmp/test5/file1 # for user
setfacl -m -R g:max:rw /tmp/test5/file1 # for group, -R for recursive
# Опция -m заставляет команду setfacl модифицировать права на файл
setfacl -m u:max:rw,g:makky:rwx /tmp/test5/file1 # for user and group

# default rights
# Назначим права RW по умолчанию для вновь создаваемых файлов в директории для пользователя max:
sudo mkdir /tmp/data
sudo setfacl -m default:u:max:rw /tmp/data
sudo getfacl /tmp/data

# В default появилась запись default:user:max:rw-.
# Теперь создадим файл и проверим его расширенные права:
sudo touch /tmp/data/test
sudo getfacl /tmp/data/test

# сохранить конфиг расширенных прав:
cd /tmp/data
sudo getfacl -R * > /tmp/accounts_facl 

# Для удаления расширенных прав используется опция -x:
sudo setfacl -x u:max:rw /tmp/data/test 

# Для удаления всех расширенных прав используется опция -b:
$sudo setfacl -b /tmp/data/test 

# Убеждаемся, что расширенные права удалены:
sudo getfacl /tmp/data/test

# восстановить конфиг:
setfacl --restore=/tmp/accounts_facl

# Проверяем, что права вернулись:
sudo getfacl /tmp/data/test
```



### Command execution out of the server

```bash
ssh devops@web01 uptime
# will exec uptime cmd on web01 server without permanently logging into it
```



