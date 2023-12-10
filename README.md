<p align="center">
    <img src="https://raw.githubusercontent.com/laravel/art/master/logo-lockup/5%20SVG/2%20CMYK/1%20Full%20Color/laravel-logolockup-cmyk-red.svg" width="400">
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

В данной ветке показан пример развёртывания и разработки проекта с использованием фреймворка Laravel

## Развёртка проекта

Начнём разворачивать проект с названием `laravel-empty`. Откроем командную строку и перейдём в ту папку, в которой мы
хотим создать папку с будущим проектом.

### Глобальный пакет

Для того чтобы развернуть проект, необходимо скачать пакет laravel/installer. Этот пакет устанавливается глобально
`composer global require laravel/installer`
Глобальный пакет появится в компьютере в папке `C:\Users\{%USER%}\AppData\Roaming\Composer\vendor\laravel\installer`
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
