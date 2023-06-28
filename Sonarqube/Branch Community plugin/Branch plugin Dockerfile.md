### Dockerfile

```bash
# setting latest version of SonarQube Community Edition
FROM sonarqube:9.9.0-community
# setting applicable version of Branch Community plugin (check plugin compatibility matrix)
ARG PLUGIN_VERSION=1.14.0
# uploading plugin to SonarQube container
ADD https://github.com/mc1arke/sonarqube-community-branch-plugin/releases/download/1.14.0/sonarqube-community-branch-plugin-1.14.0.jar /opt/sonarqube/extensions/plugins/
# switching from sonarqube user to root user to make changes in config files
USER root
# replacing config placeholder strings with plugin settings
RUN sed -i "s|#sonar.web.javaAdditionalOpts=|sonar.web.javaAdditionalOpts=-javaagent:./extensions/plugins/sonarqube-community-branch-plugin-${PLUGIN_VERSION}.jar=web|g" /opt/sonarqube/conf/sonar.properties; \
    sed -i "s|#sonar.ce.javaAdditionalOpts=|sonar.ce.javaAdditionalOpts=-javaagent:./extensions/plugins/sonarqube-community-branch-plugin-${PLUGIN_VERSION}.jar=ce|g" /opt/sonarqube/conf/sonar.properties
# switching back to sonarqube user as in the original image
USER sonarqube
```

### Find old images

```bash
docker images --filter "reference=sonar*"

# remove old image if needed
docker image rm image_id
```

### Build new image

```bash
docker build . -t rubius.azurecr.io/sonarqube-with-plugin:9.9
```

### Run container

```bash
docker run -d --name sonarqube-wth-plugin --net sonarqube_sonarnet \
    -p 9000:9000 \
    -e SONAR_JDBC_URL=jdbc:postgresql://server-pg.rubius.rubius.com:5432/sonarqube \
    -e SONAR_JDBC_USERNAME=sonaradmin \
    -e SONAR_JDBC_PASSWORD=U0jozMz97aTxODRHBxIj \
    -v sonarqube_data:/opt/sonarqube/data \
    -v sonarqube_extensions:/opt/sonarqube/extensions \
    -v sonarqube_logs:/opt/sonarqube/logs \
    -v sonarqube_temp:/opt/sonarqube/temp \
    -v sonarqube_conf:/opt/sonarqube/conf \
    rubius.azurecr.io/sonarqube-with-plugin:9.9
```

