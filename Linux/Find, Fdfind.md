### Find operator (takes a lot CPU power)

```bash
# find all the files starting with "host" in the /etc folder
find /etc -name host*

# Найти в папке все файлы в формате .log
find /var/log -name "*.log"

# find file name
find / -type f -name "3a5cbf62-74dd-466e-aa84-95231265b49a"

find / -name file1 — найти файлы и директории с именем file1. Поиск начать с корня (/)

find / -user user1 — найти файл и директорию принадлежащие пользователю user1. Поиск начать с корня (/)

find /home/user1 -name "*.bin" — найти все файлы и директории, имена которых оканчиваются на '. bin'. Поиск начать с '/ home/user1'*

find /usr/bin -type f -atime +100 — найти все файлы в '/usr/bin', время последнего обращения к которым более 100 дней

find /usr/bin -type f -mtime -10 — найти все файлы в '/usr/bin', созданные или изменённые в течении последних 10 дней

find / -name *.rpm -exec chmod 755 '{}' \; — найти все фалы и директории, имена которых оканчиваются на '.rpm', и изменить права доступа к ним

find / -xdev -name "*.rpm" — найти все файлы и директории, имена которых оканчиваются на '.rpm', игнорируя съёмные носители, такие как cdrom, floppy и т.п.

locate "*.ps" — найти все файлы, содержащие в имени '.ps'. Предварительно рекомендуется выполнить команду 'updatedb'

whereis halt — показывает размещение бинарных файлов, исходных кодов и руководств, относящихся к файлу 'halt'

which halt — отображает полный путь к файлу 'halt'
```

### Exclude results from Find

```bash
find /home/slayo -name *.txt | grep -v .cache
# excludes files from .cache folders
```

### Exclude errors from find result

```bash
find /etc -type f 2> /dev/null # 2 for stderr, so all errors will go to dev/null
# 0 for stdin
# 1 for stdout
```

### Find remains of program

```bash
sudo find / -type d -name *mplayer 2>/dev/null
```

### Find iNode

```bash
ls -li

find . -inum 115552019
```

### Script

```bash
#!/bin/bash

find / -iname $1 2> /dev/null # look for $1 in root dir

./find.sh boot.log
```

### Find particular type

```bash
# find file text.txt anywhere on the disk
find . -type f -name text.txt

# find dir with name Documents
find . -type d -name Documents 
```

### Find and remove

```bash
find . -type -f -name Documents -exec rm {} +
# or
find . -type -f -name Documents -exec rm {} \;
# -exec for run rm cmd against all found results
# {} placeholder for found items (for loop container)
# + for terminator
# \; for terminator
```

### Find files/dirs and edit permissions

```bash
# safe way to change sub files rights without changing permissions on dir
find /path/to/dir/ -type f -exec chmod 644 {} \;  # finds all the files (-type f) in dir and executes (-exec) chmod on them

# for dirs
find /path/to/dir/ -type d -exec chmod 644 {} \; 
```

### Clear log files

```bash
# list log files first
sudo find /var/log -type f -name *.log

# then truncate them
sudo find /var/log -type f -name *.log -exec truncate -s 0 {} +
# exec will reduce (truncate) the files sizes to zero (-s 0)
```

### Add to file (wipe existing contents)

```bash
find . -name *.mp3 > result.txt
# will add all the found mp3 file names to file and replace its original content

# or
find . -name *.mp3 | tee result.txt
```

### Examples

```bash
# There are many options available for use with find:
find / -type d -name conf # will find all the directories named “conf”
find / -user donc # will find all files owned by “donc”
find / -name donc # will find all files with the same name as “donc”
find -name 'index.html' # would search for any file named index.html in the current directory and any subdirectory.
find / -name 'index.html' # would search for any file named index.html in the root directory and all subdirectories from root
find -name 'sshd*' # would search for any file beginning with the text string “sshd” in the current directory and any subdirectory.
find -name '*' -size +500k # would search for any file larger then 500kb.
```

### Locate file (not a good way)

```bash
locate City.txt

# need to update db before location
sudo updatedb
```

### Best find app

```bash
# https://github.com/sharkdp/fd?ref=python-for-engineers

# for file
fd file_name
# or
fd --type f file_name

# for dir
fd --type d file_name

# in dir
fd passwd /etc

# recursive files list in dir
cd fd/tests
fd
# or
fd . fd/tests/

# for exact filename
fd -g libc.so /usr

# for hidden file
fd -H pre-commit

# search even gitignored files
fd -I num_cpu
```