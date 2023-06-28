Bash скрипт для удаления файлов старше заданного количества дней в Linux

Создадим файл с названием "DelFileNDay .sh", используя для этого утилиту "cat".

```bash
cat > DelFileNDay.sh << EOF
#!/bin/bash
#DEL file > 30 DAY
DAY=30
DIR="/var/log/test/"
find $DIR -type f -mtime +$DAY -exec rm -f {} ;
EOF
```


DAY=30 - задаем количество дней, старше которых файлы будут удаляться.

DIR="/var/log/test/" - задаем полный путь к директории, из которой будут удаляться файлы.

Даем файлу максимальные привилегии с помощью утилиты "chmod".

```bash
chmod 777 DelFileNDay.sh

./DelFileNDay.sh
```

Если нужно чтобы файлы удалялись автоматически, например раз в день или месяц, то поместите скрипт в директорию "/etc/cron.daily/" или "/etc/cron.monthly/".