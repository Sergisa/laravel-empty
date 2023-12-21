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
</p>

# Изучаем Laravel

В данном репозитории показан пример развёртывания и разработки проекта с использованием фреймворка Laravel. Смотрите
внимательнее на ветки! Коммиты в данном репозитории абсолютно не информативны. А вот каждая ветка представляет собой
развитие отдельной идеи, которая описана в файле README.md. В каждой ветке лежит своё описание.

Так же могут присутствовать Label у некоторых коммитов для обозначения важных этапов разработки. Но это маловероятно

## Почему empty?

<p align="left">
При развертке проекта Laravel предоставит вам на выбор два варианта starter-kit-ов (Стартовый набор):
    <img src="https://img.shields.io/badge/Jetstream-Breeze-grey" alt="Build Status">
</p>
Один starter-kit это пред подготовленный к сборке проект с использованием своих библиотек. В каждом наборе библиотеки разные.
Данный репозиторий имеет такое название, потому что представляет описание развертки проекта без использования какого либо
starter-kit(-а).

## Развёртка проекта

Начнём разворачивать проект с названием `laravel-empty`. Откроем командную строку и перейдём в ту папку, в которой мы
хотим создать папку с будущим проектом.
При установке рет будет идти о двух PHP библиотеках:

- `laravel/installer`
  Эта библиотека представляет собой установщик и настройщик первичного скелета вашего приложения. Он просто создаст вам
  минимально необходимый набор файлов и папок в рамках которых будет работать ваш сайт. Важно понимать что это PHP
  скрипты, которые будут запускать не в виде сайта из браузера, а в командной строке для исполнения в операционной
  системе.
- `laravel/framework`
  Эта библиотека представляет собой сам фреймворк (набор готовых функций и механизмов) благодаря которому будет работать
  ваш сайт. Этот набор PHP скриптов будет запускать, как раз, при запросе страниц вашего сайта в браузере.

### Глобальный пакет

Для того чтобы развернуть проект, необходимо скачать пакет laravel/installer. Этот пакет устанавливается глобально. Так
как пакет устанавливается глобально, то выполнять его можно в любой папке операционной системы в командной строке.

```bash
  composer global require laravel/installer
```

Глобальный пакет появится в компьютере в папке `C:\Users\{%USERNAME%}\AppData\Roaming\Composer\vendor\laravel\installer`
В папке `C:\Users\Sergisa\AppData\Roaming\Composer\vendor\bin` появится файл **laravel.bat** а сама папка должна быть
прописана в переменной PATH в настройках. Теперь в командной строке появляется возможность использовать
команду `laravel`

### Создание папки с проектом

Что бы развернуть проект нужно в командной строке выполнить команду:

```bash
    laravel new laravel-empty --force
```

Флаг `--force` указывает на то, что папку с проектом необходимо создать даже если
такая уже есть. При выполнении такой команды установщик будет задавать вопросы о том, с какими параметрами необходимо
развернуть проект. А именно:
какой набор плагинов использовать

Так же развернуть установку можно командой:

```bash
    composer create-project laravel/laravel laravel-empty
```

Для того что бы команда сработала необходимо чтобы папки **laravel-empty** не было в папке, в которой исполняется
команда.
