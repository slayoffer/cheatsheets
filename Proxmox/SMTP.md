### Config

```bash
# За этот функционал в PVE отвечает сервис Postfix, его и будем настраивать.
# Для примера я буду использовать придуманный почтовый адрес: ggost@yandex.ru. Вы конечно используете свой!

# Будем отправлять сообщения от почтового ящика зарегистрированного на Yandex. Для Gmail отличий особых не будет.

# Проверяем установлена ли библиотека libsasl2-modules

apt install libsasl2-modules
# Делаем бекап основного файла конфигурации

cp /etc/postfix/main.cf /etc/postfix/main.cf.bak
# Вносим правки в файл. Я привел его к такому виду:

# See /usr/share/postfix/main.cf.dist for a commented, more complete version
myhostname = pve1.gregory-gost.ru
default_transport = smtp
smtpd_banner = $myhostname ESMTP $mail_name (Debian/GNU)
biff = no
# appending .domain is the MUA's job.
append_dot_mydomain = no
# Uncomment the next line to generate "delayed mail" warnings
#delay_warning_time = 4h
alias_maps = hash:/etc/aliases
alias_database = hash:/etc/aliases
mydestination = $myhostname, localhost.$mydomain, localhost
relayhost = [smtp.yandex.ru]:587
mynetworks = 127.0.0.0/8
inet_interfaces = loopback-only
recipient_delimiter = +
header_checks = pcre:/etc/postfix/rewrite_subject
smtp_sender_dependent_authentication = yes
sender_dependent_relayhost_maps = hash:/etc/postfix/sender_relayhost.hash
smtp_sasl_auth_enable = yes
smtp_sasl_password_maps = hash:/etc/postfix/sasl_auth.hash
smtp_sasl_security_options = noanonymous
smtp_use_tls = yes
smtp_tls_CAfile = /etc/ssl/certs/ca-certificates.crt
smtp_generic_maps = hash:/etc/postfix/generic

# Создаем хеш файл авторизации для доступа к почтовому ящику

echo [smtp.yandex.ru]:587 ggost@yandex.ru:yourpassword > /etc/postfix/sasl_auth.hash
# Создаем хеш файл sender_relayhost (он гарантирует, что мы всегда используем свой почтовый ящик в качестве отправителя)
echo ggost@yandex.ru [smtp.yandex.ru]:587 > /etc/postfix/sender_relayhost.hash
# И скобки [] обязательны!

# У Яндекса есть проблема, а именно для успешной отправки письма, Яндекс требует, чтобы адрес отправителя в письме совпадал с адресом для авторизации на сервере.
# Если это не будет сделано, то мы получим ошибку во время отправки — Sender address rejected: not owned by auth user
# Чтобы этого избежать мы добавили в конфиг файл /etc/postfix/main.cf пункт smtp_generic_maps = hash:/etc/postfix/generic
# Происходит это потому, что отправка системных сообщений идет от локального пользователя root.
# Имя отправителя в письме у меня такое — root@pve1.gregory-gost.ru
# В данном случае pve1.gregory-gost.ru это локальное имя сервера. Мы его заменим на учетную запись Яндекса.

# Откройте файл блокнотом
nano /etc/postfix/generic
# Добавьте в файл generic одну строку:
root@pve1.gregory-gost.ru ggost@yandex.ru

# Шифруем файлы с помощью postmap
postmap /etc/postfix/sender_relayhost.hash
postmap /etc/postfix/sasl_auth.hash
postmap /etc/postfix/generic

# Устанавливаем уровень доступа 0600 на файлы sasl_auth
chmod 0600 /etc/postfix/sasl_auth.*

# Дополнительно я ввел формирование специального заголовка для темы. За это отвечает параметр
header_checks = pcre:/etc/postfix/rewrite_subject

# Давайте создадим этот файл:
nano /etc/postfix/rewrite_subject

# Добавляем в него такую строку:
/^Subject: (.*)$/ REPLACE Subject: [PVE1]: $1

# Это регулярное выражение, которое меняет заголовок письма, начинающийся с Subject. Оно добавляет в начало темы имя сервера с двоеточием — [PVE1]:
# Вы можете добавлять свой вариант. А $1 это исходное содержание темы, которое будет без изменений оставлено далее, после добавки.

# Но для того, чтобы это работало, просто создать файл и поправить конфиг мало. Необходимо доустановить специальную библиотеку postfix-pcre
# Вы ведь помните, как выглядит строка с этой настройкой: pcre:/etc/postfix/rewrite_subject
# Давайте поставим нужный сервис:
apt install postfix-pcre

# Перезапускаем Postfix
service postfix restart
# or
systemctl restart postfix.service

# Пробуем отправить тестовое сообщение адресату:

echo "Test mail from proxmox" | mail -s test my-email@gmail.com
# Проверяйте почту

# Проверка работы Postfix:
cat /var/log/mail.log | grep postfix
```

