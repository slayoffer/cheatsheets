## Создание React проекта с папкой

```
npx create-react-app (название папки без скобок)

// Доп. компоненты

npm i react-router-dom react-icons
```

Настройки для .eslintrc (если не использовать линтер Вес Боса, а стандартный eslint)

```
 "extends": [
        "eslint:recommended",
        "plugin:react/recommended",
        "plugin:react/jsx-runtime"
    ],
```

## Material UI

```
npm install @mui/material @emotion/react @emotion/styled
```

## React + Tailwind

Note that for new React projects, we highly recommend using Vite, Next.js, Remix, or Parcel instead of Create React App. They provide an equivalent or better developer experience but with more flexibility, giving you more control over how Tailwind and PostCSS are configured.

**Create your project**
Start by creating a new React project with Create React App v5.0+ if you don't have one already set up.

```
npx create-react-app my-project
cd my-project
```

Install tailwindcss and its peer dependencies via npm, and then run the init command to generate both tailwind.config.js and postcss.config.js.

```
npm install -D tailwindcss postcss autoprefixer

npx tailwindcss init -p
```

Configure your template paths. Add the paths to all of your template files in your tailwind.config.js file.

```
tailwind.config.js

/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    "./src/**/*.{js,jsx,ts,tsx}",
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

Add the Tailwind directives to your CSS
Add the @tailwind directives for each of Tailwind’s layers to your ./src/index.css file.

```
index.css

@tailwind base;
@tailwind components;
@tailwind utilities;
```

Start your build process
Run your build process with npm run start.

```
npm run start
```

