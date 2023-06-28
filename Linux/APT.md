| apt commands cheatsheet | https://itsfoss.com/ap t-command-guide/ |
| ----------------------- | --------------------------------------- |
| apt search PACKAGE      | search from available repositories      |
| apt install PACKAGE -y  | To Install Packages                     |
| apt install apache2 -y  | To Install apache2                      |
| apt reinstall PACKAGE   | To reinstall PACKAGE                    |
| apt remove PACKAGE      | To remove PACKAGE                       |
| apt update PACKAGE      | update only a package                   |
| apt grouplist                | List all available Group Packages                            |
| apt groupinstall "GROUPNAME" | Installs all the packages in a group.                        |
| apt repolist                 | List Enabled apt Repositories                                |
| apt clean all                | Clean apt Cache                                              |
| apt history                  | View History of apt                                          |
| apt show package name | Shows the information of package like version, size, source, repository etc |

### Old way

```bash
apt-get install
```

### Install with suggested

```bash
apt --install-suggests apache2
```

### Auto update

```bash
sudo apt install unattended-upgrades

dpkg-reconfigure --priority=low unattended-upgrades
```

### Update packages list from Ubuntu apps repo

```bash
sudo apt update
```

### Upgrade all packages

```bash
sudo apt upgrade
# or old way
sudo apt dist-upgrade
```

### Update apps & kernel

```bash
# with Kernel update - optimal 
sudo apt update && sudo apt upgrade

# with Kernel update and old packages/deps removal
sudo apt update && sudo apt full-upgrade
```

### Upgrade release

```bash
# update apps
sudo apt update && sudo apt upgrade

# reboot and make snapshot
sudo do-release-upgrade

# open 1022 port for extra ssh access
sudo iptables -I INPUT -p tcp --dport 1022 -j ACCEPT

# check upgrade errors
sudo less /var/log/dist-upgrade/main.log

# check postgres leftovers
sudo apt list --installed | grep postgresql

# edit upgrade blacklist
sudo vim /usr/share/ubuntu-release-upgrader/removal_blacklist.cfg

# Skipping acquire of configured file error
# add "[arch=amd64]" to .lists and "Architectures: amd64" to .sources files in "/etc/apt/sources.list.d/"
```

### Clean apt database

```bash
sudo apt clean
```

### Search apt

```bash
apt search telnet
# or old way
apt-cache search telnet
```

### Total app remove

```bash
sudo apt purge apache2
# or old way
sudo apt remove --purge apache2
```

### Show apps info

```bash
apt show apache2
# or
apt list | grep apache2
# or old way
apt-cache show apache2
```

### Adding additional repos

```bash
# main repos list
/etc/apt/sources.list

# additional list
/etc/apt/sources.list.d/ # better add new lists here via txt files

# cmd to edit repos
apt edit-sources

# main for supported apps
# restricted for supported but with bad license
# universe for community supported apps
# multiverse for not supported, risky

# add PPA repo
sudo apt-add-repository ppa:username/myawesome_software-1.0
# will add txt file to /etc/apt/sources.list.d/ for that repo
```

### Add newest apt repo (backports)

```bash
sudo vim /etc/apt/sources.list

# add line
deb http://deb.debian.org/debian bullseye-backports main contrib
```

### Install with specific repo

```bash
apt install -t bullseye-backports cockpit
```

### GNU Privacy Guard (GnuPG) for checking app signatures

```bash
sudo apt install gnupg
```

### Package restorer

```bash
sudo apt install dselect
sudo dselect update
sudo dpkg --set-selections < packages.list
sudo apt-get deselect-upgrade
```

### Apt autoremove

```bash
sudo apt autoremove
```

### Update and get newest drivers for hardware (HWE)

```bash
sudo apt install --install-recommends linux-genereic-hwe-22.04 # check & change version as needed
```

### Old cmds

```bash
apt-cache search [ package ]
# Вывести список пакетов, чье имя совпадает со строкой package

apt-get check
# Проверить зависимости

apt-cdrom install [ package ]
# Установить / обновить пакет с cdrom'а

apt-get install [ package ]
# Установить / обновить пакет

apt-get upgrade
# Обновить установленные в систему пакеты

apt-get remove [ package ]
# Удалить установленный пакет из системы, сохранив файлы конфигурации

apt-get update
# Обновить списки пакетов репозитария

apt-get clean
# Удалить загруженные архивные файлы пакетов
```