Получился Dockerfile в папке **backend**:

```dockerfile
#Это так называемый мультистейдж docker-файл. Сначала делаем образ, который соберёт нам java-приложение,
#а затем из него берём собранный артефакт и кладём его в новый образ, чтобы оставить в нём меньше лишнего

# build
FROM maven:3.8.2-openjdk-16-slim as builder
# задаём переменную VERSION, которая будет использоваться для сборки проекта
ARG VERSION=${VERSION}
WORKDIR /usr/src/app
COPY ./src ./src
COPY *.xml ./
# копируем CA-сертификат Яндекса в образ
RUN curl https://storage.yandexcloud.net/cloud-certs/CA.pem -o YandexInternalRootCA.crt \
    && mvn package -Dversion.application=${VERSION}

# release
FROM openjdk:16-jdk-alpine
ARG VERSION=${VERSION}
WORKDIR /app
COPY --from=builder /usr/src/app/YandexInternalRootCA.crt .
# подкидываем CA-сертификат Яндекса в хранилище сертификатов для Java
# создаём пользователя jaruser
RUN keytool -importcert -file YandexInternalRootCA.crt \
    -alias yandex -cacerts -storepass changeit -noprompt \
    && rm -rf YandexInternalRootCA.crt \
    && addgroup --system jaruser \
    && adduser -S -s /bin/false -G jaruser jaruser -D -H \
    && apk add --no-cache dumb-init==1.2.5-r0
COPY --chown=jaruser:jaruser \
     --from=builder /usr/src/app/target/sausage-store-${VERSION}.jar ./sausage-store.jar
EXPOSE 8080
# приложение будет запускаться под пользователем jaruser
USER jaruser
ENTRYPOINT ["dumb-init", "java", "-jar", "-Dmyserver.bindPort=8080", "./sausage-store.jar"] 
```

Пакуем фронтенд — можно не подглядывать и попробовать самостоятельно:

```dockerfile
FROM node:latest
# Create app directory
WORKDIR /usr/src/app
COPY . .
RUN npm install
RUN npm install -g http-server
RUN npm run build
EXPOSE 80
ENTRYPOINT ["http-server", "dist/frontend/", "-p", "80", "--proxy", "http://sausage-store:8080"] 
```

Теперь попробуем собрать образы фронтенда и бэкенда.

В папке `backend` выполним:

```bash
docker build --build-arg VERSION=0.0.1 -t sausage-backend:0.0.1 . 
```

А теперь в папке `frontend`:

```
docker build --build-arg VERSION=0.0.1 -t sausage-frontend:0.0.1 . 
```

Проверим, что образы собрались:

```
 docker images 
```

Выполним в папке с бэкендом подключение к общим БД, данные для которых лежат у вас на почте:

```bash
# создать файл .env с содержимым:
SPRING_DATASOURCE_URL=jdbc:postgresql://<postgresql server ip>:5432/sausagestore
SPRING_DATASOURCE_USERNAME=<Имя пользователя в Постгресе>
SPRING_DATASOURCE_PASSWORD=<Пароль пользователя в Постгресе>
SPRING_DATA_MONGODB_URI=mongodb://user:pass@monga.mdb.yandexcloud.net:27018/my_db?tls=true
 
docker run --rm -d --env-file .env --name sausage-backend --network=host sausage-backend:0.0.1 
```

Аналогично, используя сеть хоста, нужно запустить контейнер фронтенда.

Необходимо проверить, что всё работает, и страничка с фронтом отдаётся на хосте.

В целом, Docker может сделать наше приложение ещё и безопаснее. Зачем открывать оба порта 80 для фронтенда и 8080 для бэкенда наружу? Туда должен торчать только фронтенд. С помощью Docker можно создать внутреннюю сеть и подключить туда два контейнера с фронтом и бэком, а наружу выставить только фронт. Это уменьшит поверхность атаки.

Давайте попробуем:

```bash
docker network create -d bridge sausage_network 
# в папке backend:
docker run --rm -d --env-file .env --name sausage-backend --network=sausage_network sausage-backend:0.0.1
# в папке frontend:
docker run --rm -d --env-file .env --name sausage-frontend --network=sausage_network -p 8080:80 sausage-frontend:0.0.1 
```

Сниппет из `.gitlab-ci` для **бэкенда**, cвязанный с контейнеризацией. Остальная часть CI у вас есть:

Скопировать кодYAML

```yaml
# В нашем Gitlab для сборки контейнеров воспользуемся Докером в Докере :)  
# https://docs.gitlab.com/ee/ci/docker/using_docker_build.html#use-the-kubernetes-executor-with-docker-in-docker
# Для сборки образов с использованием Docker-in-Docker:
# добавить в код Downstream пайплайнов в секцию include подготовленный шаблон, содержащий необходимые настройки:
#  https://gitlab.praktikum-services.ru/templates/ci/-/blob/main/DockerInDockerTemplate.yml
# использовать в задачах сборки в качестве образа стабильную версию образа Docker:dind docker:20.10.12-dind-rootless

include:
  - project: 'templates/ci'
    file: 'DockerInDockerTemplate.yml'
    
stages:
  - build
  - release
  - deploy

build-backend:
  stage: build
  image: docker:20.10.12-dind-rootless
  before_script:
    - until docker info; do sleep 1; done
    # переменные CI_REGISTRY_USER, CI_REGISTRY_PASSWORD, CI_REGISTRY генерятся Гитлабом, их задавать не надо
    - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY
  script:
    - cd backend
    - >
      docker build
      --build-arg VERSION=$VERSION
      --tag $CI_REGISTRY_IMAGE/sausage-backend:$CI_COMMIT_SHA
      .
    - docker push $CI_REGISTRY_IMAGE/sausage-backend:$CI_COMMIT_SHA

upload-backend-latest:
  variables:
    GIT_STRATEGY: none
  image: docker:20.10.12-dind-rootless
  stage: release
  before_script:
    - until docker info; do sleep 1; done
    - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY
  script:
    - docker pull $CI_REGISTRY_IMAGE/sausage-backend:$CI_COMMIT_SHA
    # если образ прошел проверки в CI (сканирование, тесты и т.д), то тегаем latest
    - docker tag $CI_REGISTRY_IMAGE/sausage-backend:$CI_COMMIT_SHA $CI_REGISTRY_IMAGE/sausage-backend:latest
    - docker push $CI_REGISTRY_IMAGE/sausage-backend:latest

deploy-backend:
  stage: deploy
  image: alpine:3.15.0
  # если хотим сделать деплой по кнопке
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'
      when: manual
    - if: '$CI_COMMIT_BRANCH == "master"'
      when: manual
  before_script:
    - apk add openssh-client bash
    - eval $(ssh-agent -s)
    - echo "$SSH_PRIVATE_KEY" | tr -d '\r' | ssh-add -
    - mkdir -p ~/.ssh
    - chmod 700 ~/.ssh
    - echo "$SSH_KNOWN_HOSTS" >> ~/.ssh/known_hosts
    - chmod 644 ~/.ssh/known_hosts
  script:
    - ssh ${DEV_USER}@${DEV_HOST}
      "export "VERSION=${VERSION}";
       export "SPRING_DATASOURCE_URL=${PSQL_DATASOURCE}";
       export "SPRING_DATASOURCE_USERNAME=${PSQL_USER}";
       export "SPRING_DATASOURCE_PASSWORD=${PSQL_PASSWORD}";
       export "SPRING_DATA_MONGODB_URI=${MONGO_DATA}";
      /bin/bash -s " < ./backend/backend_deploy.sh 
```

`backend_deploy.sh`:

```bash
#!/bin/bash
set +e
cat > .env <<EOF
SPRING_DATASOURCE_URL=${SPRING_DATASOURCE_URL}
SPRING_DATASOURCE_USERNAME=${SPRING_DATASOURCE_USERNAME}
SPRING_DATASOURCE_PASSWORD=${SPRING_DATASOURCE_PASSWORD}
SPRING_DATA_MONGODB_URI=${SPRING_DATA_MONGODB_URI}
EOF
docker network create -d bridge sausage_network || true
docker pull <реестр Gitlab Registry>/sausage-store/sausage-backend:latest
docker stop backend || true
docker rm backend || true
set -e
docker run -d --name backend \
    --network=sausage_network \
    --restart always \
    --pull always \
    --env-file .env \
    <реестр Gitlab Registry>/sausage-store/sausage-backend:latest 
```