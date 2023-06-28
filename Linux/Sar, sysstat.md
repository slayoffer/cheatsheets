### Sar is used to troubleshoot Linux Server performance

```bash
man sar
```



### Enable sar in config

```bash
sudo nano /etc/default/sysstat

ENABLED="true"

# sysstan cron jobs
cat /etc/cron.d/sysstat

# sysstat config
cat /etc/sysstat/sysstat
```

### Check current server stats

```bash
# cpu
sar -u

# ram
sar -r

# swap
sar -S

# input & output
sar -b
```



### Check sar log

```bash
ls -l /var/log/sysstat/

sudo sar -u -f /var/log/sysstat/sa22
```

