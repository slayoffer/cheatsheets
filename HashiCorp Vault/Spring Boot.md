## Интеграция Spring Boot с HashiCorp Vault

В Spring Boot есть кунштюки для работы с Vault, которые нужно добавить в `pom.xml` зависимости:

Для начала обновим Spring до актуальной версии (**Всплывашка**:  Это важно для корректной интеграции Spring Boot с HashiCorp Vault. Да и обновлять приложения — это хорошая практика, конечно, если вы внимательно прочитали changelog и проверили обновление в тестовом окружении.).

Нужно зайти `pom.xml` и внести изменения:

```
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.6.2</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent> 
```

На момент создания инструкции актуальной версией была `2.6.2`. Дабы подстраховаться, можно поискать в интернете Maven Spring Boot и взять последнюю версию. Альтернативный вариант — в будущем развернуть такого бота, как **dependabot**, который будет сам отслеживать актуальные версии и слать вам пулл-реквесты.

Теперь надо добавить в `pom.xml` пару изменений для работы с Vault (хорошо, что в экосистеме Spring'а «всё включено»):

```
   <properties>
        <java.version>16</java.version>
        <sonar.projectKey>test_manual</sonar.projectKey>
        <sonar.qualitygate.wait>true</sonar.qualitygate.wait>
        <spring-cloud.version>2021.0.0</spring-cloud.version>
    </properties> 
```

```
 <dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-dependencies</artifactId>
            <version>${spring-cloud.version}</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement> 
```

```
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-vault-config</artifactId>
</dependency> 
```

Кстати, вы могли заметить, что мы не пишем версию зависимости для работы с Vault — Spring Boot поймёт, какие библиотеки взять, да и разработчики заведомо позаботились об их совместимости.

Так вот, мы добрались до последнего штриха, который необходимо добавить в наш сосисочный бэкенд, а именно — дописать `application.properties`:

```
spring.application.name=sausage-store
management.security.enabled=false

spring.datasource.url=jdbc:postgresql://<ваш postgres-host>:<ваш postgres-port>/sausagestore
spring.datasource.driver-class-name=org.postgresql.Driver
spring.datasource.username=postgres

spring.datasource.password=password — удаляем пароль из конфига, безопасность должна быть безопасной

spring.jpa.database-platform=org.hibernate.dialect.PostgreSQLDialect
spring.jpa.show-sql=false

# наш токен для подключения к Vault
spring.cloud.vault.token=my token for sausage-store
# протокол, по которому доступно API Vault, в проде лучше сделать https
spring.cloud.vault.scheme=http
spring.cloud.vault.host=<ваш vault host>
spring.cloud.vault.kv.enabled=true
spring.config.import=vault://secret/sausage-store/${spring.application.name} 
```

При запуске Spring Boot сходит в Vault и возьмёт секреты по указанному пути. Мы можем сохранить секреты через WEB UI Vault'а, либо через консоль. Он смотрит наружу через порт 8200

Заходим в браузере: `http://<адрес хоста с Воулт>:8200`.

Вводим наш корневой токен.

Теперь осталось закинуть туда все свои секреты, используя UI, либо консоль в контейнере Vault:

```
vault kv put secret/sausage-store spring.datasource.password=postgres 
```

Токен не стоит хранить в `application.properties`!

При старте Spring Boot может читать переменные окружения. И настройка `spring.cloud.vault.token` трансформируется в переменную окружения `SPRING_CLOUD_VAULT_TOKEN`, с помощью которой можно будет передать токен

Но есть другой путь — токен вообще нигде не будет храниться. В этом нам поможет интеграция GitLab с HashiCorp Vault