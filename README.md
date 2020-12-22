# Администрирование Linux. ДЗ №7

## Software distribution

# Цель:
> 1) Создать свой RPM пакет (можно взять свое приложение, либо собрать, например, nginx с определенными опциями);
> 2) Создать свой репозиторий и разместить там ранее собранный RPM.

## Ход работы:

Было бы полезно, если был бы собран пакет из какого-то моего решения, которое я предположительно мог бы разворачивать на несколько компьютеров. Последнее время я часто работаю с Python и мне нужны некоторые постоянные пакеты, вместе с Python. Попробуем создать пакет, только в моем случае это будет не ***RPM***, а ***DEB-пакет***, ведь используемая мною ОС - Ubuntu. ~~Как всегда не ищу легких путей.~~


1. Первым делом необходимо установить пакеты утилит, которые позволяют работать с исходными кодами пакетов.
  ```bash
  sudo apt install build-essential autoconf automake autotools-dev
  ```
2. Далее нужно установить инструменты, которые позволят сформировать deb-пакеты:
  ```bash
  sudo apt install dh-make debhelper devscripts fakeroot xutils lintian pbuilder
  ```
3. Далее необходимо создать ключ GPG, чтобы им подписывать свои пакеты. Для формирования ключа необходимо выполнить команды:
  ```bash
  gpg --gen-key
  ```
  Выбираем все по умолчанию, заполняем данные о себе, в результате получается примерно такая запись:
  ![Создание GPG-ключа](https://sun9-29.userapi.com/impf/mzzmMi76uoVD3YhR1uOaZHv1fLQZ2YO8FO6cfw/z5kZa-nQgy4.jpg?size=884x651&quality=96&proxy=1&sign=26ce7cf205150f12d8893b29e4774f91&type=album "Создание GPG-ключа")

ладно, проще в CentOS сделать

## Используемый инструментарий:
- Виртуальная машина под управлением Ubuntu 18.04.5 LTS

