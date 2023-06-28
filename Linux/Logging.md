### Clear log files

```bash
# list log files first
sudo find /var/log -type f -name *.log

# then truncate them
sudo find /var/log -type f -name *.log -exec truncate -s 0 {} +
# exec will reduce (truncate) the files sizes to zero (-s 0)
```

### Auth log

```bash
tail -f /var/log/auth.log
# useful for troubleshooting when users unable to log in
```

### Process log

```bash
# dynamic way
journalctl -fu ssh # -u for unit (service), -f for follow (dynamic)

# old way
sudo journalctl -u cron.service -e
# or last 20 lines
sudo journalctl -u cron.service -n 20
# dated
sudo journalctl -u cron.service --since "2021-07-30 14:10:10" --until "2021-08-02 12:05:50"
```

### System & apt logs

```bash
cat /var/log/syslog # system events main log

# shows system events in different style (better for hardware logs)
sudo dmesg # gives colored view
# or
cat /var/log/dmesg # no colored view

# apt install/remove logs
cat /var/log/apt/history.log

# inspect services logs
journalctl -u -f ssh # -u for unit, -f for follow

# last logged in users log
$ last

# show bad login attemtps
sudo lastb -adF # -a for hostname, -d for dns & ip. -F for times

# show and follow logging stats list
sudo tail -f /var/log/auth.log # Ubuntu, -f for follow (live stats update)
```

### Logrotate

```bash
sudo vim /etc/logrotate.d/

/var/log/sausage-store.log { 
  rotate 12 # максимальное количество файлов хранения
  monthly # годовой, месячный, недельный, дневной
  compress # архивирование и сжатие
  delaycompress # последний и предпоследний файл не будут заархивированы
  missingok # если файла лога нет, то не будет и нотификации об ошибке
  size 100M # размер лога, после которого он будет ротирован
  dateext # добавит дату ротации перед заголовком старого лога
  create # создаст пустой файл после того, как старый будет ротирован
  postrotate/endscript # выполнит после ротации какой-нибудь скрипт
    <путь к скрипту или команда>
}

# example
/opt/log/sausage-store-*.log {
    su student student # to run as user student
    hourly
    rotate 3
    size=10M
    dateext
    missingok
    delaycompress
    compress
    postrotate
                grep opt/log/sausage-store* /var/lib/logrotate/status >> /opt/student_rotate.log
    endscript
}

# for syslog
sudo vim /etc/logrotate.d/rsyslog

# to initiate changes
sudo logrotate /etc/logrotate.d/sausage-store.conf

# to test changes
sudo logrotate /etc/logrotate.d/sausage-store.conf --debug
# or
sudo logrotate /etc/logrotate.d/sausage-store.conf -d
```

### Logrotate logs

```bash
cat /var/lib/logrotate/status
```

