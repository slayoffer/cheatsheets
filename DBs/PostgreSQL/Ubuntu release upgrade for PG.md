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

# remove postgis'es
sudo apt purge postgresql-13-postgis-3

# edit upgrade blacklist
sudo vim /usr/share/ubuntu-release-upgrader/removal_blacklist.cfg

# if error "Skipping acquire of configured file" - add "[arch=amd64]" to .lists and "Architectures: amd64" to .sources files in "/etc/apt/sources.list.d/"