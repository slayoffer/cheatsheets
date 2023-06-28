## Backup a drive into a file on a remote host via ssh

We do not need to use any tool to create a backup of our disk.

```bash
dd if=/dev/sda | ssh user@server 'dd of=backup_sda.img'
```

### Мой скрипт, с помощью которого я делаю бэкапы в Linux

Обожаю UNIX-way, тут бэкапы можно делать значительно более гибкими.

Для бэкапа home директории я использую обычный tar с инкрементацией и шифрую его своим gpg ключом.

Для других файлов, например, для бэкапов моих видео, которые я записываю для ютуба я использую rsync. RSYNC более рационально использовать, когда не критична синхронизация большого количества файлов

```bash
#!/bin/bash 

NOW=$(date +%Y%m%d%H%M) MNOW=$(date +%Y%m) BACKUP_HOME="/tmp/home/" EMAIL="devpew" ARCHIVES_DIR="/tmp/backup/" DOW=`date +%a` # Day of the week e.g. 
Mon DOM=`date +%d` # Date of the Month e.g. 27 
DM=`date +%d%b` # Date and Month e.g. 27Sep 

if [[ ! -d ${ARCHIVES_DIR}${MNOW} ]]

then mkdir ${ARCHIVES_DIR}${MNOW} 
else echo &>/dev/null 

fi 

tar --exclude-from=/home/dm/mybin/.backup.excludes -v -z --create --file ${ARCHIVES_DIR}${MNOW}/${NOW}.tar.gz --listedincremental=${ARCHIVES_DIR}${MNOW}/${MNOW}.snar $BACKUP_HOME &> ${ARCHIVES_DIR}${MNOW}/${NOW}.log 

if [ $(ls -d ${ARCHIVES_DIR}${MNOW}/*.tar.gz 2> /dev/null | wc -l) != "0" ] 
then 
gpg -r $EMAIL --encrypt-files ${ARCHIVES_DIR}${MNOW}/*.tar.gz \ 
&& rm -rf ${ARCHIVES_DIR}${MNOW}/*.tar.gz 
fi 

scp ${ARCHIVES_DIR}${MNOW}/${NOW}.tar.gz.gpg ${ARCHIVES_DIR}${MNOW}/${MNOW}.snar dm@192.168.0.152:/home/dm/backup/${MNOW}
```

Если нужен более гибкий инкремент второго уровня, например, по неделям, то можно использовать такие условия

```bash
DOW=`date +%a` # Day of the week e.g. 
Mon DOM=`date +%d` # Date of the Month e.g. 27 
DM=`date +%d%b` # Date and Month e.g. 27Sep 
if [ $DOM = "01" ]; then 
echo 'this is monthly backup' 
fi 

if [ $DOW = "Sun" ]; then 
echo 'this is weekly backup' 
else 
echo 'this is daily backup' 
fi
```

### Теперь, коротко о том, что делает этот скрипт

Скрипт перейдет в указанную директорию для бекапов и создаст в ней директорию с именем года и месяца в формате “202205” если сейчас май 2022 года.

Далее все бэкапы за май будут находиться в этой папке.

Далее если в папке нет файла с инкрементом (например, мы впервые запустили скрипт или начался новый месяц) то у нас создастся полный бекап всей системы.

В дальнейшем при запуске этого скрипта будут создаваться инкременты от текущего полного бекапа

Кроме того появится файл с логом

После того как у нас сделался бекап, он у нас зашифруется нашим GPG ключом а файл TAR удалится.

После этого мы скопируем наш бэкап к нам на сервер

### Exclude

Если нужно задать исключения. То есть файлы или директории, которые не нужно бекапить, то сделать это можно в файле с исключениями. Тут нужно быть аккуратным, любой лишний пробел может все сломать

```bash
mybin cat /home/dm/mybin/.backup.excludes 
/tmp/home/Nextcloud 
/tmp/home/.cache 
/tmp/home/youtube-videos
```

Кроме того, ничего не будет работать, если вы поставите слэш в конце.

Например строка 

/tmp/home/Nextcloud будет работать, а вот строка /tmp/home/Nextcloud/  работать уже не будет. Так что будьте аккуратны

### Если нужно распаковать

У нас делаются инкрементальные бекапы и шифруются. Если нам нужно получить данные, то для начала нам нужно расшифровать файл

Расшифровать можно командой

```bash
gpg --output 202205122134.tar.gz --decrypt 202205122134.tar.gz.gpg
```

После этого, начнем распаковывать tar начиная с самого первого. Для начала распаковываем архив от первого числа.

```bash
tar --extract --verbose --listed-incremental=/dev/null --file=202205010101.tar.gz
```

И после этого распаковываем остальные инкременты, если нужно восстановить состояние системы, например, на 11 число, то нужно последовательно распаковать tar-архивы со 2 по 11 в ту же папку

Если это кажется слишком долгим процессом и вы часто восстанавливаете данные, то можно добавить инкремент второго уровня, например, недельный.

Или если вручную для вас это кажется слишком длительным процессом, можете накидать небольшой скрипт для распаковки. В простейшем случае так:

```bash
tar --extract --incremental --file file0.tar 
tar --extract --incremental --file file1.tar 
tar --extract --incremental --file file2.tar
```

Или, например, так:

```bash
for i in *.tbz2; do tar -xjGf "$i"; done;
```

Если нужно извлечь только конкретные каталоги из архива:

```bash
tar -xjGf levelX.tar --wildcards 'example/foo*' 'example/bar*'
```

### Autorun

Если бы в убунте или дебиане, то вам нужно запускать этот скрпит через крон. В арче нет крона и автозапуск делается иначе. В этом примере будем запускать наш скрипт каждый день в 03:30

Нужно создать файл

```bash
sudo nvim /usr/lib/systemd/system/backup.service

[Unit] 
Description=backup 

[Service] 
Type=simple 
ExecStart=/home/dm/mybin/backup
```

И файл

```bash
sudo nvim /usr/lib/systemd/system/backup.timer

[Unit] 
Description=my backup 

[Timer] 
OnCalendar=*-*-* 03:30:00

[Install] 
WantedBy=multi-user.target
```

После этого можем запустить наш сервис

```bash
sudo systemctl start backup.timer sudo systemctl enable backup.timer
```

После этого проверим добавился ли он командой

```bash
sudo systemctl list-timers --all
```

или командой

```bash
sudo systemctl status backup.timer
```