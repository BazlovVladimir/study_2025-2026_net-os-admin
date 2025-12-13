---
## Front matter
title: "Отчёт по лабораторной работе №15"
subtitle: "Настройка сетевого журналирования"
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

Получение навыков по работе с журналами системных событий.

# Выполнение

## Настройка сервера сетевого журнала

1. Создание конфигурационного файла для сетевого приёма логов

На сервере в каталоге `/etc/rsyslog.d` создан файл `netlog-server.conf` и в него добавлены строки, включающие модуль `imtcp` и запуск TCP-сервера на порту 514.

![Настройка netlog-server.conf](Screenshot_1.png){ #fig:001 width=80% }

2. Перезапуск rsyslog и проверка прослушиваемых портов

Служба rsyslog перезапущена, после чего выполнена проверка прослушиваемых TCP-портов. Порт 514 находится в состоянии LISTEN, что подтверждает успешную активацию сетевого приёма.

![Прослушивание порта rsyslog](Screenshot_2.png){ #fig:002 width=80% }

3. Настройка межсетевого экрана

На сервере открыт TCP-порт 514 для временного и постоянного использования. Настройки firewall применены успешно.

![Открытие порта 514/tcp](Screenshot_2.png){ #fig:003 width=80% }

## Настройка клиента сетевого журнала

1. Создание конфигурационного файла

На клиентской системе в каталоге `/etc/rsyslog.d` создан файл `netlog-client.conf`.

2. Добавление правила пересылки сообщений

В файл добавлена строка, перенаправляющая все сообщения журнала на IP-адрес сервера по TCP-порту 514.

![Конфигурация netlog-client.conf](Screenshot_3.png){ #fig:004 width=80% }

3. Перезапуск rsyslog

Служба rsyslog перезапущена для применения изменений.

## Просмотр журналов

1. Просмотр системных сообщений

На сервере выполнено наблюдение за журналом `/var/log/messages`. В выводе отражаются сообщения как от сервера, так и от клиента.

![Просмотр логов messages](Screenshot_4.png){ #fig:005 width=80% }

2. Графический просмотрщик журналов

Пользовательский инструмент `gnome-system-monitor` успешно запущен.

![Графический просмотр логов](Screenshot_5.png){ #fig:006 width=80% }

3. Использование утилиты lnav

На сервер установлен пакет `lnav`, после чего выполнен просмотр объединённых логов.

## Внесение изменений в настройки внутреннего окружения виртуальных машин

1. Подготовка структуры каталогов и копирование файлов

В каталоге `/vagrant/provision/server/` создана структура `netlog/etc/rsyslog.d`, после чего туда помещён файл `netlog-server.conf`.

![Копирование конфигурации для внутреннего окружения](Screenshot_6.png){ #fig:007 width=80% }

2. Создание provisioning-скрипта netlog.sh

В каталоге `/vagrant/provision/server` создан исполняемый файл `netlog.sh`, содержащий команды копирования конфигурации, настройки SELinux, открытия порта в firewall и перезапуска rsyslog.

![Содержимое скрипта netlog.sh (server)](Screenshot_7.png){ #fig:008 width=80% }

Также создан аналогичный скрипт для клиента, включающий установку `lnav` и копирование клиентской конфигурации.

![Содержимое скрипта netlog.sh (client)](Screenshot_8.png){ #fig:009 width=80% }

# Контрольные вопросы

1. **Какой модуль rsyslog используется для приёма сообщений от journald?**
   Для получения сообщений от системного журнала journald используется модуль **imjournal**. Он обеспечивает прямое взаимодействие rsyslog с журналом systemd.

2. **Как называется устаревший модуль, который можно использовать для приёма сообщений journald?**
   Устаревшим модулем является **imuxsock**. Он работает через Unix-сокеты и использовался до появления imjournal.

3. **Какой параметр позволяет убедиться, что устаревший метод приёма journald не используется?**
   Следует добавить параметр **`UseSyslogSocket=no`** в конфигурацию journald. Это отключает передачу сообщений через сокет `/run/systemd/journal/syslog`.

4. **В каком конфигурационном файле находятся настройки, управляющие работой журнала systemd?**
   Основной файл конфигурации journald — **`/etc/systemd/journald.conf`**.

5. **Какой параметр управляет пересылкой сообщений из journald в rsyslog?**
   Пересылка управляется параметром **`ForwardToSyslog=`**, который может быть включён (yes) или выключен (no).

6. **Какой модуль rsyslog используется для чтения сообщений из произвольного файлового журнала, не созданного rsyslog?**
   Для этого применяется модуль **imfile**, позволяющий отслеживать содержимое файлов и пересылать его в rsyslog.

7. **Какой модуль rsyslog используется для пересылки сообщений в базу данных MariaDB?**
   Для записи логов в MariaDB используется модуль **ommysql**.

8. **Какие две строки нужно включить в rsyslog.conf для приёма сообщений по TCP?**
   Требуются строки:

   ```
   $ModLoad imtcp
   $InputTCPServerRun 514
   ```

   Первая загружает модуль TCP-входа, вторая запускает сервер на порту 514.

9. **Как настроить локальный брандмауэр, чтобы разрешить приём сообщений через TCP-порт 514?**
   Необходимо открыть порт с помощью двух команд firewalld:

   ```
   firewall-cmd --add-port=514/tcp
   firewall-cmd --add-port=514/tcp --permanent
   ```

   После этого требуется перезагрузить правила или перезапустить firewalld.

# Заключение

В ходе выполнения работы была настроена система сетевого журналирования с использованием rsyslog. На сервере активирован приём сообщений по TCP-порту 514, настроены модули для обработки входящих логов и открыт необходимый порт в брандмауэре. Клиентская машина успешно перенаправляет свои журнальные сообщения на сервер, что подтверждено просмотром логов в системных файлах и с помощью инструментов анализа, таких как `gnome-system-monitor` и `lnav`.
