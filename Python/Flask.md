### Change port

```bash
# change 8080 to 5000 and change 0.0.0.0 to localhost
sudo sed -i 's/0.0.0.0/127.0.0.1/g;s/8080/5000/g' /opt/simple-webapp-flask/app.py;
```

### Run

```bash
sudo cd /opt/simple-webapp-flask/; nohup python app.py &
```

### Restart

```bash
pkill python

cd /opt/simple-webapp-flask/

nohup python app.py &
```

