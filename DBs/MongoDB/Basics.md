### Install

```bash
# community version
sudo vim /etc/yum.repos.d/mongodb-org.repo

# add text
[mongodb-org-6.0]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/6.0/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-6.0.asc

# check repo
yum repolist

# install
sudo yum install mongodb-org

# start
sudo systemctl start mongod

# check version
mongod --version
```

### Start

```bash
sudo systemctl start mongod
```

### Logs

```bash
cat /var/log/mongodb/mongod.log
```

### Port

```bash
# default port 27017
```

### Config

```bash
sudo vim /etc/mongod.conf
```

### Connect

```js
mongo

# check version
db.version()
```

### Show DBs

```js
show dbs
```

### Create DB

```js
use db_name
```

### Connect DB

```js
use db_name
```

### Show current DB

```js
db
```

### Create table

```js
db.createCo11ection ("persons")
```

### List tables

```js
show collections
```

### Insert data

```js
db.persons.insert({
	"name": "John Doe",
	"age": 45,
	"location": "New York",
	"salary": 5000
})
```

### Show table data

```js
db.persons.find()

# show specific data
db.persons.find({"name": "John Doe"})
```

Чтобы подключиться к MongoDB, следуйте шагам из [официальной инструкции](https://www.mongodb.com/docs/mongodb-shell/).

Выполним команду `show dbs` и получим список баз. В этом списке находятся как служебные базы, так и базы других пользователей.

Чтобы узнать, что интересного у нас есть на сервере, можно вызвать команду `db.stats()`

Скопировать кодJSON

```
{
    "collections" : 0,
    "objects" : 0,
    "dataSize" : 0,
    "storageSize" : 0,
    "numExtents" : 0,
    "indexes" : 0,
    "indexSize" : 0,
    "ok" : 1
} 
```

Если базы данных не существует, Mongo создаст её и коллекцию при первом сохранении данных.

Чтобы переключиться на текущую базу данных, нужно выполнить: `use <имя БД>`.

Теперь давайте убедимся, что выбрана верная БД, и создадим новую коллекцию объектов, назовем её `products`. 

Воспользуемся методом **insertOne**, чтобы вставить несколько элементов в коллекцию:

Скопировать код

```
db.products.insertOne({name: "Hot Dog", price: 70.00});
db.products.insertOne({name: "Snak", price: 54.00});
db.products.insertOne({name: "Cold Dog", price: -70.00});
db.products.insertOne({name: "Normal Dog", price: 33.00}); 
```

**Кстати**: языком запросов здесь выступает **JavaScript**.

Введём `db.` и нажмём **Tab**, чтобы увидеть, какие ещё есть методы у объекта БД. Например, можем вызвать `db.getCollectionNames()` или сокращённую форму `show collections`.

Аналогично можно исследовать любой объект в MongoDB!

### Исследуем коллекцию

Попробуем исследовать нашу коллекцию:

- введём `"db.products."`, нажмём Tab и увидим доступные методы для работы с коллекцией
- можем, например, вывести все элементы коллекции через `db.products.find()`

**Заметьте**, что ключ `_id` есть у каждого объекта коллекции.

Ничто не мешает при необходимости выстраивать в вашем запросе данных, например, отбирая позиции по цене:

```
db.products.find({price: {$lt: 60}}) 
```

Вспоминаем классические операторы сравнения: `gt`, `lt`, `eq` и другие.
