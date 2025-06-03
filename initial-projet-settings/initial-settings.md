# Устанавливаем ESLint + Prettier + Husky
## ESLint:
```bash
    npm install eslint --save-dev
```
### создаем файл eslint.config.js:
```js
    const js = require('@eslint/js'); // Подключает официальные правила для JavaScript от ESLint 
    const { defineConfig, globalIgnores } = require('eslint/config'); // Импортируются новые методы из ESLint
    // defineConfig — оборачивает весь конфиг, помогает с автодополнением и проверкой типов. 
    // globalIgnores — позволяет указать файлы и папки, которые ESLint не должен анализировать, глобально.
    const eslintPluginPrettierRecommended = require('eslint-plugin-prettier/recommended');
    // Подключает конфигурацию eslint-plugin-prettier/recommended, которая:
    // включает плагин Prettier,
    // переводит ошибки Prettier в ошибки ESLint,
    // отключает конфликтующие ESLint-правила (например, отступы, кавычки и т.п.).
    
    module.exports = defineConfig([
    // Тут начинается экспорт массива конфигураций ESLint (новый формат позволяет использовать массивы).
        globalIgnores([
    // Список путей и файлов, которые ESLint будет полностью игнорировать, даже без .eslintignore.
            'build',
            'node_modules',
            'coverage',
            'logs',
            '.github',
            '.husky',
            '.idea',
            'jest.config.js',
        ]),
        { files: ['**/*.{js,mjs,cjs}'], plugins: { js }, extends: ['js/recommended'] },
    // Это правило применяется ко всем JavaScript-файлам:
    // files: ['**/*.{js,mjs,cjs}'] — какие файлы затронет правило.
    // plugins: { js } — подключаем плагин @eslint/js.
    // extends: ['js/recommended'] — активирует рекомендуемые правила ESLint.
        eslintPluginPrettierRecommended,
    //  Включает интеграцию с Prettier через ESLint (как будто extends: ['plugin:prettier/recommended']).
        {
            rules: {
                'no-unused-vars': 0,
                'no-undef': 0,
            },
        },
    ]);
    // Последний блок вручную отключает два правила:
    // 'no-unused-vars': 0 — не ругаться на неиспользуемые переменные.
    // 'no-undef': 0 — не проверять необъявленные переменные (опасно, но может быть нужно временно).
```
## Prettier:
```bash
    npm install --save-dev prettier
```
### создаем файл prettier.config.js:
```js
    module.exports = {
      semi: true, // Ставить точку с запятой в конце каждой строки.
      singleQuote: true, // Использовать одинарные кавычки ' вместо двойных " в строках.
      tabWidth: 2, //  Количество пробелов в отступе.
      useTabs: false, // Не использовать табы (\t) для отступов, использовать пробелы.
      printWidth: 100, // Максимальная длина строки перед автоматическим переносом.
      quoteProps: 'as-needed', // Управление кавычками вокруг свойств объектов: 'as-needed' — ставить кавычки только если это необходимо ({'foo-bar': 1}, но {foo: 1}).
      trailingComma: 'es5', // Добавлять запятые в конце последнего элемента в: es5 — в объектах, массивах, но не в функциях.
      bracketSpacing: true, // Добавлять пробелы внутри фигурных скобок.
      arrowParens: 'always', // Всегда оборачивать аргументы стрелочных функций в скобки.
      requirePragma: false, // Не требовать специального комментария @format в начале файла, чтобы форматировать его.
      insertPragma: false, // Не вставлять @format в начало файла автоматически.
      proseWrap: 'preserve', // Как форматировать обычный текст в Markdown: 'preserve' — оставить как есть.
      endOfLine: 'auto', // Тип перевода строки: 'auto' — использовать стиль ОС (например, LF на Linux).
    };
```
### создаем файл .prettierignore:
```js
    docker*
    README*
    build
    .env
    logs
    .github
    .idea
    .husky
    coverage
    node_modules
```
## интеграция ESLint + Prettier:
```bash
    npm install --save-dev eslint-config-prettier eslint-plugin-prettier
```
## Husky:
```bash
    npm install husky --save-dev
```
### в корне будет папка .husky, в нее добавляем файл pre-commit :
```
    npm run fl:check
```
### в package.json, в блок "scripts" добавляем команды:
```json
    {
      "scripts": {
        "format": "prettier --check .",
        "format:fix": "prettier --write .",
        "lint": "eslint .",
        "lint:fix": "eslint . --fix",
        "fl:check": "npm run format && npm run lint",
        "fl:fix": "npm run format:fix && npm run lint:fix",
        "prepare": "husky"
      }
    }
```
#### Теперь при создании коммита будет проходить проверка.