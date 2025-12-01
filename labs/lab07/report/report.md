---
## Front matter
title: "Отчёт по лабораторной работе №7"
subtitle: " Расширенные настройки межсетевого экрана"
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

Получить навыки настройки межсетевого экрана в Linux в части переадресации портов и настройки Masquerading.

# Выполнение

## Создание пользовательской службы Firewalld

На сервере создан пользовательский файл службы **ssh-custom.xml**, основанный на стандартном файле `ssh.xml`.  
   После выполнения команды файл был помещён в каталог `/etc/firewalld/services`.

Просмотр содержимого файла показал стандартную структуру XML описания службы.

   ![Просмотр содержимого ssh-custom.xml](Screenshot_1.png){ #fig:001 width=80% }

Файл содержит следующие элементы:

- **XML-заголовок** определяет версию и кодировку документа.  
- **service** — корневой элемент, описывающий службу Firewalld.  
- **short** — краткое название службы.  
- **description** — текстовое описание назначения службы.  
- **port** — определяет порт и протокол, который должен быть открыт.

В файле изменён порт с 22 на **2022**, а описание дополнено указанием, что это модифицированная служба.

![Редактирование ssh-custom.xml](Screenshot_2.png){ #fig:002 width=80% }

Вывод списка доступных служб показал, что новая служба **ssh-custom** пока отсутствует.

![Список служб Firewalld](Screenshot_3.png){ #fig:003 width=80% }

Добавление службы в активный список позволило включить её поддержку межсетевым экраном.

## Перенаправление портов

Настроено перенаправление входящих соединений с порта 2022 на 22.

![Добавление forward-порта](Screenshot_4.png){ #fig:004 width=80% }

Подключение с клиента через порт **2022** прошло успешно, что подтвердило корректность настроек.

![Подключение по SSH на порт 2022](Screenshot_5.png){ #fig:005 width=80% }

## Настройка Port Forwarding и Masquerading

Проверка системных параметров показала, что пересылка IPv4-пакетов была отключена.  
Создан конфигурационный файл, включающий forwarding, и активировано правило Masquerading.

![Проверка forwarding](Screenshot_6.png){ #fig:006 width=80% }

Клиент успешно получил доступ к внешним ресурсам.

![Проверка доступа в Интернет](Screenshot_7.png){ #fig:007 width=80% }

##  Внесение изменений в настройки внутреннего окружения виртуальной машины

Создана директория `firewall` с подкаталогами для конфигурационных файлов Firewalld и системных параметров.  
В неё перенесены файлы `ssh-custom.xml` и `90-forward.conf`.

![Копирование файлов для provisioning](Screenshot_8.png){ #fig:008 width=80% }

В каталоге provisioning создан файл `firewall.sh`, предназначенный для автоматической настройки виртуальной машины.

![Скрипт firewall.sh](Screenshot_9.png){ #fig:009 width=80% }

# Контрольные вопросы

1. **Где хранятся пользовательские файлы firewalld?**
   Пользовательские конфигурационные файлы Firewalld размещаются в каталоге:
   **/etc/firewalld/**
   В частности, пользовательские службы хранятся в:
   **/etc/firewalld/services/**

2. **Какую строку надо включить в пользовательский файл службы, чтобы указать порт TCP 2022?**
   Внутри XML-файла службы необходимо указать строку вида:

   ```xml
   <port protocol="tcp" port="2022"/>
   ```

3. **Какая команда позволяет вам перечислить все службы, доступные в настоящее время на вашем сервере?**
   Для вывода всех доступных служб Firewalld используется команда:
   **firewall-cmd --get-services**

4. **В чём разница между трансляцией сетевых адресов (NAT) и маскарадингом (masquerading)?**

   * **NAT** — это общий механизм трансляции адресов, позволяющий изменять исходный или конечный IP-адрес и/или порт при прохождении пакетов через маршрутизатор.
   * **Masquerading** — это разновидность SNAT, при которой исходящий IP-адрес автоматически подменяется адресом интерфейса, через который идёт трафик. Обычно используется при динамически назначаемых адресах (например, PPPoE).

   Иначе говоря, masquerading — это частный случай NAT.

5. **Какая команда разрешает входящий трафик на порт 4404 и перенаправляет его в службу SSH по IP-адресу 10.0.0.10?**
   Команда перенаправления входящего TCP-трафика с порта 4404 на SSH другой машины выглядит так:

   ```
   firewall-cmd --add-forward-port=port=4404:proto=tcp:toaddr=10.0.0.10:toport=22
   ```

6. **Какая команда используется для включения маскарадинга IP-пакетов для всех пакетов, выходящих в зону public?**
   Для включения masquerading в зоне **public** применяется команда:

   ```
   firewall-cmd --zone=public --add-masquerade
   ```

   Для постоянного применения правила:

   ```
   firewall-cmd --zone=public --add-masquerade --permanent
   ```

# Заключение

В ходе работы была создана пользовательская служба Firewalld, настроено перенаправление порта и включён masquerading. Удалось проверить подключение по новому порту и обеспечить доступ клиента во внешнюю сеть. Выполненные действия подтвердили корректность работы сетевых механизмов и позволили закрепить навыки конфигурации межсетевого экрана и системных параметров в Rocky Linux.
