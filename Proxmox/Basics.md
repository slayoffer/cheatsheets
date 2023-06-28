### Add free update repo

```bash
# Disable the enterprise repository
vi /etc/apt/sources.list.d/pve-enterprise.list
# by commenting the line
#deb https://enterprise.proxmox.com/debian/pve bullseye pve-enterprise
# and enable the "no-subscription" repository by creating a new file
vi /etc/apt/sources.list.d/pve-no-subscription.list
# with
deb http://download.proxmox.com/debian/pve bullseye pve-no-subscription

# Убираем всплывающее окно о подписке
# Сохраняем резервную копию файла:
cp /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js.bak
# Подаем команду:
sed -i "s/getNoSubKeyHtml:/getNoSubKeyHtml_:/" /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js
# Команда заменяет нужную переменную getNoSubKeyHtml: на ошибочную getNoSubKeyHtml_: и окно более нам не мешает.

# Если вы устанавливаете Proxmox другой версии, то версия базовой ОС Debian может отличаться от stretch. В этом случае вам необходимо указать аналогичный тому, что есть в pve-enterprise.list.
# Также узнать имя дистрибутива можно командой:
cat /etc/*-release | grep VERSION_CODENAME
# Ответ будет примерно такой:
VERSION_CODENAME=stretch
# К примеру уже 6-я версия PVE – buster а не stretch
# А 7-я версия PVE – bullseye
```

### Update container templates registry

```bash
pveam update
```

### Show containers in registry

```bash
pveam available
```

### Download CT from registry

```bash
pveam download /local <CT name>
```



### QEMU guest agent

```bash
# enable in Proxmox GUI (VM options)
# reboot VM
# install
sudo apt install qemu-guest-agent
```

