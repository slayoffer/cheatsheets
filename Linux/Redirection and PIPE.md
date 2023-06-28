### Redirect the result of the command to some file

```bash
# redirect uptime info to file
uptime > /etc/text.txt

# redirect contents of the folder to file
ls > /etc/text.txt

# redirect (append) content without overwriting the old contents of the file
ls >> /etc/text.txt
```

#### Example of file creation

```bash
echo "#############" > /tmp/sisinfo.txt
date > /tmp/sysinfo.txt
echo "##########" >> /tmp/sysinfo.txt
uptime >> /tmp/sysinfo.txt
echo "##########" >> /tmp/sysinfo.txt
free -m >> /tmp/sysinfo.txt
echo "##########" >> /tmp/sysinfo.txt
df -h >> /tmp/sysinfo.txt
echo "##########" >> /tmp/sysinfo.txt
cat /tmp/sysinfo.txt
```

### Redirect output to nowhere

```bash
# this wont create null file (black hole)
yum install vim -y > /dev/null
```

### Clear the contents of the file

```bash
# will erase all content in sysinfo.txt
cat /dev/null > /tmp/sysinfo.txt
```

### Redirect error message to file

```bash
free -m 2>> /tmp/error.log
# error.log file will contain "-bash: freeee: command not found"
# 2 is standart error output

# 1 is standart output (will add results of command to file)
free -m 1>> /tmp/error.log

# & is for any output (will add normal or error results to file, in case of error)
free -m &>> /tmp/error.log
# this way all the LOG files work under the hood
```

### Piping ( | )

```bash
# count the files total number in the folder
ls | wc -l

# search and list files with name "host"
ls | grep host

# search for last 20 lines with "vagrant" name (case insensitive)
tail -20 /var/log/messages | grep -i vagrant

# show only Mem line of the free -m command
free -m | grep Mem

# show last 10 files of the ls command
ls -l | tail
```

