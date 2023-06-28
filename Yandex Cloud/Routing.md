Таблица маршрутизации привязывается к подсети и не может содержать повторяющихся префиксов. Трафик из подсети с привязанной таблицей будет направляться к указанным в маршрутах префиксам через соответствующий адрес шлюза.

Префикс `0.0.0.0/0` в маршруте означает, что весь трафик, если он не направлен по другим маршрутам, будет направлен через указанный для этого префикса шлюз.

Например, к подсети с CIDR `10.1.0.0/24` привязана таблица маршрутизации с такими маршрутами:

|     **Имя**     |  **Префикс**   | **Шлюз**  |
| :-------------: | :------------: | :-------: |
| another-network | 192.168.0.0/16 | 10.1.0.5  |
|    internet     |   0.0.0.0/0    | 10.1.0.10 |

В этом случае весь трафик в подсеть `192.168.0.0/16`, которая находится в другой виртуальной сети, будет направляться через ВМ с адресом `10.1.0.5` — при условии, что у ВМ есть интерфейс в другой виртуальной сети. Весь остальной трафик — через ВМ `10.1.0.10`. При этом переопределение маршрута для префикса `0.0.0.0/0` может повлиять на внешнюю доступность ВМ из подсети с таблицей, где есть такой маршрут.

В Yandex Cloud поддерживаются только префиксы назначения вне виртуальной сети (например, префиксы подсетей другой сети Yandex Cloud или вашей локальной сети).

При создании маршрута в качестве шлюза можно указать свободный внутренний IP-адрес, который не привязан ни к одной ВМ. В этом случае маршрут заработает, когда будет запущена ВМ с соответствующим IP-адресом.



### Изменение маршрутов трафика в интернет

Если в префиксе назначения у маршрута из таблицы маршрутизации указан префикс адресов из интернета, то доступ к таким адресам и с таких адресов станет невозможным через публичные IP-адреса ВМ из подсетей, к которым привязана эта таблица.

Допустим, есть машина `vm-1` с публичным IP-адресом, подключенная к подсети `my-subnet`. Если к подсети `my-subnet` привязать таблицу `my-route-table` с маршрутом для префикса `0.0.0.0/0` (все адреса) через шлюз `10.0.0.5`, то доступ через публичный адрес к `vm-1` пропадёт. Это произойдёт потому, что весь трафик в подсеть `my-subnet` и из неё теперь будет направляться через адрес шлюза (см. первую схему).

Чтобы сохранить входящую связность с облачными ресурсами через публичный адрес, вы можете:

- вынести ресурсы с публичными адресами в отдельную подсеть;
- вместо настройки маршрута в интернет включить для подсети [доступ в интернет через NAT](https://cloud.yandex.ru/docs/vpc/operations/enable-nat) (функция находится на стадии Preview и включается по запросу в техподдержку).