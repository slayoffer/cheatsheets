## Интеграция GitLab CI с HashiCorp Vault

Интеграция приложения с секретницей ограничивает разработчиков одной единственной системой хранения секретов. А при изменении окружения, способа доставки секретов или даже адреса сервера HashiCorp Vault нужно пересобирать приложение в новых реалиях. 

Чтобы унифицировать процесс работы с чувствительной информацией, такую интеграцию удобнее настроить через CI: GitLab получает значения секретов и передаёт их приложению в пайплайне развёртывания. Подружить GitLab CI с HashiCorp Vault можно с помощью JWT-токенов.

Кнопка: Что такое JWT-токен

Во время выполнения задачи в GitLab в переменной окружения `CI_JOB_JWT` находится специальный токен, сформированный по стандарту JWT. Токен JWT — это строка, состоящая из трёх частей, разделённых точкой:

- Заголовок — JSON-текст, в котором может быть указан тип и алгоритм формирования цифровой подписи токена. Такой JSON-текст кодируется с помощью алгоритма Base64Url.
- Содержимое токена — JSON-текст, в котором указаны поля полезной нагрузки: имя пользователя, время создания и время такого токена.
- Цифровая подпись токена.

Такой токен позволяет безопасно передать информацию о пользователе другой системе: содержимое токена проверяется на легитимность с помощью цифровой подписи или даже шифруется.

Внутри JWT токена GitLab CI содержатся такие поля, как имя пользователя GitLab, название и идентификатор проекта. На их основе можно настроить доступ к секретам в HashiCorp Vault: Vault проверит поля токена, проанализирует значения и примет решение давать или нет доступ к секретам.

## Настроим HashiCorp Vault

1. Включим JWT-аутентификацию в Vault:

```
$ vault auth enable jwt
Success! Enabled jwt auth method at: jwt/ 
```

1. Настроим конфигурацию JWT-аутентификации:

```
$ vault write auth/jwt/config \
    jwks_url="https://gitlab.praktikum-services.ru/-/jwks" \
    bound_issuer="gitlab.praktikum-services.ru"
Success! Data written to: auth/jwt/config 
```

По [ссылке](https://gitlab.praktikum-services.ru/-/jwks) находятся публичные ключи, которые используются для проверки цифровой подписи JWT-токенов. GitLab выполнит такую подпись своим закрытым ключом, а Vault проверит подпись с помощью публичного ключа.

1. Добавим политику, разрешающую чтение секретов сосисочной:

```
# Тут нам понадобится heredoc, поэтому наш удобный алиас не подойдет
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

Политики в Vault описываются в формате HCL и служат для аккуратной настройки доступа к секретам.

1. Для связывания такой политики с JWT-аутентификацией нужно создать роль, в которой будут описаны требуемые поля токена:

```
$ sudo docker exec -i vault vault write auth/jwt/role/sausage-store - <<EOF
{
  "role_type": "jwt",
  "policies": ["sausage-store"],
  "token_explicit_max_ttl": 60,
  "user_claim": "user_login",
  "bound_claims": {
    "project_path": "<you gitlab username>/<your project name>",
    "ref": "main",
    "ref_type": "branch"
  }
}
EOF
Success! Data written to: auth/jwt/role/sausage-store 
```

Эта роль использует способ "bound_claims": только JWT-токену с указанными полями будет разрешено читать секрет.

## Настройка GitLab CI

Теперь изменим GitLab CI. В одном из уроков этой главы вы уже настроили деплой бэкенда:

```
deploy-backend:
  stage: deploy
  image: alpine:3.15.0
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

1. Заменим значение образа для выполнения задачи на `image: vault:1.11.3` для использования консольного клиента Vault. Этот образ основан на том же образе alpine.
2. Добавим в переменные GitLab CI `VAULT_ADDR` со значением адреса вашей ВМ, на которой развёрнут сервер HashiCorp Vault: `http://<your vm ip>:8200`
3. Значение переменной `VAULT_TOKEN` заполним с помощью команды, используя созданную ранее роль в Vault:

```
vault write -field=token auth/jwt/login role=sausage-store jwt=$CI_JOB_JWT 
```

С помощью JWT-токена из переменной CI_JOB_JWT получим токен HashiCorp Vault для работы с секретами.

1. Теперь для получения значения секрета из Vault используем `vault kv get`:

```
export "SPRING_DATASOURCE_PASSWORD=$(vault kv get -field=spring.datasource.password secret/sausage-store)"; 
```

Итоговая конфигурация для CI будет выглядеть так:

```
deploy-backend:
  stage: deploy
  image: vault:1.11.3
  before_script:
    - apk add openssh-client bash
    - eval $(ssh-agent -s)
    - echo "$SSH_PRIVATE_KEY" | tr -d '\r' | ssh-add -
    - mkdir -p ~/.ssh
    - chmod 700 ~/.ssh
    - echo "$SSH_KNOWN_HOSTS" >> ~/.ssh/known_hosts
    - chmod 644 ~/.ssh/known_hosts
  script:
    - export VAULT_TOKEN="$(vault write -field=token auth/jwt/login role=sausage-store jwt=$CI_JOB_JWT)"
    - ssh ${DEV_USER}@${DEV_HOST}
      "export "VERSION=${VERSION}";
       export "SPRING_DATASOURCE_URL=${PSQL_DATASOURCE}";
       export "SPRING_DATASOURCE_USERNAME=${PSQL_USER}";
       export "SPRING_DATASOURCE_PASSWORD=$(vault kv get -field=spring.datasource.password secret/sausage-store)";
       export "SPRING_DATA_MONGODB_URI=${MONGO_DATA}";
      /bin/bash -s " < ./backend/backend_deploy.sh 
```

### Полезные материалы

1. [Manage your Database Accounts with Spring Cloud Vault Config](https://medium.com/digitalfrontiers/manage-your-database-accounts-with-spring-cloud-vault-config-48cecb837a36).
2. [Adopting HashiCorp Vault](https://www.hashicorp.com/resources/adopting-hashicorp-vault).
3. [HashiCorp Vault Architecture](https://www.vaultproject.io/docs/internals/architecture).
4. [HashiCorp Vault Secrets Engines](https://www.vaultproject.io/docs/secrets).
5. [Using external secrets in CI](https://docs.gitlab.com/ee/ci/secrets/).
6. [JWT Tokens](https://jwt.io/).