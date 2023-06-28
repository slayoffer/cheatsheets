### Without app image 

```bash
docker run -it ubuntu bash

apt update
apt install -y python python-setuptools python-dev build-essential python-pip python-mysqldb
pip install flask

# copy cource code to /opt/app.py
# run app
FLASK_APP=/opt/source-code/app/py flask run --host=0.0.0.0
```

### Create Dockerfile

```bash
vim Dockerfile

FROM Ubuntu

RUN apt-get update
RUN apt-get install -y python python-pip
RUN pip install flask
RUN pip install flask-mysql

COPY app.py /opt/app.py

ENTRYPOINT FLASK_APP=/opt/app.py flask run --host=0.0.0.0

docker build . -t slayo/flask_app # will auto find Dockerfile in the dir
```

### Build and push

```bash
docker login

docker build Dockerfile -t slayo/flask_app

docker push slayo/flask_app
```

