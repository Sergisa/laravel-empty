<p align="center">
    <img src="https://raw.githubusercontent.com/laravel/art/master/logo-lockup/5%20SVG/2%20CMYK/1%20Full%20Color/laravel-logolockup-cmyk-red.svg" width="400" alt="">
</p>
<p align="center">
    <img src="https://img.shields.io/badge/Jetstream-NO-red" alt="Build Status">
    <img src="https://img.shields.io/badge/Breeze-NO-red" alt="Build Status">
</p>
<p align="center">
    <img src="https://img.shields.io/badge/Testing-PHPUnit-indigo?style=for-the-badge" alt="Build Status">
    <img src="https://img.shields.io/badge/Database-SQLtie-ffff00?style=for-the-badge" alt="Build Status">
    <img src="https://img.shields.io/badge/WebSocket-Vite-ffff00?style=for-the-badge" alt="Build Status">
</p>

# Изучаем Laravel

В данной ветке показан пример использования сборки css.  
Создадим новые файлы:

- tailwind.config.js
- postcss.config.js

Установим новые библиотеки

- tailwindcss
- postcss
- autoprefixer

``npm install -D tailwindcss postcss autoprefixer``

## Использование модуля Vite

### Преобразование CSS и JS

Данный модуль позволит нам собирать css и js файлы нашего проекта.
Наберём в командной строке:

```shell
npm run build
```

Теперь в папке /public/build/assets появилось два файла `app-*.css` `app-*.js` это файлы, который сформировались из
файлов:

- /resources/css/app.css ⟹ /public/build/assets/`app-8459003.css`
- /resources/js/app.js ⟹ /public/build/assets/`app-ddee773b.js`

В файле **app.css** были прочитаны строки

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

Эти строки скомпилировались в css при выполнении команды ``npm run build``.

### Использование CSS в HTML коде страницы

Теперь в файле **layout.blade.php** заменим тэг **Style**

```html

<style>
    /* ! tailwindcss v3.2.4 ... */
    ...CSS...
</style>
```

на

```php
@vite('resources/css/app.css')
```

При обновлении страницы мы увидим что такая директива развернулась в путь в виде:

```html

<link rel="stylesheet" href="http://127.0.0.1:8000/build/assets/app-8459003d.css">
```

А это и есть ссылка на собранный файл

### Преобразование на ходу

Так же если в командной строке ввести команду `npm run dev` то запустится сервер по адресу `http://localhost:5173/`.
А директива `@vite ('resources/css/app.css')` превратится на стороне браузера в:

```html
<script type="module" src="http://[::1]:5173/@vite/client"></script>
<link rel="stylesheet" href="http://[::1]:5173/resources/css/app.css">
```

Получается что на _5173_ порту браузера открывается сервер с протоколом WebSocket который будет отдавать
скомпилированный CSS по запросу страницы, собирая его именно в момент запроса, а так же будет сам инициировать
перезагрузку страницы, если на стороне сервера произойдут изменения в файлах стилей. Перезагрузку страницы обеспечивает
скрипт по адресу `http://[::1]:5173/@vite/client`. Этот скрипт подключается к порту _5173_ локального сервера и при
инициации перезагрузки сообщением от сервера запускает перезапуск самой страницы командой `location.reload()`, которую
можно найти в этом скрипте. Попросту говоря этот скрипт это приёмник сообщений от сервера.
