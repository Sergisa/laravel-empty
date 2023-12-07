# No starterKit laravel-project

## Язык шаблонирования Blade

### Создание компонента Card из простого примера

В данном примере показано как страница с примером (welcome.blade.php) преобразуется в страницу home.blade.php разбиваясь
на шаблон вывода плюс компонент.

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

Проблемой такой страницы является, во-первых, то, что при создании других таких же страниц сайта, нам придётся так же
писать заголовочные теги. А во-вторых при использовании плиток интерфейса у нас будет появляться однотипный код, ведь
каждый раз когда мы будем использовать такой блок у нас всегда будет в коде содержаться такой набор тегов:

```html

<a href="https://laravel-news.com" class="scale-100 p-6 bg-white dark:bg-gray-800/50 dark:bg-gradient-to-bl from-gray-700/50 via-transparent dark:ring-1 dark:ring-inset dark:ring-white/5 rounded-lg shadow-2xl shadow-gray-500/20 dark:shadow-none flex motion-safe:hover:scale-[1.01] transition-all duration-250 focus:outline focus:outline-2 focus:outline-red-500">
    <div>
        <div class="h-16 w-16 bg-red-50 dark:bg-red-800/20 flex items-center justify-center rounded-full">
```

### Создаём отдельный компонент card.blade.php

Во-первых, нам необходимо выделить прямоугольный блок в виде отдельного компонента, чтобы потом мы могли бы его
использовать например с помощью директивы `@include`, как в обычном php. Однако у одного графического блока внутри HTML
дерева есть позиционное место для заголовка, иконки и основного контента. Директива `@include` не позволит нам выполнить
импорт с параметрами, то есть импортировать файл шаблона, передав ему дополнительные аргументы с текстом заголовка и
остального.

Начиная с 7-й версии Фреймворк Laravel позволяет выполнять подключение отдельных компонентов в файл шаблона. Для этого
необходимо создать файл шаблона в папке ``/resources/views/components`` создать файл с названием шаблона
"card.blade.php".
Также для этого можно использовать команду в командной строке:

```bash 
  php artisan make:component card --force --view
```

После выполнения такой команды у вас создастся файл в папке components.

- Флаг `--force` говорит что создать файл необходимо даже если он уже есть
- Флаг `--view` говорит о том что не нужно создавать файл `Card.php` в папке `_/app/view_`. Такой фал создаётся для того
  что бы вы сами могли там прописать что нужно сделать с компонентом при вставке его в вашу страницу.

Далее, в том месте где мы хотим использовать компонент, нужно вставить тэг ``<x-card></x-card>``.
В файле компонента мы должны указать место куда вставится основное содержимое тэга в виде: ``{{ $slot }}``

```html
<!--/resources/views/components/card.blade.php-->
<a href="#" class="justify-between scale-100 p-6 bg-white dark:bg-gray-800/50 dark:bg-gradient-to-bl from-gray-700/50 via-transparent dark:ring-1 dark:ring-inset dark:ring-white/5 rounded-lg shadow-2xl shadow-gray-500/20 dark:shadow-none flex motion-safe:hover:scale-[1.01] transition-all duration-250 focus:outline focus:outline-2 focus:outline-red-500">
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
<a class="justify-between scale-100 p-6 bg-white dark:bg-gray-800/50 dark:bg-gradient-to-bl from-gray-700/50 via-transparent dark:ring-1 dark:ring-inset dark:ring-white/5 rounded-lg shadow-2xl shadow-gray-500/20 dark:shadow-none flex motion-safe:hover:scale-[1.01] transition-all duration-250 focus:outline focus:outline-2 focus:outline-red-500">
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

```html
<!--/resources/views/components/card.blade.php-->
@props(['link'=>''])
<a href="{{ $link }}" class="justify-between scale-100 p-6 bg-white dark:bg-gray-800/50 dark:bg-gradient-to-bl from-gray-700/50 via-transparent dark:ring-1 dark:ring-inset dark:ring-white/5 rounded-lg shadow-2xl shadow-gray-500/20 dark:shadow-none flex motion-safe:hover:scale-[1.01] transition-all duration-250 focus:outline focus:outline-2 focus:outline-red-500">
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