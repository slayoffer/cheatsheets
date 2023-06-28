### Local copy

```bash
sudo rsync -rv /home/user /backup # -v for verbose, -r will recursively copy all but will set new dir permissions to root (bad way)
# better use flag -a
sudo rsync -a /home/user /backup # -a for archive (short for -rlptgoD), will keep user permissons and file metadata
```

### SSH (remote) copy

```bash
sudo rsync -av /home/user admin@192.168.1.38:/backup
```

### Copy without replacing already existing files

```bash
sudo rsync -av -e "ssh -i ~/.ssh/id_rsa" --ignore-existing -r /usr/local/apps/bim-pdm.rubius.com/ administrator@158.160.49.184:/usr/local/apps/bim-pdm.rubius.com/
```

### Sync folders

```bash
sudo rsync -av --delete /src /target # will sync both folders and auto delete files in a folder case you delete files in other folder
```

### Backup old files when replacing with new ones

```bash
sudo rsync -avb --delete /src /target # -b for backup, will rename old files before replacing them with new ones

sudo rsync -avb --delete --backup-dir=/backup/incremental /src /target # will replace old files with news one but put old replaced files in incremental dir to save copies of them
```

### SSH copy

```bash
rsync -avz -e "ssh -i ~/.ssh/id_rsa" /path/to/local/folder user@remote_server:/path/to/remote/folder
```

### Backup example

```bash
rsync -avz /var/www/nextcloud.devcraft.app/data slayo@192.168.1.38:/mnt/backup-data/nextcloud.devcraft.app/

# or better create script and add to cron
vim backup.sh

DATE=$(date '+%F') # 2022-01-07 date format

rsync -avz /var/www/nextcloud.devcraft.app/data slayo@192.168.1.38:/mnt/backup-data/nextcloud.devcraft.app/$DATE
# save and exit

chmod +x backup.sh

# another example
rsync -avb --backup-dir=/backup/incremental/08-17-22 /src /target --delete --dry-run 
# copies from /src to /target, backs up ex versions of files to /backup

-a for archive mode
-b for keep backup file in case there is already file with same name in backup dir
--backup-dir to point -b option to
-r # чтобы работать с папками и их содержимым
--exclude my_file.txt # чтобы исключить из бэкапа какой-то файл
--delete # удалять из бэкапа файлы, если они отсутствуют в источнике
-a # сохранять права и владельцы на файлы
-v # для визуализации бэкапируемых файлов
-h # для человекочитаемого вывода информации о размерах отправленного
--dry-run # для тестового прогона, без реального перемещения файлов
-z # сжатие данных 
```

### Script example

```bash
#!/bin/bash

# Check to make sure the user has entered exactly two arguments.
if [ $# -ne 2 ]
then
    echo "Usage: backup.sh <source_dir> <target_dir>"
    echo "Please try again"
    exit 1
fi

# Check to see if rsync is installed
if ! command -v rsync > /dev/null 2>&1
then
    echo "This script requires rsync to be installed."
    echo "Please make sure that you have it installed."
    exit 2
fi

# OPTIONAL - add rsync install
if [ ! -f /usr/bin/rsync ]; then
   sudo apt install -y rsync
fi

# Capture the current date, and store it in the format YYYY-MM-DD
current_date=$(date "+%Y-%m-%d")

rsync_options="-avb --backup-dir $2/$current_date --delete --dry-run"

$(which rsync) $rsync_options $1 $2/current >> backup_$current_date.log
# $(which rsync) will reform to /usr/bin/rsync
```

### Пример расчета

```bash
Возьмём сферического коня в вакууме, которым в нашем случае будет база данных с изначальным размером 10 ГБ, куда каждый час записывается по 5 МБ. Для этой базы данных предусмотрена система бэкапирования по системе «дед-отец-сын». Бэкапы делаются инкрементные, а не дифференциальные. Полные бэкапы выполняются по понедельникам. Все бэкапы делаются в конце дня. Также представим, что у нас сначала удаляется устаревшая версия бэкапа, а потом записывается новая.

Какого размера хранилище нам надо развернуть, чтобы цикл бэкапирования включал 1 месяц (предположим, что месяц у нас ровно 28 дней, то есть 4 недели)? 

Правильный ответ — ~60 ГБ, а точнее 61,7 ГБ. Как это получилось. Каждый день наша база порождает: 5 МБ * 24 часа = 120 МБ — и так к концу первой недели размер базы увеличивается до 120 МБ * 7 дней = 840 МБ + стартовые 10 ГБ. Всё это мы сохранили в хранилище.
Хранилище: 10 ГБ 840 МБ

Вторая неделя начинается с «отца», который сохраняет полный бэкап текущего состояния + то, что успело набежать за день: 10 ГБ 840 МБ + 120 МБ = 10 ГБ 960 МБ. Дальше шли обычные «сыны» 6 дней, давшие нам ещё 720 МБ. Итого за вторую неделю = 11 ГБ 680 МБ.
Хранилище: 22 ГБ 520 МБ

Третья неделя — по механике похожа на вторую. Ещё 12 ГБ 520 МБ.
Хранилище: 35 ГБ 40 МБ

Четвёртая неделя (последняя неделя месяца), как вторая и третья, но в конце мы добавляем ещё один полный бэкап — «дед». 12 ГБ 520 МБ + 840 МБ = 13 ГБ 360 МБ * 2 = 26 ГБ 720 МБ
Хранилище: 61 ГБ 760 МБ
```

