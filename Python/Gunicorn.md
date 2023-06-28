### Run server

```bash
gunicorn main:app

# with several workers
gunicorn main:app -w 2
```

### Check server

```bash
nohup gunicorn app:app -w 3 &

curl localhost:8000
```

### Check processes

```bash
ps -ef | grep gunicorn | grep -v grep
# or
ps aux | grep gunicorn
```

