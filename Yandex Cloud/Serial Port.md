### Connect via SSH

```bash
# ssh -t -p 9600 -o IdentitiesOnly=yes -i ~/.ssh/<имя закрытого ключа> <ID виртуальной машины>.<имя пользователя>@serialssh.cloud.yandex.net
ssh -t -p 9600 -o IdentitiesOnly=yes -i ~/.ssh/yandex_deploy_id_rsa fhmjlf0rbsjucpi6dj3v.slayo@serialssh.cloud.yandex.net

# Чтобы отключиться от серийной консоли, нажмите клавишу Enter, а затем введите символы ~. (тильда и точка).
```

