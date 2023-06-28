# Sequelize CLI

## Нарисовать БД:

https://drawsql.app/

## 1. Установить клиентские и дев зависимости и инициализировать SQLZ:

```
npm i sequelize pg pg-hstore

npm i -D sequelize-cli

npx sequelize - список всех команд
```

## Создать файл .sequelizerc, скопировать в него код:

```
const path = require('path');

module.exports = {
'config': path.resolve('config', 'database.json'),
'models-path': path.resolve('db', 'models'),
'seeders-path': path.resolve('db', 'seeders'),
'migrations-path': path.resolve('db', 'migrations')
};
```

## 2. Инициализация папок 

```
npx sequelize init
```

В файле config/database.json меняем:

```
"development": {
    "username": "<UserName>",
    "password": "<Password>",
    "database": "<dbName>",
    "host": "127.0.0.1",
    "dialect": "postgres"
},
```

## 3. Создать базу данных
```
npx sequelize db:create
```

## 4. Создать таблицу 

```
npx sequelize model:generate --name Director --attributes fullName:string,birth_date:integer

npx sequelize model:generate --name Doughnut --attributes calories:integer,stuffing_id:integer

npx sequelize model:generate --help - список команд для генерации модели

allowNull: false  - чтобы поле не могло быть null
```

## 5. Миграция таблицы в бд
```
npx sequelize db:migrate
```



## Команда для отмены миграции данных БД
```
npx sequelize db:migrate:undo:all
```

## Запуск DB через Sequelize

```
const db = require('./db/models');

async function connectCheck() {
  try {
    await db.sequelize.authenticate();
    console.log('Connection has been established successfully.');
  } catch (error) {
    console.error('Unable to connect to the database:', error);
  }
}

connectCheck();
```

## Добавление данных в таблицу

```
async function createDirector(fullname, lastname, year) {
  try {
    await connectCheck();
    const newDirector = await db.Director.create({
      fullname,
      last_name: lastname,
      birth_date: year,
    });
    console.log('=> ~ newDirector', newDirector);
    console.log(`Director ${newDirector.fullname} was created`);
  } catch (error) {
    console.error(error);
  }
}

createDirector(process.argv[2], process.argv[3], 1950);

//

async function createOrders(user_id, product_id) {
  await db.Order.create({
    user_id,
    product_id
  });
}

for (let i = 0; i < 5; i++) {
  const randomProduct = Math.floor(Math.random() * (1 + 7) - 1);
  const randomUser = Math.floor(Math.random() * (1 + 5) - 1);
  createOrders(randomUser, randomProduct).catch(err => console.log(err));
};
```

## Получить данные из таблицы:

```
// findAll
async function findAllDirectors() {
  try {
    await connectCheck();
    const directors = await db.Director.findAll({ raw: true });
    console.log('=> ~ directors', directors);
    // либо можно без raw: true
    console.log('=> ~ directors', directors.map(d => d.get({ plain: true })));
  } catch (error) {
    console.error(error);
  }
}

findAllDirectors();

// findOne
async function findOneDirector(name) {
  try {
    await connectCheck();
    const director = await db.Director.findOne({ where: { fullname: name }, raw: true });
    console.log('=> ~ director', director);
  } catch (error) {
    console.error(error);
  }
}

findOneDirector(process.argv[2]);

//

async function findSpecificOrders(id1, id2) {
  try {
    const specificOrder = await db.Order.findAll({ 
      where: {
        [Op.or]: [
          { user_id: id1 },
          { user_id: id2 },
        ]
      },
      raw: true, 
    });
    console.log(specificOrder);
  } catch (error) {
    console.log(error);
  }
}

//

async function destroySpecificOrder(user_id, product_id) {
  try {
    const destroyedOrder = await db.Order.destroy({ 
      where: {
        [Op.and]: [
          { user_id },
          { product_id },
        ]
      }
    });
    console.log(destroyedOrder);
  } catch (error) {
    console.log(error);
  }
}
```

## Получить данные из связанных таблиц:

```
async function findFilms(name) {
  try {
    await connectCheck();
    const films = await db.Director.findOne({
      where: { fullname: name },
      include: [db.Film],
    });
    // console.log('=> ~ films', films.dataValues);
    films.dataValues.Films.forEach((film) => {
      console.log(film.title);
    });
  } catch (error) {
    console.error(error);
  }
}
findFilms(process.argv[2]);
```

## Апдейт данных в таблице:

```
await User.update({ name: "Rauf" }, {
 where: { name: "Igor" }
}); // UPDATE users SET name = 'Rauf' WHERE name = 'Igor'
```



> ## ***ВРУЧНУЮ В ДБ ЧЕРЕЗ PGADMIN ЗАПИСИ НЕ ВНОСИТЬ!***

## Удаление данных из таблицы:

```
await User.destroy({
 where: { name: "Igor" }
}); // DELETE user WHERE name = 'Igor';

```

## Пример связей в таблице

```
module.exports = (sequelize, DataTypes) => {
  class Product extends Model {
    static associate({ User, Order }) {
      this.hasMany(User, { foreignKey: 'favourite_products' });
      this.hasMany(Order, { foreignKey: 'id' })
    }
  }
  Product.init({
    product_name: DataTypes.STRING
  }, {
    sequelize,
    modelName: 'Product',
  });
  return Product;
};

module.exports = (sequelize, DataTypes) => {
  class User extends Model {
    static associate({ Product, Order }) {
      this.belongsTo(Product, { foreignKey: 'id' });
      this.hasMany(Order, { foreignKey: 'id' })
    }
  }
  User.init({
    name: DataTypes.STRING
  }, {
    sequelize,
    modelName: 'User',
  });
  return User;
};

module.exports = (sequelize, DataTypes) => {
  class Order extends Model {

    static associate({ User, Product }) {
      this.belongsTo(User, { foreignKey: 'user_id' });
      this.belongsTo(Product, { foreignKey: 'product_id' })
    }
  }
  Order.init({
    user_id: DataTypes.INTEGER,
    product_id: DataTypes.INTEGER
  }, {
    sequelize,
    modelName: 'Order',
  });
  return Order;
};
```



## Команда для генерации сидера

```
npx sequelize seed:generate --name CreateDirectors

npx sequelize seed:generate --help - список команд для генерации модели
```

### Команда для использования сидера

```
npx sequelize db:seed:all
```

Отмена сидера
npx sequelize db:seed:undo
Отмена конкретного сидера
npx sequelize db:seed:undo --seed 20220906083531-CreateDirectors.js

Пример сидера:

```
module.exports = {
  async up(queryInterface, Sequelize) {
    await queryInterface.bulkInsert('Directors', [{
      fullname: 'Tony',
      last_name: 'Evs',
      birth_date: '1988',
      createdAt: new Date(),
      updatedAt: new Date(),
    },
    {
      fullname: 'Jane',
      last_name: 'Cool',
      birth_date: '2001',
      createdAt: new Date(),
      updatedAt: new Date(),
    },
    {
      fullname: 'Elly',
      last_name: 'Doe',
      birth_date: '1995',
      createdAt: new Date(),
      updatedAt: new Date(),
    }], {});
  },

  async down(queryInterface, Sequelize) {
    await queryInterface.bulkDelete('Directors', null, {});
  },
};
```

## Связка таблиц:

В Моделях (по типу One to Many):

```
...
    static associate({ Director }) {
      // define association here
      this.belongsTo(Director, { foreignKey: 'director_id' });
    }
  }
...

    static associate({ Film }) {
      // define association here
      this.hasMany(Film, { foreignKey: 'director_id' });
    }
```

В Миграции:

```
...
      director_id: {
        type: Sequelize.INTEGER,
        references: {
          model: 'Directors',
          key: 'id',
        },
      },
...
```



```


//=================================================//
* npx sequelize init
//=================================================//

//=================================================//
* npx sequelize db:create <----- Создаем БД
//=================================================//
* Добавляем в рабочий файл index.js
const db = require('./db/models');
//=================================================//
* Проверяем соединение с БД
async function connectionCheck() {
  try {
    await db.sequelize.authenticate();
    console.log('Connection has been established successfully.');
  } catch (error) {
    console.error('Unable to connect to the database:', error);
  }
}
connectionCheck();
//=================================================//
* npx sequelize model:generate --name <Name> --attributes <1:string,2:integer>
^
Создаем таблицу (модель и миграцию)
//=================================================//
* npx sequelize db:migrate <----- Мигрируем таблицу в БД
* npx sequelize db:migrate:undo:all <----- Отменяем миграцию (Ресет)
//=================================================//
* npx sequelize seed:generate --name <SeedName> <----- Генерация сидера
* npx sequelize db:seed --seed <seedName> <----- Использование конкретного сидера
* npx sequelize db:seed:all <----- Использование всех сидеров
* npx sequelize db:seed:undo <----- Отмена всех сидеров
* npx sequelize db:seed:undo --seed <SeedName> <----- Отмена конкретного сидера
```
## // Getting Started Sequelize Raw Queries
```
const { Sequelize } = require('sequelize');
const sequelize = new Sequelize('database', 'username', 'password', {
  host: 'localhost',
  dialect: /* one of 'mysql' | 'mariadb' | 'postgres' | 'mssql' */
});
async function connectionCheck() {
  try {
    await sequelize.authenticate();
    console.log('Connection has been established successfully.');
  } catch (error) {
    console.error('Unable to connect to the database:', error);
  }
}
connectionCheck();
```
## // *Raw queries*
```
sequelize.query(`
  CREATE TABLE groups (
    id SERIAL NOT NULL PRIMARY KEY,
    group_name VARCHAR(25),
    year SMALLINT
  );
`);
sequelize.query(`
  CREATE TABLE people (
    id SERIAL NOT NULL PRIMARY KEY,
    first_name VARCHAR(25),
    group_id INT REFERENCES groups(id)
  );
`).then(console.log);
sequelize.query(`
  INSERT INTO groups (group_name, year)
  VALUES ('Пчёлы', 2022);
`).then(console.log);
```
## // NO!!!
```
// Так делать не надо - УЯЗВИМОСТИ
const welcome = async (group) => {
  const [ groupID ] = await sequelize.query(`SELECT * FROM 
  groups WHERE group_name = '${group}'`);
  console.log(groupID);
}
```
## // YES!!!
```
const welcome2 = async (group) => {
  const [ groupID ] = await sequelize.query(`SELECT * FROM groups WHERE group_name = ?;`, {
    replacements: [group],
    }
  );
  console.log(groupID);
}
const addStudent = async (firstName, groupID) => {
  const newStudent = await sequelize.query('INSERT INTO people (first_name, group_id) VALUES (:first_name, :group_id);', {
    replacements: {
      first_name: firstName,
      group_id: groupID
    },
  });
  console.log(newStudent);
}
addStudent(process.argv[2], process.argv[3]);
```