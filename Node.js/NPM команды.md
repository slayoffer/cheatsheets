### Search npm packages

```bash
npm search <package_name>
```

### Show paths of NPM local search

```bash
node -e "console.log(module.paths)"
```

### Создание gitignore файла для node.js

```bash
npx create-gitignore node
```

### Initialize NPM

```bash
npm init
# no questions asked
npm init --yes
# short
npm init -y
```

### Версия npm

```bash
npm -v
```

### Install package

```bash
npm install package_name

# install globally
sudo npm install package_name

# global npms are installed to
ls /usr/lib/node_modules/
```

### Установить все депенданси из package.json

```bash
npm install
# or
npm i
```

### Update dependencies

```bash
npm update
```

// установить линтер Вес Боса
npx install-peerdeps --dev eslint-config-wesbos


https://github.com/wesbos/eslint-config-wesbos

for .eslintrc
{
  "extends": [ "wesbos" ]
}

// Чтобы не устанавливать и не париться с CORS, нужно в package.json прописать

"proxy": "http://localhost:4000/"

// установка dep nodemon в качестве dev (сервер с автообновлениями)
npm install -D nodemon

//nodemon запуск из node modules
nodemon index.js --ext js,jsx

## Express, Eslint, Nodemon, Morgan, React & Babel:

```
// ! 1. Инициализация проекта 
// *   - npm init -y
// *   - npm i express
// *   - npx create-gitignore node
// *   - npx eslint --init
// *   - npm i nodemon morgan
// *     "nodemon index.js --ext js,jsx"

// ! 2. Установим React Babel
// *  - npm i @babel/core @babel/preset-env @babel/preset-react @babel/register react react-dom
// *  - touch .babelrc

// ! 3. Содержимое .babelrc
{
  "presets": [
    "@babel/preset-env",
    "@babel/preset-react"
  ]
}

// ! 4. Содержимое .sequelizerc
const path = require('path');

module.exports = {
  'config': path.resolve('config', 'config.json'),
  'models-path': path.resolve('db', 'models'),
  'seeders-path': path.resolve('db', 'seeders'),
  'migrations-path': path.resolve('db', 'migrations')
};
```

## Скрипты:

```
  // Для Винды
  "scripts": {
    "start": "set NODE_ENV=production&&node server",
    "dev": "nodemon server.js"
  },
  
  // Для Мака и Линукса
    "scripts": {
    "start": "NODE_ENV=production node server",
    "dev": "nodemon server.js"
  },
```

## Tailwind CSS:

```
npm i -D tailwindcss // Установка NPM

npx tailwindcss init // Для конфига

"./src/**/*.{html,js}" // вставить в массив content конфига

создать файл input.css и вставить в него:
@tailwind base;
@tailwind components;
@tailwind utilities;

npx tailwindcss -i ./src/input.css -o ./dist/output.css --watch // для билда CSS файла

либо можно загнать в скрипт build:
"tailwindcss -i ./src/input.css -o ./dist/output.css"

и в скрипт watch:
"tailwindcss -i ./src/input.css -o ./dist/output.css --watch"
```





// убить процесс на порте
npx kill-port 3000
и/или
taskkill /f /im node.exe



// Изменение дефолтных значений npm конфига (package.json)
npm config set init-author-name "Anton Evseev"

// просмотр дефолтных значений npm конфига
npm get init-author-name
npm config get init-author-name

// удаление своих установленных значений npm конфига
npm config delete init-license
npm config delete init-author-name

// запустить дев депендэнси скрипт из package.json
// вместо дев может быть любое другое node.jsипindex.jsun dev

// запустить файл в node.js (index.js)
node index



// установка dep для юзеров
npm install uuid

// установка зависимости, чтобы она попала в package.json (флаг --save)
npm install lodash --save

// установка только основных зависимостей (без дев)
npm install --production

// удаление зависимостей
npm remove eslint
npm uninstall eslint -dev (если только из дев депенданси)
npm rm lodash

// установка определенной версии пакета
npm install lodash@4.17.3

// апдейт пакета
npm update lodash

// глобальная установка пакета
npm install -g nodemon

// обозначения в конфиге (package.json)
// ^4.17.21 - будет апдейтить только minor версию (.17)
// ~4.17.21 - будет апдейтить только patch версию (.21)
// 4.17.21 - будет устанавливать только эту версию
// если поставить вместо версии "*", тогда будет ставить самую новую версию (лучше подумать)

// список всех пакетов
npm list

// список всех пакетов без их зависимостей
npm list --depth 0

// список всех пакетов и их зависимостей первого уровня (2 и т.д.)
npm list --depth 1

// папка хранения глобальных зависимостей
npm root -g

// intellisense для jest
npm install @types/jest

// ! Установка eslint:
// * npx eslint --init
// ? настройки:
// * 3, 2, 3, No, Node, 1, 1, 3, Yes
// ? также установите расширение ESLint

// ! Проверка ноды: 
// * node -v



// NPM version update
npm install -g npm@latest

// NVM RC ???/

// Устранить vulnarabilities
npm -fix --force

// * npm i -g npm    - глобал установка послед версии

// ! Проверка npm:
// * npm -v

// ! To npm update
// * npm update

// npx kill-port 3000

// ! Сайт библиотек 
// * https://www.npmjs.com/

// ! npx VS npm
// ? npx помогает нам избежать версий, проблем с зависимостями 
// ? и установки ненужных пакетов, которые мы просто хотим попробовать

// ! Запуск файла:
// * node название файла

// ! Стандартные скрипты запуска файлов без run:
// * npm start
// * npm test
// ? остальные - npm run название скрипта
// * npm run dev

// ! Установка зависимостей:
// * npm i - если уже есть package.json с зависимостями, 
// * npm i название библиотеки - установка библиотеки в dependencies
// * npm i -D название библиотеки - установка библиотеки в devDependencies
// * npm i -g название библиотеки - установка библиотеки глобально