### Установка Jenkins

Будем устанавливать Jenkins через пакетный менеджер **apt-get**.

1. Вначале добавим репозиторий Jenkins в локальный список и получим обновления из репозитория:

```
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > \
    /etc/apt/sources.list.d/jenkins.list'
sudo apt-get update 
```

1. Запускаем установку Jenkins:

```
sudo apt-get install -y jenkins=2.289.2 
```

Если в процессе установки Jenkins возникла ошибка, в качестве лекарства используйте команду: `update-alternatives  --install /usr/bin/java java /usr/lib/jvm/java-11-openjdk-amd64/bin/java`.

1. После установки нужно в браузере зайти на `http://<ip виртуальной машины>:8080`. Jenkins попросит пароль разблокировки. Пароль можно узнать, выполнив команду:

```
sudo cat /var/lib/jenkins/secrets/initialAdminPassword 
```

1. Далее нужно **установить плагины**. Стандартный набор нас вполне устроит. Жмём `Install suggested plugins`
2. Ожидаем окончания установки.
3. Создаём пользователя, для которого вам нужно придумать логин и пароль. Это будет административная учётная запись в Jenkins.

Внимание

Обязательно задайте пароль пользователя — веб-интерфейс Jenkins будет открыт в интернет.

1. Дальнейшие настройки оставляем по умолчанию.
2. Jenkins установлен! Перед вами должен быть подобный вид:

![image](https://pictures.s3.yandex.net/resources/07-result_1629881524.png)

1. Теперь нужно установить плагин для Maven и сам Maven.

**Maven** — это утилита для сборки Java-проектов, а плагин для Maven нужен, чтобы проще и удобнее настраивать пайплайн для сборки Maven-проектов.

Начнём с плагина:

```
Manage Jenkins -> Manage Plugins -> Available -> в поиске пишем Maven -> Выбираем Maven Integration -> внизу кнопка Install without restart 
```

1. Проверим, что плагин установился:

```
Manage Jenkins -> Manage Plugins -> Installed -> ищем в списке Maven Integration Plugin 
```

1. Теперь устанавливаем сам Maven:

```
Manage Jenkins -> Global Tool Configuration -> Прокручиваем вниз -> Maven -> Install from Apache 
```

1. Сконфигурируем JDK в Jenkins:

```
Manage Jenkins -> Global Tool Configuration (Конфигурация глобальных инструментов) -> Добавить JDK 
```

Не забудьте снять галочку с **Install Automatically**.

1. В поле «Имя» прописными буквами вводим `JDK16`.
2. В поле «JAVA_HOME» вводим путь: `/usr/lib/jvm/java-16-openjdk-amd64/`.
3. Сохраняем конфигурацию по кнопке **Save**.

1. Добавляем Credentials:

В идеальных условиях, чтобы добавить Credentials, вам понадобилось бы заранее создать техническую учётную запись для доступа из Jenkins в репозиторий Gitea. Использовать свою личную учётную запись — моветон. Но в учебных целях мы можем пойти на подобные уступки.

```
Manage Jenkins -> Manage Credentials -> Jenkins -> Global Credentials -> Add credentials
Username — пользователь в Gitea.
Password — пароль пользователя в Gitea. 
```

1. Теперь создадим Jenkins проект, который будем собирать на конвейере, как на заводе. На пути к релизу продукт проходит шаг за шагом через каждую секцию конвейера. В нашем случае подобными этапами могут быть: скачивание исходников, сборка проекта и многое другое в будущем — не будем пускать спойлеры ;)

### Настройка сборки

Наш бэк написан на Java, поэтому снова обращаемся к Maven:

Скопировать код

```
New item -> Maven Project («говорящее название для проекта»)
Копируем URL репозитория в формате https://gitea.praktikum-services.ru/path/to/repo 
Вставляем в Source Code Management --> Git --> Repositories --> Repository URL
Выбираем ранее созданные credentials.
В поле Root POM пишем путь к файлу pom.xml,
  путь указываем относительно корня репозитория, то есть backend/pom.xml.
  pom.xml - файл, отвечающий за структуру проекта, зависимости, плагины и т.д, который нужен мавену, чтобы понимать как собрать наш бэкенд 
```

Чтобы вы могли скачать артефакт сборки через интерфейс Jenkins, добавляем шаг после сборки**: Post-build Actions —> Archive the artifacts**. Собираемые с помощью Maven артефакты (в нашем случае это **jar-архив**) кладутся относительно `pom.xml` в папку `target`, поэтому указываем путь: `backend/target/*.jar`.

1. Запускаем сборку:

```
Нажимаем (Build Now)
Начинает отрабатывать пайплайн, состоящий из 2-х этапов:
 1. Скачивание исходников с системы контроля версий.
 2. Сборка проекта мавеном. 
```