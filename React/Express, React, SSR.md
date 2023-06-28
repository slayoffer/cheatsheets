## // Getting started
```
* npm i express - установка Express
* npm i -D nodemon morgan - установка Nodemon && Morgan
```
## // В <package.json> прописываем скрипты
```
"scripts": {
  "start": "node src/index.js",
  "dev": "nodemon src/index.js --ext js,jsx"
},
```
## // Устанавливаем React && Babel
```
* Babel позволяет подключать jsx файлы (JavaScript, в котором можно писать HTML).
npm i @babel/core @babel/preset-env @babel/preset-react @babel/register react react-dom
```
## // Создаем файл .babelrc
```
* touch .babelrc
// Содержимое .babelrc
{
  "presets": [
    "@babel/preset-env",
    "@babel/preset-react"
  ]
}
```
## // Конфигурация <app.js>
```
* Подключаем babel для поддержки jsx
require('@babel/register');
//=================================================//
* Фреймворк веб-приложений
const express = require('express');
const morgan = require('morgan');
const path = require('path');
//=================================================//
* Подключаем .env файлы
require("dotenv").config()
//=================================================//
* Подключаем React и наши views
const ReactDOMServer = require('react-dom/server');
const React = require('react');
const Main = require('./views/Main');
//=================================================//
* Создаем наше приложение и прописываем порт
const app = express();
const PORT = process.env.PORT || 4000
//=================================================//
* Подключаем логирование деталей запросов
app.use(morgan('dev'));
//=================================================//
* Две следующих настройки нужны для того, чтобы мы могли вытащить тело POST-запроса
* Это нужно не всегда (всё зависит от того, как клиент отправляет запросы),
* но пока будем использовать всегда — на всякий случай
* Распознавание входящего объекта в POST-запросе в виде строк или массивов
app.use(express.urlencoded({ extended: true }));
* Распознавание входящего объекта в POST-запросе как объекта JSON
app.use(express.json());
//=================================================//
* Подключаем папку public со статическими файлами (картинки, стили и т.д.)
app.use(express.static(path.join(__dirname, 'public')));
//=================================================//
app.get('/', function (req, res) {
  res.send('Hello, world');
});
//=================================================//
* Постоянно слушаем порт
app.listen(PORT, () => {
  console.log(`Server started on port ${PORT}`);
});
```
## // Server Setup Template
```
/////////////////////////////////////////////////////////////
* Setup - import deps and create app object
/////////////////////////////////////////////////////////////
require('@babel/register');
const express = require('express');
const app = express();
const path = require('path');
const logger = require('morgan');
/////////////////////////////////////////////////////////////
* Declare Middleware
/////////////////////////////////////////////////////////////
app.use(logger('dev'));
app.use(express.json());
app.use(express.urlencoded({ extended: false }));
app.use(express.static(path.join(__dirname, 'public')));
/////////////////////////////////////////////////////////////
* Declare routes and Routers
/////////////////////////////////////////////////////////////
const indexRouter = require('./routes/index');
const usersRouter = require('./routes/users');
app.use('/', indexRouter);
app.use('/users', usersRouter);
/////////////////////////////////////////////////////////////
* Server Listener
/////////////////////////////////////////////////////////////
app.listen(PORT, () => console.log(`Server is listening on Port ${PORT}`);
```
## // Функция для рендера компонента
```
require('@babel/register');
const React = require('react');
const ReactDOMServer = require('react-dom/server');
const renderTemplate = (reactComponent, properties, response) => {
  const reactEl = React.createElement(reactComponent, properties);
  const html = ReactDOMServer.renderToStaticMarkup(reactEl);
  response.write('<!DOCTYPE html>');
  response.end(html);
}
module.exports = renderTemplate;
```
## // HTTP / Типы контента
```
MIME type
Описание типа данных, передаваемых по сети. Например:
  ● text/plain — простой текст
  ● text/html — HTML-документ
  ● application/json — данные в JSON-формате
  ● application/xml — XML-документ
  ● multipart/form-data — бинарные данные
Для описания типа передаваемых и принимаемых данных
используются заголовки Content-Type и Accept соответственно.
```
## // Express: виды ответов
```
res.send(text) // послать текст с кодом 200 + завершить ответ
res.json({ user: 'tobi' }) // послать json с кодом 200 + завершить ответ
res.end() // завершить ответ
res.status(403) // установить код, но НЕ завершить ответ
// установить код 500, послать json + завершить ответ
res.status(500).json({ error: 'message' })
res.status(404).end() // установить код + завершить ответ
res.redirect('/other-route') // переадресовать клиента + завершить ответ
res.render(view, {data}) // рендер шаблона + послать html + завершить ответ
```
## // React SSR в тезисах
```
● React — JS-библиотека для отрисовки пользовательских
  интерфейсов (user interfaces, UI)
● React SSR (Server Side Rendering) - рендеринг html на
  сервере с помощью React
● позволяет вставлять данные в заранее подготовленные
  HTML-шаблоны
● JSX — позволяет писать HTML прямо в js-файлах
● Шаблонизаторы отделяют View от всего остального
● Слой View в MVC
```
## // Конфигурация компонента Layout
```
* Layout — главный компонент, в который вставляем остальные.
// views/Layout.jsx
const React = require('react');
module.exports = function Layout({ title, children }) {
  return (                      └─────────┬─────────┘
    <html lang="en">                     Props
      <head>
        <title>{title}</title>
        <link rel="stylesheet" href="style.css" />
        <script src="script.js" />
      </head>
      <body>{children}</body>
    </html>
  );
};
```
## // Конфигурация компонента Home
```
// views/Home.jsx
const React = require('react');
const Layout = require('./Layout');
module.exports = function Home({ title, name }) {
  return (
    <Layout title={title}>    ┌─────── все теги должны быть закрыты
      <input type="text" /> <br />
      <h1 className="title" style={{color: "red"}}>Hello, {name}</h1>
    </Layout> └─────────┐                └─────────┐
  ); className вместо class                    style задаётся как объект,
};                                             а не как строка
```
## // React SSR: пример ответа для GET запроса
```
// app.js
const ReactDOMServer = require('react-dom/server');
const React = require('react');
const Home = require('./views/Home');
// Отображаем главную страницу с использованием компонента "Home"
app.get('/', (req, res) => {
  // создаём React-элемент на основе React-компонента
  const home = React.createElement(Home, {
    title: 'My site',
    name: 'John',
  });
  // рендерим элемент и получаем HTML (в виде строки)
  const html = ReactDOMServer.renderToStaticMarkup(home);
  // отправляем первую строку нашего HTML-документа
  res.write('<!DOCTYPE html>');
  // отправляем отрендеренный HTML и закрываем соединение
  res.end(html);
});
```
## // React SSR: JSX
```
Что можно делать:
{data} // вставить данные текстом
{obj.myProp} // вставить текстом свойство объекта
{name.toUpperCase() + '!'} // вставить любое javascript-выражение
// вставить html-разметку
// (будьте осторожны! Вставляйте только проверенный HTML.)
<div dangerouslySetInnerHTML={{ __html: myHtml }} />
// вставить другой компонент
<MyHeader theme="black" user={user}>Header Text</MyHeader>
└──┬──┘└─────────────┬──────┘ └───┬────┘
  Type             Props       Children
```
## // React SSR: условный рендеринг
```
Что можно делать:
// условный рендеринг (в зависимости от условия)
// if
{author && <h1>{author.firstName} {author.lastName}</h1>}
// if/else
{author
  ? <h1>{author.firstName} {author.lastName}</h1>
  : <h1>Unknown author</h1>}
```
## // React SSR: рендер списка
```
Что можно делать:
// ...если people - это массив
{ people: [{id: 1, name: "A", age: 15}, {id: 2, name: "B", age: 21}] }
// использовать map, чтобы отрендерить массив
<ul>
  {people.map((person) => (
    <li key={person.id}>Name:{person.name}, age: {person.age}</li>
  ))}    └─────┬─────┘
</ul>         key - уникальный ключ элемента массива
              (если его нет, можно использовать index)
```
## // Отличия GET от POST
```
* У GET - нет body, есть query
* У POST - есть body, нет query
```
## // Error Handling Function
```
* Catch any errors they throw, and pass it along to express middleware with next()
exports.catchErrors = (fn) => {
  return function(req, res, next) {
    return fn(req, res, next).catch(next);
  };
};
```
## // Not Found Error handler
```
exports.notFound = (req, res, next) => {
  const err = new Error('Not Found');
  err.status = 404;
  next(err);
};
```