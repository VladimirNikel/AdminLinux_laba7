# Администрирование Linux. ДЗ №7

## Software distribution

# Цель:
> 1) Создать свой RPM пакет (можно взять свое приложение, либо собрать, например, nginx с определенными опциями);
> 2) Создать свой репозиторий и разместить там ранее собранный RPM.

## Ход работы:

Было бы полезно, если был бы собран пакет из какого-то моего решения, которое я предположительно мог бы разворачивать на несколько компьютеров, но мы пойдем по методичке.

1. Необходимо установить пакеты, для работы с сборками пакетов.
  ```bash
  sudo yum install redhat-lsb-core wget rpmdevtools rpm-build createrepo yum-utils -y
  ```
  Ниже приведен результат выполнения команды:
  
  ![Результат выполнения установки](https://sun9-59.userapi.com/impf/FAUeGA1V_IIP_K5NeitCyHPqS97tBlhYq0pEFw/ynXDPBwAGek.jpg?size=1364x465&quality=96&proxy=1&sign=7ad24da8b86b356389c7965cb5f49437&type=album "Результат выполнения установки")
  
  
2. Далее необходимо состав пакетов в операционной системе пополнить ***NGINX'ом***, для этого выполним команду:
  ```bash
  sudo wget https://nginx.org/packages/centos/7/SRPMS/nginx-1.18.0-2.el7.ngx.src.rpm
  ```
  
  Результат выполнения указанной команды приведен на рсиунке ниже:
  
  ![скачивание NGINX](https://sun9-52.userapi.com/impf/G-RAl4llPPxQ5r5EmMOkTFn-yhuGcvZSCgyo3g/vuzWp7P8-JY.jpg?size=1363x243&quality=96&proxy=1&sign=675daed214b8b8ddf06f33eb85f79da8&type=album "скачивание NGINX")

3. Далее, необходимо полученный пакет распаковать:
  ```bash
  sudo -i
  rpm -i /home/nikel/nginx-1.18.0-2.el7.ngx.src.rpm
  ```

  Результат выполнения приведенных команд:
  
  ![распаковка пакета](https://sun9-16.userapi.com/impf/vuvzLrCeessW3nWzkbttQLTNYQhFk2Eown0UJQ/Kqwbgrfpi00.jpg?size=1046x105&quality=96&proxy=1&sign=d6605e0b3c413ab8b59a9f26f58d0bf4&type=album "распаковка пакета")


4. Так как, допустим, мне очень нужно создать пакет NGINX, которые еще и поддерживает ***openssl***, то нужно этот ***openssl*** скачать, поэтому выполним это следующим шагом:
  ```bash
  wget https://www.openssl.org/source/latest.tar.gz
  ```
  
  Результат выполнения предыдущих команд предстваавлен на рисунке ниже:
  ![установка openssl](https://sun9-5.userapi.com/impf/KKEOX8Z1RSENngctwZwOgiEQGr9v7SWUVxHEYQ/D_0j10bFIEA.jpg?size=1302x314&quality=96&proxy=1&sign=0b80d6ceed44bd2457fff9832df6c0d4&type=album "установка openssl")
  
  После этого необходимо выполнить разархивировывание данного пакета. Это делает следующей командой:
  ```bash
  tar -xvf latest.tar.gz
  ```

  В результате получилось много строк вывода:
  
  ![Много строк вывода разархивирования](https://sun9-73.userapi.com/impf/_41qt6U3yGakmOYnteUOKnUBt1vht9pHlnxpcw/eccHtz2x6u0.jpg?size=545x290&quality=96&proxy=1&sign=8810986c4564225d88acc97b0d6c4fb2&type=album "Много строк вывода разархивирования")
  
  
   Убедимся, что всё удачно (хотя иначе и быть не может):
   
   ![убедились, что папка создана](https://sun9-27.userapi.com/impf/qlf1uTTDbhIUlzBQpAnDvwNnWfOVleJRIwGeug/qJtOcQGzN98.jpg?size=720x66&quality=96&proxy=1&sign=9651ffd07078ab341eafc939e0ab590b&type=album "убедились, что папка создана")


5. Выполним установку всех зависимостей, чтобы дальше не было проблем при сборке нашего пакета. Для этого нам понадобится выполнить следующую команду:
  ```bash
  yum-builddep rpmbuild/SPECS/nginx.spec
  ```
  
  Результат выполнения как всегда приложени ниже:
  
  ![Результат установки и обновления всех необходимых пакетов для установки nginx](https://sun1-16.userapi.com/impf/NBVX7Kf3x11xc1nx4qWlt_UNtBaFr1GUATr0Hg/LfWUs6AlF68.jpg?size=1365x323&quality=96&proxy=1&sign=a7111a92b3c29a09f405c6d1f8436e68&type=album "Результат установки и обновления всех необходимых пакетов для установки nginx")
  

6. Далее необходимо было поправить файл `rpmbuild/SPECS/nginx.spec`, для этого будем использовать команду:
  ```bash
  nano rpmbuild/SPECS/nginx.spec
  ```
  
  Как было:
  
  ![Как было](https://sun9-16.userapi.com/impf/28KwZoYDHf9lMF5ujGOZmChqfik_HsuP8s3peQ/bCIB9Q97W30.jpg?size=657x552&quality=96&proxy=1&sign=3ee069de98b3473ee0c92ace79703b18&type=album "Как было")
  
  Необходимо было привести файл к следующему виду:

  ![Как стало](https://sun9-38.userapi.com/impf/yYQ1Ptu5xfltwDiDY96n0Rp0Qy26niq3j7g-BQ/W9HYO9FoFyg.jpg?size=672x454&quality=96&proxy=1&sign=79813d56c5b7cf5c8958d7469088a96b&type=album "Как стало") 
  
  
7. Выполним сборку, уже по SPEC'у нами исправленному. Делается это командой:
  ```bash
  rpmbuild -bb rpmbuild/SPECS/nginx.spec
  ```
  
  Ошибка:
  
  ![Не найден С компилятор](https://sun9-76.userapi.com/impf/RhDS7Y7866VQkoJlivEjJPbW9ZJjJC0CjiGN-g/65pVKfBmwIg.jpg?size=1362x306&quality=96&proxy=1&sign=935f19986ade865807eed1560b6663c2&type=album "Не найден С компилятор")

  Это легко исправляется установкой необходимого компилятора:
  ```bash
  yum install gcc -y
  ```
  
  ![установка компилятора](https://sun9-26.userapi.com/impf/WjP_byLaMYQIdXnNkHPfQgryH7fBZbrLeRToYA/tOKHO0cnwKc.jpg?size=1359x231&quality=96&proxy=1&sign=f0504a579b963d35de495a9085b0046a&type=album "установка компилятора")
  
  
  После этого снова запускает сборку пакета:
   ```bash
  rpmbuild -bb rpmbuild/SPECS/nginx.spec
  ```
  
  Наши ожидания оказались не бесполезными и мы можем убедиться, что пакет собран:
  ```bash
  ls -la rpmbuild/RPMS/x86_64
  ```
  
  Удачно завершенная сборка пакета:
  
  ![Удачная сборка пакета](https://sun9-13.userapi.com/impf/b-AvNhAbwrui_p3s5wtfDmTz3UDhtwSoXr9abQ/E2G0dbJDVn0.jpg?size=809x138&quality=96&proxy=1&sign=b9bfbe92bf219ea37fd623dc1cb45861&type=album "Удачная сборка пакета")
  
  
8. Теперь можно установить наш пакет и убедиться, что nginx работает корректно:
  ```bash
  yum localinstall rpmbuild/RPMS/x86_64/nginx-1.18.0-2.el8.ngx.x86_64.rpm -y
  ```

  Результат:
  
  ![локальная установка пакета](https://sun9-45.userapi.com/impf/DO9M1BGRwoXAnRmsorevO3j1qzWrQVBCXPBoOg/oUttwQZxcOk.jpg?size=873x293&quality=96&proxy=1&sign=3bc42a9656403bb17bac09a3fff6890c&type=album "локальная установка пакета")
  
  
  
  
9. Тепероь перезапустим наш nginx
  ```bash
  systemctl restart nginx
  systemctl status nginx
  ```
  
  Результат запуска/перезапуска nginx:
  ![Результат перезапуска nginx](https://sun9-76.userapi.com/impf/wN-RLWHuHGCR64wqOMhKXePO2ydnU-oVmMSK2g/EqX1utEJRLI.jpg?size=910x319&quality=96&proxy=1&sign=6458b211de677caf92850c5de4df8769&type=album "Результат перезапуска nginx")
  
  
  
10. Перейдем к создани своего репозитория
  Для начала нам нужно создать каталог **repo**. Сделаем это выполненеим команды:
  ```bash
  exit
  sudo mkdir /usr/share/nginx/html/repo
  ```
  
  Результат выполнения:
  
  ![](https://sun9-64.userapi.com/impf/Zch78d0ojPVMSSC0P57RMIMX3BvLCeK8Nkm4nw/c2FuAYGiiR4.jpg?size=556x74&quality=96&proxy=1&sign=012a4f1c9442d9d1d328a3ae031414cb&type=album "")
  
  
  Далее скопируем наш собранный RPM и полезную штуку - репозиторий Persona-Server:
  ```bash
  sudo -i
  cp rpmbuild/RPMS/x86_64/nginx-1.18.0-2.el7.ngx.x86_64.rpm /usr/share/nginx/html/repo/
  wget https://repo.percona.com/centos/7Server/RPMS/noarch/percona-release-1.0-9.noarch.rpm -O /usr/share/nginx/html/repo/percona-release-1.0-9.noarch.rpm
  ```
  
  Результат:
   
  ![копирование и установка percona](https://sun9-23.userapi.com/impf/Wn_2OSZ0u6MOIxY-nl2UJL1eTYd0H7VpyDBmuQ/QqS9GQgNxPo.jpg?size=1354x283&quality=96&proxy=1&sign=1cc4802c9a57451191b8e4c40763ee89&type=album "копирование и установка percona")
  
  
  
  Далее необходимо инициализировать репозиторий командой:
  ```bash
  createrepo /usr/share/nginx/html/repo/
  ```
  
  
  ![Инициализация репозитория](https://sun9-68.userapi.com/impf/dN3jDZezcGEodJkbwPdij3qL_Zzh84rIrUTFjg/yL1Ux5CKhoE.jpg?size=611x164&quality=96&proxy=1&sign=7afa2bc116c1f1bae4d9d5799287f0a9&type=album "Инициализация репозитория")
  
  
  Для прозрачности настроим в NGINX доступ к листингу каталога:
  В location / в файле `/etc/nginx/conf.d/default.conf` добавим директиву `autoindex on`. В результате location будет выглядеть так:
  ```bash
    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
        autoindex on;
    }
  ```

  Проверяем синтаксис и перезапускаем NGINX:
```nginx -t```

    nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
    nginx: the configuration file /etc/nginx/nginx.conf syntax is ok

```nginx -s reload```

  Результат выполнения:
  
  ![результат перезапуска после внесения изменений](https://sun9-49.userapi.com/impf/DdW9l9jWkJIkYJWcF_O_mu2XaNM6hTlRiHfYDg/a78fJqBfQcw.jpg?size=621x119&quality=96&proxy=1&sign=6a1383747bb3a1097a97ff1bcc166ab1&type=album "результат перезапуска после внесения изменений")
  
  
  А вот так выглядит обращение к репозиторию:
  
  ![обращение к репозиторию через браузер](https://sun9-72.userapi.com/impf/muLAXwTikK5YjzXnymM2VRWYfmQNz1tCxFCGsQ/NhWYe1xK3fQ.jpg?size=671x234&quality=96&proxy=1&sign=066ae31f5230eba0b48d933296394b24&type=album "обращение к репозиторию через браузер")
  
  
  
  
 11. Осталось дело за малым: добавить репозиторий в yml
  ```bash
  cat >> /etc/yum.repos.d/mai.repo << EOF
  [mai]
  name=mai-linux
  baseurl=http://localhost/repo
  gpgcheck=0
  enabled=1
  EOF
  ```
  
  
  ![создание указания для yum](https://sun9-22.userapi.com/impf/G7fjmXJC2YuULbZM6D0d1jGUz6kzS5NUZw26NA/cf1JXa6UGi8.jpg?size=556x155&quality=96&proxy=1&sign=879cffa69702663ddb5170c25b7c934b&type=album "создание указания для yum")
  
  ![](https://sun9-45.userapi.com/impf/qzBivaFOqc71AUD_kGIn4T_FLcWGfsFgg0QmvQ/drxfP4X3FC0.jpg?size=484x72&quality=96&proxy=1&sign=17b8c06c0d832bbe913c59c3274034d0&type=album "")
  
  
  почему-то перустановка nginx не захотела проводиться:
  
  ![Не проведенная переустановка nginx](https://sun9-15.userapi.com/impf/otver-HpGEbk9JU1W9pSHz-NVJxnL7jQEIYIXA/P0eNLDpE9OU.jpg?size=872x132&quality=96&proxy=1&sign=0594c535c94681f6be1106a06d3d11d0&type=album "Не проведенная переустановка nginx")
  
  
  Зато установка **percona** пошла совершенно спокойно.
  
  ![установка percona](https://sun9-38.userapi.com/impf/uF9fz2NfcmTiIZM0Z4m3oT_JCCfZdBJZve8jRg/726ey-j_bsg.jpg?size=1366x312&quality=96&proxy=1&sign=b89a6ca0c2f921182b4c6710a5728c28&type=album "установка percona")
  
  
  
  
Всё!

------------





## Используемый инструментарий:
- Виртуальная машина под управлением Ubuntu 18.04.5 LTS
- [что-то наподобии исходников при попытке работать с ubuntu](http://rus-linux.net/MyLDP/po/packages.html)

