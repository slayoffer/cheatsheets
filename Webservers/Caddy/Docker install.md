Поехали! Создадим контейнер на базе Debian и откроем в нём консоль bash.

Для того чтобы вводить с терминала, добавим ключ `-it (--interactive --tty)`.

Итак, выполним команду:

```
docker run -it --name=caddy debian 
```

Установим теперь в этом контейнере Caddy, следуя [официальной инструкции по установке Caddy](https://caddyserver.com/docs/install#debian-ubuntu-raspbian).

Обратите внимание, мы запустили контейнер и попали в него под учеткой **root**, поэтому сейчас где-то грустит один безопасник.

Залезаем в контейнер:

```
apt update && apt install curl -y
apt install -y debian-keyring debian-archive-keyring apt-transport-https
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | gpg --dearmor -o /usr/share/keyrings/caddy-stable-archive-keyring.gpg
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' | tee /etc/apt/sources.list.d/caddy-stable.list
apt update
apt install caddy 
```

Создадим папку под конфиг для Caddy, а сам конфиг сохраним в файл Caddyfile

```
mkdir caddy
cd caddy
tee -a Caddyfile << END
# Содержимое файла Caddyfile
:8080 {
        respond "Hi there"
}
END 
```

Запустим сервис. Кэдди будет отдавать ответ "Hi there" по http на порту 8080:

```
caddy run  
```

Откроем новую сессию и выполним внутри контейнера команду `curl`, дабы убедиться, что всё работает:

```
docker exec -it caddy curl localhost:8080 
```

В итоге консоль должна вернуть "Hi there".

Выйдем из контейнера и выполним:

```
   docker commit caddy caddy-image 
```

Итак, мы только что создали свой образ с Caddy на базе Дебиана. Докер-демон финализировал верхний слой, в котором мы работали с контейнером, и добавил этот слой в локальный репозиторий.

Можно проверить, что образ действительно создался:

```
 docker images 
```

Если сделать `docker history caddy-image` — можно увидеть, что образ состоит из других слоёв. Вдогонку идёт команда `docker inspect`, которая позволит получить детали об объектах, созданных Докером, то есть образы, контейнеры, сети и другое.

Попробуем удалить запущенный контейнер `caddy` и сделать новый уже из подготовленного образа, а не вручную, как раньше:

```
 docker stop caddy
 docker rm caddy 
```

Теперь запустим новый контейнер из нашего образа-шаблона `caddy-image`:

```
 docker run --rm --name caddy caddy-image caddy run --config /caddy/Caddyfile 
```

Ключ `--rm` нужен, чтобы контейнер автоматически удалился, после остановки. 

Проверим, что всё ок:

```
 docker exec -it caddy curl http://localhost:8080/    
```