# No starterKit laravel-project

## Язык шаблонирования Blade

При установке фреймворка Laravel в папке ``/resources/views/`` содержится файл демо-страницы ``welcome.blade.php``.
Страница имеет четыре визуальных блока описанных в виде тэгов.
В данной ветке репозитория показано как страница с примером `welcome.blade.php` преобразуется в страницы:

- `home.blade.php` (Файл страницы)
- `card.blade.php` (Файл компонента)
- `layout.blade.php` (Файл шаблона)

Исходный файл `welcome.blade.php` мы оставим нетронутым, что бы на него опираться.

### 1. Создаем правильную страницу, вместо имеющейся

#### 1.1 Разбиваем страницу `welcome.blade.php` на основной скелет (`layout.blade.php`) и само содержимое (`home.blade.php`)

- Создадим файл шаблона `layout.blade.php`
- Создадим файл основной страницы `home.blade.php`

В примере страницы ``welcome.blade.php`` содержатся так же и заголовки HTML страницы, а именно тэги html, head, body,
meta, style и так далее. Получается что при создании новых страниц эти же тэги сайта будут снова присутствовать в файле.

Самым правильным вариантом будет отделить основное содержимое страницы от базового скелета. Поэтому мы создадим
файл `layout.blade.php` и поместим туда контент, который будет присутствовать на каждой странице. А в файл
`home.blade.php` поместим лишь то, что будет относиться к данной странице.
Можно просто создать файлы в папке `/resources/views`, а можно в командной строке набрать:

```bash 
  php artisan make:view home --force --view
```

Такая команда создаст файл ``home.blade.php`` в папке `/resources/views`

Теперь, содержимое тэга body, имеющее отношение к одной лишь странице мы выносим в файл `home.blade.php`, оформив это
следующим образом:

```html
<!--/resources/views/home.blade.php-->
@extends('layout')

@section('content')
... Content here ...
@endsection
```

Сверху с помощью директивы ``@extends`` указывается тот шаблон, в рамках которого необходимо выводить этот файл.
Компилятор будет искать в папке `/resources/views` будет искать файл `layout.blade.php`. Далее указывается закрываемый
блок `@section('content')` с указанием названия секции - **content**. Компилятор языка при использовании
файла `layout.blade.php` начнет искать в нем строку `@yield('content')` и содержимое секции **content** поместит вместо
этой строки. Тем самым получится что мы просто содержимое этой секции обернём набором постоянных заголовочных тэгов
HTML.
Так же можно указывать и другие директивы. Например, в файле страницы мы можем указать
строку `@section('title', 'WelcomePage')`. Это точно такая же секция, но её содержимое состоит из одной строки. В файле
шаблона тэг title оформим следующим образом: ``<title>@yield('title')</title>``. Теперь получается что при загрузке
страницы у нас будет меняться и заголовок в файле шаблона.

#### 1.2 Создание нового пути сайта

Если мы откроем файл ``/routes/web.php`` То увидим там строки:

```php
Route::get('/', function () {
    return view('welcome');
});
```

Это значит что когда пользователь в браузере наберёт адрес сайта `localhost/` или `mysite.ru/` фреймворк Laravel начнёт
грузить файл с названием ``welcome.blade.php``. Пусть оно так и остаётся. Рядом нам необходимо объявить новую ссылку
которая будет ссылаться на созданный нами файл. Для этого напишем:

```php
Route::get('/home', function(){
    return view('home');
});
```

Теперь у нас появился новый путь у сайта: ``mysite.ru/home``. При запросе этого пути будет открываться
файл ``home.blade.php``, который в конечном итоге компиляции языка blade будет выглядеть ровно так же как и страница по
ссылке, описанной выше.

### 2. Создание компонента Card из простого примера

#### 2.1 Что не так с основной страницей

Как видно изначально страница содержит четыре блока с некоторым содержимым. Один блок выглядит следующим образом:

```html
<!--/resources/views/welcome.blade.php-->
<a href="https://laravel-news.com" class="scale-100 p-6 bg-white dark:bg-gray-800/50 dark:bg-gradient-to-bl from-gray-700/50 via-transparent dark:ring-1 dark:ring-inset dark:ring-white/5 rounded-lg shadow-2xl shadow-gray-500/20 dark:shadow-none flex motion-safe:hover:scale-[1.01] transition-all duration-250 focus:outline focus:outline-2 focus:outline-red-500">
    <div>
        <div class="h-16 w-16 bg-red-50 dark:bg-red-800/20 flex items-center justify-center rounded-full">
            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" class="w-7 h-7 stroke-red-500">
                <path stroke-linecap="round" stroke-linejoin="round" d="M12 7.5h1.5m-1.5 3h1.5m-7.5 3h7.5m-7.5 3h7.5m3-9h3.375c.621 0 1.125.504 1.125 1.125V18a2.25 2.25 0 01-2.25 2.25M16.5 7.5V18a2.25 2.25 0 002.25 2.25M16.5 7.5V4.875c0-.621-.504-1.125-1.125-1.125H4.125C3.504 3.75 3 4.254 3 4.875V18a2.25 2.25 0 002.25 2.25h13.5M6 7.5h3v3H6v-3z"/>
            </svg>
        </div>

        <h2 class="mt-6 text-xl font-semibold text-gray-900 dark:text-white">Laravel News</h2>

        <p class="mt-4 text-gray-500 dark:text-gray-400 text-sm leading-relaxed">
            Laravel News is a community driven portal and newsletter aggregating all of the latest and most important
            news in the Laravel ecosystem, including new package releases and tutorials.
        </p>
    </div>

    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" class="self-center shrink-0 stroke-red-500 w-6 h-6 mx-6">
        <path stroke-linecap="round" stroke-linejoin="round" d="M4.5 12h15m0 0l-6.75-6.75M19.5 12l-6.75 6.75"/>
    </svg>
</a>
```

При использовании плиток интерфейса у нас будет появляться однотипный код, ведь каждый раз когда мы будем использовать
такой блок у нас всегда будет в коде содержаться такой набор тегов:

```html

<a href="https://laravel-news.com" class="scale-100 p-6 bg-white dark:bg-gray-800/50 dark:bg-gradient-to-bl from-gray-700/50 via-transparent dark:ring-1 dark:ring-inset dark:ring-white/5 rounded-lg shadow-2xl shadow-gray-500/20 dark:shadow-none flex motion-safe:hover:scale-[1.01] transition-all duration-250 focus:outline focus:outline-2 focus:outline-red-500">
    <div>
        <div class="h-16 w-16 bg-red-50 dark:bg-red-800/20 flex items-center justify-center rounded-full">
```

#### 2.2 Создаём отдельный файл-компонент `card.blade.php`

Во-первых, нам необходимо выделить прямоугольный блок в виде отдельного компонента, чтобы потом мы могли бы его
использовать например с помощью директивы `@include`, как в обычном php. Однако у одного графического блока внутри HTML
дерева есть позиционное место для заголовка, иконки и основного контента. Директива `@include` не позволит нам выполнить
импорт с параметрами, то есть импортировать файл шаблона, передав ему дополнительные аргументы с текстом заголовка и
остального.

Начиная с 7-й версии Фреймворк Laravel позволяет выполнять подключение отдельных компонентов в файл шаблона. Для этого
необходимо в папке ``/resources/views/components`` создать файл с названием шаблона "card.blade.php".
Также для этого можно использовать команду в командной строке:

```bash 
  php artisan make:component card --force --view
```

После выполнения такой команды у вас создастся файл в папке components.

- Флаг `--force` говорит что создать файл необходимо даже если он уже есть
- Флаг `--view` говорит о том что не нужно создавать файл `Card.php` в папке `/app/view`. Такой файл создаётся для того
  что бы вы сами могли там прописать что нужно сделать с компонентом при вставке его в вашу страницу.

Далее, в том месте где мы хотим использовать компонент, нужно вставить тэг ``<x-card></x-card>``.
В файле компонента мы должны указать место куда вставится основное содержимое тэга в виде: ``{{ $slot }}``

```html
<!--/resources/views/components/card.blade.php-->
<a href="#" class="justify-between  ...">
    <div>
        ...
        <h2 class="mt-6 text-xl font-semibold text-gray-900 dark:text-white">...</h2>
        <p class="mt-4 text-gray-500 dark:text-gray-400 text-sm leading-relaxed">{{ $slot }}</p>
    </div>
    ...
</a>
```

Теперь используем описанный нами компонент на основной странице сайта.

```html
<!--/resources/views/home.blade.php-->
<x-card>
    Laravel has wonderful documentation covering every aspect of the framework. Whether you are a
    newcomer or have prior experience with Laravel, we recommend reading our documentation from
    beginning to end.
</x-card>
```

Компилятор blade языка при обработке данной странницы будет чувствителен к тегам типа ``<x-*></x-*>`` и будет искать в
папке components файл с тем названием которое идет после символов **x-**
Далее мы должны прописать дополнительные передаваемые данные компонента. Так как компонент у нас обладает не только
основным содержимым, но и заголовком и иконкой. Для этого внесем дополнения в файл компонента:

```html
<!--/resources/views/components/card.blade.php-->
<a class="justify-between  ...">
    <div>
        <div class="h-16 w-16 bg-red-50 dark:bg-red-800/20 flex items-center justify-center rounded-full">
            {{ $icon }}
        </div>
        <h2 class="mt-6 text-xl font-semibold text-gray-900 dark:text-white">{{ $title }}</h2>
        <p class="mt-4 text-gray-500 dark:text-gray-400 text-sm leading-relaxed">{{ $slot }}</p>
    </div>

    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" class="self-center shrink-0 stroke-red-500 w-6 h-6 mx-6">
        <path stroke-linecap="round" stroke-linejoin="round" d="M4.5 12h15m0 0l-6.75-6.75M19.5 12l-6.75 6.75"/>
    </svg>
</a>
```

При использовании компонента на странице мы теперь должны добавить содержимое, которое мы хотим передать в
переменные `icon` и `title`. Изменяем использование компонента в файле страницы:

```html
<!--/resources/views/home.blade.php-->
<x-card>
    <x-slot:icon>
        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" class="w-7 h-7 stroke-red-500">
            <path stroke-linecap="round" stroke-linejoin="round" d="M12 6.042A8.967 8.967 0 006 3.75c-1.052 0-2.062.18-3 .512v14.25A8.987 8.987 0 016 18c2.305 0 4.408.867 6 2.292m0-14.25a8.966 8.966 0 016-2.292c1.052 0 2.062.18 3 .512v14.25A8.987 8.987 0 0018 18a8.967 8.967 0 00-6 2.292m0-14.25v14.25"/>
        </svg>
    </x-slot:icon>
    <x-slot:title>Documentation</x-slot:title>
    ...Main Content...
</x-card>
```

Так же следует вспомнить, что компонент у ная является ссылкой. И каждый раз подключая этот компонент на страницу нам
необходимо указать ссылкой на какой ресурс будет являться этот блок. У языка есть возможность передать дополнительную
информацию в подключаемый компонент. Передаются дополнительные параметры в виде обычных HTML свойств
вида ``optional="value"``. Изменим использование компонента на странице до вида:

```html
<!--/resources/views/home.blade.php-->
<x-card link="yandex.ru"> ...</x-card>
```

Начало файла компонента изменим следующим образом, указав куда необходимо вставлять значение, передаваемое в
свойстве `link`:

```html
<!--/resources/views/components/card.blade.php-->
@props(['link'=>''])
<a href="{{ $link }}" class="justify-between ..."> ... </a>
```

