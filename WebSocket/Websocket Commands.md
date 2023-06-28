1. Инициализация проекта 
  - npm init -y
  - npm i express dotenv
  - npm i pg pg-hstore sequelize sequelize-cli
  - npx create-gitignore node
  - npx eslint --init
  - npm i -D nodemon morgan
    "dev": "nodemon src/index.js --ignore sessions --ext js,jsx"

  - npm i express-session session-file-store bcrypt  

  - npm i ws
    const color = `#${(`${Math.random().toString(16)}000000`).substring(2, 8).toUpperCase()}`

2. Установим React Babel
  - npm i @babel/core @babel/preset-env @babel/preset-react @babel/register react react-dom
  - touch .babelrc