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

Данный модуль позволит нам собирать css и js файлы нашего проекта. Наберём в командной строке:

```shell
npm run build
```

Теперь в папке /public/build/assets появилось два файла `app-*.css` `app-*.js` это файлы, которые сформировались из
файлов:

- /resources/css/app.css ⟹ /public/build/assets/`app-8459003.css`
- /resources/js/app.js ⟹ /public/build/assets/`app-ddee773b.js`

В файле **app.css** были прочитаны строки

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

Как видно такие команды CSS интерпретатором браузера не будут распознаны. Для преобразования в CSS их необходимо
обработать. Эти строки скомпилировались в css при выполнении команды ``npm run build``. Такие директивы в процессе
сборки были прочитаны компилятором CSS с использованием плагина tailwind. Одна такая директива подразумевает под собой
блок объявленных CSS свойств. Настройки использования плагина tailwind описаны в файле `tailwind.config.js`

В файле **app.js** были прочитаны строки со словом ` import '...js' ` и были преобразованы в содержимое файлов,
указанных в виде импорта.

### Использование CSS в HTML коде страницы

Теперь в файле **layout.blade.php** заменим тэг **Style**, так как изначально там лежит часть фреймворка tailwind.

```html

<style>
    /* ! tailwindcss v3.2.4 ... */
    ...CSS...
</style>
```

Изменим содержимое на

```php
@vite('resources/css/app.css')
```

При обновлении страницы мы увидим что такая директива развернулась в путь в виде:

```html

<link rel="stylesheet" href="http://127.0.0.1:8000/build/assets/app-8459003d.css">
```

А это и есть ссылка на собранный файл

### Преобразование на ходу

Если в командной строке ввести команду `npm run dev` то запустится сервер по адресу `http://localhost:5173/`. А
директива `@vite ('resources/css/app.css')` превратится на стороне браузера в:

```html

<script type="module" src="http://[::1]:5173/@vite/client"></script>
<link rel="stylesheet" href="http://[::1]:5173/resources/css/app.css">
```

Получается что на _5173_ порту браузера открывается сервер с протоколом WebSocket который будет отдавать
скомпилированный CSS по запросу страницы, собирая его именно в момент запроса, а так же будет сам инициировать
перезагрузку страницы, если на стороне сервера произойдут изменения в файлах стилей. Перезагрузку страницы обеспечивает
скрипт по адресу `http://[::1]:5173/@vite/client`. Этот скрипт подключается к порту _5173_ локального сервера и при
инициации перезагрузки сообщением от сервера запускает перезапуск самой страницы командой `location.reload()`, которую
можно найти в этом скрипте. Попросту говоря этот скрипт это приёмник сообщений от сервера. Теперь пока у вас запущен и
работает _dev_ скрипт, при каждом изменении файлов страница в браузере будет перезагружаться.

Что бы понять как это работает, для начала следует взглянуть на содержимое файла `vite.config.js`. Там мы увидим
использование плагина `laravel-vite-plugin`. Плагин представляет собой готовый написанный блок конфигураций для модуля
Vite. Vite это всего лишь плагин, помогающий вам собирать ваши файлы для проекта. А laravel-vite-plugin это пред
подготовленный перечень настроек готовящий Vite для работы в рамках фреймворка Laravel.

```js
import laravel from 'laravel-vite-plugin';
```

Если вы посмотрите в файл по адресу `/node_modules/laravel-vite-plugin/index.mjs`, вы найдёте там следующие строки:

```js
const refreshPaths = [
    "app/View/Components/**",
    "resources/views/**",
    "resources/lang/**",
    "lang/**",
    "routes/**"
];
```

Эти строки как раз и диктуют Vite модулю какие пути следует контролировать на наличие изменений. Изменения файлов в этом
перечне путей будет служить триггером для обновления страницы.

Если посмотреть на содержимое файла `vite.config.js` то можно обнаружить там передаваемую конфигурацию в виде

```js
laravel({
    input: [ 'resources/css/app.css', 'resources/js/app.js' ],
    refresh: true
})
```

Этот перечень инструкций сообщает Vite плагину какие файлы следует подвергнуть сборке при исполнении
команды `npm run build` и команды `npm run dev`. При исполнении второй команды файлы будут сами собираться при внесении
в них изменений.

### Создание описания CSS классов

Для начала нам необходимо установить сам пакет tailwind:

```
    npm install tailwindcss postcss autoprefixer -D
```

У фреймворка tailwind есть множество описанных классов, которые можно присваивать тегам и тем самым управлять их
отображением и графическим поведением. Например, тегу можно присвоить следующий набор классов:

```html

<a class="justify-between scale-100 p-6 bg-white dark:bg-gray-800/50 dark:bg-gradient-to-bl from-gray-700/50 via-transparent dark:ring-1 dark:ring-inset dark:ring-white/5 rounded-lg shadow-2xl shadow-gray-500/20 dark:shadow-none flex motion-safe:hover:scale-[1.01] transition-all duration-250 focus:outline focus:outline-2 focus:outline-red-500"></a>
```

Так же можно создать и своё определение класса в css файле и использовать его в HTML:

```css
.card {
    @apply justify-between scale-100 p-6 bg-white dark:bg-gray-800/50 dark:bg-gradient-to-bl from-gray-700/50 via-transparent dark:ring-1 dark:ring-inset dark:ring-white/5 rounded-lg shadow-2xl shadow-gray-500/20 dark:shadow-none flex motion-safe:hover:scale-[1.01] transition-all focus:outline focus:outline-2 focus:outline-red-500
}
```

Для класса `.card` мы назначили перечень классов, описанных в библиотеке tailwind. То есть нашему классу будет присвоены
все свойства которыми обладают перечисленные внутри классы после директивы `@apply`. Такую надпись не сможет распознать
браузер, а tailwind плагин сможет

```html
<a class="card"></a>
```

При сборке css файла все классы в строке после слова @apply развернутся в виде CSS свойств. 