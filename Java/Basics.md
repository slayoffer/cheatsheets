### Install

```bash
sudo curl https://download.java.net/java/GA/jdk13.0.2/d4173c853231432d94f001e99d882ca7/8/GPL/openjdk-13.0.2_linux-x64_bin.tar.gz --output /opt/openjdk-13.0.2_linux-x64_bin.tar.gz

sudo tar -xf /opt/openjdk-13.0.2_linux-x64_bin.tar.gz -C /opt/

# to verify install
jdk-13.0.2/bin/java -version

java -version
# or refresh your bash profile
source /etc/profile.d/jdk13.sh

# list of java cmds
ls jdk-13.0.2/bin/

# set PATH var
sudo PATH=$PATH:~/opt/jdk-13.0.2/bin/
```

### Compile & run (build)

```bash
# compile
javac MyClass.java

# run
cd /java/dir/; java MyClass
```

### Create JAR

```bash
# create jar MyApp.jar
jar cf MyApp.jar MyClass.class Service1.class Service2.class
```

### Run package

```bash
java -jar jarfilename.jar
```

### Run with class path

```bash
java -cp /opt/maven/my-app/target/my-app-1.0-SNAPSHOT.jar com.mycompany.app.App # -cp for class path
```

### Documentation

```bash
javadoc -d doc MyClass.java
```

