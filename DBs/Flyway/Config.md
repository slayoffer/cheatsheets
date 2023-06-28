```sql
У нас есть конфигурационный файл, содержащий основные настройки flyway.conf; также в нём можно указывать настройки подключения к базе.
# указываем папки, в которых ведётся поиск миграцинных скриптов
flyway.locations=filesystem:db/migration,filesystem:db/bottom

# url для подключения к базе, включающий имя драйвера, хост, порт и имя базы
flyway.url=jdbc:sqlserver://192.168.1.30:1433;database=test

# путь к драйверу, который используется для подключения к базе
# flyway.driver=

# пользователь для подключения к базе
flyway.user=user  

# пароль для подключения к базе
flyway.password=sec_password

# количество попыток подключения к базе
flyway.connectRetries=3

# выбираем, до какой версии выполнять миграционные скрипты
flyway.target=4

# https://flywaydb.org/documentation/configuration/configfile.html
```
