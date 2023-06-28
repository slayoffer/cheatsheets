### Install

```bash
# need to have java installed
yum install java-1.8.0-openjdk-devel

# download tomcat
sudo curl -OL https://downloads.apache.org/tomcat/tomcat-8/v8.5.84/bin/apache-tomcat-8.5.84.tar.gz

# unzip
sudo tar -xvf apache-tomcat-8.5.84.tar.gz
```

### Start

```bash
sudo /opt/apache-tomcat-8/bin/startup.sh

# check
curl localhost:8081; ps -ef | grep tomcat
```

### Run app

```bash
# place app in /opt/apache-tomcat-8/webapps/
cd /opt/
sudo mv /opt/sample.war /opt/apache-tomcat-8/webapps/ # will auto run after moving app

# to unpack
jar -cvf app.war
# or
mvn package
# or
gradle build
```

### Config file

```bash
vim /apache-tomcat-8.5/conf/server.xml
```

### Change port

```bash
# from 8080 to 9090
sudo sed -i 's/8080/9090/g' /opt/apache-tomcat-8/conf/server.xml

# stop tomcat
sudo /opt/apache-tomcat-8/bin/shutdown.sh

# start
sudo /opt/apache-tomcat-8/bin/startup.sh

# check
curl localhost:9090; ps -ef | grep tomcat
```

