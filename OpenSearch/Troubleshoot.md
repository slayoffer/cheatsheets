### Timeout issue

```bash
# to get rid  of the TIMEOUT issues add these two lines at the end of /etc/systemd/system/elasticsearch.service.d/override.conf

[Service]
LimitMEMLOCK=infinity

# Then when `systemctl daemon-reload && systemctl enable elasticsearch.service && systemctl start elasticsearch.service`, you no longer get the timeout issue.
```

