### Clean temp files older than 3 days and non-root owned

```bash
sudo find /tmp -type f \( ! -user root \) -atime +3 -delete

# or make cron
sudo crontab -e
0 0 * * * sudo find /tmp -type f ! -user root -atime +3 -delete
```