### Install

```bash
# version 2
sudo yum install python2

# version 3
sudo yum install python36
# or
sudo yum install python3

# check version
python2 -V
# or
python3 -V
```

### Run shell (REPL)

```bash
python2
# or
python3

# ESC or Ctrl+D to get out or
exit()
```

### Run app

```bash
python2 main.py
```

### Install PIP

```bash
sudo yum install python3-pip
```



### Check PIP (Python package manager)

```bash
pip2 -V
# or
pip3 -V
# or
pip -V
```

### Packages location

```bash
ls /usr/lib/python3.6/site-packages
```



### Install package

```bash
pip install flask
# or for multiple
pip install flask some_app some_other_app
```

### Package.json equivalent

```bash
vim requirements.txt

# add package names line by line (for latest versions)
flask
some_app
some_other_app
# or with specific versions
flask==0.1
some_app==0.1.8
some_other_app==0.0.1

# then
pip install -r requirements.txt
```

### Update package

```bash
pip install flask --upgrade

# update pip itself
pip install pip --upgrade
```

### Remove package

```bash
pip uninstall flask
```

### Show package location

```bash
pip show flask
# for several
pip show gunicorn Jinja2 Werkzeug markupsafe
```

### Package import location

```bash
python2 -c "import sys; print(sys.path)"
```

### Other package managers

```bash
# .egg format - old way
easy_install install app

# .whl format - newest way
pip install app.whl
```

