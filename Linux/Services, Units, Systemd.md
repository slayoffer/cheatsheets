### Check init system

```bash
ls -l /sbin/init
# should show systemd in case new version OS
```

### Существующие типы юнитов systemd

```bash
- `target` — группирует модули
- `service` — отвечает за запуск сервисов (служб) и поддерживает вызов интерпретаторов для исполнения пользовательских скриптов
- `mount` — занимается монтированием файловых систем
- `automount` — автомонтирование файловых систем, используется при обращении к точке монтирования
- `swap` — отвечает за подключение файла подкачки
- `timer` — запускает модули по расписанию, аналог `cron`
- `socket` — запуск модуля при подключении к сокету
- `slice` — группировка других модулей в контейнер (дерево) `cgroups`
- `device` — использует реакцию на подключение какого-либо устройства
- `path` — запуск модуля по событию доступа по конкретному пути в файловой системе
```

### Targets (runlevels)

```bash
runlevel
# 5 for GUI mode - graphical.target
# 3 for CLI mode - multiuser.target

systemctl get-default
> graphical.target

ls -ltr /etc/systemd/system/default.target

# list targets
systemctl list-units --all

# change target
sudo systemctl set-default multi-user.target

# The term runlevels is used in the sysV init systems. These have been replaced by systemd targets in systemd based systems.

# The complete list of runlevels and the corresponding systemd targets:
runlevel 0 -> poweroff.target
runlevel 1 -> rescue.target
runlevel 2 -> multi-user.target
runlevel 3 -> multi-user.target
runlevel 4 -> multi-user.target
runlevel 5 -> graphical.target
runlevel 6 -> reboot.target
```



### Basic cmds

```bash
# check whether system has systemd on it
ps --no-headers -o comm 1

systemctl list-units --type service --all # просмотр всех юнитов в системе
systemctl list-unit-files --type service # список установленных юнит-файлов сервисов 
systemctl start name    # запустить сервис
systemctl stop name    # остановить сервис
systemctl restart name # перезапустить сервис
systemctl status name # посмотреть статус сервиса
systemctl reload name    # RECOMMENDED way - doesnt go down, but reloads the service settings
systemctl daemon-reload   # перечитать конфигурацию для всех
systemctl try-restart name    # перезапустить, если запущен
systemctl enable name    # включить автозапуск сервиса
systemctl disable name    # отключить автозапуск сервиса
# enable and then start
sudo systemctl enable --now httpd
```

### Show all the background services, service files, etc

```bash
sudo systemctl
# better way
sudo systemctl list-unit-files --type=service
# or
systemctl list-units
```

### Check process status

```bash
sudo systemctl status httpd

# long list status
sudo systemctl -l ssh
```
### Check whether service is active

```bash
systemctl is-active httpd
```

### Check whether service is enabled for the boot time

```bash
systemctl is-enabled httpd
```
### Soft system reload after setting service

```bash
sudo systemctl daemon-reload
```

### System reboot & shutdown

```bash
sudo reboot
# or
sudo systemctl reboot

# You can still specify a delayed time using the hh:mm format
# disadvantage with systemctl is the inability to delay the process.

# Use halt to halt the system without powering it off
sudo halt

# To power off the machine
sudo poweroff
# or
sudo shutdown -h now. 
# or
sudo systemctl poweroff
```

### UEFI

```bash
# reboot to check UEFI settings
systemctl reboot --firmware-setup

# service file for UEFI reboot
sudo vim /usr/share/applications/uefi-reboot.desktop

[Desktop Entry]
Name=UEFI Firmware Setup (Reboot)
Comment=Access the motherboard configuration utility
Exec=systemctl reboot --firmware-setup
Icon=system-restart
Terminal=false
Type=Application
Categories=System;Settings;
```

### Remove service

```bash
systemctl stop [servicename]
systemctl disable [servicename]
rm /etc/systemd/system/[servicename]
rm /etc/systemd/system/[servicename] # and symlinks that might be related
rm /usr/lib/systemd/system/[servicename] 
rm /usr/lib/systemd/system/[servicename] # and symlinks that might be related
systemctl daemon-reload
systemctl reset-failed
```



### Shutdown

```bash
# shutdown at 10 PM
sudo shutdown 22:00

# For example, to halt the system at 10pm, type:
sudo shutdown --halt 22:00

# It's more likely that you'll want to halt the operating system a few minutes into the future. In that case, specify the number of minutes from now to begin the shutdown process.

# For example, to halt the system after a five-minute delay, type:
sudo shutdown --halt +5

# You can append a message to all users by entering it after the time specification, like this:
# sudo shutdown --halt +5 “Attention. The system is going down in five minutes.”

# Cancel a timed shutdown by using the `-c` option:
sudo shutdown -c

# You can also use the `systemctl` command to shut down the system. For example, type `systemctl halt` or `systemctl poweroff` to achieve similar results to the `shutdown` command. The main disadvantage of using `systemctl` is losing the ability to schedule or cancel the shutdown process.
```
### Sleep mode

```bash
sudo systemctl suspend

sudo systemctl hibernate

sudo systemctl hybrid-sleep
# The systemctl hybrid-sleep command both suspends and hibernates the system.
```
### Syncthing

```bash
sudo systemctl start syncthing@slayo # gotta input username
```

### Services folder

```bash
ls /etc/systemd/system
# user added services
ls /usr/lib/systemd/system
```

### Edit service

```bash
sudo systemctl edit project-mercury.service --full
```

### Logging

```bash
# standard
journalctl

# logs from the current boot
journalctl -b

# logs for specific unit
journalctl -u docker.service

# dynamic way
journalctl -fu ssh # -u for unit (service), -f for follow (dynamic)
```

### List services

```bash
# list units
sudo systemctl list-units

# list services
sudo systemctl list-unit-files

# list running services
sudo systemctl list-units -t service

# list all services
sudo systemctl list-units --all
```

### Mask service

```bash
sudo systemctl mask nginx

# unmask
sudo systemctl unmask nginx
```



### Create a service

```bash
# the simplest script service
sudo vim /etc/systemd/system/mercury.service

[Unit]
Description=Python Django for Preoject Mercury
Documentation=https://wiki.dev/etc
After=postgresql.service # start after PostreSQL

[Service]
ExecStart= /bin/bash /usr/bin/project-script.sh
User=project_service_user # start by service user
Restart=on-failure
RestartSec=10 # restart in 10 sec after failure

[Install]
WantedBy=graphical.target # OS GUI mode

# simple python service example
sudo vim /etc/systemd/system/my.service

[Unit]
Description=some service

[Service]
ExecStart=/usr/bin/python3 /opt/code/my_app.py # service location and exec location
ExecStartPre=/opt/code/configure_db.sh # do before service starts
ExecStartPost=/opt/code/email_status.sh # do after service starts
Restart=always # restart service in case it stops

[Install]
WantedBy=multi-user.target # run on server bootup

# reload services daemon
sudo systemctl daemon-reload

# more complex example
sudo vim /etc/systemd/system/syncthing.service

[Unit]
Description=Syncthing - Open Source Continuous File Synchronization for %I
Documentation=man:syncthing(1)
After=network.target # start only after some process runs

[Service]
User=%i # username
ExecStart=/usr/local/bin/syncthing -no-browser -gui-address="0.0.0.0:8384" -no-restart -logflags=0
Restart=on-failure
SuccessExitStatus=3 4
RestartForceExitStatus=3 4

# Hardening
ProtectSystem=full
PrivateTmp=true
SystemCallArchitectures=native
MemoryDenyWriteExecute=true
NoNewPrivileges=true

[Install]
WantedBy=multi-user.target
```

### Yandex back-end job

```bash
sudo vim /etc/systemd/system/sausage-store-backend.service
# or
sudo systemctl edit sausage-store-backend.service

[Unit]
Description=Sausage-store
After=syslog.target
After=network.target

[Service]
User=jarservice
Type=simple
Environment=REPORT_PATH=/logs/reports
Environment=LOG_PATH=/logs
StandardOutput=file:/logs/out.log
StandardError=file:/logs/error.log
Restart=always
RestartSec=5s
ExecStart=/usr/bin/java \
-Dmyserver.basePath='/home/jarservice/' \
-Dmyserver.bindAddr='127.0.0.1' \
-Dmyserver.bindPort='8080' \
-Dmyserver.hostName='Sausage-store' \
-jar '/home/jarservice/sausage-store.jar'
ExecStop=/bin/kill -9 $MAINPID
SuccessExitStatus=143

[Install]
WantedBy=multi-user.target

sudo vim /etc/systemd/system/sausage-store-backend.service

# original version
[Unit]
Description=Sausage store backend job
After=syslog.target
After=network.target

[Service]
SuccessExitStatus=143
User=root
Group=root
Type=simple
Environment="LOG_PATH=/logs/"
Environment="REPORT_PATH=/var/www-data/htdocs/"
WorkingDirectory= /var/jarservice
ExecStart=/bin/java -jar sausage-store-0.0.1-SNAPSHOT.jar
ExecStop=/bin/kill -9 $MAINPID
StandardOutput=append:/logs/out.log
StandardError=append:/logs/error.log
Restart=always
RestartSec=5s

[Install]
WantedBy=multi-user.target
```

### Yandex front-end job

```bash
sudo vim /etc/systemd/system/sausage-store-frontend.service

# to allow non-root users to use ports under 1024
# log in under user which needs this ports and then
sudo apt-get install libcap2-bin
sudo setcap cap_net_bind_service=+eip /path/to/binary
# or can use AmbientCapabilities=CAP_NET_BIND_SERVICE in service file

[Unit]
Description=Sausage store front-end job
After=syslog.target
After=network.target

[Service]
SuccessExitStatus=143
User=front-user
Group=front-user
Type=simple
WorkingDirectory= /var/www-data
ExecStart=/bin/http-server ./dist/frontend/ -p 80 --proxy http://localhost:8080
ExecStop=/bin/kill -9 $MAINPID
StandardOutput=append:/logs/out-front.log
StandardError=append:/logs/error-front.log
Restart=always
RestartSec=5s
AmbientCapabilities=CAP_NET_BIND_SERVICE

[Install]
WantedBy=multi-user.target
```

