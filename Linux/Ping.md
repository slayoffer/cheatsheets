### Указание Количества ECHO_REQUEST  

```bash
# Параметр команды -c используется для указания количества пакетов или запросов, которые хочет выполнить пользователь. Синтаксис будет выглядеть так:
ping –c * primerdomen.com
# Здесь * — количество пингов, которые вы хотите выполнить.
```

### Звуковой Пинг 

```bash
# Опция команды Linux ping -a создаёт звуковой сигнал, чтобы проверить, является ли хост активным или нет, таким образом сообщая вам об этом. Команда будет выглядеть так:
ping –a primerdomen.com
```

### Установка Интервалов 

```bash
# Опция –i в Linux позволяет пользователю устанавливать интервалы в секундах между каждым пакетом. Команда имеет ту же структуру, что и предыдущие:
ping –i 2 –c 7 primerdomen.com 
# Цифры, которые вы видите в команде могут быть изменены по вашему желанию
```

### Получать Только Сводку Команды Ping 

```bash
# Чтобы получить только сводную информацию о сети, используйте параметр - q в командной строке терминала Linux:
ping –c 7 –q primerdomen.com
```

### Тестируем Нагрузку на Сеть

```bash
# Команда ping позволяет отправлять 100 или более пакетов в секунду с помощью следующей команды:
ping –f primerdomen.com
# Это отличный вариант, если вы хотите проверить, как ваш сайт или сервер справляется с нагрузкой на сеть — большим количеством запросов.
```
