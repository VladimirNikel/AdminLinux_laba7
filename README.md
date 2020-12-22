# Администрирование Linux. ДЗ №7

## Software distribution

# Цель:
> 1) Создать свой RPM пакет (можно взять свое приложение, либо собрать, например, nginx с определенными опциями);
> 2) Создать свой репозиторий и разместить там ранее собранный RPM.

## Ход работы:

Было бы полезно, если был бы собран пакет из какого-то моего решения, которое я предположительно мог бы разворачивать на несколько компьютеров, но мы пойдем по методичке.

1. Необходимо установить пакеты, для работы с сборками пакетов.
  ```bash
  sudo yum install -y redhat-lsb-core wget rpmdevtools rpm-build createrepo yum-utils
  ```
  Ниже приведен результат выполнения команды:
  
  ![Результат выполнения установки](https://sun9-27.userapi.com/impf/la1vhaWw6XErjoWpFO8YCEsEKvpRIrNqcMb90Q/oIOxzl5KEZU.jpg?size=1023x408&quality=96&proxy=1&sign=6c204a81cd5394b4d8a693fdd6b85e46&type=album "Результат выполнения установки")
  
  
2. Далее необходимо состав пакетов в операционной системе пополнить ***NGINX'ом***, для этого выполним команду:
  ```bash
  wget https://nginx.org/packages/centos/7/SRPMS/nginx-1.18.0-2.el7.ngx.src.rpm
  ```
  
  Результат выполнения указанной команды приведен на рсиунке ниже:
  
  ![установка NGINX](https://sun9-11.userapi.com/impf/hxP2h85OMBrtD-BqxxtjPsrnjkpn5KK-k4OcVQ/RRho2u4EvpY.jpg?size=961x235&quality=96&proxy=1&sign=85f70e64e9eccd2d90d8580af3e44d36&type=album "установка NGINX")


3. Так как, допустим, мне очень нужно создать пакет NGINX, которые еще и поддерживает ***openssl***, то нужно этот ***openssl*** скачать, поэтому выполним это следующим шагом:
  ```bash
  wget https://www.openssl.org/source/latest.tar.gz
  ```
  
  После этого необходимо выполнить разархивировывание данного пакета. Это делает следующей командой:
  ```bash
  tar -xvf latest.tar.gz
  ```

  Результат выполнения предыдущих команд предстваавлен на рисунке ниже:
  ![установка openssl и разархивировывание](https://sun9-10.userapi.com/impf/LpDcG8BcDLrenJczRC_a5ujaa1vJVLg3K176SQ/zhrCL6dW9Q8.jpg?size=961x617&quality=96&proxy=1&sign=d1a4cacc86deccdd8caa5387d08c704d&type=album "установка openssl и разархивировывание")


4. Выполним установку всех зависимостей, чтобы дальше не было проблем при сборке нашего пакета. Для этого нам понадобится выполнить следующую команду:
  ```bash
  sudo yum-builddep rpmbuild/SPECS/nginx.spec
  ```
  
  
  


  ```bash
  sudo rpm -i nginx-1.18.0-2.el7.ngx.src.rpm
  ```
  
  
  
  
  

## Используемый инструментарий:
- Виртуальная машина под управлением Ubuntu 18.04.5 LTS
- [что-то наподобии исходников при попытке работать с ubuntu](http://rus-linux.net/MyLDP/po/packages.html)

