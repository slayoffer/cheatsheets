Так вот, если мы настраиваем связку Flyway и Java, то необходимые зависимости добавляются в `pom.xml`:

```
<dependencies>
    <dependency>
        <groupId>org.flywaydb</groupId>
        <artifactId>flyway-core</artifactId>
        <version>8.0.5</version>
    </dependency>
</dependencies> 
```

В том же `pom.xml` настраивается сборщик:

```
<build>
    <plugins>
        <plugin>
            <groupId>org.flywaydb</groupId>
            <artifactId>flyway-maven-plugin</artifactId>
            <version>8.0.5</version>
            <configuration>
                <locations>
                    <location>classpath:db/migration</location>
                </locations>
            </configuration>
        </plugin>
    </plugins>
</build> 
```

По умолчанию SQL-скрипты создаются в `src/main/resources/db/migration`.

И далее вызываются в коде, например:

```
import org.flywaydb.core.Flyway;

public class App {
    public static void main(String[] args) {
        Flyway flyway = Flyway.configure().dataSource("jdbc:h2:file:./target/foobar", "sa", null).load();
        flyway.migrate();
    }
} 
```

Но добавлять вызов Flyway в Spring Boot не обязательно, встроенный механизм запустит Flyway при старте приложения.

Если начать править скрипты уже мигрированные, то с гарантией получим ошибку:

```
ERROR: Validate failed: Migration checksum mismatch for migration version 20180523130900 
```

Так как хэш-сумма изменённого файла перестала совпадать с сохранённой в таблице `flyway_schema_history`, выправить это можно редактированием таблицы с историей или вообще удалением из истории — тогда скрипт выполнится ещё раз. Но лучше не заниматься переписыванием истории, а честно создать новый файл, чтобы исправить ситуацию так, как вам нужно.

Кстати, если ты запустишь приложение с Flyway для уже сформированной базы данных, то можешь получить ошибку вида:

```
org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'flywayInitializer' defined in class path resource [org/springframework/boot/autoconfigure/flyway/FlywayAutoConfiguration$FlywayConfiguration.class]: Invocation of init method failed; nested exception is org.flywaydb.core.api.FlywayException: Found non-empty schema(s) "public" but no schema history table. Use baseline() or set baselineOnMigrate to true to initialize the schema history table. 
```

Из лога ошибки видны все проблемы. Надо зачистить схему перед началом использования Flyway.

В отличие от Flyway, у Liquibase есть свои особенности:

- Настраивается через XML, JSON, YAML-файлы, хотя с версии 2.0 на чистом SQL тоже можно.
- Поддерживает версионирование для конкретного объекта — таблицы, базы и.т.д.
