### Install

```bash
sudo apt install python3-virtualenv

# for arch
sudo pacman -S python-virtualenv
```

### Run

```bash
virtualenv -p /usr/bin/python3 my-project
```

### Activate

```bash
source bin/activate

# turn off
deactivate
```

### Activate Venv and apply DB migrations

```bash
cd /project/folder

source venv/bin/activate # activate

cd /opt/caleston-code/mercuryProject

python3 manage.py migrate # apply migrations

# run server
python3 manage.py runserver 0.0.0.0:8000
```



### Install package

```bash
pip install Flask

# remove
pip uninstall Flask
```

### List packages

```bash
pip list
```

