---
## Front matter
title: "Отчёт по лабораторной работе №2"
subtitle: "Настройка DNS-сервера"
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

Приобретение практических навыков по установке и конфигурированию DNSсервера, усвоение принципов работы системы доменных имён.

# Выполнение

## Установка и настройка кэширующего DNS-сервера BIND

1. Загружена операционная система и выполнен переход в каталог проекта.

2. Виртуальная машина *server* успешно запущена средствами Vagrant.

3. Выполнен вход под пользователем и произведён переход в режим суперпользователя.

4. На виртуальную машину установлены пакеты BIND и bind-utils.

5. Проверена работа внешних DNS-серверов при помощи утилиты `dig`.  
   На экран выводятся секции запроса: QUESTION и ANSWER.  
   ANSWER содержит несколько A-записей — IP-адреса ресурса. Запрос отправляется на DNS, указанный в `resolv.conf`.

   ![Запрос к внешнему DNS — dig www.yandex.ru](Screenshot_1.png){ #fig:001 width=85% }

Проанализированы файлы конфигурации:

- `/etc/resolv.conf` — настройки DNS резолвера для системы;
- `/etc/named.conf` — основной конфигурационный файл службы BIND;
- `/var/named/named.ca` — содержит список корневых DNS-серверов;
- `/var/named/named.localhost` и `/var/named/named.loopback` — файлы локальных зон.

Файл `/etc/named.conf` содержит параметры работы сервера: интерфейсы прослушивания, разрешение запросов, пути хранения файлов.

![Файл /etc/named.conf](Screenshot_2.png){ #fig:002 width=85% }

Файл `/var/named/named.ca` содержит список корневых name-server.

![Файл /var/named/named.ca](Screenshot_3.png){ #fig:003 width=85% }

Файлы локальных зон содержат записи localhost и loopback.

![Файлы named.localhost и named.loopback](Screenshot_4.png){ #fig:004 width=85% }

DNS-сервер запущен и добавлен в автозапуск.

Был выполнен сравнительный DNS-запрос:

- `dig www.yandex.ru` — запрос отправляется на внешний DNS-сервер;
- `dig @127.0.0.1 www.yandex.ru` — запрос направляется на локальный BIND.

В выводе заметно отличие: при обращении к локальному серверу увеличено время выполнения первого запроса, так как он формирует кэш.

![Сравнение запросов dig www.yandex.ru и dig @127.0.0.1](Screenshot_5.png){ #fig:005 width=85% }

В настройках интерфейса `eth0` изменён используемый DNS-сервер — теперь это `127.0.0.1`.  
После сохранения конфигурации файл `/etc/resolv.conf` обновился.

![Изменение resolv.conf через nmcli](Screenshot_6.png){ #fig:006 width=85% }

В `/etc/named.conf` внесены изменения:

- сервер начинает слушать все интерфейсы, а не только `127.0.0.1`,
- разрешены DNS-запросы от адресов внутренней сети (`192.168.0.0/16`).


![Изменения allow-query и listen-on](Screenshot_7.png){ #fig:007 width=85% }

Для разрешения DNS-трафика в firewall добавлена служба DNS.  
Проверено, что процесс `named` слушает порт 53 по UDP — запросы теперь проходят через локальный DNS-сервер.

![DNS слушает порт 53 (UDP)](Screenshot_8.png){ #fig:008 width=85% }

## Конфигурирование первичного DNS-сервера

В каталог `/etc/named` был скопирован шаблон `named.rfc1912.zones` и переименован в `vabazlov.net`.  
Файл включён в основную конфигурацию `/etc/named.conf` через строку:

`include "/etc/named/vabazlov.net";`

В файле заменены стандартные локальные зоны на зоны домена `vabazlov.net`:

- прямая зона: `vabazlov.net`
- обратная зона: `1.168.192.in-addr.arpa`

![Файл конфигурации зон vabazlov.net](Screenshot_9.png){ #fig:009 width=85% }

В `/var/named` созданы каталоги:

- `master/fz` — файлы прямой зоны
- `master/rz` — файлы обратной зоны

Файлы шаблонов были скопированы и переименованы.

Файл **/var/named/master/fz/vabazlov.net** был изменён:

- задан домен `vabazlov.net`
- указан DNS-сервер `server.vabazlov.net`
- записи типа `A` указывают на адрес `192.168.1.1`
- добавлена запись сервера `ns`


![Файл прямой зоны vabazlov.net](Screenshot_10.png){ #fig:010 width=85% }

Файл **/var/named/master/rz/192.168.1** был изменён:

- домен обратной зоны `1.168.192.in-addr.arpa.`
- запись `PTR` сопоставляет IP-адресу доменное имя


![Файл обратной зоны 192.168.1](Screenshot_11.png){ #fig:011 width=85% }

После создания файлов были настроены права доступа и восстановлены SELinux-метки.  
После перезапуска службы BIND сервер загрузил новые зоны:

![Перезапуск named и загрузка зон](Screenshot_12.png){ #fig:012 width=85% }


Запрос через `dig` для `ns.vabazlov.net` возвращает A-запись с IP-адресом сервера:

![Проверка dig](Screenshot_13.png){ #fig:013 width=85% }

Команды `host` подтверждают корректную работу прямой и обратной зоны:

![Проверки host](Screenshot_14.png){ #fig:014 width=85% }

## Подготовка конфигурации для автоматического развёртывания

В каталоге `/vagrant/provision/server/dns` создана структура:

- `/etc/named/` — конфигурационные файлы
- `/var/named/master/` — зоны

В каталоге создан provisioning-скрипт `dns.sh`.

![Копирование конфигурации в Vagrant окружение](Screenshot_15.png){ #fig:016 width=85% }

Содержимое `dns.sh` включает:

- установку пакетов
- копирование конфигураций
- настройку SELinux
- применение DNS-настроек к `eth0`

![dns.sh — provisioning-script](Screenshot_16.png){ #fig:017 width=85% }

# Контрольные вопросы

1. **Что такое DNS?**  
   DNS — это распределённая система доменных имён, которая преобразует удобные для человека доменные имена (например, `example.com`) в IP-адреса, используемые сетевыми устройствами.

2. **Каково назначение кэширующего DNS-сервера?**  
   Кэширующий DNS-сервер сохраняет ответы на DNS-запросы, сокращая время обработки повторных запросов и уменьшая нагрузку на внешние DNS-серверы.

3. **Чем отличается прямая DNS-зона от обратной?**  
   Прямая зона сопоставляет доменные имена IP-адресам (A-записи), тогда как обратная зона сопоставляет IP-адреса доменным именам (PTR-записи).

4. **В каких каталогах и файлах располагаются настройки DNS-сервера? Кратко охарактеризуйте, за что они отвечают.**  
   Основные каталоги: `/etc/named.conf` — главный конфигурационный файл, `/etc/named/` — настройки зон, `/var/named/` — файлы DNS-зон. Файлы определяют поведение сервера, зоны доменов и записи.

5. **Что указывается в файле resolv.conf?**  
   В `resolv.conf` указываются DNS-серверы, которые используются системой для выполнения DNS-запросов.

6. **Какие типы записи описания ресурсов есть в DNS и для чего они используются?**  
   Основные: `A` — соответствие имени IP-адресу, `PTR` — обратное соответствие IP-адреса имени, `NS` — указание DNS-серверов зоны, `SOA` — административная информация о зоне, `MX` — почтовые серверы.

7. **Для чего используется домен in-addr.arpa?**  
   Для обратного DNS-разрешения — поиска доменного имени по IP-адресу.

8. **Для чего нужен демон named?**  
   `named` — сервис BIND, который обрабатывает DNS-запросы и управляет DNS-зонами.

9. **В чём заключаются основные функции slave-сервера и master-сервера?**  
   Master хранит оригинальные файлы зоны, slave получает их копии и обслуживает запросы в режиме репликации.

10. **Какие параметры отвечают за время обновления зоны?**  
   В записи `SOA`: `serial`, `refresh`, `retry`, `expire`, `minimum`.

11. **Как обеспечить защиту зоны от скачивания и просмотра?**  
   Ограничить доступ к зонам через `allow-transfer` и включить ACL.

12. **Какая запись RR применяется при создании почтовых серверов?**  
   `MX` — Mail Exchanger.

13. **Как протестировать работу сервера доменных имён?**  
   С помощью утилит `dig`, `nslookup`, `host`.

14. **Как запустить, перезапустить или остановить службу в системе?**  
   Через `systemctl`: `start`, `restart`, `stop`, `status`.

15. **Как посмотреть отладочную информацию при запуске службы?**  
   Использовать `systemctl status` или `journalctl -xe`.

16. **Где хранится отладочная информация по работе системы и служб? Как её посмотреть?**  
   В журнале systemd (`journalctl`). Просмотр: `journalctl -f`, `journalctl -xe`.

17. **Как посмотреть, какие файлы использует процесс?**  
   Использовать `lsof` или `ls /proc/<PID>/fd`. Например: `lsof -p <PID>`.

18. **Примеры изменения сетевого соединения с помощью nmcli.**  
   Установка DNS: `nmcli connection modify eth0 ipv4.dns 127.0.0.1`  
   Отключение авто-DNS: `nmcli connection modify eth0 ipv4.ignore-auto-dns yes`

19. **Что такое SELinux?**  
   SELinux — механизм принудительного контроля доступа, ограничивающий действия процессов относительно файлов и ресурсов.

20. **Что такое контекст (метка) SELinux?**  
   Метка SELinux определяет политику доступа процесса к объектам и используется системой безопасности.

21. **Как восстановить контекст SELinux после изменений в файлах?**  
   Командой `restorecon -Rv <путь>`.

22. **Как создать разрешающие правила политики SELinux из журналов?**  
   Использовать `audit2allow` для генерации модулей из логов блокировок SELinux.

23. **Что такое булевый переключатель в SELinux?**  
   Это параметр, позволяющий включать или отключать определённые функции политик SELinux.

24. **Как посмотреть список переключателей SELinux и их состояние?**  
   Команда `getsebool -a`.

25. **Как изменить значение переключателя SELinux?**  
   `setsebool имя_переключателя on/off`, для постоянного изменения `setsebool -P`.

# Заключение

В процессе выполнения работы был развёрнут и настроен первичный DNS-сервер на базе BIND. Созданы и сконфигурированы прямая и обратная зоны домена, внесены необходимые записи типа A и PTR. DNS-сервер был включён в автозагрузку, настроены сетевые параметры и межсетевой экран, обеспечена корректная работа SELinux. Проведено тестирование с использованием утилит `dig` и `host`, что подтвердило успешное разрешение доменных имён и функционирование зон.
