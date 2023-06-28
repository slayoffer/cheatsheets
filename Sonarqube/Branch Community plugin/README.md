### (Ре)деплой SonarQube + SonarQube Community Branch plugin через Volume и Compose

Плагин версии 1.14.0  уже лежит в volume `sonarqube_extensions` по пути `/var/lib/docker/volumes/sonarqube_extensions/_data`

Если необходимо его обновить, тогда сперва нужно удалить там старый .jar файл плагина и загрузить туда же новый.

**Важно: перед обновлением плагина нужно свериться с [compatibility matrix](https://github.com/mc1arke/sonarqube-community-branch-plugin#compatibility) версий плагина и версий Сонаркуба**.

Настройки плагина уже прописаны в volume `sonarqube_conf` по пути `/var/lib/docker/volumes/sonarqube_conf/_data/sonar.properties`.

Если конфиг необходимо обновить новыми настройками плагина, то его можно править прямо в volume, и контейнер Сонара подхватит этот конфиг при перезапуске.

Для обновления необходимо перезапустить контейнер Сонаркуба (желательно через `Compose` файл, который лежит в `/home/administrator/sonarqube`):

```bash
cd /home/administrator/sonarqube

docker compose up -d
```

При необходимости запуска через `docker run` можно использовать следующую команду:

```bash
docker run -d --name sonarqube-with-plugin --net sonarqube_sonarnet \
    -p 9000:9000 \
    -e SONAR_JDBC_URL=jdbc:postgresql://server-pg.rubius.rubius.com:5432/sonarqube \
    -e SONAR_JDBC_USERNAME=sonaradmin \
    -e SONAR_JDBC_PASSWORD=U0jozMz97aTxODRHBxIj \
    -v sonarqube_data:/opt/sonarqube/data \
    -v sonarqube_extensions:/opt/sonarqube/extensions \
    -v sonarqube_logs:/opt/sonarqube/logs \
    -v sonarqube_temp:/opt/sonarqube/temp \
    -v sonarqube_conf:/opt/sonarqube/conf \
    sonarqube:9.9.0-community
```

### (Ре)деплой SonarQube + SonarQube Community Branch plugin через Dockerfile и Compose

> Текущая версия SonarQube: 9.9

> Текущая версия плагина SonarQube Community Branch plugin: 1.14.0

> Текущий образ: rubius.azurecr.io/sonarqube-with-plugin:9.9

При необходимости, все версии можно поменять в `Dockerfile`, который находится в папке `/home/administrator/sonarqube` на Докер хосте, и пересобрать образ:

```
docker build . -t rubius.azurecr.io/sonarqube-with-plugin:latest
```

Проверить имя образа в файле `docker-compose.yml` (должно быть`rubius.azurecr.io/sonarqube-with-plugin:latest`) и запустить контейнер:

```
docker compose up -d
```



##### Обновление SonarQube и SonarQube Community Branch plugin

1) Остановить контейнер Сонара либо зайти в папку `/home/administrator/sonarqube` и написать команду

```
docker compose down
```

2) Поменять версии SonarQube и/или SonarQube Community Branch plugin в `Dockerfile`, который находится в папке `/home/administrator/sonarqube` на Докер хосте, и пересобрать образ

```
docker build . -t rubius.azurecr.io/sonarqube-with-plugin:latest
```

3) Проверить имя образа в файле `docker-compose.yml` (должно быть`rubius.azurecr.io/sonarqube-with-plugin:latest`) и запустить контейнер

```
docker compose up -d
```

###### Опционально:

Найти докер образы с именем `sonar*`:

```bash
docker images --filter "reference=sonar*"
```

Удалить старый образ:

```bash
docker image rm rubius.azurecr.io/sonarqube-with-plugin:latest
```



### При ошибке "нехватки памяти OS для JVM" на старте контейнера

Необходимо обновить версию Docker на хосте.
