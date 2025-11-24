---
## Front matter
title: "Отчёт по лабораторной работе №5"
subtitle: "Расширенная настройка HTTP-сервера Apache"
author: "Владимир Базлов"

## Generic otions
lang: ru-RU
toc-title: "Содержание"

## Bibliography
bibliography: bib/cite.bib
csl: pandoc/csl/gost-r-7-0-5-2008-numeric.csl

## Pdf output format
toc: true
toc-depth: 2
lof: true
lot: true
fontsize: 12pt
linestretch: 1.5
papersize: a4
documentclass: scrreprt
## I18n polyglossia
polyglossia-lang:
  name: russian
  options:
    - spelling=modern
    - babelshorthands=true
polyglossia-otherlangs:
  name: english
## I18n babel
babel-lang: russian
babel-otherlangs: english
## Fonts
mainfont: IBM Plex Serif
romanfont: IBM Plex Serif
sansfont: IBM Plex Sans
monofont: IBM Plex Mono
mathfont: STIX Two Math
mainfontoptions: Ligatures=Common,Ligatures=TeX,Scale=0.94
romanfontoptions: Ligatures=Common,Ligatures=TeX,Scale=0.94
sansfontoptions: Ligatures=Common,Ligatures=TeX,Scale=MatchLowercase,Scale=0.94
monofontoptions: Scale=MatchLowercase,Scale=0.94,FakeStretch=0.9
mathfontoptions:
## Biblatex
biblatex: true
biblio-style: "gost-numeric"
biblatexoptions:
  - parentracker=true
  - backend=biber
  - hyperref=auto
  - language=auto
  - autolang=other*
  - citestyle=gost-numeric
## Pandoc-crossref LaTeX customization
figureTitle: "Рис."
tableTitle: "Таблица"
listingTitle: "Листинг"
lofTitle: "Список иллюстраций"
lotTitle: "Список таблиц"
lolTitle: "Листинги"
## Misc options
indent: true
header-includes:
  - \usepackage{indentfirst}
  - \usepackage{float}
  - \floatplacement{figure}{H}
---

# Цель работы

Приобретение практических навыков по расширенному конфигурированию HTTPсервера Apache в части безопасности и возможности использования PHP.

# Выполнение

## Конфигурирование HTTP-сервера для работы через протокол HTTPS

1. После загрузки виртуальной машины выполнен переход в рабочий каталог проекта и запуск виртуальной машины *server*.

2. На виртуальной машине выполнен вход под пользователем, затем выполнен переход в режим суперпользователя.

3. В каталоге */etc/ssl* создан каталог *private* и символьная ссылка на него.
   Затем выполнен переход в */etc/pki/tls/private* и создан ключ и самоподписанный сертификат для домена *[www.vabazlov.net](http://www.vabazlov.net)*.

   ![Генерация SSL-сертификата](Screenshot_1.png){ #fig:001 width=80% }

   При генерации сертификата заполнены все необходимые поля: страна — RU, регион — Russia, город — Moscow, организация и подразделение — *vabazlov*, доменное имя — *vabazlov.net*, e-mail — *[vabazlov@vabazlov.net](mailto:vabazlov@vabazlov.net)*.

4. Сгенерированный сертификат перемещён в каталог */etc/pki/tls/certs*.

5. Выполнено редактирование файла */etc/httpd/conf.d/[www.vabazlov.net.conf](http://www.vabazlov.net.conf)*, настроены два виртуальных хоста — на порту 80 и на порту 443.

   ![Конфигурация виртуальных хостов](Screenshot_2.png){ #fig:002 width=80% }

### Пояснение параметров конфигурации

**Виртуальный хост на порту 80 (HTTP):**
— указаны адрес администратора сайта, рабочая директория сайта, основные имена домена;
— настроены файлы журналов ошибок и доступа;
— включён механизм переписывания URI;
— настроено принудительное перенаправление всех запросов с HTTP на HTTPS.

**Виртуальный хост на порту 443 (HTTPS):**
— включён SSL-движок;
— настроены те же параметры домена и журналов;
— подключён файл сертификата и приватный ключ;
— весь HTTPS-трафик обслуживается с использованием ранее созданного сертификата.

6. В межсетевом экране разрешён доступ к сервису HTTPS.
   Работа с брандмауэром завершилась успешно.

   ![Настройка firewall](Screenshot_3.png){ #fig:003 width=80% }

7. Веб-сервер Apache перезапущен.

8. При открытии сайта *[www.vabazlov.net](http://www.vabazlov.net)* в браузере выполнено корректное автоматическое перенаправление с HTTP на HTTPS.

   ![Перенаправление на HTTPS](Screenshot_4.png){ #fig:004 width=80% }

   После добавления сайта в исключения браузера был просмотрен сертификат:

   ![Просмотр сертификата](Screenshot_5.png){ #fig:005 width=80% }

## Конфигурирование HTTP-сервера для работы с PHP

1. Установлены необходимые пакеты PHP.

   ![Установка PHP](Screenshot_6.png){ #fig:006 width=80% }

2. В каталоге веб-сайта *[www.vabazlov.net](http://www.vabazlov.net)* файл *index.html* заменён на *index.php*, предназначенный для вывода полной информации о конфигурации PHP.

3. Права на каталог */var/www* изменены так, чтобы владелец и группа соответствовали пользователю *apache*.

4. В ОС настроены корректные контексты SELinux для каталогов */etc* и */var/www*.

5. Apache перезапущен.

6. На клиентской машине в браузере подтверждена корректная работа PHP — открылась страница с информацией о версии PHP.

   ![Работа phpinfo()](Screenshot_7.png){ #fig:007 width=80% }

## Внесение изменений во внутреннее окружение виртуальной машины

1. С сервера в директорию *vagrant provisioning* скопированы:
   — конфигурационные файлы Apache,
   — структура веб-каталогов,
   — каталоги с сертификатом и приватным ключом.

   ![Копирование конфигурации для provisioning](Screenshot_8.png){ #fig:008 width=80% }

2. В скрипт */vagrant/provision/server/http.sh* добавлены строки для установки PHP, настройки firewall и разрешения HTTPS-трафика.

   ![Изменённый provisioning-скрипт](Screenshot_9.png){ #fig:009 width=80% }

# Контрольные вопросы

1. **В чём отличие HTTP от HTTPS?**
   HTTP — это протокол передачи данных без шифрования, работающий в открытом виде. Он не обеспечивает защиту от перехвата, подмены или чтения данных.
   HTTPS — это расширение HTTP, использующее шифрование с помощью протокола TLS. HTTPS обеспечивает защищённый канал связи между клиентом и сервером, предотвращая перехват и модификацию передаваемых данных.

2. **Каким образом обеспечивается безопасность контента веб-сервера при работе через HTTPS?**
   Безопасность достигается с помощью TLS-шифрования. При установлении соединения клиент и сервер выполняют криптографический обмен, формируя общий сеансовый ключ. Данные, передаваемые между клиентом и сервером, шифруются, что защищает их от прочтения и подмены злоумышленниками. Дополнительно сертификат подтверждает подлинность сервера.

3. **Что такое сертификационный центр? Приведите пример.**
   Сертификационный центр — это доверенная организация, которая выпускает цифровые сертификаты. Она проверяет владельца домена или организации и удостоверяет подлинность ключей, используемых для HTTPS.
   Примеры сертификационных центров: *Let’s Encrypt*, *DigiCert*, *GlobalSign*, *Sectigo*.

# Заключение

В ходе работы был настроен веб-сервер для безопасной передачи данных по HTTPS, создан самоподписанный сертификат и выполнена корректная конфигурация виртуальных хостов. Обеспечена работа PHP, настроены права доступа и контексты SELinux. Изменения сохранены в provisioning-окружении, что обеспечивает автоматическое развёртывание сервера с поддержкой HTTPS и PHP в дальнейшем.
