### Date & time codes

```bash
1: Minutes (0-59)
2: Hours (0-23)
3: Days (1-31)
4: Month (1-12)
5: Day of the week(1-7)

55 23 * * * /path/script.sh # run script at 23:55 every day

*/1 7-19 * * 1 /home/jarservice/my.sh # каждую минуту понедельника с 7 до 19

0 3 * * * # каждый день в 3 утра
0 4 * * 1 # каждый понедельник в 4 утра
0 5 28 * * # каждое 28 число в 5 утра
30 20 * * 1-5 /usr/local/bin/script.sh # run monday-friday at 20:30
* * * * 1-5 /opt/scripts/monit.sh >> /var/log/monit_httpd.log # run script every minute every hour every day every month monday-friday
* * * * * date >> ~/date.txt
# backup example for www-data apache system user
0 3 * * * /usr/local/bin/website_backup.sh
```

### Basic

```bash
# check existing cron jobs
crontab -l # -l for list

# check for another user
crontab -u root -l 

# remove cron
crontab -r

# check last edit time of file
crontab -v
```

### Create a cron job

```bash
# open crontab
crontab -e # -e for edit
# or
crontab -e -u root # -u for user, only for root cron jobs!
# or
sudo vim /etc/crontab # only for root cron jobs!
```

### Set hourly, daily, etc

```bash
@hourly echo "Hello World"
```

### Do at reboot

```bash
@reboot echo "Hello World"
```

### At cmd

```bash
# run script at 15:32
at 15:32 -f ./myscript.sh
# or with date
at 18:00 081622 -f ./myscript.sh # 6 PM august 16 2022

# check current jobs
atq

# delete job
atrm 3
```

### Allow cron for users

```bash
sudo vim /etc/cron.allow

# disallow
sudo vim /etc/cron.deny
```

### Backup removal cron

```bash
0 3 * * * find /mnt/sausage_backup/ -name "sausage-store.jar" -type f -mtime +14 -delete
# каждый день в 3 утра будет запускаться команда по поиску файлов с бэкендом старше, чем 2 недели и удалять их
```

### Cron logs

```bash
grep CRON /var/log/syslog
```

