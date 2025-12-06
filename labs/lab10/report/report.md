---
## Front matter
title: "Отчёт по лабораторной работе №10"
subtitle: "Расширенные настройки SMTP-сервера"
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

Приобретение практических навыков по конфигурированию SMTP-сервера в части настройки аутентификации.

# Выполнение

## Настройка LMTP в Dovecot и интеграция с Postfix

На виртуальной машине server выполнен вход под пользователем и выполнено получение прав суперпользователя.

В дополнительном терминале был запущен мониторинг журнала maillog для отслеживания работы почтовых сервисов.

В конфигурационном файле `/etc/dovecot/dovecot.conf` добавлен протокол LMTP:

protocols = imap pop3 lmtp

![Добавление LMTP в список протоколов](Screenshot_1.png){ #fig:001 width=80% }

В файле `/etc/dovecot/conf.d/10-master.conf` выполнено определение сервиса LMTP:

service lmtp {  
&nbsp;&nbsp;unix_listener /var/spool/postfix/private/dovecot-lmtp {  
&nbsp;&nbsp;&nbsp;&nbsp;group = postfix  
&nbsp;&nbsp;&nbsp;&nbsp;user = postfix  
&nbsp;&nbsp;&nbsp;&nbsp;mode = 0600  
&nbsp;&nbsp;}  
}

Эта конфигурация создаёт unix-сокет, через который Postfix будет передавать сообщения в Dovecot.

![Настройка unix_listener для LMTP](Screenshot_2.png){ #fig:002 width=80% }

Postfix настроен на передачу писем через LMTP-сокет `private/dovecot-lmtp`.

В файле `/etc/dovecot/conf.d/10-auth.conf` указан формат:

auth_username_format = %Ln

Это позволяет использовать логин без доменной части.

![auth_username_format](Screenshot_3.png){ #fig:003 width=80% }

Postfix и Dovecot были перезапущены для применения конфигурации.

С клиента было отправлено тестовое письмо.  
При мониторинге maillog наблюдалась следующая последовательность:

- получение письма Postfix;
- помещение сообщения в очередь;
- передача его по LMTP в Dovecot;
- успешная доставка в INBOX.

![Логи прохождения письма](Screenshot_4.png){ #fig:004 width=80% }

Почтовый ящик пользователя был просмотрен. Отправленное письмо корректно доставлено в INBOX.

![Просмотр полученных сообщений](Screenshot_5.png){ #fig:005 width=80% }

## Настройка SMTP-аутентификации

В файле `/etc/dovecot/conf.d/10-master.conf` добавлена конфигурация:

service auth {  
&nbsp;&nbsp;unix_listener /var/spool/postfix/private/auth {  
&nbsp;&nbsp;&nbsp;&nbsp;group = postfix  
&nbsp;&nbsp;&nbsp;&nbsp;user = postfix  
&nbsp;&nbsp;&nbsp;&nbsp;mode = 0660  
&nbsp;&nbsp;}  
&nbsp;&nbsp;unix_listener auth-userdb {  
&nbsp;&nbsp;&nbsp;&nbsp;mode = 0600  
&nbsp;&nbsp;&nbsp;&nbsp;user = dovecot  
&nbsp;&nbsp;}  
}

Пояснение:

- service auth — определение службы аутентификации;
- unix_listener /var/spool/postfix/private/auth — сокет для взаимодействия с Postfix;
- group/user postfix — Postfix получает доступ к сокету;
- mode 0660 — разрешения для пользователя и группы;
- auth-userdb — внутренний сокет Dovecot, доступный только самому Dovecot.

![service auth конфигурация](Screenshot_6.png){ #fig:006 width=80% }

Postfix настроен на использование SASL-аутентификации через Dovecot.

В Postfix добавлен набор ограничений:

reject_unknown_recipient_domain  
permit_mynetworks  
reject_non_fqdn_recipient  
reject_unauth_destination  
reject_unverified_recipient  
permit

Пояснение:

- отклонение доменов, которых не существует;
- разрешение для клиентов из mynetworks;
- защита от неполных адресов;
- запрет использования сервера как почтового релея;
- проверка существования получателя;
- разрешение дальнейшей обработки.

![postconf ограничения](Screenshot_7.png){ #fig:007 width=80% }

В Postfix указано, что доверенная сеть — только 127.0.0.0/8.

В файл `/etc/postfix/master.cf` добавлены параметры:

smtp inet n - n - - smtpd  
&nbsp;&nbsp;-o smtpd_sasl_auth_enable=yes  
&nbsp;&nbsp;-o smtpd_recipient_restrictions=reject_non_fqdn_recipient,reject_unknown_recipient_domain,permit_sasl_authenticated,reject

![master.cf настройки](Screenshot_8.png){ #fig:008 width=80% }

Postfix и Dovecot были перезапущены.

На клиенте была получена строка аутентификации в формате Base64.  
После подключения к SMTP-серверу введены команды EHLO и AUTH PLAIN.  
Аутентификация прошла успешно.

![Проверка через telnet](Screenshot_9.png){ #fig:009 width=80% }

## Настройка SMTP over TLS

Для активации TLS использованы временные сертификаты Dovecot.  
Файлы сертификата и приватного ключа были скопированы из каталога `/etc/pki/dovecot` в соответствующие директории `/etc/pki/tls/` с целью корректной работы SELinux.

![Копирование TLS-файлов и настройка Postfix](Screenshot_10.png){ #fig:010 width=80% }

После копирования были выполнены настройки Postfix:

- указаны пути к сертификату и ключу;
- активирована база TLS-сессий;
- установлен уровень безопасности `may` для принимающей и исходящей почты.

В файле `/etc/postfix/master.cf` внесены изменения:

1. Стандартная секция `smtp` оставлена в упрощённом виде — только вызов демона `smtpd`.
2. Добавлена новая секция `submission`, предназначенная для работы по порту 587.

Она включает:

- обязательное шифрование (`smtpd_tls_security_level=encrypt`);
- поддержку SASL-аутентификации;
- ограничения на приём писем, аналогичные ранее настроенным.

![Конфигурация master.cf для 587-го порта](Screenshot_11.png){ #fig:011 width=80% }

Для разрешения работы службы `smtp-submission` выполнены следующие действия:

- просмотр доступных служб;
- включение службы в текущей и постоянной конфигурации;
- перезагрузка параметров firewall.

Это разрешило входящие соединения на порт 587.

Postfix был перезапущен для применения TLS-настроек и параметров порта 587.

На клиенте выполнено подключение к серверу на порту 587 с использованием команды:

- запуск TLS-сессии;
- получение сертификата сервера;
- выполнение команд EHLO и AUTH PLAIN.

Проверка подтвердила успешную аутентификацию.

![Проверка TLS-подключения через openssl](Screenshot_12.png){ #fig:012 width=80% }

На клиентской машине в почтовой программе Evolution были скорректированы параметры SMTP:

- порт: **587**;
- метод защиты: **STARTTLS**;
- тип аутентификации: **обычный пароль**.

После изменения настроек выполнена отправка тестового письма, которое успешно доставлено на сервер.

![Проверка отправки через Evolution](Screenshot_13.png){ #fig:013 width=80% }

При активном мониторинге `/var/log/maillog` зафиксированы следующие действия:

- успешная TLS-аутентификация пользователя;
- получение письма через SMTP-submission;
- передача сообщения Postfix-ом в Dovecot по LMTP;
- успешная доставка сообщения в INBOX.

Логи подтверждают корректность TLS-шифрования и работы SASL.

![Фрагмент логов доставки письма](Screenshot_14.png){ #fig:014 width=80% }

## Внесение изменений в настройки внутреннего окружения виртуальной машины

На сервере выполнен переход в каталог конфигурации и копирование всех настроек Dovecot и Postfix в структуру директорий provisioning-среды.

- скопирован `dovecot.conf`;
- скоплены файлы `10-master.conf` и `10-auth.conf`;
- создан каталог для файлов Postfix и перенесён `master.cf`.

![Копирование конфигурационных файлов в окружение Vagrant](Screenshot_15.png){ #fig:015 width=80% }


- установка необходимых пакетов;
- копирование конфигураций;
- настройка SELinux-контекстов;
- добавление правил firewalld;
- обновлённая конфигурация Postfix (включая mydomain, myorigin, TLS, SASL и др.).

![Расширенная версия mail.sh](Screenshot_16.png){ #fig:016 width=80% }

Также обновлён provisioning-скрипт клиента — туда добавлена установка Evolution и telnet.

![Provisioning-скрипт клиента](Screenshot_17.png){ #fig:017 width=80% }

# Контрольные вопросы

1. **Приведите пример задания формата аутентификации пользователя в Dovecot в форме логина с указанием домена.**
   Для аутентификации в виде *логин@домен* используется переменная `%Lu`, сохраняющая имя пользователя в нижнем регистре вместе с доменной частью.
   Пример строки конфигурации в `10-auth.conf`:

   ```
   auth_username_format = %Lu
   ```

   В этом случае пользователь должен вводить логин полностью, включая домен
   (например: *[user@example.net](mailto:user@example.net)*).

2. **Какие функции выполняет почтовый Relay-сервер?**
   Relay-сервер — это промежуточный SMTP-сервер, передающий сообщения от одного почтового узла к другому. Его основные функции:

   * приём почты от внешних или внутренних клиентов;
   * маршрутизация сообщений к целевым почтовым серверам по SMTP;
   * обеспечение доставки в случае, если конечный сервер временно недоступен (очередь сообщений);
   * контроль и фильтрация трафика (антиспам, антивирус при соответствующей настройке);
   * балансировка нагрузки между несколькими MTA.

3. **Какие угрозы безопасности могут возникнуть при настройке почтового сервера как Relay-сервера?**
   Если сервер неправильно настроен и становится *open relay*, возникают серьёзные риски:

   * злоумышленники могут использовать сервер для массовой спам-рассылки;
   * IP-адрес сервера будет добавлен в чёрные списки, что нарушит доставку легитимной почты;
   * увеличится нагрузка на систему (переполнение очередей, исчерпание ресурсов);
   * возможны атаки типа spoofing — рассылка писем с поддельными адресами отправителей;
   * компрометация репутации домена и нарушение безопасности почтовой инфраструктуры.

   Поэтому важно ограничивать пересылку только доверенным сетям и аутентифицированным пользователям.

# Заключение

В ходе выполненной работы была произведена полная настройка почтовой системы на базе Postfix и Dovecot, включая поддержку LMTP, SMTP-аутентификации и работу SMTP over TLS. Настроены взаимодействие Postfix с Dovecot через unix-сокеты, формат аутентификации пользователей, ограничения на приём почты и параметры безопасности. Проведены проверки доставки сообщений с клиента, а также протестирована работа почтового клиента с использованием защищённого соединения по порту 587.

Дополнительно выполнена интеграция конфигурационных файлов в систему автоматического развёртывания Vagrant, что обеспечивает воспроизводимость и переносимость настроенного окружения. Полученные навыки позволяют уверенно управлять компонентами почтовой инфраструктуры, понимать механизмы взаимодействия MTA и MDA, а также реализовывать безопасные каналы передачи электронной почты.
