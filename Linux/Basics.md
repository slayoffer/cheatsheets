### Помощь по командам

```bash
man sudo
# or better way
curl https://cheat.sh/rsync

# Вывести все команды со словом ping
apropos ping
```

### System information

```bash
# System info
uname -a
# Kernel info
uname -r
# or
cat /proc/version
# or
neofetch

# show kernel logs
dmesg

# check OS release
lsb_release -a
# or more detailed
cat /etc/os-releasae
# or universal
cat /etc/*release*

# system boot file contents (welcome message when log in)
cat /etc/issue

# The following command will display all available information about our current distribution, the package management, and the base details:
echo /etc/*_ver* /etc/*-rel*; cat /etc/*_ver* /etc/*-rel*
```

### Check app version

```bash
ansible --version
```

### Check runlevels (targets)

```bash
runlevel
# 5 for GUI mode - graphical.target
# 3 for CLI mode - multiuser.target
```

### Check command type

```bash
type update
```

### Echo

```bash
# -e for edit
echo -e "\aHello World" # \a for audio output

echo -e "This is a\bLinux server" # \b for backspace (delete) - This is Linux server

echo -e "This is a Linux\c server" # \c for crop - This is Linux

echo -e "This is a Linux\n server" # \n for new line

echo -e "This is a\t Linux\t server" # \t for tabs
```

### Check where the service is located

```bash
which ls # => /usr/bin/ls
# or
command -v ls
```

### Check shell type

```bash
echo $SHELL
```

### Exit codes & Data streams (stdin, stdout, stderr)

```bash
# Show code result of last cmd
echo $?
# 0 for success result
# 1,2+ for error result

# Data Streams
# 0 for stdout
# 2 for stderr

find /etc -type f 2> /dev/null # 2 for stderr, so all errors will go to dev/null
# dev/null is a blackhole

# send standard output to results.txt and errors to errors.txt
find /etc -type f 1> ~/results.txt 2> ~/errors.txt
# or
find /etc -type f > ~/results.txt 2> ~/errors.txt # same result

# удалить весь вывод команды (1 и 2)
cronload.sh > /dev/null 2>&1

# stdin
#!/bin/bash
echo "Please enter your name:"
read myname # read for stdin
echo "Your name is: $myname"
```

### Hush login details

```bash
touch .hushlogin
```

### Change command prompt

```bash
PS1="slayo-server"
# with date and time
echo 'PS1="[ \d \t \u@\h:\w ] $"' >> ~/.profile

# [Wed Apr 22]bob@caleston-lp10:~$
echo 'PS1="[\d]\u@\h:\w$"' >> ~/.profile

\d : the date in "Weekday Month Date" format (e.g., "Tue May 26")
\e : an ASCII escape character (033)
\h : the hostname HQDN
\H : the complete hostname
\n : newline
\r : carriage return
\s : the name of the shell
\t : the current time in 24-hour HH:MM:SS format
\T : the current time in 12-hour HH:MM:SS format
\@ : the current time in 12-hour am/pm format
\A : the current time in 24-hour HH:MM format
\u : the username of the current user
\w : the current working directory, with $HOME abbreviated with a tilde
\W : the basename of the current working directory, with $HOME abbreviated
with a tilde
\$ : if the effective UID is 0, a #, otherwise a $
```

### Change shell

```bash
sudo chsh -s /bin/sh slayo # change to original (Bourne) shell
```

### Uptime check

```bash
uptime
```

### Change hostname

```bash
# change in the file
vim etc/hostname

# then change with command
hostname smartpc
```

### Шорткат для копирования и вставки предыдущей команды:

```bash
!!

# Еще можно и после !! дописать нужный аргумент
apt install mc
sudo !! # sudo apt install mc
sudo !! vim # sudo apt install mc vim
```

### Перейти на уровень выше

```bash
cd .. 
```

### Перейти в домашнюю директорию

```bash
cd ~
```

### Вернуться в прошлую директорию

```bash
cd - # (минус)
```

### Поставить коммент для будущего использования через историю и т.д.

```bash
# comment (# sudo ...)
```

### Для коррекции аргумента из прошлой команды

```bash
# ^прошлый-аргумент^на-что-заменить 
sudo systemctl restart apache2
^restart^status
sudo systemctl status apache2 
```

### Запуск сразу нескольких команд

```bash
# Разделяем их через ;
mkdir mydir ; cd mydir

# Если первая команда завершится с ошибкой, то вторая все равно запустится. Поэтому еще есть зависимое выполнение команд

# Через && (и) - если первая не запустится, то и вторая не запустится
mkdir root/mydir && cd /root/mydir

# Через || (или) - если первая НЕ запустится, то запустится вторая. Если первая запустится, то вторая НЕ запустится.
mkdir root/mydir || cd /root/mydir
```

### Вывести содержимое текстового файла

```bash
# обычный вывод текстового файла
cat text.txt

# or better
batcat text.txt # needs bat package

# построчный вывод с номерами строк
nl text.txt
```

### Очистка терминала

```bash
# Очистка видимой области терминала
CTRL + L / команда clear

# Очистка всего терминала
clear && clear

# Восстановление изначального состояния терминала
reset

# or
printf "\ec"
printf "3c"
printf "\x1Bc"
```

### Remember dir and get back into it later

```bash
cd /etc
pushd /var # will remember the /etc dir and move to /var dir
popd # will take you back to /etc dir
```

### Show output in column view

```bash
mount | column -t
```

## Show a notification when a shell command is completed

If we add “;notify-send done” to the end of a command, we will receive a notification when the command completes.

```
ls -l ;notify-send done
```

## Equivalent to the ENTER key on our terminal

Very useful if, for some reason, the “ENTER” key does not work.

```
CTRL + m or CRL + j
```

## One-liner **countdown in terminal**

We do not need either any third-party application to launch a countdown from the command line and be notified when it reaches zero.

```
$ for i in `seq 10 -1 1` ; do echo -ne "\r$i " ; sleep 1 ; done ;notify-send ZERO!
```