https://sysadminium.ru/adm_serv_linux-repositories/

**Оглавление**  

[Что такое репозиторий]

[Конфиги со списком репозиториев](## Что такое репозиторий)

[Разбираем конфиг](https://sysadminium.ru/adm_serv_linux-repositories/#Razbiraem_konfig)

[Классы релизов в Debian](https://sysadminium.ru/adm_serv_linux-repositories/#Klassy_relizov_v_Debian)

[Файл Release](https://sysadminium.ru/adm_serv_linux-repositories/#Fajl_Release)

[Архитектура пакетов](https://sysadminium.ru/adm_serv_linux-repositories/#Arhitektura_paketov)

[Вариант использования официальных репозиториев](https://sysadminium.ru/adm_serv_linux-repositories/#Variant_ispolzovania_oficialnyh_repozitoriev)

[Добавление сторонних репозиториев](https://sysadminium.ru/adm_serv_linux-repositories/#Dobavlenie_storonnih_repozitoriev)

[Источник репозитория](https://sysadminium.ru/adm_serv_linux-repositories/#Istocnik_repozitoria)

[Приоритет репозитория](https://sysadminium.ru/adm_serv_linux-repositories/#Prioritet_repozitoria)

[Целевой выпуск](https://sysadminium.ru/adm_serv_linux-repositories/#Celevoj_vypusk)

[Открытый ключ репозитория](https://sysadminium.ru/adm_serv_linux-repositories/#Otkrytyj_kluc_repozitoria)

[Проверка добавленного репозитория](https://sysadminium.ru/adm_serv_linux-repositories/#Proverka_dobavlennogo_repozitoria)

[Итог](https://sysadminium.ru/adm_serv_linux-repositories/#Itog)


## Что такое репозиторий

**Репозиторий** – это своеобразное хранилище приложений. У многих **GNU/Linux** дистрибутивов есть свои **репозитории**. А также разработчики какого-нибудь отдельного программного обеспечения могут создать свой репозиторий. Но в этом случае такой репозиторий нужно разделить на ветки, одна ветка будет подходить для одной Linux системы, а другая для другой.

В репозиториях, которые подходят для **Debian** и **Ubuntu** приложения хранятся в виде архивов. Такие архивы называются **пакетами**. Эти пакеты имеют особый формат – **deb**. Есть ещё другой, популярный, формат пакетов – **rpm**, но системы полагающиеся на такие пакеты я не рассматриваю.

![Linux сервера и их репозитории](https://sysadminium.ru/wp-content/uploads/2022/05/Linux-%D1%80%D0%B5%D0%BF%D0%BE%D0%B7%D0%B8%D1%82%D0%BE%D1%80%D0%B8%D0%B82.jpg)

Linux сервера и их репозитории

Репозитории могут быть доступны с использованием различных протоколов, но самым распространённым является **http**. Такие репозитории могут быть открыты с помощью обычного браузера. И от туда можно скачать необходимый пакет для установки. Но правильнее устанавливать пакеты в систему из репозиториев с помощью специальных утилит – **пакетных менеджеров**.

На этом уроке мы познакомимся с подключением системных и сторонних репозиториев. А пакетные менеджеры рассмотрим позже.

## Конфиги со списком репозиториев

Пакетные менеджеры, которые умеют устанавливать пакеты из репозиториев, должны знать адреса репозиториев. И эти адреса записываются в конфиг – **/etc/apt/sources.list.** А также можно создавать дополнительные конфиги с расширением **.list** в каталоге **/etc/apt/sources.list.d/**. Всё это справедливо и для **Debian** и для **Ubuntu**.

Если помните, в процессе установки систем мы выбирали репозиторий:

-   для Debian – **deb.debian.org**;
-   для Ubuntu – **ru.archive.ubuntu.com**.

Давайте теперь посмотрим какие репозитории прописались на наших Linux системах (с помощью 

egrep -v '^#|^$'

 я убрал комментарии, а с помощью 

cat -n

 добавил нумерацию строк):

alex@ubu:~$ egrep -v '^#|^$' /etc/apt/sources.list | cat -n

![[Снимок экрана 2022-10-23 в 12.50.22.png]]

### Разбираем конфиг

Этот файл состоит из строк, а строки состоят из следующих столбцов:

-   Тип пакетов:
    -   **deb** — архив с уже откомпилированной и готовой к установке программой;
    -   **deb-src** — архив с исходным кодом программы, который перед установкой нужно будет откомпилировать.
-   Адрес репозитория:
    -   **http://ru.archive.ubuntu.com/ubuntu** – для Ubuntu;
    -   **http://deb.debian.org/debian/** – для Debian;
    -   **http://security.debian.org/debian-security** – обновления безопасности для Debian.
-   Ветки репозитория:
    -   для **Ubuntu**:
        -   **jammy** – приложения для этой версии Ubuntu;
        -   **jammy-updates** – рекомендуемые обновления;
        -   **jammy-backports** – обновления из более новой системы. Допустим для Ubuntu 20.04 определённый пакет уже не обновляется, но из этой ветки можно попробовать его обновить (конечно есть вероятность повредить систему);
        -   **jammy-security** – важные обновления безопасности, без которых ваш сервер легче будет взломать.
    -   для **Debian**:
        -   **bullseye** – приложения для этой версии **Debian**;
        -   **bullseye-updates** – рекомендуемые обновления;
        -   **bullseye-security** – важные обновления безопасности, без которых ваш сервер легче будет взломать.
-   И в самом конце компоненты, их можно записывать через пробел в одной строке:
    -   Для **Ubuntu**:
        -   **main** – здесь находятся пакеты, которые официально поддерживаются компанией Canonical;
        -   **restricted** – содержит поддерживаемое ПО с закрытым исходным кодом, например MP3 или Flash;
        -   **universe** – содержит ПО, которое поддерживается сообществом пользователей и разработчиков Ubuntu;
        -   **multiverse** – содержит ПО, которое каким-либо образом ограничено либо условиями лицензии, либо юрисдикцией.
    -   Для **Debian** компоненты делятся по критериям свободного ПО (**[DFSG](https://ru.wikipedia.org/wiki/%D0%9A%D1%80%D0%B8%D1%82%D0%B5%D1%80%D0%B8%D0%B8_Debian_%D0%BF%D0%BE_%D0%BE%D0%BF%D1%80%D0%B5%D0%B4%D0%B5%D0%BB%D0%B5%D0%BD%D0%B8%D1%8E_%D1%81%D0%B2%D0%BE%D0%B1%D0%BE%D0%B4%D0%BD%D0%BE%D0%B3%D0%BE_%D0%BF%D1%80%D0%BE%D0%B3%D1%80%D0%B0%D0%BC%D0%BC%D0%BD%D0%BE%D0%B3%D0%BE_%D0%BE%D0%B1%D0%B5%D1%81%D0%BF%D0%B5%D1%87%D0%B5%D0%BD%D0%B8%D1%8F)**)
        -   **main** – пакеты полностью соответствуют критериям DFSG. Эти пакеты считаются частью дистрибуции Debian;
        -   **contrib** – пакеты тоже соответствуют критериям DFSG, но зависят от других пакетов, которые не соответствуют этим критериям;
        -   **non-free** – содержит всё остальное ПО, которое не соответствует DFSG.

### Классы релизов в Debian

Рассматривая выше ветки репозиториев Debian мы увидели следующее:

-   **bullseye**;
-   **bullseye-updates**;
-   **bullseye-security**.

Но, помимо кодовых имён версий системы, в названиях веток, можно использовать специальные классы релизов:

-   **stable** – ссылается на текущей стабильный репозиторий Debian, сейчас это **bullseye**. Как только выйдет новая версия Debian, то stable будет ссылаться на более новую версию;
-   **oldstable** – ссылается на предыдущий стабильный репозиторий;
-   **testing** – ссылается на специальную ветку репозитория для разработки нового стабильного релиза;
-   **unstable** – ссылается на самые свежие, но не протестированные пакеты;
-   **experimental** – здесь хранятся пакеты, которые только начали разрабатывать;
-   **backports** – ссылается на **testing** и **unstable**, но только для обновлений безопасности.

То есть вы можете изменить свои репозитории на testing, и быть на острие прогресса:

### Это только пример, существует большая вероятность что система очень скоро повредится из за непроверенных обновлений ###

deb http://deb.debian.org/debian/ testing main

deb-src http://deb.debian.org/debian/ testing main

deb http://security.debian.org/debian-security testing-security main

deb-src http://security.debian.org/debian-security testing-security main

deb http://deb.debian.org/debian/ testing-updates main

deb-src http://deb.debian.org/debian/ testing-updates main

## Файл Release

Каждый репозиторий, для каждой ветки содержит текстовый файл **Release**. Например, откройте в браузере репозиторий для Ubuntu: [http://ru.archive.ubuntu.com/ubuntu/](http://ru.archive.ubuntu.com/ubuntu/). Дальше откройте ветку (**dists**) для **jammy**. И здесь вы можете скачать и посмотреть файл **Release**.

![Файл Release в репозитории для Ubuntu Jammy](https://sysadminium.ru/wp-content/uploads/2022/05/image-38.png)

Файл Release в репозитории для Ubuntu Jammy

Он содержит информацию о данной ветке репозитория, например для **Ubuntu Jammy** файл содержит следующее:

Origin: Ubuntu

Label: Ubuntu

Suite: jammy

Version: 22.04

Codename: jammy

Date: Thu, 21 Apr 2022 17:16:08 UTC

Architectures: amd64 arm64 armhf i386 ppc64el riscv64 s390x

Components: main restricted universe multiverse

Description: Ubuntu Jammy 22.04

MD5Sum:

*** а здесь контрольные суммы для каждого пакета из репозитория ***

**Файл Release** – один из самых важных файлов для работы репозитория. Когда **пакетный менеджер** обновляет список пакетов, то он открывает адрес репозитория и читает этот файл. Если этого файла нет, то репозиторий будет помечен как неисправный и не будет использоваться.

## Архитектура пакетов

Если вы ещё раз посмотрите на файл **Release** в репозитории, то можете заметить там строчку:

Architectures: amd64 arm64 armhf i386 ppc64el riscv64 s390x

Здесь прописаны архитектуры пакетов, которые хранятся в репозитории. Прописывая источник репозитория, например в конфиге **/etc/apt/sources.list** вы можете указать определённую архитектуру, чтобы предотвратить скачивание и установку пакетов для других архитектур.

Это делается таким способом:

### Пример для Debian ###

deb [arch=amd64] http://deb.debian.org/debian/ bullseye main

## Вариант использования официальных репозиториев

Для того чтобы уменьшить вероятность поломки вашей системы из-за непроверенных обновлений, можно немного сократить количество репозиториев в системах Debian и в Ubuntu.

Вообще в Debian дан список самых безопасных репозиториев по умолчанию. Можем лишь закомментировать репозитории с исходниками, так как скорее всего вам они пока не понадобятся. Напомню, что такие строки начинаются со слова **deb-src**. А если понадобятся вы их просто раскомментируете. После правки у нас осталось 3 источника пакетов:

alex@deb:~$ egrep -v '^#|^$' /etc/apt/sources.list | cat -n

1 deb http://deb.debian.org/debian/ bullseye main

2 deb http://security.debian.org/debian-security bullseye-security main

3 deb http://deb.debian.org/debian/ bullseye-updates main

Ubuntu при установке прописала намного больше своих репозиториев. Но их тоже можно свести к трем. Например, я считаю нужным отключить **universe**, **multiverse** и **jammy-backports** репозитории на сервере. После правки список репозиториев также состоит из 3-ёх строк:

alex@ubu:~$ egrep -v '^#|^$' /etc/apt/sources.list | cat -n

1 deb http://ru.archive.ubuntu.com/ubuntu jammy main restricted

2 deb http://ru.archive.ubuntu.com/ubuntu jammy-updates main restricted

3 deb http://ru.archive.ubuntu.com/ubuntu jammy-security main restricted

Чтобы применить изменения, выполните на обоих системах команду:

$ sudo apt update

Эта команда подключится к каждому репозиторию, посмотрит какие пакеты можно обновить и из каких источников. И сохранит локальных кэш. После выполнения этой команды система будет знать какие пакеты из каких репозиториев можно получить, а также версии этих пакетов. Но если в репозиторий добавят более новую версию какого-нибудь пакета, то система об этом узнает лишь после следующего выполнения этой команды.

А если хотите обновить систему, то выполните команду:

$ sudo apt upgrade

Эта команда уже скачивает все обновления и устанавливает их.

Кстати утилита **apt** – это и есть **менеджер пакетов**. Рассмотрим её и другие менеджеры пакетов в следующих статьях.

## Добавление сторонних репозиториев

### Источник репозитория

Некоторые разработчики создают свои собственные репозитории. Например у веб сервера **Nginx** есть свой репозиторий для разных систем, в том числе для **Debian** и **Ubuntu**. Вот его адрес: [https://nginx.org/packages/mainline/](https://nginx.org/packages/mainline/).

Добавлять репозитории можно в основной конфиг: **/etc/apt/sources.list** или создавать отдельные конфиги в каталоге **/etc/apt/sources.list.d/**. Сам я считаю что правильнее для каждого стороннего репозитория создавать отдельные конфиги.

Например, чтобы подключить репозиторий **nginx** создайте следующий конфиг. Для **Debian**:

alex@deb:~$ sudo nano /etc/apt/sources.list.d/nginx.list

deb http://nginx.org/packages/debian bullseye nginx

Или для **Ubutnu**:

alex@ubu:~$ sudo nano /etc/apt/sources.list.d/nginx.list

deb http://nginx.org/packages/ubuntu jammy nginx

### Приоритет репозитория

Допустим, мы прописали дополнительный репозиторий для **nginx**, но как системе понять из какого репозитория брать пакет для установки? Ведь пакеты для nginx есть и в системном репозитории и в репозитории от самого nginx. Чтобы ответить на этот вопрос придумали приоритеты репозиториев.

Чтобы задать приоритет репозитория нужно создать файл **/etc/apt/preferences.d/XX<имя_репозитория>**, где XX это номер файла, чем он выше, тем обработается позднее, то есть будет иметь приоритет над другими настройками.

По нашему примеру для nginx нужно создать следующий файл:

$ sudo nano /etc/apt/preferences.d/99nginx

Package: *

Pin: origin nginx.org

Pin: release o=nginx

Pin-Priority: 900

Разберём написанное:

-   **Package**: имя пакета. Можно поставить знак * чтобы применить приоритет для всех пакетов из этого репозитория. Также можно указать несколько имён через пробел;
-   **Pin**: опции прикрепления. Существует много опций, я разберу лишь некоторые:
    -   **origin** “имя автора или поставщика”;
    -   **release o=nginx** – означает что в файле Release репозитория есть поставщик (Origin = o) с именем nginx;
    -   **release l=Debian** – означает что в файле Release репозитория есть Label (l) с именем Debian;
-   **Pin-Priority**: приоритет.

То-есть **Package** и **Pin** это условия для назначения приоритета, а **Pin-Priority** это действие (назначение приоритета). В нашем примере получается следующее: если имя пакета любое, но владелец репозитория **nginx.org** и в файле **Release** прописано “**Origin: nginx**“, то для таких пакетов ставим приоритет 900.

Приоритет может быть в следующих диапазонах:

-   **P >= 1000** – пакет будет установлен из этого репозитория, даже если это приведет к понижению версии уже установленного пакета;
-   **990 <= P < 1000** – пакет будет установлен из этого репозитория, если не установлена более новая версия;
-   **500 <= P < 990** – пакет будет установлен, если нет пакета принадлежащего к **целевому выпуску** или не установлена более новая версия;
-   **100 <= P < 500** – пакет будет установлен, если нет кандидатов из других репозиториев или установленного пакета более новой версии;
-   **0 < P < 100** – пакет будет установлен, только если он ещё не установлен (любой версии) и если нет кандидатов из других репозиториев;
-   **P < 0** – пакет не будет установлен ни при каких условиях;
-   **P = 0** – не используется.

#### Целевой выпуск

Приоритеты с 500 по 990 и с 990 по 1000 очень похожи. Чтобы их отличить нужно понять что такое **целевой выпуск**. Для Ubuntu или Debian это название версии дистрибутива. Например для Ubuntu – **jammy**, а для Debian – **bullseye**. Но это имя ещё нужно задать таким способом:

alex@deb:~$ sudo nano /etc/apt/apt.conf.d/default

APT::Default-Release "bullseye";

alex@ubu:~$ sudo nano /etc/apt/apt.conf.d/default

APT::Default-Release "jammy";

### Открытый ключ репозитория

И так, репозиторий мы добавили, приоритет настроили. Давайте попробуем применить изменения:

alex@deb:~$ sudo apt update

Пол:1 http://nginx.org/packages/debian bullseye InRelease [2 860 B]

Ошб:1 http://nginx.org/packages/debian bullseye InRelease

Следующие подписи не могут быть проверены, так как недоступен открытый ключ: NO_PUBKEY ABF5BD827BD9BF62

Пол:2 http://security.debian.org/debian-security bullseye-security InRelease [44,1 kB]

Сущ:3 http://deb.debian.org/debian bullseye InRelease

Пол:4 http://deb.debian.org/debian bullseye-updates InRelease [39,4 kB]

Чтение списков пакетов… Готово

W: Ошибка GPG: http://nginx.org/packages/debian bullseye InRelease: Следующие подписи не могут быть проверены, так как недоступен открытый ключ: NO_PUBKEY ABF5BD827BD9BF62

E: Репозиторий «http://nginx.org/packages/debian bullseye InRelease» не подписан.

N: Обновление из этого репозитория нельзя выполнить безопасным способом, поэтому по умолчанию он отключён.

N: Информацию о создании репозитория и настройках пользователя смотрите в справочной странице apt-secure(8).

И тут мы видим ошибку, что нам не хватает открытого ключа. Я привёл пример для Debian, но в Ubuntu будет подобная ситуация. Дело в том что современные репозитории шифруются с помощью закрытого ключа и чтобы его использовать, нам нужно установить в систему открытый ключ.

Чтобы это сделать, вначале установим необходимые инструменты:

### Для Debian ###

alex@deb:~$ sudo apt install curl gnupg2 ca-certificates lsb-release debian-archive-keyring

### Для Ubuntu ###

alex@ubu:~$ sudo apt install curl gnupg2 ca-certificates lsb-release ubuntu-keyring

**Внимание!** Утилита **gnupg2** для **Ubuntu** доступна только в репозитории **universe**, если вы закомментировали эти источники, то разкомментируете их. Это строчки:
deb http://ru.archive.ubuntu.com/ubuntu jammy universe
deb http://ru.archive.ubuntu.com/ubuntu jammy-updates universe
И не забудьте применить изменения, выполнив:
$ sudo apt update

А дальше скачаем открытый ключ (с помощью **wget**):

$ wget https://nginx.org/keys/nginx_signing.key

Дальше выполняется конвейер команд, это когда вывод одной команды идет на вход другой команде. Такие конвейеры мы будем проходить позже. Но всё равно постараюсь объяснить следующую команду. С помощью **cat** мы читаем файл ключа, и прочитанное передаём утилите gpg. Утилита **gpg** переводит прочитанное в необходимый формат и передаёт вывод уже следующей команде tee. Утилита **tee** (под sudo) сохраняет полученный текст в файл. В конце добавляем **>/dev/null**, чтобы не было никакого вывода на терминал. Вот сама команда:

$ cat nginx_signing.key | gpg --dearmor | sudo tee /usr/share/keyrings/nginx-archive-keyring.gpg >/dev/null

Вот так мы получили открытый ключ, который теперь хранится в файле **/usr/share/keyrings/nginx-archive-keyring.gpg**.

Или можно сразу в одной команде скачать файл (с помощью **wget** или **curl**) и сохранить ключ в формате **gpg**:

### Пример для wget ###

$ wget -O- https://nginx.org/keys/nginx_signing.key | gpg --dearmor | sudo tee /usr/share/keyrings/nginx-archive-keyring.gpg >/dev/null

### Пример для curl ###

$ curl https://nginx.org/keys/nginx_signing.key | gpg --dearmor | sudo tee /usr/share/keyrings/nginx-archive-keyring.gpg >/dev/null

После этого необходимо указать в конфиге репозитория, что нужно использовать этот ключ. Для этого в конфиге репозитория между deb и адресом репозитория вставляем **[signed-by=/путь/к/ключу]**:

alex@deb:~$ sudo nano /etc/apt/sources.list.d/nginx.list

deb [signed-by=/usr/share/keyrings/nginx-archive-keyring.gpg] http://nginx.org/packages/debian bullseye nginx

И пробуем ещё раз применить изменения:

alex@deb:~$ sudo apt update

Сущ:1 http://security.debian.org/debian-security bullseye-security InRelease

Сущ:2 http://deb.debian.org/debian bullseye InRelease

Сущ:3 http://deb.debian.org/debian bullseye-updates InRelease

Пол:4 http://nginx.org/packages/debian bullseye InRelease [2 860 B]

Пол:5 http://nginx.org/packages/debian bullseye/nginx amd64 Packages [7 633 B]

Получено 7 633 B за 1с (9 420 B/s)

Чтение списков пакетов… Готово

Построение дерева зависимостей… Готово

Чтение информации о состоянии… Готово

Все пакеты имеют последние версии.

На этот раз всё прошло успешно.

Кстати, если вы хотите в источнике пакетов прописать архитектуру и открытый ключ, то это делается через пробел:

### Пример ###

deb [arch=amd64 signed-by=/usr/share/keyrings/nginx-archive-keyring.gpg] http://nginx.org/packages/debian bullseye nginx

### Проверка добавленного репозитория

Ну и чтобы понять из какого репозитория будет установлен пакет, выполните команду:

alex@deb:~$ apt-cache policy nginx

nginx:

Установлен: (отсутствует)

Кандидат: 1.20.2-1~bullseye

Таблица версий:

1.20.2-1~bullseye 990

990 http://nginx.org/packages/debian bullseye/nginx amd64 Packages

1.20.1-1~bullseye 990

990 http://nginx.org/packages/debian bullseye/nginx amd64 Packages

1.18.0-6.1 990

990 http://deb.debian.org/debian bullseye/main amd64 Packages

Из вывода становится ясно что пакет **nginx** ещё не установлен в систему, а кандидатом на установку является пакет с версией **1.20.2** из репозитория **http://nginx.org/packages/debian**. Приоритет у всех пакетов, кстати стал равным = **990**. Это произошло после того, как мы установили целевой выпуск = **bullseye**. Так как все репозитории относятся к этому выпуску, то на назначенный мною приоритет система перестала смотреть, а назначила репозиториям для **bullseye** такой приоритет.

## Итог

Мы узнали что такое репозитории. Узнали что есть официальные репозитории для системы и они прописываются в конфиг **/etc/apt/sources.list**. А также есть сторонние репозитории и для них лучше создавать свои конфиги в каталоге **/etc/apt/sources.list.d/**.

Научились добавлять сторонний репозиторий на примере **nginx**. Узнали про **приоритеты** репозиториев и открытые **ключи**. А также узнали что такое **целевой выпуск**.

Полезные источники:

-   Очень хорошо про приоритеты и повышение или понижение версий пакетов написано [здесь](https://interface31.ru/tech_it/2016/03/ispolzuem-apt-pinning-dlya-zakrepleniya-paketov-v-debian-ubuntu.html).
-   [Инструкция для подключения nginx репозитория в различные системы](https://nginx.org/ru/linux_packages.html).
-   Существует [Репозиторий от Yandex](https://mirror.yandex.ru/) – он хранит ветки для большинства систем Linux.
-   [Репозиторий Ubuntu](http://ru.archive.ubuntu.com/ubuntu/)
-   [Репозиторий Debian](https://deb.debian.org/debian/)