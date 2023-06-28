### Sort files by update time

```bash
# recent files first
ls -lt

# old files first
ls -ltr
```


### Less

```bash
less anaconda-ks.cfg
# can use up and down buttons to look through the file
# can use / to search for words
# press Q to quit the reader
```

### More

```bash
more anaconda-ks.cfg
# cant use arrow keys, gotta press Enter key
# Q to quit
```

### Head

```bash
head /var/log/syslog

# first 25 lines
head -25 /var/log/syslog
# or
head -n 25 /var/log/syslog

# double pipe example
cat /var/log/syslog | grep ssh | head
```

### Tail

```bash
tail /var/log/syslog

# last 20 lines
tail -20 /var/log/syslog
# or
tail -n 20 /var/log/syslog

# this file will be updated dynamically upon changes
tail -f /var/log/syslog
# CTRL + C to quit
```

### Cut operator

```bash
# show only first column of the file
$ cut -d: -f1 /etc/passwd
# -d: is to separate by ":"
# -f1 is to include first word

#
cut -d: -f1 < /etc/passwd | sort | grep u > /tmp/usernames.txt
```

### Search and replace operator - sed

```bash
# search and replace coronavirus word with covid word in all the .txt files in the folder and on all the lines (g - globally)
# THIS WILL ONLY PRINT BUT NOT CHANGE THE FILE!
$ sed ' s/coronavirus/covid/g' *.txt

# WILL CHANGE THE CONTENT in the file
$ sed -i ' s/coronavirus/covid/g' *.txt
```

### Locate operator (less CPU resource needed)

```bash
# have to install first
yum install mlocate

# update locate db
updatedb

# find all the files starting with "host"
locate host
```

### Show number of lines in file

```bash
cat /etc/ssh/ssh_config | wc -l
```

