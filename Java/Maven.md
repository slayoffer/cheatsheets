### Install

```bash
# CentOs
sudo yum install maven
```

### Compile & package

```bash
cd /opt/maven/my-app/; sudo mvn package
```

### Run package

```bash
java -jar jarfilename.jar
```

### Run with class path

```bash
java -cp /opt/maven/my-app/target/my-app-1.0-SNAPSHOT.jar com.mycompany.app.App # -cp for class path
```

