﻿# VestaCP загрузка резервных копий в облако MEGA [![](https://ga-spi.appspot.com/t/VestaCP-Sync-Backups-To-Mega/readme-ru?gtid=UA-77529928-1&mr)](https://ga-spider.appspot.com)

(*v-sync-backups-to-mega - скрипт для синхронизации бэкапов с облаком MEGA*)

С помощью данного скрипта вы можете автоматизировать загрузку резервных копий в облако **MEGA**, которое бесплатно предоставляет **50 GiB** пространства в облаке. Также есть платные тарифы.

Скрипт очень прост в использовании. Его несложно установить на собственный сервер. Разумеется, если есть навыки работы с консолью, панелью управления **VestaCP** и немного сообразительности. Вперёд, в мир автоматизированного резервного копирования в облако!

[![](https://img.shields.io/badge/LICENSE-MIT-blue.svg?style=flat-square)](https://ga-spi.appspot.com/t/VestaCP-Sync-Backups-To-Mega/readme-ru?gtid=UA-77529928-1&mr&mgo&go=https://git.io/vrxu8) [![](https://img.shields.io/badge/README-EN-green.svg?style=flat-square)](https://ga-spi.appspot.com/t/VestaCP-Sync-Backups-To-Mega/readme-ru?gtid=UA-77529928-1&mr&mgo&go=https://git.io/vosBb) [![](https://img.shields.io/badge/RELEASE-V2.0-blue.svg?style=flat-square)](https://ga-spi.appspot.com/t/VestaCP-Sync-Backups-To-Mega/readme-ru?gtid=UA-77529928-1&mr&mgo&go=https://github.com/By-Vasiliy/VestaCP-Sync-Backups-To-Mega/releases/tag/v2.0)

# Зависимости 

(*Предварительная установка пакетов от которых зависит скрипт*)

Для работы скрипта необходимо установить [MegaTools](https://ga-spi.appspot.com/t/VestaCP-Sync-Backups-To-Mega/readme-ru?gtid=UA-77529928-1&mr&mgo&go=https://megatools.megous.com/).

Скрипт автоматической установки для Linux Ubuntu 14.04: [install-megatools.sh](https://ga-spi.appspot.com/t/VestaCP-Sync-Backups-To-Mega/readme-ru?gtid=UA-77529928-1&mr&mgo&go=https://git.io/vrq6v) (где его искать? Смотрите файлы в git репозитории!).

*Также MegaTools и скрипт(v-sync-backups-to-mega)  можно установить под любую Linux OS, но инструкция написана под Ubuntu 14.04 и не проверялась под другими системами. Так что, если вы захотите плясок  бубном и у вас всё получится, можете составить или дополнить инструкцию.*

**Проверка, установился ли MegaTools:** От имени любого пользователя выполните команду ***megareg***, и
если результат такой:

```
ERROR: You must specify either --register or --verify option
```

то всё хорошо, а если такой:

```
megareg: command not found
```

то **MegaTools** не установился - ищите проблему (почти всегда всё ставится без плясок с бубном, но могут встретиться проблемы).


# Установка 

(*Установка и настройка скрипта*)

Необходимо разместить скрипт (v-sync-backups-to-mega) в папке **/usr/local/vesta/bin/** и внести небольшие изменения в скрипт:

Найдите в файле скрипта следующую строку:

```
CLOUD_BACKUPS_DIR="DEFAULT_DIR"; #Name the backup directory in the MEGA cloud
```

и замените *DEFAULT_DIR* на необходимое вам название папки. Скрипт автоматически создаст в облаке папку с таким названием (если она отсутствует), и будет загружать в неё резервные копии (пример: vps1 — но лучше что-нибудь вразумительное, без пробелов).

После всех изменений выполняем команды:

```
chmod 770 /usr/local/vesta/bin/v-sync-backups-to-mega
chown root:root /usr/local/vesta/bin/v-sync-backups-to-mega
```

Далее необходимо создать файл **.megarc** в домашней директории **root** (/root/.megarc) со следующими данными:

```
[Login]
Username = Your_Mega_Username
Password = Your_Mega_Password
```

**Проверяем результат:** От имени **root** выполняем команду ***megals*** ( можно просто выполнить команду ***sudo megals*** #под Linux Ubuntu/Debian), должно вывести что-то похожее:

```
/Contacts
/Inbox
/Root
/Trash
```

Если в результате выполнения команды произошла ошибка, то неправильно заполнен конфигурационный файл **.megarc**,  либо вы выполнили команду не от имени **root**.

# CRON задание 

(*CRON задание – синхронизация резервных копий по расписанию*)

Через панель управления Vesta создаём крон-задание с командой:

```
sudo /usr/local/vesta/bin/v-sync-backups-to-mega $1 $2
```

**$1** - Целое число резервных копий для 1 пользователя, хранимое в облаке. При превышении этого числа наиболее старая резервная копия удаляется. По умолчанию хранится 21 копия. Если у вас мало пользователей в панели и размеры резервных копий не превышают 500-800 MiB, то это вполне приемлемая цифра. Но рассчитывать её желательно индивидуально и периодически проверять облако).

**$2** – Максимальное количество строк в лог-файле. После того, как это значение будет превышено, лог очистится, и в нём останется только сообщение об очистке (по умолчанию, данное значение равно 200000 строк ~ 50 MiB).

# CRON пример 

![VestaCP скрипт загрузки бэкапов в облако MEGA пример cron задания](http://i.imgur.com/8GXtDhR.png)

*На изображении показан пример CRON задания (каждый день в 7:15 утра резервные копии будут синхронизироваться с облаком, максимальное число копий - 21, и максимальное число строк в файле лога 200000 ~ 50 MiB).*

# Лог файл 

(*Лог поможет всегда знать, как идут дела с синхронизацией резервных копий*)

Лог пишется в стандартную папку **/var/log/**, название файла лога генерируется по схеме **название_скрипта.log (/var/log/название_скрипта.log)**. Лог файл по умолчанию:

```
/var/log/v-sync-backups-to-mega.log
```

# Поддержи разработчика

(*Без доната трудновато*)

***WebMoney***
* R808133397823
* Z347790264030
* E175788944874
* X060454244628
* G149817486110
* U448946356404
* K200344065989

***Yandex, Visa, MasterCard***

[![Пожертвования в поддержку разработчика Василия Василюка: github.com/By-Vasiliy bitbucket.org/By-Vasiliy](http://i.imgur.com/gR66Yx3.png)](https://ga-spi.appspot.com/t/VestaCP-Sync-Backups-To-Mega/readme-ru?gtid=UA-77529928-1&mr&mgo&go=http://yasobe.ru/na/developer_by_vasiliy)

# Лицензия

Программное обеспечение распространяется под [MIT](https://ga-spi.appspot.com/t/VestaCP-Sync-Backups-To-Mega/readme-ru?gtid=UA-77529928-1&mr&mgo&go=https://git.io/vrxu8) лицензией.

# Copyright (c) 2016 Vasilyuk Vasiliy