### Preparation

```bash
apt install mdadm

yum install mdadm

dnf install mdadm

fdisk -l

# Создание таблицы разделов.

# Для накопителей меньше 2 Терабайт:
parted /dev/sdb mklabel msdos
parted /dev/sdc mklabel msdos
# Для больших накопителей больше 2 терабайт или компьютеров с UEFI:
parted /dev/sdb mklabel gpt
parted /dev/sdc mklabel gpt

# Далее необходимо заняться созданием разделов на предпочтенных HDD.
# Создадим раздел размером 95 мегабайт.
parted /dev/sdb mkpart primary ext4 20 95MG
# И
parted /dev/sdс mkpart primary ext4 20 95MG
# Исходя их указанной команды видно, что мы основали раздел объемом 95 Мегабайт. Процесс подготовки жестких дисков к добавлению массива окончен. Самое время преступить к следующему пункту

# Рассмотрим синтаксис команды для добавления RAID в Linux
# mdadm --create /dev/наименование_массива --level=режим_работы --raiddevices=количество_дисков список дисков
# Рейд 1:
mdadm --create /dev/md0 --level=1 --raid-devices=2 /dev/sdb /dev/sdc
# Рейд 0:
mdadm --create /dev/md0 --level=0 --raid-devices=2 /dev/sdb /dev/sdc

# Сохранение конфигурации массива

# Уже сейчас собранный массив работает. Однако, для стабильной работы требуется сохранить конфигурацию Рейд.
mdadm --detail --scan --verbose | sudo tee -a /etc/mdadm/mdadm.conf

# Если каталога нет:
mdadm --detail --scan --verbose | sudo tee -a /usr/sbin/mdadm/mdadm.conf

# Чтобы при загрузке ОС Линукс автоматически монтировался рейд, необходимо отредактировать файл fstab.
nano /etc/fstab

/dev/md0 /mnt/ ext4 defaults 0 0

# Как посмотреть данные о RAID

# Для этих целей есть специальная команда. Задайте в терминале следующий текст:
cat /proc/mdstat

# Основные данные о массиве в Линукс появятся на экране. Если информации недостаточно, потребуется детализировать данные. Это можно сделать следующим способом:
sudo mdadm --detail /dev/md0

# Удаление массива (при необходимости):
sudo mdadm --remove /dev/md0

# Принцип основания RAID 1 в Линукс осуществляется тем же способом, только значения 0 нужно заменить на 1. К примеру, названия дисков теперь будут прописаны в виде: /dev/sda1 и так далее
```

