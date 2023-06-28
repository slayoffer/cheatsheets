Для запуска сервера HashiCorp Vault мы будем использовать Docker: свежие версии образов доступны для скачивания на [hub.docker.com](https://hub.docker.com/), но для управления сервисом применим известную вам механику операционной системы — systemd. Такая интеграция облегчает сопровождение инфраструктуры и позволяет в том же Ansible использовать существующие модули для управления состоянием сервиса.

Создайте на своей виртуальной машине systemd unit-файл `/etc/systemd/system/vault.service` со следующим содержимым:

```bash
[Unit]
Description=HashiCorp Vault Service (Docker container)
After=docker.service
Requires=docker.service

[Service]
TimeoutStartSec=0
Restart=always

# Версия Vault и порт для сервера
Environment=VAULT_VERSION=1.11.3
Environment=VAULT_PORT=8200

ExecStartPre=-/usr/bin/docker exec vault stop
ExecStartPre=-/usr/bin/docker rm vault
ExecStartPre=/usr/bin/docker pull vault:${VAULT_VERSION}

# Запуск контейнера: IPC_LOCK даёт разрешение на блокировку сброса
# памяти на диск (например, во время свопа), а для передачи серверу
# конфигурации и хранения данных мы используем docker volumes.
# (Мы будем использовать файловое хранилище для секретов)
# Переменная VAULT_ADDR понадобится для работы консольного клиента Vault.
ExecStart=/usr/bin/docker run --rm --name vault \
    --cap-add=IPC_LOCK \
    -v /opt/vault/config:/vault/config \
    -v /opt/vault/file:/vault/file \
    -v /opt/vault/logs:/vault/logs \
    -e VAULT_ADDR="http://127.0.0.1:${VAULT_PORT}" \
    -p ${VAULT_PORT}:${VAULT_PORT} \
    vault:${VAULT_VERSION} server

[Install]
WantedBy=default.target 
```

Важно

HashiCorp Vault также можно запустить в ознакомительном «Dev Server Mode». В таком режиме сервер хранит данные только в памяти и большинство настроек получают небезопасные значения по умолчанию. Запуск не требует процедуры «распечатывания», так как данные нигде не хранятся и потеряются при перезапуске. Стоит ли говорить, что никогда-никогда-никогда не следует использовать этот режим для рабочих задач как с точки зрения сохранности данных, так и безопасности.

Создайте директории для docker volumes:

```bash
sudo mkdir -p /opt/vault/{config,file,logs} 
```

Теперь подготовим конфигурацию сервиса. 

HashiCorp Vault использует тот же язык конфигурации, который применяется для Terraform — HashiCorp Configuration Language. Создайте конфигурационный файл `/opt/vault/config/config.hcl` со следующим содержимым:

```bash
// Используем файловый бэкенд для хранения секретов
storage "file" {
  path    = "/vault/file"
}

// Для учебных целей запустим сервер на HTTP
listener "tcp" {
  address     = "0.0.0.0:8200"
  tls_disable = "true"
}

disable_mlock = true

api_addr = "http://127.0.0.1:8200"
cluster_addr = "https://127.0.0.1:8201"

// Включим пользовательский Web-интерфейс
ui = true 
```

Обратите внимание

Сервер работает по HTTP. Взаимодействие с ним выполняется в открытом виде. Стоит ли говорить...? Ах да, стоит. В качестве бонусного задания предлагаем вам запустить сервер на HTTPS.

Перечитаем конфигурацию systemd и запустим сервис:

```bash
sudo systemctl daemon-reload
sudo systemctl enable vault --now
sudo systemctl status vault 
```

Команда `systemctl enable vault --now` включает автозапуск сервиса при перезапуске виртуальной машины и запускает его.

Теперь проверим логи запущенного контейнера vault:

```bash
sudo journalctl -u vault 
```

Убедимся, что сервис запустился:

```bash
==> Vault server configuration:
             Api Address: http://127.0.0.1:8200
                     Cgo: disabled
         Cluster Address: https://127.0.0.1:8201
              Go Version: go1.17.13
              Listener 1: tcp (addr: "0.0.0.0:8200", cluster address: "0.0.0.0:8201", max_request_duration: "1m30s", max_request_size: "33554432", tls: "disabled")
               Log Level: info
                   Mlock: supported: true, enabled: false
           Recovery Mode: false
                 Storage: file
                 Version: Vault v1.11.3, built 2022-08-26T10:27:10Z
             Version Sha: 17250b25303c6418c283c95b1d5a9c9f16174fe8
==> Vault server started! 
```

Для работы с HashiCorp Vault из командной строки нам понадобится выполнять команды внутри контейнера. Применяя `docker exec` для контейнера с именем `vault`, мы будем вызывать консольный клиент `vault`, а раз контейнер запущен под `root`, потребуется `sudo`:

```
sudo docker exec -it vault vault 
```

`-it - i` — чтобы можно было вводить в терминал, а `-t` — чтобы был сам терминал.

Для удобства использования такой конструкции возьмём командный `alias`:

```shell
$ alias vault='sudo docker exec -it vault vault'
$ vault status
Key                Value
---                -----
Seal Type          shamir
Initialized        false
Sealed             true
Total Shares       0
Threshold          0
Unseal Progress    0/0
Unseal Nonce       n/a
Version            1.11.3
Build Date         2022-08-26T10:27:10Z
Storage Type       file
HA Enabled         false 
```

Статус сообщает, что сервер ещё не инициализирован — не создан корневой ключ шифрования секретов. Оператор (то есть вы) должен выполнить команду:

```
$ vault operator init
Unseal Key 1: hqK8dAVWs76QD2zJL+x6F/b6F/a1kRYnXn8RwtSRQCLV
Unseal Key 2: S54ucyxA6uotAsp+7Q7hBEu8GfwJAJzEaJrDgBCfMzMS
Unseal Key 3: u8/u/khbo+BoXhqpVX5IBCLpG35ePEEvQiY06uXWAkQT
Unseal Key 4: R/EZ6hA5q1neEY3/h1jFbDJts9Y5w24D7Kaodv7hh2Yk
Unseal Key 5: oGVyhv+z5zhZIACh7oJlO3rnJHsi6AU++ReIv9bIyuJ2

Initial Root Token: hvs.eZHP3OPWmolACx0spWui7x5h 
```

Схема разделения секретов «Шамира» создала пять ключей распечатывания, которые нужно передать ответственным администраторам по безопасным каналам. Для распечатывания сервера необходимы три из этих пяти.

Внимание

Запомните токен (`Initial Root Token`) — вы будете использовать его в следующих практических заданиях. А сейчас выполним операцию запечатывания и распечатывания для проверки корректности работы сервера.

1. Перезапустим сервис:

```
sudo systemctl restart vault 
```

1. Убедимся, что сервер перешёл в запечатанное состояние:

```
$ vault status
Key                Value
---                -----
Seal Type          shamir
Initialized        true
Sealed             true # Sealed указывает на состояние сервера
Total Shares       5
Threshold          3
Unseal Progress    0/3
Unseal Nonce       n/a
Version            1.11.3
Build Date         2022-08-26T10:27:10Z
Storage Type       file
HA Enabled         false 
```

1. Введём последовательно три ключа распечатывания с помощью команды:

```
vault operator unseal 
```

1. После успешного выполнения убедимся в изменении статуса:

```
$ vault status
Key             Value
---             -----
Seal Type       shamir
Initialized     true
Sealed          false
Total Shares    5
Threshold       3
Version         1.11.3
Build Date      2022-08-26T10:27:10Z
Storage Type    file
Cluster Name    vault-cluster-ba5a684e
Cluster ID      01e32171-8ee5-76bd-7161-8d78815bc2af
HA Enabled      false 
```

### Подготовим хранилище для секретов

Выполним команду `vault login` и введём токен (`Initial Root Token`):

```
$ vault login
Token (will be hidden):
Success! You are now authenticated. The token information displayed below
is already stored in the token helper. You do NOT need to run "vault login"
again. Future Vault requests will automatically use this token.

Key                  Value
---                  -----
token                hvs.eZHP3OPWmolACx0spWui7x5h
token_accessor       QPbKiJDfyeoLbrV2TuaPUwt6
token_duration       ∞
token_renewable      false
token_policies       ["root"]
identity_policies    []
policies             ["root"] 
```

Мы будем использовать key/value хранилище и его нужно сначала «включить»

```
$ vault secrets enable -version=2 -path=secret kv
Success! Enabled the kv secrets engine at: secret/ 
```

Различные механизмы («engines») хранения секретов в HashiCorp Vault получают свой собственный «путь» при работе с API сервера. Мы включили «KV/2 engine» для пути `secret/` и любое обращение к этому пути будет маршрутизироваться в хранилище ключ-значение

Что ж, попробуем сохранить свой первый секрет

Воспользуемся командой `vault kv put`:

```
vault kv put secret/pass passcode=my-long-passcode
```

Получим:

```
Key                Value
---                -----
created_time       2022-09-03T17:01:04.336447615Z
custom_metadata    <nil>
deletion_time      n/a
destroyed          false
version            1 
```

Эти данные можно использовать для аудита.

Мы положили в хранилище Vault пароль с именем `pass` и значением `my-long-passcode` и сохранили его в пространстве `secret`.

Чтобы обновить пароль, выполним ещё раз команду `vault kv put secret/pass passcode=my-long-passcode`

Теперь попробуем извлечь секрет. Для начала выведем список секретов.

Выполним: `vault kv list secret`

Теперь извлечём наш ключ с именем `pass`.

Выполним: `vault kv get secret/pass`

По умолчанию Vault будет извлекать самую последнюю версию секрета, но если мы хотим получить предыдущую версию, можно использовать директиву `-version`:

```
vault kv get -version=1 secret/pass
```

Так что, версионирование секретов весьма удобно, особенно если где-то понадобился старый пароль

Если секрет больше не нужен, его можно уничтожить. Есть два варианта удаления: `delete` и `destroy`

Попробуем удалить первую версию нашего секрета:

```
vault kv delete -versions=1 secret/pass
```

Эта команда помечает данные как удалённые (**deleted**) и предотвращает их извлечение в обычных запросах GET, но фактически **не удаляет** данные:

Выполним: `vault kv get -version=1 secret/pass`

Видим, что флаг `destroyed=false`. 

Чтобы данные действительно были удалены без возможности восстановления, необходимо использовать команду `destroy`:

```
$ vault kv destroy -versions=1 secret/pass
Success! Data written to: secret/destroy/pass 
```

Вместо того чтобы просто пометить данные как удалённые и ограничить доступ к ним, команда `destroy` удалит их полностью, что сделает невозможным последующее извлечение.

## Создание роли для сосисочной

Для интеграции приложения с секретницей понадобится новый токен, который отличается от рутового. Такой токен будет ограничен в правах, ведь ему нужно только читать секреты по определённому пути.

По умолчанию новый токен наследует все разрешения родительского токена — того токена, с помощью которого был выпущен новый токен. Для изменения этого поведения нужно указать политику (`policy`): именованный набор правил, где указано, что можно делать и с какими секретами.

Политики создаются с помощью HashiCorp Configuration Language. Добавим политику, разрешающую чтение секретов сосисочной:

```
# Тут нам понадобится heredoc, поэтому наш удобный алиас не подойдёт
sudo docker exec -i vault vault policy write sausage-store - <<EOF
# Policy name: sausage-store
#
# Read-only permission on 'secret/data/sausage-store' KV 2 path
path "secret/data/sausage-store" {
  capabilities = [ "read" ]
}
EOF

Success! Uploaded policy: sausage-store 
```

Внимание

В политике определён список доступа (`ACL`), в котором указывается путь к секретам (`path`) и разрешённые операции (`capabilities`). Путь к секретам состоит из имени движка хранения секретов (`secret` для KV 2), обязательного префикса `data` и имени хранилища: "sausage-store".

аких политик может быть несколько, а для создания токена ещё понадобится указать время жизни и тип токена. Для того чтобы собрать все этим параметры в одном месте, нужно создать роль — именованный набор параметров, который можно использовать для производства токена:

Скопировать кодSHELL

```
$ vault write auth/token/roles/sausage-store \
    allowed_policies="sausage-store" \
    orphan=true \
    period=8h

Success! Data written to: auth/token/roles/sausage-store 
```

Этой командой мы создали роль с именем "sausage-store". Параметр `orphan` позволяет всем токенам этой роли быть независимыми от времени жизни корневого токена, а `period` указывает интервал времени, в течение которого нужно обновлять время жизни. Если в течение 8 часов время жизни токена не обновить, токен будет аннулирован.

Теперь можно выпустить токен, используя новую роль:

Скопировать кодSHELL

```
$ vault token create -role=sausage-store

Key                  Value
---                  -----
token                hvs.CAESIBuHeWXIc-hNB5cpQGAUJWZv_At7sapmhzVVbY6EiGDqGh4KHGh2cy5hNm5SdWpzVVkxVlBCdDBCdHp5R01WSkU
token_accessor       1rMoML5cc24iUxtaMMW9WUth
token_duration       8h
token_renewable      true
token_policies       ["default" "sausage-store"]
identity_policies    []
policies             ["default" "sausage-store"] 
```

Сохраните строку токена, вам она понадобится для настройки интеграции Spring Boot с HashiCorp Vault.

Рассмотрим два варианта интеграции: с помощью библиотеки фреймворка Spring Boot и через GitLab CI с HashiCorp Vault.