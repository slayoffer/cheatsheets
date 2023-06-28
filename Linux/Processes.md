### Show PID

```bash
pidof cron # 867
# or
ps -ux | grep cron
# or
pgrep -l cron
```

### Background & Foreground

```bash
# minimize process
CTRL + Z

# show bg processes
jobs

# return minimized process
fg # return to foreground last process

fg 2 # return process #2

bg # check processes in background

# run service in the background
htop &
```

### Show process envs

```bash
cat /proc/867/environ
```

### Show all the processes dynamically (load avg., etc)

```bash
top
# or better
htop
```

### PS

```bash
ps

ps a # for extra info with STAT
# D uninteruptable sleep
# S for interuptable sleep, waiting for user input
# s for root (parent) process
# R in the run queue
# T stopped (minimized by CTRL+Z also)
# Z defunct (zombie)

# TTY:
# tty1 for terminal (can open up to 7 terminals, press Ctrl + Alt + F1-F7)
# pts for virtual terminal

ps x

# Show processes sorted by user, CPU and MEM
ps au
# or
ps aux # список всех процессов 

# show specific process
ps --quick-pid 599890
# or
ps au | grep vim
# or
ps u -C vim

# show process relations
ps -He # -e for show all processes running, -H displays relationships between processes
 
# отображение дерева процессов, включая PPID, easier to understand view with lines
ps -axjf
 
# PPID - parent process id
# SID - session id
# PGID - parent group process id
# TPGID - terminal id which runs process
# UID - user id which runs process

# show processes with parent processes
ps -ef
# -e is used to select every process
# -f gets details in full format

ps -eo user,cmd # show only user and cmd columns
ps -eo user,uid,gid,pid,ppid,cmd # show all these columns
```

### Show what files are used by processes

```bash
lsof
#
lsof -i tcp:80
```

### Show what folder process use (pwdx)

```bash
ps aux | grep cron

sudo pwdx 612

> 612: /var/spool/cron
```



### Stop processes

```bash
# need to know process ID (PID)
kill 1420 # SIGTERM signal, will kill child processes too in case main process stops
# or
killall apache2 # cant be used with partial names, e.g. apach
# or
pkill apache2
# or
pkill apach # partial process name
# or
kill -SIGTERM 1479
# or 
kill -15 1479
# or
kill -SIGTERM $(pgrep httpd) # -SIGSTOP for pause


# kill if process worked less than 40 minutes
killall -y 40m <name>

# for help
man 7 signal
# or
kill -l
```

### Pause, resume process

```bash
# pause
kill -SIGSTOP $(pgrep httpd)

# resume
kill -SIGCONT $(pgrep httpd)
```



### Forceful stop processes

```bash
kill -9 1476 # SIGKILL signal, but it doesnt kill child processes
# or
kill -SIGKILL 1476
# or
killall -9 vim

# kill several
kill -9 63772 45116 23465

# to kill child (orphaned) processes
ps −ef | grep httpd | grep -v 'grep' | awk '{ print $ 2 }' # to list PIDs

ps −ef | grep httpd | grep -v 'grep' | awk '{ print $ 2 }' | xargs kill -9 # to kill the children (xargs takes results of last pipe command (PIDs) as an argument)
```

### Restart process

```bash
kill -HUP 1476
```

### Start/Change process priority/nice

```bash
# -20 is lowest nice
# 19 is highest nice

# 80 is starting PRI

# start with nice
nice -n 10 vim
# start with lowest set priority & nohup
nohup nice --20 ping www.google.com & # instead of nohup can use screen, tmux, etc
# for highest
nohup nice -19 ping www.google.com &

# change priority
renice -n 17 -p <PID>
# or
renice 17 -p <PID>
```



### To clear processes of zombie processes

```bash 
systemctl reboot
```

## pstree command to show the process tree

```bash
sudo apt install psmisc

pstree

pstree -p # to show process pids
```

## tree command to show the process tree

```bash
sudo apt install tree

tree /proc

tree -d /proc # to show only dirs

tree -f /proc # to show full pathnames
```

