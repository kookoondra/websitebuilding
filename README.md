# Автоматизация установки и настройки учебного хостинга

Этот набор скриптов автоматизирует установку и настройку хостинга, а также устанавливает на него последнюю версию CMS WordPress. Скрипты рассчитаны на работу с ***Debian 9***.

Скрипты были написаны для того, чтобы автоматизировать работу с учебным хостингом в рамках курса по сайтостроению и WordPress. Но также их можно применить и просто для того, чтобы быстро развернуть LAMP-хостинг для своих проектов.

## Установка

1. Установите свежий инстанс Debian 9 на VPS/VDS хостинг. Изначально можно установить любой Debian, но потом все равно нужно будет сделать апгрейд до последней версии, иначе пакеты не установятся.
2. Под пользователем ***root*** скопируйте в командную строку содержимое файла [under_root](https://raw.githubusercontent.com/koo2018/websitebuilding/master/under_root). Этот скрипт создаст пользователя ***teacher***, попросит установить ему пароль, в рабочую директорию ***~teacher*** скачает скрипт [wsite_install](https://raw.githubusercontent.com/koo2018/websitebuilding/master/wsite_install), сделает его читаемым только пользователем ***teacher*** и выйдет из рутового аккаунта.
3. Вернитесь в систему под пользователем ***teacher***
4. Запустите скрипт [wsite_install](https://raw.githubusercontent.com/koo2018/websitebuilding/master/wsite_install). Надо будет указать основной домен вашего сервера, пароль администратора MySQL, а также стандартный пароль, который получат пользователи по умолчанию.
5. Скрипт установит все пакеты, скачает WordPress, скачает и настроит скрипты автоматизации создния пользовательских аккаунтов. По завершению работы скрипт удалит сам себя и перезагрузит сервер. Вернитесь и начинайте работу с аккаунтами пользователей/студентов. 


## Что будет после установки

В результате работы скрипта [wsite_install](https://raw.githubusercontent.com/koo2018/websitebuilding/master/wsite_install) в директории появится несколько файлов
* **delstudent** - скрипт удаление пользовательского/студенческого аккаунта
* **newstudent** - скрипт создания пользовательского/студенческого аккаунта  
* **tarbkp** - скрипт бэкапа пользовательских/студенческих аккаунтов архиватором tar+gzip     
* **zipbkp** - скрипт бэкапа пользовательских/студенческих аккаунтов архиватором zip
* **.websitebuilding** - файл-метка, что система уже установлена, чтобы случайной переустановкой не поломать настройки. Лучше его не трогать


## Порядок работы со студенческими аккаунтами

1. Вы создаете в системе группу, обозначающую группу студентов

```bash
sudo addgroup grp1801
```

2. Далее вы создаете в этой группе студентов

```bash
./newstudent grp1801 grp1801st01
```

3. Повторяете эту процедуру столько, сколько нужно. Копи-пейст и стрелочка вверх в помощь

 ```bash
./newstudent grp1801 grp1801st02

./newstudent grp1801 grp1801st03
  
...

./newstudent grp1801 grp1801st99
 ```

4. Каждому студенту будет назначен пароль. Это будет студенческий/пользовательский пароль, который запрашивал скрипт [wsite_install](https://raw.githubusercontent.com/koo2018/websitebuilding/master/wsite_install).

5. По адресу http://grp1801st01.вашдомен появляется пустой сайт, а по адресу http://grp1801st01.вашдомен/wordpress появляется ваш сайт на WordPress, при первой загрузке которого вам предложат его установить и настроить. Админка студенческого сайта http://grp1801st01.вашдомен/wordpress/wp-admin.

5. При [установке WordPress](https://github.com/koo2018/websitebuilding/blob/master/README.md#%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5-%D1%81%D0%B0%D0%B9%D1%82%D0%B0-%D0%BD%D0%B0-wordpress) у вас спросят имя базы данных, которой сможет воспользоваться система – это имя студента, например, grp1801st01; спросят имя пользователя базы данных – это тоже имя студента, например, grp1801st01; и спросят пароль доступа этого пользователя к БД – вы его задавали при установке системы. Имя сервера всегда – localhost. Префикс может быть любым или отсутствовать.

5. Параметры FTP и доступа на хостинг по ssh:

* Сервер: <вашдомен>
* Имя пользователя: имя студента, например, grp1801st01
* Пароль: пароль пользователя, который вы задавали, создавая пользователя 

## Создание сайта на WordPress

Для каждого аккаунта автоматически устанавливается экземпляр CMS WordPress, который доступен по адресу http://логин_студента.ваш_домен/wordpress. При первом заходе по этому адресу пользователю предложат установить WordPress - появится форма, в которую надо будет внести параметры.

* **Имя базы данных** - должно совпадать с именем пользователя, например, grp1801st01
* **Имя пользователя** - это название студенческого или пользовательского аккаунта
* **Пароль** - это пароль для студенческих аккаунтов, который вводился при запуске скрипта [wsite_install](https://raw.githubusercontent.com/koo2018/websitebuilding/master/wsite_install)
* **Сервер баз данных** - localhost
* **Префикс таблиц** - может быть любой или никакого. Можно оставить, как есть

Если все правильно, WordPress установится и предложит создать свое медиа: прописать его название и пользователей, кто может им управлять. Как вы понимаете, это не те же пользователи, что в вашей linux-системе, это уже уровень работы с WordPress. Это важно не путать.

## Удаление пользовательского/студенческого аккаунта

**delstudent** – скрипт создания пользовательского/студенческого аккаунта. Этот скрипт вызывается с одним параметром - именем пользователя:

```bash
./delstudent studentName,
```

где studentName – идентификатор студента. Например, в моей работе эта команда выглядела как 

```bash
./delstudent grp161st01
```

Что делает скрипт:
1.	Удаляет пользователя, указанного в параметре скрипта, со всей домашней директорией рекурсивно, если такой пользователь существует.
2.	Выключает сайт пользователя, удаляет файл конфигурации и перезапускает apache2.
3.	Стирает все базы данных пользователя и удаляет его аккаунт в Mysql.

***ВАЖНО! СКРИПТ ВОПРОСОВ НЕ ЗАДАЕТ И БЭКАПОВ НЕ ДЕЛАЕТ! БУДЬТЕ АККУРАТНЫ!*** 

## Смена паролей

### Студент забыл пароль от хостинга/FTP

Тут все просто. Надо просто под **sudo** установить новый пароль

```bash
sudo passwd <studentName>
``` 

### Студент забыл пароль от своего сайта на WordPress

Здесь все несколько сложнее. Тем не менее, существует несколько способов поменять пароль пользователя сайта на WordPress. Мы сделаем это для пользователя editor админки wordpress через клиентскую программу MySQL. Набираем в командной строке:

```bash
mysql -uroot -p -hlocalhost
```

и после введения пароля попадаем в среду работы с базой данных MariaDB/MySQL. Сначала нам надо переключиться в базу данных этого забывчивого студента, которому мы меняем пароль (точка с запятой в конце важна):

```mysql
USE tester01;
```

Смотрим, есть ли вообще такой пользователь его админки, которому студент хочет поменять пароль.

```mysql
SELECT user_login FROM wp_users;
```

Видим, что есть.


Меняем пароль на 54321, предварительно зашифровав его алгоритмом md5, как того требует WordPress:

```mysql
UPDATE wp_users SET user_pass = MD5('54321') WHERE user_login = 'editor';
```

Если ваш пользователь админки не editor или вы хотите другой пароль, внесите соответствующие изменения в последнюю команду.


## Бэкап системы
Установочный скрипт скачал, записал и настроил две утилиты для архивирования **tarbkp** и **zipbkp**, котрые делают бэкап при помощи архиваторов tar+gzip и zip, соответственно. Решение с tar и gzip работает существенно быстрее и сжимает лучше процентов на пять. Оба этих скрипта работают одинаково. Они создают в домашней директории пользователя **teacher** папку **backups** и складывают туда в сжатом виде студенческие директории и дамп **mysql**. Получается файл, в названии которого текущая дата, a расширение .tgz или .zip, в зависимости от расширения. Например, 2018-11-04.zip или 2018-11-04.tgz. Причем, скрипт бэкапа сотрет файлы старше 14 дней в папке backups, чтобы не засорять диск.

Можно настроить систему, чтобы она автоматически «бэкапила» данные, например, по ночам. Для этого надо разместить задание в планировщике cron.

```cron
/usr/bin/crontab -e
```

Добавьте в конец файла следующую строку

```
30 6 * * * /home/teacher/tarbkp
```

Ее смысл таков: в 30 минут, 6 часов утра, каждый день месяца, каждый месяц, каждый день недели (* * * - об этом) выполни скрипт, который находится по пути **/home/teacher/tarbkp**. Вы можете назначить свой график.

Не забудьте нажать Enter после этой строки в файле – так требует cron.

### Уведомление о безопасности

Не принимая на себя никакой ответственности за последствия использования этого скрипта, все-таки хочу отметить, что в файлах **delstudent**, **newstudent**, **tarbkp**, **zipbkp** пароли хранятся в ***ОТКРЫТОМ ВИДЕ***. И хотя файлы может просматривать только сам пользователь, это далеко от настоящей секьюрности. И все это - риски пользователя. Если у кого-то есть решение лучше, дайте знать.

