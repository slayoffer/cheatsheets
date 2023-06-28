### Run

```bash
pm2 start app.js

# with multiple instances (for loadbalancer, etc)
sudo pm2 start app.js -i 4
```

### Check forks

```bash
ps -ef | grep node

# confirm
ps -ef | grep PM2
```

### Delete fork

```bash
sudo pm2 delete app.js
```

### Check status

```bash
pm2 status
```

